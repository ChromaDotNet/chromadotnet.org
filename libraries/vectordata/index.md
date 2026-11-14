Source: https://chromadotnet.org/libraries/vectordata/

[Libraries](https://chromadotnet.org/libraries/)

# CommunityToolkit.VectorData.Chroma

The Chroma provider for Microsoft.Extensions.VectorData, in the AI Community Toolkit of the .NET Foundation, built on ChromaDotNet.Client. Use it with Semantic Kernel, Agent Framework or any code written against the abstractions.

  * Latest release **1.1.0-preview**
  * [NuGet](https://www.nuget.org/packages/CommunityToolkit.VectorData.Chroma)
  * [GitHub](https://github.com/CommunityToolkit/AI/tree/main/MEVD/src/Chroma)
  * [Documentation](https://github.com/CommunityToolkit/AI/blob/main/MEVD/src/Chroma/README.md)
  * [Microsoft.Extensions.VectorData](https://learn.microsoft.com/dotnet/ai/vector-stores/overview)

## Install

`dotnet add package CommunityToolkit.VectorData.Chroma --prerelease`

## Example

Start Chroma, for example with Docker:

`docker run -p 8000:8000 chromadb/chroma:1.5.9`

Then store two records with a data model and search the nearest one:

Program.cs
    
    
    using ChromaDB.Client;
    using CommunityToolkit.VectorData.Chroma;
    using Microsoft.Extensions.VectorData;
    
    using var store = new ChromaVectorStore(new ChromaClient("http://localhost:8000"), ownsClient: true);
    var fruits = store.GetCollection<string, Fruit>("fruits");
    await fruits.EnsureCollectionExistsAsync();
    
    await fruits.UpsertAsync([
        new Fruit { Key = "apple", Text = "A crunchy apple", Vector = new[] { 1f, 0f, 0f } },
        new Fruit { Key = "lemon", Text = "A sour lemon", Vector = new[] { 0f, 1f, 0f } },
    ]);
    
    await foreach (var result in fruits.SearchAsync(new ReadOnlyMemory<float>([0f, 0.9f, 0.1f]), top: 1))
        Console.WriteLine($"{result.Record.Key}: {result.Record.Text} ({result.Score})");
    
    class Fruit
    {
        [VectorStoreKey] public string Key { get; set; } = "";
        [VectorStoreData] public string Text { get; set; } = "";
        [VectorStoreVector(3)] public ReadOnlyMemory<float> Vector { get; set; }
    }

Tested on 14 November 2026 with CommunityToolkit.VectorData.Chroma 1.1.0-preview on Chroma 1.5.9.

With dependency injection, `AddChromaVectorStore` registers the vector store.

The limitations on Chroma Cloud and on self-hosted Chroma are in the [README](https://github.com/CommunityToolkit/AI/blob/main/MEVD/src/Chroma/README.md#limitations).

## Used with

Recommended by [Semantic Kernel](https://github.com/microsoft/semantic-kernel/blob/7e68276a4a85022dd4da255beae3263ab5cbf059/dotnet/src/VectorData/Chroma/README.md#L1) and listed among the [.NET community projects](https://learn.microsoft.com/en-us/agent-framework/integrations/community-projects#context-providers) of Microsoft Agent Framework.

Samples: [Semantic Kernel](https://chromadotnet.org/samples/semantic-kernel/) and [Microsoft Agent Framework](https://chromadotnet.org/samples/agent-framework/).
