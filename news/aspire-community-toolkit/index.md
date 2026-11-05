Source: https://chromadotnet.org/news/aspire-community-toolkit/

[News](https://chromadotnet.org/news/)

# The Aspire integration of ChromaDotNet joins the Aspire Community Toolkit

Federico Laurianti, 25 October 2026

[Aspire](https://github.com/microsoft/aspire) runs the services of a distributed application, and the resources they depend on, from one AppHost project. The Chroma integrations of ChromaDotNet, one for hosting and one for the client, are now part of the [Aspire Community Toolkit](https://github.com/CommunityToolkit/Aspire): the pull request [CommunityToolkit/Aspire#2219](https://github.com/CommunityToolkit/Aspire/pull/2219) was merged on 12 October 2026.

Until the toolkit publishes its own packages, the same integrations are on NuGet as [ChromaDotNet.Aspire.Hosting](https://www.nuget.org/packages/ChromaDotNet.Aspire.Hosting) and [ChromaDotNet.Aspire.Client](https://www.nuget.org/packages/ChromaDotNet.Aspire.Client), version 0.2.9.

## What it does

  * **ChromaDotNet.Aspire.Hosting** runs a Chroma container in the AppHost, `chromadb/chroma:1.5.9` by default, and keeps its data in a volume or in a folder of your choice.
  * **ChromaDotNet.Aspire.Client** gives each service a `ChromaClient` of [ChromaDotNet.Client](https://chromadotnet.org/libraries/client/), from the connection that the AppHost passes to it, with health checks, traces and metrics. Each of the three can be turned off in the settings.

## How to use it

In the AppHost:

Shell
    
    
    dotnet add package ChromaDotNet.Aspire.Hosting

C#
    
    
    var chroma = builder.AddChroma("chroma").WithDataVolume();
    
    builder.AddProject<Projects.ApiService>("api")
        .WithReference(chroma)
        .WaitFor(chroma);

In the service:

Shell
    
    
    dotnet add package ChromaDotNet.Aspire.Client

C#
    
    
    builder.AddChromaClient("chroma");

The service then takes the `ChromaClient` from dependency injection.

## Thanks

  * [Aaron Powell](https://github.com/aaronpowell), for the review of the integrations and for merging them into the toolkit.

## Links

  * Packages: [ChromaDotNet.Aspire.Hosting 0.2.9](https://www.nuget.org/packages/ChromaDotNet.Aspire.Hosting/0.2.9) and [ChromaDotNet.Aspire.Client 0.2.9](https://www.nuget.org/packages/ChromaDotNet.Aspire.Client/0.2.9)
  * Code: [ChromaDotNet/ChromaDB.Aspire](https://github.com/ChromaDotNet/ChromaDB.Aspire)
  * Pull request in the toolkit: [CommunityToolkit/Aspire#2219](https://github.com/CommunityToolkit/Aspire/pull/2219)

_ChromaDotNet is a personal open source project of Federico Laurianti. Independent project. Not affiliated with, endorsed by or sponsored by Chroma Inc. CHROMA is a registered trademark of Chroma Inc., used here only to describe compatibility._
