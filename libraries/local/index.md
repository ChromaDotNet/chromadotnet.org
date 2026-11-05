Source: https://chromadotnet.org/libraries/local/

[Libraries](https://chromadotnet.org/libraries/)

# ChromaDotNet.Local

Chroma on your machine, without Docker and without Python. `ChromaLocalServer.StartAsync` starts the server of an engine package as a child process on 127.0.0.1, for ChromaDotNet.Client, and stops it with `Dispose` or when the application ends.

  * Latest release **1.5.9.1-preview.7**
  * [NuGet](https://www.nuget.org/packages/ChromaDotNet.Local.ChromaDB)
  * [GitHub](https://github.com/ChromaDotNet/ChromaDB.Local)
  * [Documentation](https://github.com/ChromaDotNet/ChromaDB.Local#readme)
  * [Try it in your browser](https://codespaces.new/ChromaDotNet/ChromaDB.Local?quickstart=1)

## Engines

  * [ChromaDotNet.Local.ChromaDB](https://www.nuget.org/packages/ChromaDotNet.Local.ChromaDB): Chroma 1.5.9, with the executables for Windows x64, Linux x64 and arm64 (glibc 2.28 and later), macOS x64 and arm64, in its runtime packages. With the property `ChromaDotNetLocalChromaDBDownload` set to `true` in the project, the first start downloads the one of its platform instead.
  * [ChromaDotNet.Local.VecturaDB](https://www.nuget.org/packages/ChromaDotNet.Local.VecturaDB): [VecturaDB](https://chromadotnet.org/vecturadb/) 1.0.0-preview.3. The first start downloads the executable of its platform, for Windows x64, Linux x64 and arm64, macOS x64 and arm64, from the GitHub release of VecturaDB, and checks its SHA-256 hash.

## Install

`dotnet add package ChromaDotNet.Local.ChromaDB --prerelease`

It brings the base, ChromaDotNet.Local.

## Example

Start Chroma in your application and create a collection:

Program.cs
    
    
    using ChromaDotNet.Local;
    
    await using var chroma = await ChromaLocalServer.StartAsync(new() { DataPath = "./chroma-data" });
    using var client = chroma.CreateClient();
    
    var collection = await client.GetOrCreateCollectionAsync("docs");
    Console.WriteLine(collection.Name);

Tested on 28 October 2026 with ChromaDotNet.Local.ChromaDB 1.5.9.1-preview.7 on Windows x64.

The [GettingStarted sample](https://github.com/ChromaDotNet/ChromaDB.Local/blob/main/samples/GettingStarted/Program.cs) goes further.
