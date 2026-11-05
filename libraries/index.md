Source: https://chromadotnet.org/libraries/

## Libraries

ChromaDotNet.Client is the .NET client for Chroma. The Microsoft.Extensions.VectorData provider and the Aspire client integration are built on it.

Your app: Semantic Kernel, Agent Framework, Aspire services

CommunityToolkit.VectorData.ChromaMicrosoft.Extensions.VectorData provider, in the AI Community Toolkit

ChromaDotNet.Aspire.ClientAspire client integration

ChromaDotNet.Clientthe .NET client for the Chroma API

Chroma and Chroma Cloud

ChromaDotNet.Local, ChromaDotNet.Testcontainers and ChromaDotNet.Aspire.Hosting start Chroma: in your app without Docker, in your tests, and in the Aspire app host.

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

### ChromaDotNet.Aspire.Hosting / .Client

Aspire integrations. Add Chroma to the app host and the client to your services, with health checks, traces and metrics.

`dotnet add package ChromaDotNet.Aspire.Hosting`

`dotnet add package ChromaDotNet.Aspire.Client`

[Docs](https://chromadotnet.org/libraries/aspire/)[GitHub](https://github.com/ChromaDotNet/ChromaDB.Aspire)[NuGet (hosting)](https://www.nuget.org/packages/ChromaDotNet.Aspire.Hosting)[NuGet (client)](https://www.nuget.org/packages/ChromaDotNet.Aspire.Client)

## VecturaDB

A single-node server built from Chroma: a complement of Chroma that adds sparse vectors, BM25, the Search API and collection fork to its single node. ChromaDotNet.Client works with it as with Chroma.

[Docs](https://chromadotnet.org/vecturadb/) · [vecturadb.org](https://vecturadb.org/)
