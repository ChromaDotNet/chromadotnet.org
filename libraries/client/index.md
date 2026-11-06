Source: https://chromadotnet.org/libraries/client/

[Libraries](https://chromadotnet.org/libraries/)

# ChromaDotNet.Client

A .NET client for Chroma and Chroma Cloud, tested with Chroma 0.4.10 to 1.5.9, with VecturaDB 1.0.0-preview.8, and with distributed Chroma, the one Chroma Cloud runs on, both on Chroma Cloud and on Kubernetes with ChromaDB.Operator 0.1.3. It covers collections, tenants and databases, records and nearest neighbor queries with filters, the Search API with hybrid BM25 search, conditional transactions, and OpenTelemetry traces and metrics.

  * Latest release **2.20.0**
  * [NuGet](https://www.nuget.org/packages/ChromaDotNet.Client)
  * [GitHub](https://github.com/ChromaDotNet/ChromaDB.Client)
  * [Documentation](https://github.com/ChromaDotNet/ChromaDB.Client#readme)
  * [Tested versions](https://github.com/ChromaDotNet/ChromaDB.Client/blob/main/docs/COMPATIBILITY.md)
  * [OpenSSF Best Practices: passing](https://www.bestpractices.dev/projects/15264)

## Install

`dotnet add package ChromaDotNet.Client`

For Microsoft.Extensions.DependencyInjection, with `AddChromaClient`, `AddKeyedChromaClient` and `CreateChromaClient`:

`dotnet add package ChromaDotNet.Client.DependencyInjection`

The packages are named `ChromaDotNet.*`. The namespaces keep their original names, such as `ChromaDB.Client`.

## Example

Start Chroma, for example with Docker:

`docker run -p 8000:8000 chromadb/chroma:1.5.9`

Then add two records with their embeddings and query the nearest one:

Program.cs
    
    
    using ChromaDB.Client;
    using ChromaDB.Client.Models;
    
    using var client = new ChromaClient("http://localhost:8000");
    var collection = await client.GetOrCreateCollectionAsync("fruits");
    var fruits = client.GetCollectionClient(collection);
    
    await fruits.AddAsync(["apple", "lemon"],
        embeddings: [new([1f, 0f, 0f]), new([0f, 1f, 0f])],
        documents: ["A crunchy apple", "A sour lemon"]);
    
    var results = await fruits.QueryAsync(new ReadOnlyMemory<float>([0f, 0.9f, 0.1f]),
        nResults: 1, include: ChromaQueryInclude.Documents | ChromaQueryInclude.Distances);
    
    foreach (var result in results)
        Console.WriteLine($"{result.Id}: {result.Document} ({result.Distance})");

Tested on 6 November 2026 with ChromaDotNet.Client 2.20.0 on Chroma 1.5.9.

[Try it in your browser](https://codespaces.new/ChromaDotNet/ChromaDB.Client?quickstart=1): a GitHub Codespace with .NET and Chroma, then `dotnet run Samples/quickstart.cs`.

## Documentation

  * [Connecting](https://github.com/ChromaDotNet/ChromaDB.Client/blob/main/docs/connecting.md): the URI, the `HttpClient`, threads, the API version, authentication and connection strings.
  * [Dependency injection](https://github.com/ChromaDotNet/ChromaDB.Client/blob/main/docs/dependency-injection.md): `AddChromaClient` and `AddKeyedChromaClient`.
  * [Collections](https://github.com/ChromaDotNet/ChromaDB.Client/blob/main/docs/collections.md): collections, tenants and databases, collections by id.
  * [Records](https://github.com/ChromaDotNet/ChromaDB.Client/blob/main/docs/records.md): URIs, large writes, deletes and base64 embeddings.
  * [Embeddings](https://github.com/ChromaDotNet/ChromaDB.Client/blob/main/docs/embeddings.md): embeddings from an `IEmbeddingGenerator`, query texts and the default embedding function of Chroma.
  * [Metadata values](https://github.com/ChromaDotNet/ChromaDB.Client/blob/main/docs/metadata.md): values, lists, deletions in updates and upserts, document copies, conversions.
  * [Queries and filters](https://github.com/ChromaDotNet/ChromaDB.Client/blob/main/docs/queries-and-filters.md): queries, `GetAsync` in pages, where and document filters.
  * [Upsert strategies](https://github.com/ChromaDotNet/ChromaDB.Client/blob/main/docs/upsert-strategies.md): `Server` and `SkipUnchangedEmbeddings`.
  * [Index settings](https://github.com/ChromaDotNet/ChromaDB.Client/blob/main/docs/index-settings.md): the distance, the HNSW and SPANN settings, `ModifyConfigurationAsync`.
  * [Errors](https://github.com/ChromaDotNet/ChromaDB.Client/blob/main/docs/errors.md): `ChromaException` and `CollectionExistsAsync`.
  * [Mocks in tests](https://github.com/ChromaDotNet/ChromaDB.Client/blob/main/docs/mocking.md): mocking `ChromaClient` and `ChromaCollectionClient`.
  * [Health, traces and metrics](https://github.com/ChromaDotNet/ChromaDB.Client/blob/main/docs/observability.md): `HealthcheckAsync` and OpenTelemetry.
  * [Chroma Cloud](https://github.com/ChromaDotNet/ChromaDB.Client/blob/main/docs/chroma-cloud.md): options, identity, batches, quotas and the operations of Chroma Cloud.
  * [Search](https://github.com/ChromaDotNet/ChromaDB.Client/blob/main/docs/search.md): the Search API and hybrid search with BM25.
  * [Sparse vectors and schema](https://github.com/ChromaDotNet/ChromaDB.Client/blob/main/docs/sparse-vectors-and-schema.md): sparse vectors, BM25 indexes and the indexes of a schema.
  * [Platforms](https://github.com/ChromaDotNet/ChromaDB.Client/blob/main/docs/platforms.md): the builds, trimming and NativeAOT.

The [compatibility table](https://chromadotnet.org/compatibility/) shows every check of the latest release on Chroma 1.5.9 and on Chroma Cloud, on each .NET runtime and platform.
