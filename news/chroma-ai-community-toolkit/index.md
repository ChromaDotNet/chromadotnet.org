Source: https://chromadotnet.org/news/chroma-ai-community-toolkit/

[News](https://chromadotnet.org/news/)

# The Chroma provider for Microsoft.Extensions.VectorData joins the AI Community Toolkit

Federico Laurianti, 13 October 2026, updated 25 October 2026

_Updated on 25 October 2026: the latest version of the package, and the addresses of the examples._

The [Chroma](https://www.trychroma.com/) provider for Microsoft.Extensions.VectorData is now in the [AI Community Toolkit](https://github.com/CommunityToolkit/AI), the .NET Foundation project that brings together the providers for the vector abstractions of .NET. The package is [CommunityToolkit.VectorData.Chroma](https://www.nuget.org/packages/CommunityToolkit.VectorData.Chroma), version 1.0.1-preview.

I wrote it in [ChromaDotNet.VectorData](https://github.com/ChromaDotNet/ChromaDB.VectorData) and proposed it to the toolkit, where it was accepted after review. Since 9 October it has been developed there.

## What the AI Community Toolkit is

The [AI Community Toolkit](https://github.com/CommunityToolkit/AI) is the [.NET Foundation](https://dotnetfoundation.org) project where the providers of [Microsoft.Extensions.VectorData](https://learn.microsoft.com/dotnet/ai/vector-stores/overview) live: the .NET abstractions for vector stores used by [Semantic Kernel](https://github.com/microsoft/semantic-kernel) and [Microsoft Agent Framework](https://github.com/microsoft/agent-framework). With these abstractions the same code stores and searches records on different databases: to change database, you change the provider.

The providers used to be part of Semantic Kernel. They moved to the toolkit to give them a neutral home, not tied to a single product. The toolkit has Qdrant, Redis, PostgreSQL, SQL Server, Azure AI Search, Cosmos DB and the others. Reviews and releases are done by the Microsoft maintainers of Microsoft.Extensions.VectorData.

Now Chroma is there too.

## How to use it

Docker starts Chroma:

Shell
    
    
    docker run -p 127.0.0.1:8000:8000 chromadb/chroma:1.5.9

Then the package:

Shell
    
    
    dotnet add package CommunityToolkit.VectorData.Chroma --prerelease

A model, a collection, a search with a filter:

C#
    
    
    using ChromaDB.Client;
    using CommunityToolkit.VectorData.Chroma;
    using Microsoft.Extensions.VectorData;
    
    using var httpClient = new HttpClient();
    var client = new ChromaClient(new ChromaConfigurationOptions("http://127.0.0.1:8000"), httpClient);
    using var vectorStore = new ChromaVectorStore(client, ownsClient: false);
    
    var hotels = vectorStore.GetCollection<string, Hotel>("hotels");
    await hotels.EnsureCollectionExistsAsync();
    
    await hotels.UpsertAsync(new Hotel { Id = "h1", Name = "Grand", Rating = 5, Embedding = new float[] { 0.1f, 0.2f, 0.3f, 0.4f } });
    
    await foreach (var result in hotels.SearchAsync(new float[] { 0.1f, 0.2f, 0.3f, 0.4f }, top: 3, new() { Filter = h => h.Rating >= 4 }))
    {
        Console.WriteLine($"{result.Record.Name}: {result.Score}");
    }
    
    public sealed class Hotel
    {
        [VectorStoreKey]
        public string Id { get; set; } = "";
    
        [VectorStoreData(IsFullTextIndexed = true)]
        public string? Name { get; set; }
    
        [VectorStoreData]
        public int Rating { get; set; }
    
        [VectorStoreVector(4)]
        public ReadOnlyMemory<float> Embedding { get; set; }
    }

The embeddings are written by hand to keep the example short. In a real project they come from a model, for example through [Microsoft.Extensions.AI](https://learn.microsoft.com/dotnet/ai/microsoft-extensions-ai).

With dependency injection it takes one line:

C#
    
    
    services.AddChromaVectorStore("http://127.0.0.1:8000");

## What is underneath

The provider is built on [ChromaDotNet.Client](https://github.com/ChromaDotNet/ChromaDB.Client), a client for the whole Chroma v2 API, Chroma Cloud included. The client is tested with Chroma 0.4.10 to 1.5.9 and with Chroma Cloud; the [compatibility table](https://chromadotnet.org/compatibility/) shows its results on Chroma 1.5.9 and on Chroma Cloud.

On [Chroma Cloud](https://www.trychroma.com/cloud) the provider also does hybrid search, vectors and keywords together, with `HybridSearchAsync`.

In the CI of the toolkit, the provider runs the conformance tests of Microsoft.Extensions.VectorData against a real Chroma, like the other providers.

The client and the other ChromaDotNet libraries are introduced in an [earlier article](https://chromadotnet.org/news/chromadotnet/).

## Thanks

  * [Adam Sitnik](https://github.com/adamsitnik), for the review and for accepting the provider into the toolkit.
  * [Jiri Cincura](https://github.com/cincuranet), [Nedeljko Savic](https://github.com/ssone95), [Rich Lander](https://github.com/richlander) and [Hamed Hasani](https://github.com/inlineHamed), authors of [ChromaDB.Client](https://github.com/ssone95/ChromaDB.Client), the client ChromaDotNet started again from.
  * [Luis Quintanilla](https://github.com/luisquintanilla), who with Jiri Cincura introduced ChromaDB.Client on the [.NET blog](https://web.archive.org/web/20250302232802/https://devblogs.microsoft.com/dotnet/announcing-chroma-db-csharp-sdk/) in 2025.
  * [Shay Rojansky](https://github.com/roji), for Microsoft.Extensions.VectorData and its conformance tests, which the provider runs.
  * The authors of the [Qdrant provider](https://github.com/CommunityToolkit/AI/tree/main/MEVD/src/Qdrant), which gave the Chroma provider its structure.

## Links

  * Package: [CommunityToolkit.VectorData.Chroma](https://www.nuget.org/packages/CommunityToolkit.VectorData.Chroma)
  * Code: [CommunityToolkit/AI](https://github.com/CommunityToolkit/AI/tree/main/MEVD/src/Chroma)
  * Pull request: [CommunityToolkit/AI#58](https://github.com/CommunityToolkit/AI/pull/58)
  * Client: [chromadotnet.org](https://chromadotnet.org)

_ChromaDotNet is a personal open source project of Federico Laurianti. Independent project. Not affiliated with, endorsed by or sponsored by Chroma Inc. CHROMA is a registered trademark of Chroma Inc., used here only to describe compatibility._
