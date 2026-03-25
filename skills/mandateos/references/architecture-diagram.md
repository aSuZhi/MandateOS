# Architecture Diagram

Use these Mermaid diagrams in GitHub, OpenClaw docs, or your competition deck.

## System Architecture

```mermaid
flowchart LR
    U[User / Trader] --> P[Short Prompt]
    P --> OC[OpenClaw]
    OC --> MS[MandateOS Skill]

    subgraph Core[MandateOS Control Layer]
        MP[Mandate Profile]
        IR[Intent Resolver]
        RR[Regime Router]
        DG[Discipline Gate]
        OCN[Output Contract]
    end

    MS --> MP
    MS --> IR
    MP --> RR
    IR --> RR
    RR --> DG
    DG --> OCN

    subgraph OKX[Lower-Level OKX Skills]
        MK[okx-cex-market]
        PF[okx-cex-portfolio]
        TR[okx-cex-trade]
        ER[okx-cex-earn]
        BT[okx-cex-bot]
    end

    RR --> MK
    RR --> PF
    RR --> TR
    RR --> ER
    RR --> BT

    subgraph Exec[Execution Layer]
        CLI[OKX CLI / agent-tradekit]
        API[OKX Exchange APIs]
    end

    MK --> CLI
    PF --> CLI
    TR --> CLI
    ER --> CLI
    BT --> CLI
    CLI --> API

    OCN --> R[市场判断 / 行动提案 / 纪律校验]
    R --> U
```

## Request Flow

```mermaid
flowchart TD
    A[User Prompt] --> B[Resolve Active Mandate]
    B --> C[Classify Intent]
    C --> D[Read Market / Account Context]
    D --> E[Generate Proposal Variants]
    E --> F{Execution Prompt + explicit-execution?}

    F -- No --> G[Shadow Mode<br/>Return Proposal Only]
    F -- Yes --> H[Choose Allowed Module Path]
    H --> I[Route to trade / earn / bot]
    I --> J[Post-Action Verification]
    J --> K[Review Against Original Mandate]

    G --> L[Output Contract]
    K --> L
    L --> M[市场判断 / 行动提案 / 纪律校验]
```

## Slide-Friendly Framing

Use this one-liner under the diagram:

`MandateOS adds a mandate-aware control layer above OKX skills, so one short prompt becomes a disciplined multi-skill workflow instead of a single raw API call.`
