Source: https://chromadotnet.org/compatibility/

# ChromaDotNet.Client conformance: Chroma single server and Chroma Cloud

  * **Client:** ChromaDotNet.Client 2.18.1. The checks ran on 2.18.0, which has the same code: 2.18.1 changes only the descriptions, the README and the icon of the packages
  * **Chroma:** `chromadb/chroma:1.5.9`, a single server in Docker
  * **Chroma Cloud:** AWS us-east-1, run on 2026-10-21 with the client 2.18.0 on .NET 10.0.13; every collection and database deleted at the end
  * **OpenTelemetry SDK:** 1.19.1; **ChromaDotNet.Testcontainers:** 0.1.7 (Testcontainers 4.15.0)
  * **Chroma image digest:** chromadb/chroma@sha256:1e0b73a187a28757c572acba508c46f48c9e8b0acaf5c20e6d95cdedce1acdf6
  * **Docker:** 29.7.2 (linux/amd64)
  * **.NET SDK:** 10.0.401
  * **Host:** Windows 10.0.26200, x86_64
  * **Client package:** the 2.18.0 packages of nuget.org (same SHA-512): ChromaDotNet.Client 2sknDkLr4sS9pcffsF5PBZOUFaszaEhWyRAFWvMm6mAR+2sSzoNY5vdBnQTSq9Unwm5Fmf1Lpda1ZD1B27GuUw==, ChromaDotNet.Client.DependencyInjection GLSman3n/ULch5D8EIhaBkOwQQK0Hm0K8D38w0sn1XmTykUli3HXpMSWY0GEb7moy+9PEUVOry7mGw6z827Lrg==
  * **Arm64:** the Linux arm64 images of .NET, emulated by Docker on this x86-64 host
  * **Environments:** 31, 1937 checks run
  * **Generated:** 2026-10-21 13:22 UTC

✓ every check passed · ✗ a check failed · — not on a single server: the Chroma docs put it in Chroma Cloud, and the check verified that the client reports the error of the server as documented · on Chroma Cloud, — is what an API key cannot do, with the error the check verified · ¹ a note under the table · a blank cell: the row is not about that column (the quotas of Chroma Cloud on a single server; on Chroma Cloud, the checks with no server or with a local container; a feature the build of the client for that column does not have, as the embeddings of a generator on .NET Core 3.1 to 7, which take the build for .NET Standard 2.0)

## By feature

Chroma docs section | Feature | Windows · .NET Framework 4.6.2, 4.8 · x64, x86 | Windows · .NET Core 3.1, .NET 5, 6, 7 · x64 | Windows · .NET 8, 9, 10 · x64, x86 | Linux (Debian 11, Debian 12, Ubuntu 24.04) · .NET 6, 8, 9, 10 · x64 | Alpine 3.24 (musl) · .NET 8, 10 · x64 | Linux (Debian 12, Ubuntu 24.04) · .NET 8, 10 · arm64 | NativeAOT, Ubuntu 24.04 · .NET 10 · x64 | Chroma Cloud, AWS us-east-1 · .NET 10 · x64  
---|---|---|---|---|---|---|---|---|---  
**Run Chroma** | Clients: local server, Chroma Cloud, authentication | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓  
| Tenants and databases | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓¹  
**Collections** | Manage collections | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓  
| Add, update, delete data (large writes in batches) | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓  
| Configure: HNSW, SPANN, embedding function | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓  
| collections created when missing (lazy and now) | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓  
**Querying collections** | Query and get: l2, cosine, ip | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓  
| Metadata filtering, array metadata | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓  
| Full-text search and regex | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓  
**Chroma Cloud** | Collection forking | — | — | — | — | — | — | — | ✓  
| Schema: sparse vector search, BM25 | — | — | — | — | — | — | — | ✓  
| Search API (basics, filtering, ranking, group by, hybrid, pagination, batch) | — | — | — | — | — | — | — | ✓  
| Quotas & Limits: the client keeps to them or reports the error of Chroma |  |  |  |  |  |  |  | ✓  
**Beyond the Chroma docs** | OpenTelemetry traces and metrics | ✓ | ✓³ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓  
| Cancellation, timeouts, concurrent calls | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓  
| Dependency injection | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓  
| Testcontainers module | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |   
| Mocks in tests | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |   
| BM25 vectors computed by the client | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |   
| Search and filter requests built by the client | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |   
**Embeddings** | embeddings from a generator (the builds for .NET 8 and .NET Framework) | ✓ |  | ✓ | ✓ | ✓ | ✓ | ✓ | ✓  
  
