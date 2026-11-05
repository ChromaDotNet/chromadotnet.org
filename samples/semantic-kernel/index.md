Source: https://chromadotnet.org/samples/semantic-kernel/

[Samples](https://chromadotnet.org/samples/)

# Semantic Kernel samples with Chroma

Semantic Kernel samples that use Chroma as the vector store through [CommunityToolkit.VectorData.Chroma](https://chromadotnet.org/libraries/vectordata/), the Chroma provider of the AI Community Toolkit. They follow the structure of the Semantic Kernel .NET samples. The samples that use a chat model also come with models that run locally in Ollama.

  * [GitHub](https://github.com/ChromaDotNet/ChromaDB.SemanticKernel.Sample)
  * [Documentation](https://github.com/ChromaDotNet/ChromaDB.SemanticKernel.Sample#readme)

## The samples

  * [Vector search](https://github.com/ChromaDotNet/ChromaDB.SemanticKernel.Sample/blob/main/samples/Concepts/Memory/VectorStore_VectorSearch_MultiStore_Chroma.cs): ingests a glossary into Chroma and searches it, with and without a filter.
  * [Data models](https://github.com/ChromaDotNet/ChromaDB.SemanticKernel.Sample/blob/main/samples/Concepts/Memory/VectorStore_DynamicDataModel_Interop_Chroma.cs): writes records as dictionaries described by a record definition, then reads them back as a .NET data model.
  * [Paging](https://github.com/ChromaDotNet/ChromaDB.SemanticKernel.Sample/blob/main/samples/Concepts/Memory/VectorStore_VectorSearch_Paging_Chroma.cs): uses `Top` and `Skip` to page through vector search results over 1,000 records. Needs no model.
  * [Hybrid search](https://github.com/ChromaDotNet/ChromaDB.SemanticKernel.Sample/blob/main/samples/Concepts/Memory/VectorStore_HybridSearch_Simple_Chroma.cs): searches a glossary on Chroma Cloud with a vector and keywords, with and without a filter.
  * [RAG agent](https://github.com/ChromaDotNet/ChromaDB.SemanticKernel.Sample/blob/main/samples/Concepts/Agents/ChatCompletion_Rag_Chroma.cs): a `ChatCompletionAgent` that answers through a `TextSearchProvider`, from documents that a `TextSearchStore` keeps in Chroma.
  * [Text search](https://github.com/ChromaDotNet/ChromaDB.SemanticKernel.Sample/blob/main/samples/GettingStartedWithTextSearch/Step5_Search_With_Chroma.cs): searches records that have their own data model in Chroma, through `VectorStoreTextSearch`.

## Run the samples without a model

The tests run the scenario of each sample with the same configuration, except the hybrid search, which needs Chroma Cloud. Chroma runs in a container that ChromaDotNet.Testcontainers starts on a free port. They need the .NET 10 SDK and Docker, but no model:

Terminal
    
    
    git clone https://github.com/ChromaDotNet/ChromaDB.SemanticKernel.Sample
    cd ChromaDB.SemanticKernel.Sample
    dotnet test test/ChromaDB.SemanticKernel.Sample.Tests

Tested on 25 October 2026: 10 tests of 10 pass.

To run the other samples with Azure OpenAI or with models in Ollama, see the [README](https://github.com/ChromaDotNet/ChromaDB.SemanticKernel.Sample#readme).
