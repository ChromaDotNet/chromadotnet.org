Source: https://chromadotnet.org/migrate-from-chroma-cloud-kubernetes/

[Operator](https://chromadotnet.org/operator/)

# Migrate from Chroma Cloud to self-hosted distributed Chroma on Kubernetes

**Disclaimer.** This guide is provided as is, without warranty of any kind, as stated in the Apache License 2.0. Your data, versions and setup may behave differently. Back up your data and test the migration on a copy first. You are responsible for your data and for every migration you run.

Chroma Cloud stops accepting writes on 7 December 2026 and stops serving reads on 22 December 2026 ([Chroma's notice](https://www.trychroma.com/cloud-wind-down)). Distributed Chroma is the open-source, multi-service form of Chroma that Chroma Cloud is built on. It runs on Kubernetes, and it has what open-source Chroma refuses on a single node: the Search API, sparse vectors, BM25, SPANN and collection fork.

[ChromaDB.Operator](https://chromadotnet.org/operator/) installs it for you: one `Chroma` resource gives the components of Chroma's Helm chart `distributed-chroma`, on your Postgres and your S3 bucket. ChromaDotNet is an independent project, not affiliated with Chroma.

Need a single node? See [Migrate from Chroma Cloud to a self-hosted Chroma server (single node, with hybrid search and BM25)](https://vecturadb.org/migrate-from-chroma-cloud/).

## What you need

  * **Nodes with AVX-512.** The published images of the query, compaction and other worker services stop with SIGILL (exit code 132) on a CPU without it. Check the nodes before you start: `kubectl run avx --rm -i --restart=Never --image=busybox:1.36 -- grep -c avx512f /proc/cpuinfo` must print a number above 0. On Azure, `Standard_D4s_v4` has it. Check the quota of the family in your region too: `az vm list-usage --location <region>`.
  * **Admin rights on the cluster.** The manifest of the operator installs a CustomResourceDefinition and a ClusterRole.
  * **A Postgres** for the system catalog, with a database `sysdb`. Its username and password may contain only letters, digits and `. _ ~ -`. Its host is a lowercase DNS name or an IP. For a Postgres that requires TLS, add `sslMode: require` under `postgres`.
  * **An S3 bucket** , on AWS or on an S3-compatible server. For a server inside the cluster, write its endpoint with the IP of its Service, not its name: the services of Chroma address the bucket in the path only when the endpoint is an IP.

The operator checks Postgres and the bucket before it starts anything, and never creates or deletes them. Plan their backups: they hold your data.

## 1\. Install the operator
    
    
    kubectl apply -f https://github.com/ChromaDotNet/ChromaDB.Operator/releases/latest/download/chroma-operator.yaml
    

## 2\. Create the distributed Chroma

The Pods of distributed Chroma use hostPath volumes and have no securityContext, as in Chroma's chart: the namespace must not enforce the Pod Security level `baseline` or `restricted`. One distributed Chroma per namespace.

The credentials go in two Secrets, in the namespace of the Chroma:
    
    
    kubectl create namespace chroma
    kubectl -n chroma create secret generic chroma-postgres --from-literal=username=<user> --from-literal=password=<password>
    kubectl -n chroma create secret generic chroma-storage --from-literal=accessKeyId=<key id> --from-literal=secretAccessKey=<secret>
    
    
    
    apiVersion: chroma.chromadotnet.org/v1alpha1
    kind: Chroma
    metadata:
      name: chroma
      namespace: chroma
    spec:
      mode: distributed
      distributed:
        version: 52ce844
        postgres:
          host: <postgres host>
          credentialsSecret: {name: chroma-postgres}
        objectStorage:
          s3:
            endpoint: http://<IP>:<port>   # remove this line for AWS S3
            bucket: <your bucket>
            region: <region>               # us-east-1 by default
            credentialsSecret: {name: chroma-storage}
    
    
    
    kubectl apply -f chroma.yaml
    kubectl -n chroma get chroma chroma
    

`version` is the commit of Chroma whose images run: the operator knows `52ce844` (chart 0.1.94) and `26e51e7` (chart 0.1.93). The operator migrates Postgres with a Job, then starts the components tier by tier, the frontend last. The Chroma is `Ready` when they all are. If Postgres or the bucket is wrong, the conditions `PostgresReady` and `ObjectStorageReady` say why.

For a trial, the end-to-end tests of the operator have a Postgres and an S3 server in one file, [distributed-deps.yaml](https://github.com/ChromaDotNet/ChromaDB.Operator/blob/v0.1.3/tests/ChromaDotNet.Operator.E2E/distributed-deps.yaml). It creates the Secrets `chroma-postgres` and `chroma-storage` too, and overwrites those of the step above. Its data is in `emptyDir` volumes, so it is not for your data.

## 3\. Reach the frontend

From the machine that runs the copy:
    
    
    kubectl -n chroma port-forward svc/rust-frontend-service 8000:8000
    

Inside the cluster, clients use `http://rust-frontend-service.chroma.svc:8000`, which is also in `status.endpoint`.

## 4\. Copy your collections

The `chroma` CLI is inside the image `chromadb/chroma:1.5.9`. It needs your Chroma Cloud API key and the name of your database:
    
    
    export CHROMA_API_KEY=<your Chroma Cloud API key>
    docker run --rm -e CHROMA_API_KEY --entrypoint bash chromadb/chroma:1.5.9 -c \
      'chroma login --api-key "$CHROMA_API_KEY" --profile cloud &&
       chroma copy --from-cloud --to-local --host http://host.docker.internal:8000 --db <database> --collections <name> --collections <name>'
    

`host.docker.internal` reaches the port-forward of the host with Docker Desktop. Repeat `--collections` once per collection:

  * `--all` does not work against Chroma Cloud with this CLI: it asks for 10000 collections in one call, and Chroma Cloud answers `422 Quota exceeded`. Our script [list_for_copy.py](https://github.com/VecturaDB/VecturaDB/blob/96f2fff1e412e3e6d71594de9d2fbf7edd86b965/migration/leaving-chroma-cloud/list_for_copy.py) prints the `--collections` options for every collection of a database.
  * The CLI exits with code 0 even when the copy fails. Read its output: each collection ends with `Copy Completed!` or with an error.

What arrives: records, documents, metadata, embeddings, URIs, the metadata of each collection, its schema with sparse and BM25 indexes, and its SPANN settings.

Collections created with the old `hnsw:*` settings in their metadata fail: `Cannot set both collection config and schema simultaneously`. Copy those with our script [copy_without_sparse.py](https://github.com/VecturaDB/VecturaDB/blob/96f2fff1e412e3e6d71594de9d2fbf7edd86b965/migration/leaving-chroma-cloud/copy_without_sparse.py). It writes the sparse vectors to a file instead of the collection, so use it only for those collections.

## 5\. Check the copy

[verify.py](https://github.com/VecturaDB/VecturaDB/blob/96f2fff1e412e3e6d71594de9d2fbf7edd86b965/migration/leaving-chroma-cloud/verify.py) compares a collection on both sides: count, ids, documents, metadata with the sparse vectors, URIs, embeddings, and the distances of three dense queries.
    
    
    curl -O https://raw.githubusercontent.com/VecturaDB/VecturaDB/96f2fff1e412e3e6d71594de9d2fbf7edd86b965/migration/leaving-chroma-cloud/verify.py
    pip install chromadb==1.5.9
    export CHROMA_API_KEY=<key> CHROMA_TENANT=<tenant> CHROMA_DATABASE=<database>
    python verify.py 127.0.0.1 8000 <collection> <collection>
    

## 6\. Point your app at the cluster

Only the line that creates the client changes. The collections are in the default tenant and database. With a port-forward use `127.0.0.1`: with Docker Desktop on Windows, `localhost` can go to the IPv6 port.

### Python
    
    
    import chromadb
    
    # Before:
    # client = chromadb.CloudClient(api_key="...", tenant="...", database="...")
    client = chromadb.HttpClient(host="127.0.0.1", port=8000)
    

### JavaScript
    
    
    import { ChromaClient } from "chromadb";
    
    // Before:
    // const client = new CloudClient({ apiKey: "...", tenant: "...", database: "..." });
    const client = new ChromaClient({ host: "127.0.0.1", port: 8000, ssl: false });
    

### .NET

[ChromaDotNet.Client](https://chromadotnet.org/libraries/client/):
    
    
    using ChromaDB.Client;
    
    // Before:
    // var options = ChromaConfigurationOptions.FromConnectionString("Endpoint=https://api.trychroma.com;Token=...;Tenant=...;Database=...");
    var options = ChromaConfigurationOptions.FromConnectionString("Endpoint=http://127.0.0.1:8000");
    using var http = new HttpClient();
    var client = new ChromaClient(options, http);
    

### API keys

Distributed Chroma has no API keys. Keep it inside your network, or behind a proxy that checks a key.

### Hosted embedding models

`chroma-cloud-qwen` and `chroma-cloud-splade` run on the servers of Chroma Cloud and stop with it. See [Hosted embedding models](https://vecturadb.org/leaving-chroma-cloud/hosted-embedding-models): the same open models, run locally.

## Upgrades

A new `version` in the spec runs the migration Job, then updates the components tier by tier. Going back to an older version is refused (`Downgrade`), since Postgres was migrated. If a change of the Chroma is not applied, see [Known limitations](https://github.com/ChromaDotNet/ChromaDB.Operator#known-limitations).

When you delete the Chroma with the operator stopped, the MemberLists stay: `kubectl -n chroma delete memberlists --all`.

The operator is v0.1, and its distributed mode is a preview: the API is `v1alpha1` and can change. The full documentation is in the [README](https://github.com/ChromaDotNet/ChromaDB.Operator#a-distributed-chroma).