¹ On cloud_aws-us-east-1, “create and get a tenant” is not available: an API key of Chroma Cloud cannot create a tenant: Chroma Cloud answers 403 "Permission denied.", which the client throws as a ChromaException; on cloud_aws-us-east-1, “collections in another tenant and database” is not available: an API key of Chroma Cloud cannot create a tenant for the collections of another tenant: Chroma Cloud answers 403 "Permission denied.", which the client throws as a ChromaException.  
³ On win_netcoreapp3.1, win_netcoreapp3.1_ja-JP, “the duration histogram and its buckets, with the OpenTelemetry SDK” is not available: the OpenTelemetry SDK exports no metric on this runtime, also from a meter of its own. 

## Same answers in every environment

The seeded fuzz of the test bench sends the same random requests from each environment to a fresh Chroma 1.5.9 and compares what comes back, suite by suite, with the answers on win_net10.0: 30 environments, two seeds. ✓ the same answers in every environment of the column, ✗ different somewhere.

Suite | Windows · .NET Framework 4.6.2, 4.8 · x64, x86 | Windows · .NET Core 3.1, .NET 5, 6, 7 · x64 | Windows · .NET 8, 9, 10 · x64, x86 | Linux (Debian 11, Debian 12, Ubuntu 24.04) · .NET 6, 8, 9, 10 · x64 | Alpine 3.24 (musl) · .NET 8, 10 · x64 | Linux (Debian 12, Ubuntu 24.04) · .NET 8, 10 · arm64 | NativeAOT, Ubuntu 24.04 · .NET 10 · x64  
---|---|---|---|---|---|---|---  
bm25 | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓  
concurrency | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓  
documents | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓  
edges | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓  
embeddings | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓  
filters | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓  
metadata | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓  
names | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓  
paging | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓  
serialization | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓  
  
## Environments

