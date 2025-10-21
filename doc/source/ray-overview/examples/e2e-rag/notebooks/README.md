
# Evaluate RAG using Batch Inference with Ray Data LLM
```mermaid
---
config:
  flowchart:
    nodeSpacing: 400
    rankSpacing: 100
    curve: linear
  themeVariables:
    fontSize: 50px
---
flowchart LR
    %% === Top pipeline ===
    A[CSV Evaluation Data]:::data -->|64 Requests| B[Embedder]:::node
    B -->|Relevant Text Chunks| C[Chroma]:::node
    C --> D[Prompt Template]:::node
    D -->|64 Prompts| E[Rendered Prompt]:::node
    E --> F[LLM Batch Processor]:::node
    F --> G[Final Response]:::node
    G -->|64 Responses| H[CSV Output]:::data

    %% === Colored section containers ===
    subgraph R1["Ray Data"]
        style R1 fill:#e8f2ff,stroke:#3a6cb0,stroke-width:1px,rx:10,ry:10;
        B
        C
        D
        E
        N1["🖥️ on CPU workers"]:::note
    end

    subgraph R2["Ray Data LLM"]
        style R2 fill:#e6f7e9,stroke:#27ae60,stroke-width:1px,rx:10,ry:10;
        F
        G
        N2["🖥️ on GPU workers"]:::note
    end

    %% === Styles for nodes ===
    classDef data fill:#ffffff,stroke:#333,color:#000,rx:6,ry:6;
    classDef node fill:#ffffff,stroke:#555,color:#000,rx:6,ry:6;
    classDef note fill:#f9fbff,stroke:none,color:#3a6cb0,font-style:italic;
```

# Evaluate RAG using Batch Inference with Ray Data LLM on only CPU Workers

## Solve GPU Availbiltiy problems
   - Ray Cluster which processes data might not have GPU workers.  
   - Solve the problem by moving LLM infernece to CPU workers with a smaller model close to Data. 
## Save Cost
  - smaller model like Llama3.1 8B performs well on Intel Xeon 6.
  - save cost by serving small model on Xeon 6

```mermaid
---
config:
  flowchart:
    nodeSpacing: 400
    rankSpacing: 100
    curve: linear
  themeVariables:
    fontSize: 50px
---
flowchart LR
    %% === Top pipeline ===
    A[CSV Evaluation Data]:::data -->|64 Requests| B[Embedder]:::node
    B -->|Relevant Text Chunks| C[Chroma]:::node
    C --> D[Prompt Template]:::node
    D -->|64 Prompts| E[Rendered Prompt]:::node
    E --> F[LLM Batch Processor]:::node
    F --> G[Final Response]:::node
    G -->|64 Responses| H[CSV Output]:::data

    %% === Colored section containers ===
    subgraph R1["Ray Data"]
        style R1 fill:#e8f2ff,stroke:#3a6cb0,stroke-width:1px,rx:10,ry:10;
        B
        C
        D
        E
        N1["🖥️ on CPU workers"]:::note
    end

    subgraph R2["Ray Data LLM"]
        style R2 fill:#e8f2ff,stroke:#3a6cb0,stroke-width:1px,rx:10,ry:10;
        F
        G
        N2["🖥️ on CPU workers"]:::note
    end

    %% === Styles for nodes ===
    classDef data fill:#ffffff,stroke:#333,color:#000,rx:6,ry:6;
    classDef node fill:#ffffff,stroke:#555,color:#000,rx:6,ry:6;
    classDef note fill:#f9fbff,stroke:none,color:#3a6cb0,font-style:italic;
```




