Source: https://chromadotnet.org/news/chromadotnet/

[News](https://chromadotnet.org/news/)

# ChromaDotNet: open source .NET libraries for Chroma and Chroma Cloud

Federico Laurianti, 5 October 2026, updated 25 October 2026

_Updated on 25 October 2026: the Microsoft.Extensions.VectorData provider is now in the AI Community Toolkit, and ChromaDotNet has new libraries._

[Chroma](https://www.trychroma.com/) is one of the most used open source vector databases for semantic search and RAG, where a language model gets the right documents to answer from. From Python it takes two lines.

For .NET, in February 2025 Microsoft introduced the community library [ChromaDB.Client](https://github.com/ssone95/ChromaDB.Client) on the .NET Blog ([archived copy](https://web.archive.org/web/20250302232802/https://devblogs.microsoft.com/dotnet/announcing-chroma-db-csharp-sdk/), [Chinese edition](https://devblogs.microsoft.com/dotnet-ch/%e4%bd%bf%e7%94%a8chroma%e6%9e%84%e5%bb%ba-net-ai%e5%ba%94%e7%94%a8/)). That post is no longer on the English blog, and the library has stopped. It is still on the v1 API, which Chroma 1.x no longer serves.

ChromaDotNet picks up that work where it stopped and carries it on. The original code and its authors are in the history of the project.

## What's inside

  * **[ChromaDotNet.Client](https://github.com/ChromaDotNet/ChromaDB.Client)** : a client for the whole Chroma v2 API, Chroma Cloud included, tested with Chroma 0.4.10 to 1.5.9. It runs on .NET 8 and later, .NET 10 included, on .NET Framework 4.6.2 and on .NET Standard 2.0 platforms, with OpenTelemetry traces and metrics.
  * **[CommunityToolkit.VectorData.Chroma](https://www.nuget.org/packages/CommunityToolkit.VectorData.Chroma)** : the provider for Microsoft.Extensions.VectorData, the abstractions that Semantic Kernel and Microsoft Agent Framework use. With Chroma as the vector store, the application code stays the same. It started here as ChromaDotNet.VectorData; since 9 October it is in the AI Community Toolkit of the .NET Foundation, and ChromaDotNet.VectorData is deprecated. See [The Chroma provider for Microsoft.Extensions.VectorData joins the AI Community Toolkit](https://chromadotnet.org/news/chroma-ai-community-toolkit/).
  * **[ChromaDotNet.Local](https://chromadotnet.org/libraries/local/)** : Chroma without Docker. It starts the Chroma executable as a child process of your application, on 127.0.0.1.
  * **[ChromaDotNet.Embeddings](https://chromadotnet.org/libraries/embeddings/)** : the default embedding function of Chroma for .NET, all-MiniLM-L6-v2 in ONNX, with the same vectors as the Python client.
  * **[ChromaDotNet.Testcontainers](https://github.com/ChromaDotNet/ChromaDB.Testcontainers)** : a module for Testcontainers for .NET, to start a throwaway Chroma in your tests.
  * **[ChromaDotNet.Aspire.Hosting](https://github.com/ChromaDotNet/ChromaDB.Aspire)** and **[ChromaDotNet.Aspire.Client](https://github.com/ChromaDotNet/ChromaDB.Aspire)** : the Aspire integrations, with Chroma in the app host and the client in the services.
  * **[ChromaDotNet.Operator](https://chromadotnet.org/operator/)** : a Kubernetes operator for Chroma, written in .NET.
  * **Two sample repositories** , for [Semantic Kernel](https://github.com/ChromaDotNet/ChromaDB.SemanticKernel.Sample), with Azure OpenAI, and for [Agent Framework](https://github.com/ChromaDotNet/ChromaDB.AgentFramework.Sample), with Microsoft Foundry. The chat samples also run locally with Ollama.

## Try it in two minutes

Docker starts Chroma:

Shell
    
    
    docker run -p 127.0.0.1:8000:8000 chromadb/chroma:1.5.9

Then, in a .NET console app:

Shell
    
    
    dotnet add package ChromaDotNet.Client

C#
    
    
    using ChromaDB.Client;
    
    using var client = new ChromaClient("http://127.0.0.1:8000");
    var collection = await client.GetOrCreateCollectionAsync("docs");
    var docs = client.GetCollectionClient(collection);
    
    await docs.AddAsync(["doc-1"],
        embeddings: [new([0.1f, 0.4f, 0.2f])],
        documents: ["Chroma from .NET"]);
    
    var results = await docs.QueryAsync(
        new ReadOnlyMemory<float>([0.1f, 0.4f, 0.2f]), nResults: 1);
    
    Console.WriteLine(results[0].Document);

The embeddings are written by hand to keep the example short. In a real project they come from a model, for example through Microsoft.Extensions.AI.

## Why

More and more .NET applications have an AI part, and almost all of them need a vector store. Chroma starts locally in a container and scales on Chroma Cloud, but without an up-to-date client it was out of reach from .NET. Each library is tested in CI against real Chroma releases, and the provider runs the Microsoft.Extensions.VectorData conformance tests.

ChromaDotNet is a personal open source project of Federico Laurianti, MIT licensed. Independent project. Not affiliated with, endorsed by or sponsored by Chroma Inc. CHROMA is a registered trademark of Chroma Inc., used here only to describe compatibility. If you try it and something does not work, open an issue on [GitHub](https://github.com/ChromaDotNet).