Column | Environment | System | Architecture | Runtime | Client build | Culture | Checks  
---|---|---|---|---|---|---|---  
Windows · .NET Framework 4.6.2, 4.8 · x64, x86 | win_net462 | Microsoft Windows 10.0.26200 | X64 | .NET Framework 4.8.9363.0 | .NETFramework,Version=v4.6.2 | en-GB | 60 ✓ · 3 —  
Windows · .NET Framework 4.6.2, 4.8 · x64, x86 | win32_net48 | Microsoft Windows 10.0.26200 | X86 | .NET Framework 4.8.9363.0 | .NETFramework,Version=v4.6.2 | en-GB | 60 ✓ · 3 —  
Windows · .NET Framework 4.6.2, 4.8 · x64, x86 | win_net48 | Microsoft Windows 10.0.26200 | X64 | .NET Framework 4.8.9363.0 | .NETFramework,Version=v4.6.2 | en-GB | 60 ✓ · 3 —  
Windows · .NET Framework 4.6.2, 4.8 · x64, x86 | win_net48_de-DE | Microsoft Windows 10.0.26200 | X64 | .NET Framework 4.8.9363.0 | .NETFramework,Version=v4.6.2 | de-DE | 60 ✓ · 3 —  
Windows · .NET Framework 4.6.2, 4.8 · x64, x86 | win_net48_fa-IR | Microsoft Windows 10.0.26200 | X64 | .NET Framework 4.8.9363.0 | .NETFramework,Version=v4.6.2 | fa-IR | 60 ✓ · 3 —  
Windows · .NET Framework 4.6.2, 4.8 · x64, x86 | win_net48_tr-TR | Microsoft Windows 10.0.26200 | X64 | .NET Framework 4.8.9363.0 | .NETFramework,Version=v4.6.2 | tr-TR | 60 ✓ · 3 —  
Windows · .NET Core 3.1, .NET 5, 6, 7 · x64 | win_netcoreapp3.1 | Microsoft Windows 10.0.26200 | X64 | .NET Core 3.1.28 | .NETStandard,Version=v2.0 | en-GB | 55 ✓ · 4 —  
Windows · .NET Core 3.1, .NET 5, 6, 7 · x64 | win_netcoreapp3.1_ja-JP | Microsoft Windows 10.0.26200 | X64 | .NET Core 3.1.28 | .NETStandard,Version=v2.0 | ja-JP | 55 ✓ · 4 —  
Windows · .NET Core 3.1, .NET 5, 6, 7 · x64 | win_net5.0 | Microsoft Windows 10.0.26200 | X64 | .NET 5.0.16 | .NETStandard,Version=v2.0 | en-GB | 56 ✓ · 3 —  
Windows · .NET Core 3.1, .NET 5, 6, 7 · x64 | win_net6.0 | Microsoft Windows 10.0.26200 | X64 | .NET 6.0.6 | .NETStandard,Version=v2.0 | en-GB | 56 ✓ · 3 —  
Windows · .NET Core 3.1, .NET 5, 6, 7 · x64 | win_net6.0_ar-SA | Microsoft Windows 10.0.26200 | X64 | .NET 6.0.6 | .NETStandard,Version=v2.0 | ar-SA | 56 ✓ · 3 —  
Windows · .NET Core 3.1, .NET 5, 6, 7 · x64 | win_net7.0 | Microsoft Windows 10.0.26200 | X64 | .NET 7.0.20 | .NETStandard,Version=v2.0 | en-GB | 56 ✓ · 3 —  
Windows · .NET 8, 9, 10 · x64, x86 | win32_net8.0 | Microsoft Windows 10.0.26200 | X86 | .NET 8.0.31 | .NETCoreApp,Version=v8.0 | en-GB | 60 ✓ · 3 —  
Windows · .NET 8, 9, 10 · x64, x86 | win_net8.0 | Microsoft Windows 10.0.26200 | X64 | .NET 8.0.32 | .NETCoreApp,Version=v8.0 | en-GB | 60 ✓ · 3 —  
Windows · .NET 8, 9, 10 · x64, x86 | win_net8.0_de-DE | Microsoft Windows 10.0.26200 | X64 | .NET 8.0.32 | .NETCoreApp,Version=v8.0 | de-DE | 60 ✓ · 3 —  
Windows · .NET 8, 9, 10 · x64, x86 | win_net9.0 | Microsoft Windows 10.0.26200 | X64 | .NET 9.0.20 | .NETCoreApp,Version=v8.0 | en-GB | 60 ✓ · 3 —  
Windows · .NET 8, 9, 10 · x64, x86 | win32_net10.0 | Microsoft Windows 10.0.26200 | X86 | .NET 10.0.12 | .NETCoreApp,Version=v8.0 | en-GB | 60 ✓ · 3 —  
Windows · .NET 8, 9, 10 · x64, x86 | win_net10.0 | Microsoft Windows 10.0.26200 | X64 | .NET 10.0.13 | .NETCoreApp,Version=v8.0 | en-GB | 60 ✓ · 3 —  
Windows · .NET 8, 9, 10 · x64, x86 | win_net10.0_th-TH | Microsoft Windows 10.0.26200 | X64 | .NET 10.0.13 | .NETCoreApp,Version=v8.0 | th-TH | 60 ✓ · 3 —  
Windows · .NET 8, 9, 10 · x64, x86 | win_net10.0_tr-TR | Microsoft Windows 10.0.26200 | X64 | .NET 10.0.13 | .NETCoreApp,Version=v8.0 | tr-TR | 60 ✓ · 3 —  
Linux (Debian 11, Debian 12, Ubuntu 24.04) · .NET 6, 8, 9, 10 · x64 | linux_net6.0 | Debian GNU/Linux 11 (bullseye) | X64 | .NET 6.0.36 | .NETStandard,Version=v2.0 | invariant | 56 ✓ · 3 —  
Linux (Debian 11, Debian 12, Ubuntu 24.04) · .NET 6, 8, 9, 10 · x64 | linux_net8.0 | Debian GNU/Linux 12 (bookworm) | X64 | .NET 8.0.31 | .NETCoreApp,Version=v8.0 | invariant | 60 ✓ · 3 —  
Linux (Debian 11, Debian 12, Ubuntu 24.04) · .NET 6, 8, 9, 10 · x64 | linux_net9.0 | Debian GNU/Linux 12 (bookworm) | X64 | .NET 9.0.20 | .NETCoreApp,Version=v8.0 | invariant | 60 ✓ · 3 —  
Linux (Debian 11, Debian 12, Ubuntu 24.04) · .NET 6, 8, 9, 10 · x64 | linux_net10.0 | Ubuntu 24.04.5 LTS | X64 | .NET 10.0.12 | .NETCoreApp,Version=v8.0 | invariant | 60 ✓ · 3 —  
Linux (Debian 11, Debian 12, Ubuntu 24.04) · .NET 6, 8, 9, 10 · x64 | linux_net10.0_tr-TR | Ubuntu 24.04.5 LTS | X64 | .NET 10.0.12 | .NETCoreApp,Version=v8.0 | tr-TR | 60 ✓ · 3 —  
Alpine 3.24 (musl) · .NET 8, 10 · x64 | alpine_net8.0 | Alpine Linux v3.24 | X64 | .NET 8.0.31 | .NETCoreApp,Version=v8.0 | invariant | 60 ✓ · 3 —  
Alpine 3.24 (musl) · .NET 8, 10 · x64 | alpine_net10.0 | Alpine Linux v3.24 | X64 | .NET 10.0.12 | .NETCoreApp,Version=v8.0 | invariant | 60 ✓ · 3 —  
Linux (Debian 12, Ubuntu 24.04) · .NET 8, 10 · arm64 | arm64_net8.0 | Debian GNU/Linux 12 (bookworm) | Arm64 | .NET 8.0.31 | .NETCoreApp,Version=v8.0 | invariant | 60 ✓ · 3 —  
Linux (Debian 12, Ubuntu 24.04) · .NET 8, 10 · arm64 | arm64_net10.0 | Ubuntu 24.04.5 LTS | Arm64 | .NET 10.0.12 | .NETCoreApp,Version=v8.0 | invariant | 60 ✓ · 3 —  
NativeAOT, Ubuntu 24.04 · .NET 10 · x64 | aot_net10.0 | Ubuntu 24.04.5 LTS | X64 | .NET 10.0.12, NativeAOT | .NETCoreApp,Version=v8.0 | invariant | 60 ✓ · 3 —  
Chroma Cloud, AWS us-east-1 · .NET 10 · x64 | cloud_aws-us-east-1 | Chroma Cloud, AWS us-east-1; the client on Microsoft Windows 10.0.26200 | X64 | .NET 10.0.13 | .NETCoreApp,Version=v8.0 | en-GB | 73 ✓ · 2 —  
  
