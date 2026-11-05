Source: https://chromadotnet.org/samples/agent-framework/

[Samples](https://chromadotnet.org/samples/)

# Microsoft Agent Framework samples with Chroma

Microsoft Agent Framework samples that use Chroma as the vector store through [CommunityToolkit.VectorData.Chroma](https://chromadotnet.org/libraries/vectordata/), the Chroma provider of the AI Community Toolkit. They follow the form of the Agent Framework .NET samples, and each sample also comes in a variant with models that run locally in Ollama.

  * [GitHub](https://github.com/ChromaDotNet/ChromaDB.AgentFramework.Sample)
  * [Documentation](https://github.com/ChromaDotNet/ChromaDB.AgentFramework.Sample#readme)

## The samples

  * [Memory with Chroma](https://github.com/ChromaDotNet/ChromaDB.AgentFramework.Sample/tree/main/samples/AgentWithMemory_Step10_MemoryUsingChroma): persists chat history in Chroma with `ChatHistoryMemoryProvider` and recalls it in a new session.
  * [RAG with Chroma](https://github.com/ChromaDotNet/ChromaDB.AgentFramework.Sample/tree/main/samples/AgentWithRAG_Step06_ChromaRAG): answers from documentation stored in Chroma with a custom schema, through `TextSearchProvider`.
  * [RAG with Chroma and the TextSearchStore](https://github.com/ChromaDotNet/ChromaDB.AgentFramework.Sample/tree/main/samples/AgentWithRAG_Step07_ChromaBasicTextRAG): answers from documents stored in Chroma, through `TextSearchProvider`.

Each one uses Microsoft Foundry models, and has a variant with models that run locally in Ollama.

## Run the samples without a model

The tests run the scenario of each sample with the same configuration, against Chroma in a container started with ChromaDotNet.Testcontainers. They need the .NET 10 SDK and Docker, but no model:

Terminal
    
    
    git clone https://github.com/ChromaDotNet/ChromaDB.AgentFramework.Sample
    cd ChromaDB.AgentFramework.Sample
    dotnet test

Tested on 25 October 2026: 10 tests of 10 pass.

To run the samples with Microsoft Foundry or with models in Ollama, see the [README](https://github.com/ChromaDotNet/ChromaDB.AgentFramework.Sample#readme).
