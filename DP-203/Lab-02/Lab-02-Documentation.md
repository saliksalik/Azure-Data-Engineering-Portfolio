# Lab 02: Run Interactive Queries Using Serverless SQL Pools

## Module 2

In this lab, I worked with files stored in the data lake and external file sources using T-SQL statements executed by a serverless SQL pool in Azure Synapse Analytics. I also configured security using Azure Active Directory groups, RBAC, and ACLs.

---

## Exercise 1: Querying a Data Lake Store using serverless SQL pools

### Task 1: Query sales Parquet data with serverless SQL pools
- Opened Synapse Studio and navigated to the Data hub.
- Explored the ADLS Gen2 account and selected the Parquet file for querying.

![Task 1 Query sales Parquet data with serverless SQL pools](Task%201%20Query%20sales%20Parquet%20data%20with%20serverless%20SQL%20pools.png)

- Ran a query to select the top 100 rows and performed aggregate operations.

![Modify the SQL query to perform aggregates and grouping operations(2)](Modify%20the%20SQL%20query%20to%20perform%20aggregates%20and%20grouping%20operations(2).png)

- Verified the output.

![The cell output shows the query results from the Parquet file](The%20cell%20output%20shows%20the%20query%20results%20from%20the%20Parquet%20file.png)

### Task 2: Create an external table for 2019 sales data
- Created an external table for Parquet files in the data lake.

![Task 2 Create an external table for 2019 sales data](Task%202%20Create%20an%20external%20table%20for%202019%20sales%20data.png)

- Connected to the serverless SQL pool and ran the script.

![Make sure the script is connected to the serverless SQL pool (Built-in)](Make%20sure%20the%20script%20is%20connected%20to%20the%20serverless%20SQL%20pool%20(Built-in).png)

![Run the modified script](Run%20the%20modified%20script.png)

- Verified the output.

![The cell output shows the query results from the Parquet file.](The%20cell%20output%20shows%20the%20query%20results%20from%20the%20Parquet%20file..png)

### Task 3: Create an external table for CSV files
- Created a master key, credential, external data source, and external file format for CSV files.

![create an external table named population](create%20an%20external%20table%20named%20population.png)

![we create an external file format called QuotedCsvWithHeader](we%20create%20an%20external%20file%20format%20called%20QuotedCsvWithHeader.png)

- Created the external table and ran queries to filter population data.

![query results, select the Chart view](query%20results,%20select%20the%20Chart%20view.png)

### Task 4: Create a view with a serverless SQL pool
- Created a view to wrap a SQL query for easier access and integration with tools like Power BI.

![Task 4 Create a view with a serverless SQL pool](Task%204%20Create%20a%20view%20with%20a%20serverless%20SQL%20pool.png)

- Ran the script and verified the results.

![Run to execute the script](Run%20to%20execute%20the%20script.png)

---

## Exercise 2: Securing Access to Data

### Task 1: Create Azure Active Directory security groups
- Created security groups for history owners, readers, current writers, and 2019 writers.

![Ensure that the Security group type is selected](Ensure%20that%20the%20Security%20group%20type%20is%20selected.png)

![Home page, expand the portal menu](Home%20page,%20expand%20the%20portal%20menu.png)

![Select Groups in the left-hand menu.](Select%20Groups%20in%20the%20left-hand%20menu..png)

### Task 2: Add group members
- Added my account and other groups as members to the security groups.

![Select Members on the left then select Add members](Select%20Members%20on%20the%20left%20then%20select%20Add%20members.png)

![select your tailwind-current-writers group](select%20your%20tailwind-current-writers%20group.png)

![Select Overview in the left-hand menu, then copy the Object Id.](Select%20Overview%20in%20the%20left-hand%20menu,%20then%20copy%20the%20Object%20Id..png)

### Task 3: Configure data lake security - RBAC
- Assigned Storage Blob Data Reader and Owner roles to the security groups.

![Data hub, select the Workspace tab](Data%20hub,%20select%20the%20Workspace%20tab.png)

### Task 4: Configure data lake security - ACLs
- Set ACLs for the Year=2019 folder to control access and permissions.

![Task 4 Configure data lake security - Access Control Lists (ACLs)](Task%204%20Configure%20data%20lake%20security%20-%20Access%20Control%20Lists%20(ACLs).png)

### Task 5: Test permissions
- Tested read and write permissions using SQL and Spark notebooks.

![Task 5 Test permissions](Task%205%20Test%20permissions.png)

![select New Notebook, then select Load to DataFrame.](select%20New%20Notebook,%20then%20select%20Load%20to%20DataFrame..png)

![Attach your SparkPool01 Spark pool to the notebook.](Attach%20your%20SparkPool01%20Spark%20pool%20to%20the%20notebook..png)

![Run the new cell you just added You should see a 403 error in the output](Run%20the%20new%20cell%20you%20just%20added%20You%20should%20see%20a%20403%20error%20in%20the%20output.png)

![Click Run All](Click%20Run%20All.png)

- After updating group membership, successfully wrote to the data lake and verified the new file.

![verify that a new file has been added to this folder](verify%20that%20a%20new%20file%20has%20been%20added%20to%20this%20folder.png)

---

## Comments
This lab demonstrates my ability to work with serverless SQL pools, external tables, views, and secure data access using RBAC and ACLs in Azure Synapse Analytics. Screenshots are attached for each major step to showcase my hands-on work and troubleshooting skills.