## Checks

### Clients: local server, Chroma Cloud, authentication

  * ✓ the address alone, with /api/v2 and with /api/v2/
  * ✓ constructors: uri, options, options and HttpClient
  * ✓ connection strings
  * ✓ Chroma Cloud at another address: WithChromaCloud
  * ✓ version, heartbeat, healthcheck, pre-flight checks, identity
  * ✓ authentication headers: X-Chroma-Token, Bearer, Basic
  * ✓ the v1 API on Chroma 1.x answers 410 Gone
  * ✓ errors: status, error type, CollectionExistsAsync

### Tenants and databases

  * ✓ create and get a tenant; — on cloud_aws-us-east-1: an API key of Chroma Cloud cannot create a tenant: Chroma Cloud answers 403 "Permission denied.", which the client throws as a ChromaException
  * ✓ databases: create, get, list with paging, delete
  * ✓ collections in another tenant and database; — on cloud_aws-us-east-1: an API key of Chroma Cloud cannot create a tenant for the collections of another tenant: Chroma Cloud answers 403 "Permission denied.", which the client throws as a ChromaException

### Manage collections

  * ✓ create, get, get or create, exists, delete
  * ✓ list with paging, count
  * ✓ modify name and metadata
  * ✓ by id, collection clients, dimension, version
  * ✓ names and metadata the client stops: dots, lists, lone surrogates
  * ✓ deleteRecordsFirst when the records stay after their delete: an error, the collection kept

### Add, update, delete data (large writes in batches)

  * ✓ add and read back embeddings, metadata, documents, URIs
  * ✓ metadata without keys: no metadata; a mixed list: stopped; a record without keys reads back empty
  * ✓ a write that fails after some of its batches: how many records went, not run again
  * ✓ update, upsert, null deletes a key
  * ✓ 12,000 records: written in batches, read in pages
  * ✓ batches: one request, and a smaller size
  * ✓ delete: by ids, by filters, with a limit
  * ✓ count at a read level

### Configure: HNSW, SPANN, embedding function

  * ✓ space: l2, cosine, inner product
  * ✓ HNSW settings, changed after the creation
  * ✓ SPANN on a single server: the client tells
  * ✓ embedding function declared
  * ✓ schema: value indexes off, full-text search off, space
  * ✓ index settings, changed after the creation: SPANN on Chroma Cloud
  * ✓ the index Chroma Cloud does not use: HNSW, the client tells

### collections created when missing (lazy and now)

  * ✓ lazy: no request until the first operation, which creates the collection
  * ✓ lazy: first operations at the same time share one request
  * ✓ now: GetOrCreateCollectionClientAsync creates it at once, and keeps an existing one
  * ✓ with a definition: the metadata and the space of the definition
  * ✓ deleted elsewhere: the next operation creates it again, empty, with the definition, once
  * ✓ createIfMissing: false never creates the collection

