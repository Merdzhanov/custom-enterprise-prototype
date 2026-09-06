# Custom Executive Prototype Architecture

## Overview

- **Compute Target:** Cloud Run
- **Session Store:** Cloud SQL for PostgreSQL
- **Vector Memory:** Cloud SQL for PostgreSQL with pgvector
- **Git Provider:** GITHUB

## Components

- **Cloud Run**
- **Cloud SQL for PostgreSQL**
- **Cloud Pub/Sub**
- **Vertex AI Embedding API**
- **Cloud Load Balancing**
- **Cloud Build**
- **Artifact Registry**
- **Secret Manager**
- **GitHub**

## Topology Diagram

```mermaid
graph TD
    subgraph "User Request Flow"
        direction TB
        User[Executive User] --> LB(Cloud Load Balancing)
        LB --> CR(Cloud Run Service)
    end

    subgraph "Backend Logic"
        direction TB
        CR -- "1. Auth & Session (RLS)" --> CSQL(Cloud SQL for PostgreSQL)
        CR -- "2. Generate Embeddings" --> VAI(Vertex AI Embedding API)
        VAI -- "text-embedding-005" --> CR
        CR -- "3. Store Data & Vectors (pgvector)" --> CSQL
        CR -- "4. Vector Similarity Search" --> CSQL
        CR -- "5. Publish Async Task" --> PS(Cloud Pub/Sub)
        PS -- "6. Trigger Subscriber" --> CR
        CR -- "Get Secrets" ---> SM(Secret Manager)
    end

    subgraph "CI/CD Pipeline"
        direction LR
        Dev[Developer] -- git push --> GH(GitHub Repo)
        GH -- webhook --> CB(Cloud Build)
        CB -- build --> AR(Artifact Registry)
        CB -- deploy --> CR
    end

    style User fill:#EA4335,color:#fff,stroke:#333
    style LB fill:#4285F4,color:#fff,stroke:#333
    style CR fill:#4285F4,color:#fff,stroke:#333
    style CSQL fill:#34A853,color:#fff,stroke:#333
    style VAI fill:#EA4335,color:#fff,stroke:#333
    style PS fill:#FBBC05,color:#000,stroke:#333
    style SM fill:#FBBC05,color:#000,stroke:#333
    style GH fill:#24292e,color:#fff,stroke:#333
    style CB fill:#4285F4,color:#fff,stroke:#333
    style AR fill:#34A853,color:#fff,stroke:#333
```

---
*Generated autonomously by the Enterprise Fleet (ArchitectAgent).*
