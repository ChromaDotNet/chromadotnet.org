Source: https://chromadotnet.org/vecturadb/

[Home](https://chromadotnet.org/)

# VecturaDB

VecturaDB is a single-node server built from Chroma: a complement of Chroma, not a substitute. It adds sparse vectors, BM25, the Search API and collection fork to the single node of Chroma. It serves the Chroma API, so ChromaDotNet.Client works with it as it does with Chroma.

  * Latest release **1.0.0-preview.3.post1**
  * [vecturadb.org](https://vecturadb.org/)
  * [GitHub](https://github.com/VecturaDB/VecturaDB)
  * [Docker image](https://github.com/VecturaDB/VecturaDB/pkgs/container/vecturadb)
  * [Leaving Chroma Cloud](https://vecturadb.org/leaving-chroma-cloud/)

## Example

Start VecturaDB with Docker:

`docker run -p 8000:8000 -v ./data:/data ghcr.io/vecturadb/vecturadb:preview`

Then use [ChromaDotNet.Client](https://chromadotnet.org/libraries/client/) as with Chroma:

Program.cs
    
    
    using ChromaDB.Client;
    using ChromaDB.Client.Models;
    
    using var client = new ChromaClient("http://localhost:8000");
    var collection = await client.GetOrCreateCollectionAsync("fruits");
    var fruits = client.GetCollectionClient(collection);
    
    await fruits.AddAsync(["apple", "lemon"],
        embeddings: [new([1f, 0f, 0f]), new([0f, 1f, 0f])],
        documents: ["A crunchy apple", "A sour lemon"]);
    
    var results = await fruits.QueryAsync(new ReadOnlyMemory<float>([0f, 0.9f, 0.1f]),
        nResults: 1, include: ChromaQueryInclude.Documents | ChromaQueryInclude.Distances);
    
    foreach (var result in results)
        Console.WriteLine($"{result.Id}: {result.Document} ({result.Distance})");

Tested on 28 October 2026 with ChromaDotNet.Client 2.18.1 on VecturaDB 1.0.0-preview.3.post1.

## Without Docker

ChromaDotNet.Local.VecturaDB, the engine of [ChromaDotNet.Local](https://chromadotnet.org/libraries/local/) for VecturaDB, is not on NuGet yet.

VecturaDB is not affiliated with Chroma.
