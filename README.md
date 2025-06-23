# Azure Serverless Billing Record Cost Optimization

This repository provides a solution for optimizing storage costs for a read-heavy billing record system on Azure, leveraging a serverless architecture with tiered storage.

## Table of Contents

- [Problem Statement](#problem-statement)
- [Solution Overview](#solution-overview)
- [Architecture](architecture/architecture-diagram.md)
- [Key Azure Services Used](#key-azure-services-used)
- [Implementation Details](implementation/)
- [DevOps (CI/CD) Strategy](devops/cicd-strategy.md)
- [Cost Optimization Benefits](cost-optimization/savings-summary.md)
- [How to Deploy](deployment/how-to-deploy.md)
- [Running the Backfill](deployment/backfill-guide.md)
- [Configuration](deployment/configuration.md)
- [Monitoring & Alerting](monitoring/monitoring-alerts.md)
- [Future Enhancements](enhancements/future-roadmap.md)
- [Contributing](#contributing)
- [License](LICENSE)

## Problem Statement

Our existing Azure Cosmos DB system stores billing records, with each record potentially up to 300 KB. The database has grown to over 2 million records, leading to significant Cosmos DB costs. Records older than three months are rarely accessed yet consume high-performance storage. We need a solution that ensures no data loss, no downtime, and no changes to existing APIs.

## Solution Overview

A **tiered storage strategy** is used:
- **Hot Data:** Cosmos DB stores recent records.
- **Cold/Archived Data:** Older records are moved to Azure Data Lake Storage Gen2 (Cool/Archive tiers).
- Smart retrieval ensures archived data is accessible within seconds via the same API.

## Key Azure Services Used
- Azure Cosmos DB
- Azure Blob Storage (Data Lake Gen2)
- Azure Functions
- Azure API Management / Gateway
- Azure Logic Apps
- Azure DevOps
