# Lab 02: Serverless SQL Exploration

## Phase 1: Data Lake Preparation
- Verified the structure and permissions of the Data Lake Storage Gen2 account.
- Ensured the /users/data/ directory contains the required product.csv dataset.

## Phase 2: Serverless SQL Pool Setup
- Connected to the Built-in Serverless SQL Pool in Synapse Studio.
- Configured workspace firewall rules to allow access from my client IP.

## Phase 3: Querying Data with OPENROWSET
- Developed a T-SQL query using the OPENROWSET function to read product.csv directly from the Data Lake.
- Demonstrated "Schema-on-Read" by querying the CSV file without importing it into a database.

## Phase 4: Data Validation & Troubleshooting
- Validated query results for accuracy and completeness.
- Resolved any access or connectivity issues by adjusting IAM roles and firewall settings.

## Phase 5: Documentation & Best Practices
- Documented the process and SQL scripts used for querying external data sources.
- Followed best practices for security and cost management in serverless environments.

---

*Attach relevant screenshots and SQL scripts here as you complete the lab.*
