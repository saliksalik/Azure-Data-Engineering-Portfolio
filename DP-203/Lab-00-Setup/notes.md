# Lab 00: Manual Environment Setup 🛠️

## Overview
Instead of using the automated lab scripts (which often fail on Student Subscriptions), I manually provisioned the Azure Data Engineering infrastructure.

## 🏗️ Architecture Configured
* **Resource Group:** `dp203-training-rg` (East US)
* **Storage:** Azure Data Lake Storage Gen2 (Hierarchical Namespace Enabled)
* **Compute:** Azure Synapse Workspace with a dedicated Apache Spark Pool (Small Node Size).

## 💡 Key Learnings
* Learned the difference between Blob Storage and ADLS Gen2 (Hierarchical Namespace).
* Configured proper IAM roles (Storage Blob Data Contributor) to allow Synapse to talk to the Data Lake.
