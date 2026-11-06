Source: https://chromadotnet.org/libraries/aspire/

[Libraries](https://chromadotnet.org/libraries/)

# Aspire integrations for Chroma

The Chroma integrations for Aspire are part of the Aspire Community Toolkit, merged with [CommunityToolkit/Aspire#2219](https://github.com/CommunityToolkit/Aspire/pull/2219). They are on NuGet in beta, as CommunityToolkit.Aspire.Hosting.Chroma and CommunityToolkit.Aspire.Chroma. They started here as ChromaDotNet.Aspire.

  * Latest release **17.0.0-beta.1018**
  * [NuGet (hosting)](https://www.nuget.org/packages/CommunityToolkit.Aspire.Hosting.Chroma)
  * [NuGet (client)](https://www.nuget.org/packages/CommunityToolkit.Aspire.Chroma)
  * [GitHub](https://github.com/CommunityToolkit/Aspire/tree/main/src/CommunityToolkit.Aspire.Chroma)
  * [Documentation](https://github.com/CommunityToolkit/Aspire/tree/main/src/CommunityToolkit.Aspire.Chroma#readme)
  * [Aspire Community Toolkit](https://github.com/CommunityToolkit/Aspire)

## What they do

  * [CommunityToolkit.Aspire.Hosting.Chroma](https://www.nuget.org/packages/CommunityToolkit.Aspire.Hosting.Chroma): for the AppHost: `AddChroma`, `WithDataVolume`, `WithDataBindMount`. Image `chromadb/chroma:1.5.9`, health check on `/api/v2/heartbeat`. It needs Aspire 17.
  * [CommunityToolkit.Aspire.Chroma](https://www.nuget.org/packages/CommunityToolkit.Aspire.Chroma): for a service: `AddChromaClient`, `AddKeyedChromaClient`, with ChromaDotNet.Client, a health check, traces and metrics. It connects to a Chroma server or to Chroma Cloud.

Until Aspire 17 is on NuGet, use [ChromaDotNet.Aspire.Hosting](https://www.nuget.org/packages/ChromaDotNet.Aspire.Hosting) 0.2.10 in the AppHost: it has the same `AddChroma`, `WithDataVolume` and `WithDataBindMount`, for Aspire 13.6.

## Install

In the service:

`dotnet add package CommunityToolkit.Aspire.Chroma --prerelease`

In the AppHost, until Aspire 17 is on NuGet:

`dotnet add package ChromaDotNet.Aspire.Hosting --version 0.2.10`

The packages of the toolkit are in beta: `--prerelease` installs the latest one.

## Example

Add Chroma to the AppHost, and the client to the service that uses it:

AppHost and service
    
    
    // AppHost
    var chroma = builder.AddChroma("chroma").WithDataVolume();
    builder.AddProject<Projects.ApiService>("api").WithReference(chroma).WaitFor(chroma);
    
    // Service
    builder.AddChromaClient("chroma");

Tested on 6 November 2026: the AppHost code builds with ChromaDotNet.Aspire.Hosting 0.2.10, and the service code connects to Chroma 1.5.9 with CommunityToolkit.Aspire.Chroma 17.0.0-beta.1009.
