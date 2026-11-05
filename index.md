Source: https://chromadotnet.org/

# .NET client and libraries for Chroma and Chroma Cloud

A client for the [Chroma](https://www.trychroma.com/) API, a Microsoft.Extensions.VectorData provider, a Testcontainers module, Aspire integrations, Chroma without Docker, and the default embedding function of Chroma. We test the client with Chroma 0.4.10 to 1.5.9 and with Chroma Cloud: see the [compatibility table](https://chromadotnet.org/compatibility/).

Independent project. Not affiliated with, endorsed by or sponsored by Chroma Inc. CHROMA is a registered trademark of Chroma Inc., used here only to describe compatibility.

dotnet add package ChromaDotNet.Client
    
    
    using ChromaDB.Client;
    
    using var client = new ChromaClient("http://localhost:8000");
    var collection = await client.GetOrCreateCollectionAsync("docs");
    var docs = client.GetCollectionClient(collection);
    
    await docs.AddAsync(["doc-1"],
        embeddings: [new([0.1f, 0.4f, 0.2f])],
        documents: ["Chroma from .NET"]);
    
    var results = await docs.QueryAsync(
        new ReadOnlyMemory<float>([0.1f, 0.4f, 0.2f]), nResults: 1);

The packages are named `ChromaDotNet.*`. The namespaces keep their original names, such as `ChromaDB.Client`.

[Try it in your browser](https://codespaces.new/ChromaDotNet/ChromaDB.Client?quickstart=1): a GitHub Codespace with .NET and Chroma, then `dotnet run Samples/quickstart.cs`.

## Used by

  * [AI Community Toolkit](https://github.com/CommunityToolkit/AI/blob/546aa1b8e306e7518b0225341ea62cd86c916297/MEVD/src/Chroma/Chroma.csproj#L18)CommunityToolkit.VectorData.Chroma: the Chroma provider for Microsoft.Extensions.VectorData, in the AI Community Toolkit of the .NET Foundation. It works with Semantic Kernel and Agent Framework.

Used by

[**Generative AI for Beginners .NET** A RAG sample in the Microsoft course.](https://github.com/microsoft/Generative-AI-for-beginners-dotnet/blob/359ffe5adf0cb74a397fd99af8786e14d0930ca3/samples/CoreSamples/RAGSimple-05MEAIVectorsChroma/RAGSimple-05MEAIVectorsChroma.csproj#L12) [**LangChain .NET** A RAG sample with Chroma.](https://github.com/tryAGI/LangChain/blob/4d8062616101cced1c6737aac7bc76535f6efc86/examples/LangChain.Samples.Chroma/LangChain.Samples.Chroma.csproj#L16) [**LangChain.Databases** The integration tests.](https://github.com/tryAGI/LangChain.Databases/blob/17c118f863ef1437b4ecaa0222081eb045b38a87/src/IntegrationTests/LangChain.Databases.IntegrationTests.csproj#L19)

Cited in

[**Semantic Kernel** Recommended for Chroma in .NET.](https://github.com/microsoft/semantic-kernel/blob/7e68276a4a85022dd4da255beae3263ab5cbf059/dotnet/src/VectorData/Chroma/README.md#L1) [**LangChain.Databases** Recommended for Chroma in its README.](https://github.com/tryAGI/LangChain.Databases/blob/17c118f863ef1437b4ecaa0222081eb045b38a87/README.md#L36) [**Microsoft Agent Framework** Listed among the .NET community projects.](https://learn.microsoft.com/en-us/agent-framework/integrations/community-projects#context-providers)

  * [Aspire Community Toolkit](https://github.com/CommunityToolkit/Aspire/blob/523b84b27c5d019d242d95c2252046ba3619303c/src/CommunityToolkit.Aspire.Chroma/CommunityToolkit.Aspire.Chroma.csproj#L12)CommunityToolkit.Aspire.Chroma: the Chroma client integration, in the Aspire Community Toolkit of the .NET Foundation, in beta on nuget.org.
  * [BotSharp](https://github.com/SciSharp/BotSharp/blob/c8f632bf13bb22aed53c8ba63d41ee9e0e0fb75b/src/Plugins/BotSharp.Plugin.Chroma/BotSharp.Plugin.Chroma.csproj#L14)BotSharp.Plugin.Chroma: the Chroma vector database plugin of BotSharp, by SciSharp.
  * [LLM Tornado](https://github.com/lofcz/LLMTornado/blob/d619b5d4c6558094cc2fcb663a5d6b55e6871233/src/LlmTornado.VectorDatabases.ChromaDB/LlmTornado.VectorDatabases.ChromaDB.csproj#L17)LlmTornado.VectorDatabases.ChromaDB: the Chroma connector of LLM Tornado.
  * [eShopLite](https://github.com/Azure-Samples/eShopLite/blob/ae63e987b17e8db076914415ad289b28a18a76e0/scenarios/04-chromadb/src/Products/Products.csproj#L33)The Chroma scenario of the Azure sample, on Chroma 1.x.
  * [Testcontainers for .NET](https://github.com/testcontainers/testcontainers-dotnet/blob/a6ce741af4dde9e8eb5f8c49bf2b90040b981187/tests/Testcontainers.Chroma.Tests/Testcontainers.Chroma.Tests.csproj#L14)Testcontainers.Chroma: the Chroma module of Testcontainers for .NET, tested with ChromaDotNet.Client. It ships with their next release.
  * [Semantic Kernel documentation](https://learn.microsoft.com/en-us/semantic-kernel/concepts/vector-store-connectors/out-of-the-box-connectors/chroma-connector?pivots=programming-language-csharp)The C# page of the Chroma connector on Microsoft Learn: CommunityToolkit.VectorData.Chroma, built on ChromaDotNet.Client.
  * [Microsoft Agent Framework community projects](https://github.com/microsoft/agent-framework/blob/c808d823d9a00ee5f1209f8618fc2a62b27024d7/dotnet/samples/community-projects.md#L20)CommunityToolkit.VectorData.Chroma, built on ChromaDotNet.Client, in the catalog of the .NET community projects, on Microsoft Learn.

### On the way

  * [Semantic Kernel samples](https://github.com/microsoft/semantic-kernel/pull/14602)Proposed. Chroma in the multi-store vector search sample and in the VectorStoreRAG demo of Semantic Kernel, through CommunityToolkit.VectorData.Chroma.
  * [Semantic Kernel documentation, hybrid search](https://github.com/MicrosoftDocs/semantic-kernel-docs/pull/459)Proposed. Hybrid search and two limitations on the C# page of the Chroma connector.
  * [awesome-dotnet](https://github.com/quozd/awesome-dotnet/pull/1541)Proposed. ChromaDotNet.Client in the list of .NET libraries.
  * [Microsoft Agent Framework samples](https://github.com/microsoft/agent-framework/issues/9244)Requested. A sample that keeps the chat history of an agent in Chroma, through CommunityToolkit.VectorData.Chroma.
  * [Chroma documentation](https://github.com/chroma-core/chroma/pull/7877)Proposed. Semantic Kernel and Agent Framework pages for .NET, through CommunityToolkit.VectorData.Chroma.
  * [Chroma reference](https://github.com/chroma-core/chroma/pull/7851)Proposed. ChromaDotNet.Client in the list of the community SDKs of the Chroma reference.

## Libraries

ChromaDotNet.Client is the .NET client for Chroma. The Microsoft.Extensions.VectorData provider and the Aspire client integration are built on it.

Your app: Semantic Kernel, Agent Framework, Aspire services

CommunityToolkit.VectorData.ChromaMicrosoft.Extensions.VectorData provider, in the AI Community Toolkit

CommunityToolkit.Aspire.ChromaAspire client integration

ChromaDotNet.Clientthe .NET client for the Chroma API

Chroma and Chroma Cloud

ChromaDotNet.Local, ChromaDotNet.Testcontainers and the Aspire hosting integration start Chroma: in your app without Docker, in your tests, and in the Aspire app host.

### ChromaDotNet.Client

The client covers every operation in the Chroma v2 API, including Chroma Cloud. It runs on .NET 8 and later (including .NET 10), .NET Framework 4.6.2 and .NET Standard 2.0 platforms. For dependency injection, use ChromaDotNet.Client.DependencyInjection.

`dotnet add package ChromaDotNet.Client`

[Docs](https://chromadotnet.org/libraries/client/)[GitHub](https://github.com/ChromaDotNet/ChromaDB.Client)[NuGet](https://www.nuget.org/packages/ChromaDotNet.Client)[Tested versions](https://github.com/ChromaDotNet/ChromaDB.Client/blob/main/docs/COMPATIBILITY.md)

### CommunityToolkit.VectorData.Chroma

The Chroma provider for Microsoft.Extensions.VectorData, in the AI Community Toolkit of the .NET Foundation. Use it with Semantic Kernel, Agent Framework or any code written against the abstractions. It supports hybrid search on Chroma Cloud. Recommended by [Semantic Kernel](https://github.com/microsoft/semantic-kernel/blob/7e68276a4a85022dd4da255beae3263ab5cbf059/dotnet/src/VectorData/Chroma/README.md#L1) and listed among the [.NET community projects](https://learn.microsoft.com/en-us/agent-framework/integrations/community-projects#context-providers) of Microsoft Agent Framework.

`dotnet add package CommunityToolkit.VectorData.Chroma --prerelease`

[Docs](https://chromadotnet.org/libraries/vectordata/)[GitHub](https://github.com/CommunityToolkit/AI/tree/main/MEVD/src/Chroma)[NuGet](https://www.nuget.org/packages/CommunityToolkit.VectorData.Chroma)

### ChromaDotNet.Local

Chroma on your machine, without Docker and without Python. It starts the Chroma executable as a child process of your app, and stops it when the app ends. Windows, Linux and macOS. ChromaDotNet.Local.ChromaDB brings Chroma 1.5.9 with the executables of every platform; with the property ChromaDotNetLocalChromaDBDownload, the first start downloads the one of its platform instead. Preview.

`dotnet add package ChromaDotNet.Local.ChromaDB --prerelease`

[Docs](https://chromadotnet.org/libraries/local/)[GitHub](https://github.com/ChromaDotNet/ChromaDB.Local)[NuGet](https://www.nuget.org/packages/ChromaDotNet.Local.ChromaDB)[Sample](https://github.com/ChromaDotNet/ChromaDB.Local/blob/main/samples/GettingStarted/Program.cs)[Try it in your browser](https://codespaces.new/ChromaDotNet/ChromaDB.Local?quickstart=1)

### ChromaDotNet.Embeddings

The default embedding function of Chroma for .NET: all-MiniLM-L6-v2, with the same vectors as the Python client. Give it to ChromaDotNet.Client, and add or query records by their text. It also computes the hosted functions of Chroma Cloud, Qwen3-Embedding and SPLADE, on your machine. Preview.

`dotnet add package ChromaDotNet.Embeddings --prerelease`

[Docs](https://chromadotnet.org/libraries/embeddings/)[GitHub](https://github.com/ChromaDotNet/ChromaDB.Embeddings)[NuGet](https://www.nuget.org/packages/ChromaDotNet.Embeddings)[Try it in your browser](https://codespaces.new/ChromaDotNet/ChromaDB.Local?quickstart=1)

### ChromaDotNet.Testcontainers

A Testcontainers for .NET module. It gives your tests a throwaway Chroma container.

`dotnet add package ChromaDotNet.Testcontainers`

[Docs](https://chromadotnet.org/libraries/testcontainers/)[GitHub](https://github.com/ChromaDotNet/ChromaDB.Testcontainers)[NuGet](https://www.nuget.org/packages/ChromaDotNet.Testcontainers)

### CommunityToolkit.Aspire.Chroma

Aspire integrations, in the Aspire Community Toolkit. Add Chroma to the app host and the client to your services, with health checks, traces and metrics. Beta. Until Aspire 17 is on NuGet, use ChromaDotNet.Aspire.Hosting 0.2.10 in the AppHost.

`dotnet add package CommunityToolkit.Aspire.Chroma --prerelease`

`dotnet add package ChromaDotNet.Aspire.Hosting --version 0.2.10`

[Docs](https://chromadotnet.org/libraries/aspire/)[GitHub](https://github.com/CommunityToolkit/Aspire/tree/main/src/CommunityToolkit.Aspire.Chroma)[NuGet (hosting)](https://www.nuget.org/packages/CommunityToolkit.Aspire.Hosting.Chroma)[NuGet (client)](https://www.nuget.org/packages/CommunityToolkit.Aspire.Chroma)

### ChromaDB.Operator

A Kubernetes operator for Chroma 1.x, single-node and distributed, and for VecturaDB single-node. It creates the volume, the Pod and the Service of each Chroma, and gives its address when it is ready.

`kubectl apply -f https://github.com/ChromaDotNet/ChromaDB.Operator/releases/latest/download/chroma-operator.yaml`

[Docs](https://chromadotnet.org/operator/)[GitHub](https://github.com/ChromaDotNet/ChromaDB.Operator)[Releases](https://github.com/ChromaDotNet/ChromaDB.Operator/releases)

## Samples

The Semantic Kernel samples run with Azure OpenAI. The Agent Framework samples run with Microsoft Foundry. The chat samples also run locally with Ollama.

  * [Semantic Kernel](https://chromadotnet.org/samples/semantic-kernel/)Vector search and an agent that answers from documents stored in Chroma.
  * [Microsoft Agent Framework](https://chromadotnet.org/samples/agent-framework/)Chat history kept in Chroma, and an agent that answers from documentation stored there.

## About

ChromaDotNet is an independent community project that provides .NET libraries and tools for Chroma. Independent project. Not affiliated with, endorsed by or sponsored by Chroma Inc. CHROMA is a registered trademark of Chroma Inc., used here only to describe compatibility. We use the name "Chroma" only to say which software our projects work with. It does not mean that Chroma Inc. made, approved or supports them.
