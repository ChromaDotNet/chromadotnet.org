Source: https://chromadotnet.org/libraries/embeddings/

[Libraries](https://chromadotnet.org/libraries/)

# ChromaDotNet.Embeddings

The default embedding function of Chroma for .NET: all-MiniLM-L6-v2 in ONNX, with the same vectors as the Python client. An `IEmbeddingGenerator` of Microsoft.Extensions.AI, for ChromaDotNet.Client. It also computes the two hosted embedding functions of Chroma Cloud on your machine: `chroma-cloud-qwen` and `chroma-cloud-splade`.

  * Latest release **1.0.0-preview.6**
  * [NuGet](https://www.nuget.org/packages/ChromaDotNet.Embeddings)
  * [GitHub](https://github.com/ChromaDotNet/ChromaDB.Embeddings)
  * [Documentation](https://github.com/ChromaDotNet/ChromaDB.Embeddings#readme)
  * [Try it in your browser](https://codespaces.new/ChromaDotNet/ChromaDB.Local?quickstart=1)

## Install

`dotnet add package ChromaDotNet.Embeddings --prerelease`

## Example

Start Chroma, for example with Docker:

`docker run -p 8000:8000 chromadb/chroma:1.5.9`

Then add three records by their text and query them with a sentence:

Program.cs
    
    
    using ChromaDB.Client;
    using ChromaDB.Client.Models;
    using ChromaDotNet.Embeddings;
    
    using var embeddings = await ChromaDefaultEmbeddingGenerator.CreateAsync();
    using var client = new ChromaClient("http://localhost:8000");
    var collection = await client.GetOrCreateCollectionAsync(
        new ChromaCollectionDefinition("fruits") { EmbeddingGenerator = embeddings });
    var fruits = client.GetCollectionClient(collection).WithEmbeddingGenerator(embeddings);
    
    await fruits.AddAsync(["apple", "lemon", "banana"],
        documents: ["A crunchy apple", "A sour lemon", "A sweet banana"]);
    
    var results = await fruits.QueryAsync("something sour", nResults: 1,
        include: ChromaQueryInclude.Documents | ChromaQueryInclude.Distances);
    
    foreach (var result in results)
        Console.WriteLine($"{result.Id}: {result.Document} ({result.Distance})");

Tested on 1 November 2026 with ChromaDotNet.Embeddings 1.0.0-preview.6 and ChromaDotNet.Client 2.18.1 on Chroma 1.5.9: the nearest record to “something sour” is the lemon.

The first start downloads the model, 83 MB, into the cache folder of Python, and checks it with its SHA-256.

## The hosted functions of Chroma Cloud

Chroma Cloud computes two embedding functions on its servers. This package computes them on your machine, on the CPU, with the same models in ONNX. It needs no account and no API key.

  * `ChromaQwenEmbeddingGenerator` is `chroma-cloud-qwen`: [Qwen3-Embedding-0.6B](https://huggingface.co/Qwen/Qwen3-Embedding-0.6B), with vectors of 1024 dimensions. A text can have up to 8192 tokens, with its instruction: a longer one is refused with an `ArgumentException`. It downloads 1.2 GB in float16, the default, or 2.4 GB in float32.
  * `ChromaSpladeEmbeddingGenerator` is `chroma-cloud-splade`: the sparse vectors of [SPLADE, prithivida/Splade_PP_en_v1](https://huggingface.co/prithivida/Splade_PP_en_v1). It downloads 533 MB.

With Chroma started as above, add two documents with Qwen and query them with a question:

Program.cs
    
    
    using ChromaDB.Client;
    using ChromaDB.Client.Models;
    using ChromaDotNet.Embeddings;
    
    using var qwen = await ChromaQwenEmbeddingGenerator.CreateAsync();
    using var client = new ChromaClient("http://localhost:8000");
    var collection = await client.GetOrCreateCollectionAsync(new ChromaCollectionDefinition("code"));
    var forDocuments = client.GetCollectionClient(collection).WithEmbeddingGenerator(qwen);
    var forQueries = client.GetCollectionClient(collection).WithEmbeddingGenerator(qwen.Queries);
    
    await forDocuments.AddAsync(["add", "lemon"],
        documents: ["def add(a, b): return a + b", "A sour lemon"]);
    
    var results = await forQueries.QueryAsync("How do I add two numbers?", nResults: 1,
        include: ChromaQueryInclude.Documents | ChromaQueryInclude.Distances);
    
    foreach (var result in results)
        Console.WriteLine($"{result.Id}: {result.Document} ({result.Distance})");

Tested on 1 November 2026 with ChromaDotNet.Embeddings 1.0.0-preview.6 and ChromaDotNet.Client 2.18.1 on Chroma 1.5.9: the nearest record to “How do I add two numbers?” is the add function.

The first start downloads the files of the model from Hugging Face, at a fixed revision, and checks them with their SHA-256.

Qwen reads documents and queries with different instructions, as Chroma Cloud does: `qwen` embeds the documents, and `qwen.Queries` the queries.

SPLADE gives a `ChromaSparseEmbedding`. Pass its indices and values to ChromaDotNet.Client as a `ChromaSparseVector`.

On the texts of the tests, the vectors are those of Chroma Cloud within small differences: 1 − cosine up to 4.53e-6 for Qwen, on the runners of our CI, and values up to 0.045 apart for SPLADE.

## Speed

In our tests, these are the times of one text at a time, on the CPU, with the files already downloaded. They are the median of 5 runs, on an Azure VM Standard_D8ads_v6: 8 logical processors of an AMD EPYC 9V74, 32 GB, Ubuntu 24.04, .NET 8 and the default 4 threads.

On a Standard_D8ads_v6| Qwen in float16| SPLADE  
---|---|---  
A short query: 28 to 38 tokens with the instruction of Qwen, 7 to 16 for SPLADE| 98 ms| 15 ms  
A document of about 100 tokens| 283 ms| 70 ms  
A document of 661 tokens for Qwen, truncated to 512 for SPLADE| 2.0 s| 337 ms  
A document of 3322 tokens for Qwen, truncated to 512 for SPLADE| 18.8 s| 334 ms  
Peak working set of the process| 4.3 GB| 0.83 GB  
  
Qwen is slow to index many long documents: the time of each token grows with the length of the text. On a document of about 100 tokens, SPLADE takes about a quarter of the time of Qwen. SPLADE reads 512 tokens at most, so a longer document takes it no more time.

On the same VM, for Qwen in float16, onnxruntime 1.28.0 in Python with the same files takes the same times up to 661 tokens, and 19.1 s on the document of 3322 tokens. A NativeAOT build takes the same times, within the differences between runs. The [README](https://github.com/ChromaDotNet/ChromaDB.Embeddings#their-speed) has the load, float32 and the conditions of the runs.

## Platforms

Windows x64 and arm64, Linux x64 and arm64, and macOS arm64 on .NET 8 and later; .NET Framework 4.6.2 and later on Windows, in a 64-bit process. Intel Macs and 32-bit processes are not supported. The generators of the hosted models run on the same platforms.

On .NET 8 and later the default model runs through ElBruno.LocalEmbeddings by Bruno Capuano; on .NET Framework, and for the hosted models, on ONNX Runtime. Tokenizer: Hugging Face, through Tokenizers.DotNet.
