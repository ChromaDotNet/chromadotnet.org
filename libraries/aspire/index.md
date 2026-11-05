Source: https://chromadotnet.org/libraries/aspire/

[Libraries](https://chromadotnet.org/libraries/)

# Aspire integrations for Chroma

The Chroma integrations for Aspire are part of the Aspire Community Toolkit, merged with [CommunityToolkit/Aspire#2219](https://github.com/CommunityToolkit/Aspire/pull/2219). They arrive with the next release of the toolkit, as CommunityToolkit.Aspire.Hosting.Chroma and CommunityToolkit.Aspire.Chroma. Until then, try them with ChromaDotNet.Aspire.Hosting and ChromaDotNet.Aspire.Client.

  * Latest release **0.2.9**
  * [NuGet (hosting)](https://www.nuget.org/packages/ChromaDotNet.Aspire.Hosting)
  * [NuGet (client)](https://www.nuget.org/packages/ChromaDotNet.Aspire.Client)
  * [GitHub](https://github.com/ChromaDotNet/ChromaDB.Aspire)
  * [Documentation](https://github.com/ChromaDotNet/ChromaDB.Aspire#readme)
  * [Aspire Community Toolkit](https://github.com/CommunityToolkit/Aspire)

## What they do

  * [ChromaDotNet.Aspire.Hosting](https://www.nuget.org/packages/ChromaDotNet.Aspire.Hosting): for the AppHost: `AddChroma`, `WithDataVolume`, `WithDataBindMount`. Image `chromadb/chroma:1.5.9`, health check on `/api/v2/heartbeat`.
  * [ChromaDotNet.Aspire.Client](https://www.nuget.org/packages/ChromaDotNet.Aspire.Client): for a service: `AddChromaClient`, `AddKeyedChromaClient`, with ChromaDotNet.Client, a health check, traces and metrics. It connects to a Chroma server or to Chroma Cloud.

The source files of ChromaDotNet.Aspire 0.2.9 are the same as those of the Chroma integrations in the Aspire Community Toolkit, except in the two project files: the package descriptions say that the project is independent of Chroma Inc., and the hosting package references MessagePack 2.5.303, for the advisory [GHSA-qhrr-8q5h-9q3h](https://github.com/advisories/GHSA-qhrr-8q5h-9q3h). We compared them on 28 October 2026.

## Install

Today, in the AppHost and in the service:

`dotnet add package ChromaDotNet.Aspire.Hosting`

`dotnet add package ChromaDotNet.Aspire.Client`

Next release of the Aspire Community Toolkit

`dotnet add package CommunityToolkit.Aspire.Hosting.Chroma`

`dotnet add package CommunityToolkit.Aspire.Chroma`

These two packages are not on NuGet yet: they come with the next release of the toolkit.

## Example

Add Chroma to the AppHost, and the client to the service that uses it:

AppHost and service
    
    
    // AppHost
    var chroma = builder.AddChroma("chroma").WithDataVolume();
    builder.AddProject<Projects.ApiService>("api").WithReference(chroma).WaitFor(chroma);
    
    // Service
    builder.AddChromaClient("chroma");

Tested on 28 October 2026: the AppHost code builds with ChromaDotNet.Aspire.Hosting 0.2.9, and the service code connects to Chroma 1.5.9 with ChromaDotNet.Aspire.Client 0.2.9.
