# Architecture Overview

```mermaid
graph TD
    A[Existing Write API] --> B(Azure Function / API Gateway)
    B --> C[Cosmos DB (Hot Data)]
    C -- Data Archival Trigger (Azure Function / Logic App) --> D[Azure Blob Storage (Archive - Cool/Archive Tier)]
    E[Existing Read API] --> F{Azure Function / API Gateway}
    F -- Check Cosmos DB First --> C
    F -- If not found, check Blob Storage --> G(Azure Data Lake Storage Gen2 / Blob Storage for Queries)
    G -- If old record requested --> H[Azure Functions / Azure Logic App for Retrieval]
    H -- Hydration/Retrieval Logic --> I(Cosmos DB - Temporary Hot Data for old record)
    I --> F

    subgraph Data Archival Workflow
        C --> J[Change Feed Processor (Azure Function)]
        J --> K[Azure Function (Data Transformation/Serialization)]
        K --> D
    end

    subgraph Data Retrieval Workflow for Archived Data
        F --> L[Azure Function (Query Blob Storage)]
        L -- If record found in Blob --> M[Azure Function (De-serialization/Rehydration)]
        M --> I
    end

    N[Azure DevOps Pipelines] -- Deploys & Manages --> O[Azure Resources]
```