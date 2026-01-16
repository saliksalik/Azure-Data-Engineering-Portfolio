# Lab 02: Serverless SQL Exploration

## Phase 1: Data Lake Preparation
- Verified the structure and permissions of the Data Lake Storage Gen2 account.
- Ensured the /users/data/ directory contains the required product.csv dataset.

![Data hub, select the Workspace tab](Data%20hub,%20select%20the%20Workspace%20tab.png)

## Phase 2: Serverless SQL Pool Setup
- Connected to the Built-in Serverless SQL Pool in Synapse Studio.
- Configured workspace firewall rules to allow access from my client IP.

![Ensure Built-in is selected in the Connect to dropdown list above the query window](Ensure%20Built-in%20is%20selected%20in%20the%20Connect%20to%20dropdown%20list%20above%20the%20query%20window.png)

## Phase 3: Querying Data with OPENROWSET
- Developed a T-SQL query using the OPENROWSET function to read product.csv directly from the Data Lake.
- Demonstrated "Schema-on-Read" by querying the CSV file without importing it into a database.

![Modify the SQL query to perform aggregates and grouping operations(2)](Modify%20the%20SQL%20query%20to%20perform%20aggregates%20and%20grouping%20operations(2).png)
![Run the modified script](Run%20the%20modified%20script.png)
![The cell output shows the query results from the Parquet file](The%20cell%20output%20shows%20the%20query%20results%20from%20the%20Parquet%20file.png)

## Phase 4: Data Validation & Troubleshooting
- Validated query results for accuracy and completeness.
- Resolved any access or connectivity issues by adjusting IAM roles and firewall settings.

![Task 5 Test permissions](Task%205%20Test%20permissions.png)
![Run the new cell you just added You should see a 403 error in the output](Run%20the%20new%20cell%20you%20just%20added%20You%20should%20see%20a%20403%20error%20in%20the%20output.png)
![verify that a new file has been added to this folder](verify%20that%20a%20new%20file%20has%20been%20added%20to%20this%20folder.png)

## Phase 5: Documentation & Best Practices
- Documented the process and SQL scripts used for querying external data sources.
- Followed best practices for security and cost management in serverless environments.

![create an external table named population](create%20an%20external%20table%20named%20population.png)
![we create an external file format called QuotedCsvWithHeader](we%20create%20an%20external%20file%20format%20called%20QuotedCsvWithHeader.png)
![query results, select the Chart view](query%20results,%20select%20the%20Chart%20view.png)



