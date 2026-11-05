Source: https://chromadotnet.org/operator/

[Home](https://chromadotnet.org/)

# ChromaDB.Operator

A Kubernetes operator for Chroma 1.x, single-node and distributed, and for [VecturaDB](https://chromadotnet.org/vecturadb/) 1.0.0-preview.3, single-node, written in .NET with KubeOps. v0.1: the API is `v1alpha1` and can change. The distributed mode and VecturaDB are previews.

  * Latest release **v0.1.2**
  * [GitHub](https://github.com/ChromaDotNet/ChromaDB.Operator)
  * [Releases](https://github.com/ChromaDotNet/ChromaDB.Operator/releases)
  * [Documentation](https://github.com/ChromaDotNet/ChromaDB.Operator#readme)
  * [Image](https://github.com/ChromaDotNet/ChromaDB.Operator/pkgs/container/chroma-operator)
  * [OpenSSF Best Practices: passing](https://www.bestpractices.dev/projects/15903)

## Install

The manifest of the latest release:

`kubectl apply -f https://github.com/ChromaDotNet/ChromaDB.Operator/releases/latest/download/chroma-operator.yaml`

Its image, `ghcr.io/chromadotnet/chroma-operator:0.1.2`, is public, for amd64 and arm64, with SLSA provenance and a Sigstore signature. To verify it:

`gh attestation verify oci://ghcr.io/chromadotnet/chroma-operator:0.1.2 --owner ChromaDotNet`

## Example

A single-node Chroma with a 1 GiB volume:

demo.yaml
    
    
    apiVersion: chroma.chromadotnet.org/v1alpha1
    kind: Chroma
    metadata:
      name: demo
    spec:
      image: docker.io/chromadb/chroma:1.5.9
      storage:
        size: 1Gi
      resources:
        requests: {cpu: 100m, memory: 256Mi}
        limits: {memory: 1Gi}

`kubectl apply -f demo.yaml`

The operator creates the PersistentVolumeClaim, the Pod and the Service. When the Chroma is ready, `status.endpoint` gives its address, like `http://demo-rw.<namespace>.svc:8000`.

Tested on 28 October 2026 with the release v0.1.2 on k3d: the Chroma was ready in 8 s and answered on its Service. The end-to-end tests of the operator apply the same manifest and find all 1,000 records written through the Service.

Not yet: backups, resize of the claim, hibernation, metrics, a Helm chart, authentication.