### Query and get: l2, cosine, ip

  * ✓ nearest records and distances, in each space
  * ✓ several queries, ids, filters, include, ChromaQuery
  * ✓ get: one, many, missing, limit and offset, include
  * ✓ floats the server cannot write: read as NaN or infinity, the other records intact

### Metadata filtering, array metadata

  * ✓ every operator on ints, floats, strings, bools
  * ✓ & and |, nested, long chains
  * ✓ a condition on the ids with a repeated id: each id sent once
  * ✓ lists in metadata: $contains, $not_contains
  * ✓ metadata values read exactly, or inferred

### Full-text search and regex

  * ✓ $contains, $not_contains, combined
  * ✓ $regex, $not_regex

### Collection forking

  * ✓ ForkAsync and ForkCountAsync; — on alpine_net10.0, alpine_net8.0, aot_net10.0, arm64_net10.0, arm64_net8.0, linux_net10.0, linux_net10.0_tr-TR, linux_net6.0, linux_net8.0, linux_net9.0, win32_net10.0, win32_net48, win32_net8.0, win_net10.0, win_net10.0_th-TH, win_net10.0_tr-TR, win_net462, win_net48, win_net48_de-DE, win_net48_fa-IR, win_net48_tr-TR, win_net5.0, win_net6.0, win_net6.0_ar-SA, win_net7.0, win_net8.0, win_net8.0_de-DE, win_net9.0, win_netcoreapp3.1, win_netcoreapp3.1_ja-JP: Chroma docs: "Forking is available in Chroma Cloud only"; a single server answers 501, which the client throws as a ChromaException

### Schema: sparse vector search, BM25

  * — a sparse vector index in the schema: Chroma docs: sparse vector search is in the Chroma Cloud section; a single server answers 400, which the client throws as a ChromaException
  * ✓ a sparse vector index with BM25: written, read back, searched

### Search API (basics, filtering, ranking, group by, hybrid, pagination, batch)

  * — SearchAsync: Chroma docs: "Search API is available in Chroma Cloud only"; a single server answers 501, which the client throws as a ChromaException
  * ✓ basics: knn, limit, select
  * ✓ filtering: where, where_document, ids
  * ✓ ranking: sums and products of rankings
  * ✓ group by: the nearest of each category
  * ✓ hybrid: RRF of a dense and a BM25 ranking
  * ✓ pagination: limit and offset
  * ✓ batch: several searches in one request

### Quotas & Limits: the client keeps to them or reports the error of Chroma

  * ✓ 300 records per write: the client splits; one request beyond gets the error
  * ✓ 300 results: a get reads beyond in pages; a query or search beyond
  * ✓ 10 reads or writes at a time per collection: 30 at once
  * ✓ 8 predicates per filter
  * ✓ 256 characters in a full-text search or a regex
  * ✓ 4,096 dimensions
  * ✓ 16,384 bytes per document
  * ✓ 128 bytes per id, collection name and database name
  * ✓ 8,182 bytes per metadata value
  * ✓ 32 metadata keys

### OpenTelemetry traces and metrics

  * ✓ a span for each operation, with the attributes of the conventions
  * ✓ the duration histogram and its buckets, with the OpenTelemetry SDK; — on win_netcoreapp3.1, win_netcoreapp3.1_ja-JP: the OpenTelemetry SDK exports no metric on this runtime, also from a meter of its own

### Cancellation, timeouts, concurrent calls

  * ✓ a canceled token, a timeout
  * ✓ tasks at the same time on one client: 24, or 10 on Chroma Cloud

### Dependency injection

  * ✓ AddChromaClient, AddKeyedChromaClient, CreateChromaClient

### Testcontainers module

  * ✓ a Chroma container from ChromaDotNet.Testcontainers

### Mocks in tests

  * ✓ a subclass without a server

### BM25 vectors computed by the client

  * ✓ the vectors of the Python client of Chroma for 216 texts

### Search and filter requests built by the client

  * ✓ the JSON of ranks and filters

### embeddings from a generator (the builds for .NET 8 and .NET Framework)

  * ✓ add, update and upsert with documents only: the embeddings of the generator
  * ✓ query by text: the records of the query by its embedding
  * ✓ the With methods of the collection client keep the generator
  * ✓ a new collection with the generator of Chroma declares the function default
