# Lab 07: Transform data with Azure Data Factory or Azure Synapse Pipelines 🚀

This lab teaches you how to build data integration pipelines to ingest from multiple data sources, transform data using mapping data flows and notebooks, and perform data movement into one or more data sinks.

After completing this lab, you will be able to:

- Execute code-free transformations at scale with Azure Synapse Pipelines
- Create data pipeline to import poorly formatted CSV files
- Create Mapping Data Flows

## Lab Setup and Pre-requisites 🛠️

Before starting this lab, you must complete Lab 5: Ingest and load data into the Data Warehouse.

This lab uses the dedicated SQL pool you created in the previous lab. You should have paused the SQL pool at the end of the previous lab, so resume it by following these instructions:

- Open Synapse Studio (https://web.azuresynapse.net/).
- Select the Manage hub.
- Select SQL pools in the left-hand menu. If the SQLPool01 dedicated SQL pool is paused, hover over its name and select ▷.
- When prompted, select Resume. It will take a minute or two to resume the pool.

Continue to the next exercise while the dedicated SQL pool resumes.

**Important:** Once started, a dedicated SQL pool consumes credits in your Azure subscription until it is paused. If you take a break from this lab, or decide not to complete it; follow the instructions at the end of the lab to pause your SQL pool!

## Exercise 1: Code-free transformation at scale with Azure Synapse Pipelines 🔄

Tailwind Traders would like code-free options for data engineering tasks. Their motivation is driven by the desire to allow junior-level data engineers who understand the data but do not have a lot of development experience build and maintain data transformation operations. The other driver for this requirement is to reduce fragility caused by complex code with reliance on libraries pinned to specific versions, remove code testing requirements, and improve ease of long-term maintenance.

Their other requirement is to maintain transformed data in a data lake in addition to the dedicated SQL pool. This gives them the flexibility to retain more fields in their data sets than they otherwise store in fact and dimension tables, and doing this allows them to access the data when they have paused the dedicated SQL pool, as a cost optimization.

Given these requirements, you recommend building Mapping Data Flows.

Mapping Data flows are pipeline activities that provide a visual way of specifying how to transform data, through a code-free experience. This feature offers data cleansing, transformation, aggregation, conversion, joins, data copy operations, etc.

**Additional benefits:**

- Cloud scale via Spark execution
- Guided experience to easily build resilient data flows
- Flexibility to transform data per user's comfort
- Monitor and manage data flows from a single pane of glass

### Task 1: Create SQL table 📊

The Mapping Data Flow we will build will write user purchase data to a dedicated SQL pool. Tailwind Traders does not yet have a table to store this data. We will execute a SQL script to create this table as a pre-requisite.

#### How I Did It

I navigated to the Develop hub in Synapse Analytics Studio. In the + menu, I selected SQL script. In the toolbar menu, I connected to the SQLPool01 database. In the query window, I replaced the script with the code to create the UserTopProductPurchases table, then ran it. I repeated the process to create the CampaignAnalytics table.

### Task 2: Create linked service 🔗

Azure Cosmos DB is one of the data sources that will be used in the Mapping Data Flow. Tailwind Traders has not yet created the linked service.

#### How I Did It

I navigated to the Manage hub. I opened Linked services and selected + New. I selected Azure Cosmos DB (SQL API), named it asacosmosdb01, selected the Cosmos DB account and CustomerProfile database, tested the connection, and created it.

### Task 3: Create data sets 📁

User profile data comes from two different data sources. I created datasets for the SQL tables and sources.

#### How I Did It

I went to the Data hub. For each dataset, I selected + Integration dataset, chose the appropriate type (Cosmos DB, ADLS Gen2, Azure Synapse Analytics), configured the settings as specified, and created them. I previewed the Cosmos DB data to verify.

### Task 4: Create campaign analytics dataset 📈

I created a dataset for the poorly formatted CSV file.

#### How I Did It

In the Data hub, I selected + Integration dataset, chose Azure Data Lake Storage Gen2, selected DelimitedText format, configured the path to the CSV file, left first row as header unchecked, imported schema, reviewed settings, previewed data, and published all.

### Task 5: Create campaign analytics data flow 🌊

I created a Mapping Data Flow to transform the campaign analytics data.

#### How I Did It

I navigated to the Develop hub, selected + Data flow, named it asal400_lab2_writecampaignanalyticstoasa. I added a source with the campaign analytics dataset, configured options including skip line count 1. I used the script to set the schema. Then I added Select, Derived Column, another Select, and Sink transformations as specified, configuring each step with the required mappings and expressions. Finally, I published the data flow.

### Task 6: Create campaign analytics data pipeline 🔧

I created a pipeline to run the data flow.

#### How I Did It

I went to the Integrate hub, selected + Pipeline, named it "Write Campaign Analytics to ASA". I dragged a Data flow activity, named it, selected the data flow, and published the pipeline.

### Task 7: Run the campaign analytics data pipeline ▶️

I triggered and monitored the pipeline run.

#### How I Did It

I selected Add trigger > Trigger now, confirmed, then monitored in the Monitor hub until it completed successfully.

### Task 8: View campaign analytics table contents 👀

I verified the data in the SQL table.

#### How I Did It

In the Data hub, I expanded the database, right-clicked the table, selected New SQL script > Select TOP 100 rows. I ran additional queries and viewed the chart.

## Exercise 2: Create Mapping Data Flow for top product purchases 🛒

Tailwind Traders needs to combine top product purchases with user preferred products. I built a mapping data flow for this.

### Task 1: Create Mapping Data Flow 🌊

I created the data flow for user profiles.

#### How I Did It

In the Develop hub, I selected + Data flow, named it write_user_profile_to_asa. I added sources for e-commerce user profiles and Cosmos DB, configured them. I added Derived Column, Flatten, Join, another Derived Column, Filter, and two Sinks (one for SQL pool, one for data lake), configuring each with the specified settings, mappings, and optimizations. I published the data flow.

## Exercise 3: Orchestrate data movement and transformation in Azure Synapse Pipelines 🎼

Tailwind Traders wants to orchestrate data activities.

### Task 1: Create pipeline 🔧

I created a pipeline for the user profile data flow.

#### How I Did It

In the Integrate hub, I selected + Pipeline, named it "Write User Profile Data to ASA". I added a Data flow activity, configured it with the data flow, staging options, and published.

### Task 2: Trigger, monitor, and analyze the user profile data pipeline 📊

I ran and analyzed the pipeline.

#### How I Did It

I triggered the pipeline now, monitored it in the Monitor hub, viewed activity runs and data flow details, analyzed performance metrics.

**Important: Pause your SQL pool** ⏸️

I paused the SQL pool to free up resources: In Manage hub, selected SQL pools, hovered over SQLPool01, selected pause, and confirmed.

---

*Completed this lab as part of the Azure Data Engineering Portfolio. All steps followed the provided instructions, and the pipelines ran successfully.*
