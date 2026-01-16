# Lab 01: Compute and Storage Exploration

## Phase 1: Foundation & Storage Setup
- Provisioned a Resource Group in a stable region (Central India) to avoid subscription quota restrictions.
- Created an Azure Data Lake Storage (ADLS) Gen2 account (`datalakesalikahmad`).
- Crucial Step: Enabled Hierarchical Namespace during setup to support big data analytics workloads.

![The Overview page of your Storage Account](The%20Overview%20page%20of%20your%20Storage%20Account.png)

## Phase 2: Synapse Workspace Deployment
- Deployed the Azure Synapse Analytics Workspace (`synapse-ws-salikahmad`).
- Configured the primary ADLS Gen2 account as the default storage for logs and job output.
- Identity Management: Integrated both SQL Local Auth and Microsoft Entra ID for secure access.

![The Overview page of your Synapse Workspace](The%20Overview%20page%20of%20your%20Synapse%20Workspace.png)

## Phase 3: Identity & Access Management (IAM)
- Role Assignment: Granted the Storage Blob Data Contributor role to my user account and the Synapse Managed Identity.
- Security Principle: Applied the Principle of Least Privilege to ensure the workspace could read/write data safely without granting full "Owner" permissions.
- Resolved initial "Access Denied" errors by ensuring the IAM roles propagated across the storage account.

![The Role assignments list showing My Name(Salik)](The%20Role%20assignments%20list%20showing%20My%20Name(Salik).png)

## Phase 4: Data Ingestion (Manual)
- Structured the Data Lake by creating a dedicated `/users/data/` directory.
- Ingested the sample `product.csv` dataset into the lake using the Storage Browser.

![The file list inside my container](The%20file%20list%20inside%20my%20container.png)

## Phase 5: Data Discovery with Serverless SQL
- Explored "Schema-on-Read" capabilities using the Built-in Serverless SQL Pool.
- Technical Skill: Developed a T-SQL script using the OPENROWSET function to query CSV data directly from the lake without moving it into a database.
- Troubleshooting: Resolved networking and port 1443 connectivity issues by configuring the workspace firewall.

![Notebook with the code and the table results](Notebook%20with%20the%20code%20and%20the%20table%20results.png)

## Phase 6: Big Data Processing with PySpark
- Created an Apache Spark Pool (`SparkPool01`) with a "Small" node configuration to balance performance and cost.
- Developed a PySpark Notebook to load the CSV into a Spark DataFrame.
- Visualization: Used the `display()` function to preview data schema and content.

## Phase 7: Resource Optimization & Cost Management
- Monitoring: Used the Monitor Hub to track active Spark sessions and application status.
- Credit Protection: Enabled Auto-pause (15-minute idle time) and manually cancelled active sessions to preserve Azure for Students credits.

![Saving cost by closing the pools](Saving%20cost%20by%20closing%20the%20pools.png)
