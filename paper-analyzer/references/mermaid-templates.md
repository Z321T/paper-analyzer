# Mermaid Diagram Templates for Paper Analysis

Use these templates as starting points. Adapt node labels and structure to the specific paper. Always wrap in ```mermaid code blocks.

---

## Template 1: Paper Overview Mindmap

Used in Deep mode at the start to give a structural overview.

```mermaid
mindmap
  root((Paper Title<br/>Authors, Year))
    Problem
      [Core problem being solved]
      [Why existing methods fail]
    Method
      [Module 1 name]
        [Sub-component A]
        [Sub-component B]
      [Module 2 name]
        [Sub-component C]
      [Module 3 name]
    Experiments
      [Dataset 1]
      [Dataset 2]
      [Key metric]
    Contributions
      [Contribution 1]
      [Contribution 2]
      [Contribution 3]
```

**Adaptation notes**:
- If the paper has 4+ modules, group them under 2-3 category nodes
- Key metrics in the Experiments branch should include specific numbers when impressive

---

## Template 2: Method Flowchart (Pipeline/Architecture)

Used in both Skim (simplified) and Deep (full) modes. This is the most important diagram.

### Basic version (for Skim mode)

```mermaid
flowchart LR
    A[Input] --> B[Stage 1:<br/>Description]
    B --> C[Stage 2:<br/>Description]
    C --> D[Stage 3:<br/>Description]
    D --> E[Output]
```

### Detailed version (for Deep mode)

```mermaid
flowchart TB
    subgraph Input
        A1[Text Input]
        A2[Image Input]
        A3[Other Input]
    end

    subgraph Stage1["Stage 1: Name"]
        B1[Step 1.1]
        B2[Step 1.2]
        B3[Step 1.3]
        B1 --> B2 --> B3
    end

    subgraph Stage2["Stage 2: Name"]
        C1[Step 2.1]
        C2[Step 2.2]
        C1 --> C2
    end

    subgraph Output
        D1[Output 1]
        D2[Output 2]
    end

    A1 & A2 & A3 --> Stage1
    Stage1 --> Stage2
    Stage2 --> D1
    Stage2 --> D2

    style Stage1 fill:#e1f5fe
    style Stage2 fill:#f3e5f5
```

**Design guidelines**:
- Use subgraphs to group related steps
- Color-code: input (green), processing (blue/purple), output (orange), memory/storage (gray)
- Label edges with data flow descriptions when non-obvious
- Show iterative/loop-back connections with dashed lines
- Show memory/database interactions with distinct node shapes `[(database)]`

---

## Template 3: Training Pipeline

Use when the training process has multiple stages or is non-trivial.

```mermaid
flowchart TB
    subgraph DataPrep["Data Preparation"]
        D1[Raw Data] --> D2[Preprocessing]
        D2 --> D3[Train/Val/Test Split]
    end

    subgraph Stage1["Stage 1: Name"]
        S1[Model Init] --> S2[Training Step]
        S2 --> S3[Checkpoint]
        S3 -.->|N epochs| S2
    end

    subgraph Stage2["Stage 2: Name"]
        T1[Load S1 Checkpoint] --> T2[Training Step]
        T2 --> T3[Final Model]
        T2 -.->|N epochs| T2
    end

    DataPrep --> Stage1
    Stage1 --> Stage2
    T3 --> E[Evaluation]

    style DataPrep fill:#e8f5e9
    style Stage1 fill:#e1f5fe
    style Stage2 fill:#f3e5f5
```

---

## Template 4: Memory / Data Flow Architecture

Use for papers with complex data storage and retrieval (RAG, memory systems, databases).

```mermaid
flowchart TB
    subgraph Query["Query Side"]
        Q[User Query] --> P[Preprocessor]
    end

    subgraph Retrieval["Retrieval"]
        P -->|Condition 1| R1[(Store A)]
        P -->|Condition 2| R2[(Store B)]
        R1 --> M[Merger/Reranker]
        R2 --> M
    end

    subgraph Generation["Generation"]
        M --> G[Generator]
        G --> O[Output]
    end

    subgraph Update["Update (async)"]
        O -.->|Post-processing| U[Updater]
        U -.->|Write| R1
        U -.->|Write| R2
    end

    style Query fill:#fff3e0
    style Retrieval fill:#e1f5fe
    style Generation fill:#e8f5e9
    style Update fill:#fce4ec,stroke-dasharray: 5 5
```

---

## Template 5: Experiment Pipeline

```mermaid
flowchart LR
    subgraph Setup["Setup"]
        direction TB
        M[Model Variants] --> D[Datasets]
        D --> B[Baselines]
    end

    subgraph Run["Execution"]
        direction TB
        T[Train] --> E[Evaluate]
        E --> A[Analyze]
    end

    subgraph Report["Reporting"]
        direction TB
        R1[Main Results Table]
        R2[Ablation Table]
        R3[Qualitative Examples]
    end

    Setup --> Run --> Report
```

---

## Template 6: Comparison Matrix (for multi-paper comparison mode)

```mermaid
flowchart TB
    subgraph PaperA["Paper A: Title"]
        A1[Method Route A]
        A2[Key Strength]
        A3[Key Limitation]
    end

    subgraph PaperB["Paper B: Title"]
        B1[Method Route B]
        B2[Key Strength]
        B3[Key Limitation]
    end

    subgraph Common["Common Ground"]
        C1[Shared Problem]
        C2[Shared Dataset]
    end

    Common --> PaperA
    Common --> PaperB

    style Common fill:#f5f5f5
```

---

## Template 7: Decision / Logic Flow in Method

Use when the method involves branching logic or decision points.

```mermaid
flowchart TD
    A[Start] --> B{Condition?}
    B -->|Yes| C[Action 1]
    B -->|No| D[Action 2]
    C --> E{Sufficient?}
    D --> E
    E -->|Yes| F[Output]
    E -->|No| G[Re-plan]
    G -.-> B
```

---

## General Mermaid Best Practices

1. **Node labels**: Keep them short (under 50 chars). Move details to surrounding text.
2. **Direction**: 
   - Pipeline/sequential = TB (top to bottom)
   - Data flow with branches = LR (left to right)
   - Decision trees = TD
3. **Shapes**:
   - `[rectangle]` = process step
   - `{diamond}` = decision/condition
   - `[(cylinder)]` = database/storage
   - `([rounded])` = start/end
   - `((circle))` = connector
4. **Subgraphs**: Use to group logically related steps and reduce visual complexity
5. **Colors**: Use sparingly. Blue for processing, green for data, purple for model, gray for external/optional
6. **Dashed lines** (`-.->`): Use for async, optional, or iterative connections
7. **Thick lines** (`==>`) : Use for the main/happy path
8. **Always test**: If a diagram has more than 30 nodes, split into sub-diagrams
