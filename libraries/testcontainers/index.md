Source: https://chromadotnet.org/libraries/testcontainers/

[Libraries](https://chromadotnet.org/libraries/)

# ChromaDotNet.Testcontainers

A Testcontainers for .NET module for Chroma: start a throwaway Chroma container in your tests. The container is ready when the Chroma API answers its heartbeat, so the module works with any Chroma image, whatever its tag.

  * Latest release **0.2.0**
  * [NuGet](https://www.nuget.org/packages/ChromaDotNet.Testcontainers)
  * [GitHub](https://github.com/ChromaDotNet/ChromaDB.Testcontainers)
  * [Documentation](https://github.com/ChromaDotNet/ChromaDB.Testcontainers#readme)

## Install

`dotnet add package ChromaDotNet.Testcontainers`

The example also uses the client:

`dotnet add package ChromaDotNet.Client`

## Example

Start Chroma in a container and connect ChromaDotNet.Client to it. Docker must be running.

Program.cs
    
    
    using ChromaDB.Client;
    using Testcontainers.Chroma;
    
    await using var chroma = new ChromaBuilder("chromadb/chroma:1.5.9").Build();
    await chroma.StartAsync();
    
    using var client = new ChromaClient(chroma.GetConnectionString());
    var collection = await client.CreateCollectionAsync("documents");
    Console.WriteLine($"{collection.Name} on {chroma.GetConnectionString()}");

Tested on 29 October 2026 with ChromaDotNet.Testcontainers 0.2.0 and ChromaDotNet.Client 2.18.1.

## Origin

The module follows the structure of the Testcontainers for .NET modules. It was merged into Testcontainers for .NET as Testcontainers.Chroma in [testcontainers/testcontainers-dotnet#1784](https://github.com/testcontainers/testcontainers-dotnet/pull/1784). It ships with the next release of Testcontainers for .NET.
