# Lab 08: Integrate data from notebooks with Azure Data Factory or Azure Synapse Pipelines

You will learn how to create linked services, and orchestrate data movement and transformation in Azure Synapse Pipelines.

After completing this lab, you will be able to:

- Orchestrate data movement and transformation in Azure Synapse Pipelines

## Lab Setup and Pre-requisites

Before starting this lab, you should complete Lab 6: Transform data with Azure Data Factory or Azure Synapse Pipelines.

Note: If you have not completed lab 6, but you have completed the lab setup for this course, you can complete these steps to create the required linked services and datasets.

In Synapse Studio, on the Manage hub, add a new Linked service for Azure Cosmos DB (SQL API) with the following settings:
- Name: asacosmosdb01
- Cosmos DB account name: asacosmosdbxxxxxxx
- Database name: CustomerProfile

On the Data hub, create the following Integration datasets:
- asal400_customerprofile_cosmosdb:
  - Source: Azure Cosmos DB (SQL API)
  - Name: asal400_customerprofile_cosmosdb
  - Linked service: asacosmosdb01
  - Collection: OnlineUserProfile01
- asal400_ecommerce_userprofiles_source
  - Source: Azure Data Lake Storage Gen2
  - Format: JSON
  - Name: asal400_ecommerce_userprofiles_source
  - Linked service: asadatalakexxxxxxx
  - File path: wwi-02/online-user-profiles-02
  - Import schema: From connection/store

Setup warnings that you can safely ignore:
The following error may also occur and can safely be ignored: > 07-create-wwi-perf-sale-heap with label CTAS : Sale_Heap. Cannot index into a null array.

## Exercise 1: Create mapping data flow and pipeline

In this exercise, you create a mapping data flow that copies user profile data to the data lake, then create a pipeline that orchestrates executing the data flow, and later on, the Spark notebook you create later in this lab.

### Task 1: Create mapping data flow

Open Synapse Studio (https://web.azuresynapse.net/).

Navigate to the Develop hub.

In the + menu, select Data flow to create a new data flow.

In the General settings of the Properties blade of the new data flow, update the Name to user_profiles_to_datalake. Make sure the name exactly matches exactly.

Select the {} Code button at the top-right above the data flow properties.

Replace the existing code with the following, changing SUFFIX in the asadatalakeSUFFIX sink reference name on line 25 to the unique suffix for your Azure resources in this lab:

[JSON code provided]

Select OK.

![Completed Data Flow](user-profiles-data-flow.png)

### Task 2: Create pipeline

In this step, you create a new integration pipeline to execute the data flow.

On the Integrate hub, in the + menu, select Pipeline.

In the General section of the Properties pane of the new data flow, update the Name to User Profiles to Datalake. Then select the Properties button to hide the pane.

Expand Move & transform within the Activities list, then drag the Data flow activity onto the pipeline canvas.

Under the General tab beneath the pipeline canvas, set the Name to user_profiles_to_datalake.

![Pipeline Data Flow General](pipeline-data-flow-general-datalake.png)

On the Settings tab, select the user_profiles_to_datalake data flow, ensure AutoResolveIntegrationRuntime is selected. Choose the Basic (General purpose) compute type and set the core count to 4 (+ 4 Driver cores).

![Pipeline Data Flow Settings](pipeline-user-profiles-datalake-data-flow-settings.png)

Select Publish all then Publish to save your pipeline.

### Task 3: Trigger the pipeline

At the top of the pipeline, select Add trigger, then Trigger now.

There are no parameters for this pipeline, so select OK to run the trigger.

Navigate to the Monitor hub.

Select Pipeline runs and wait for the pipeline run to successfully complete (which will take some time). You may need to refresh the view.

## Exercise 2: Create Synapse Spark notebook to find top products

Tailwind Traders uses a Mapping Data flow in Synapse Analytics to process, join, and import user profile data. Now they want to find the top 5 products for each user, based on which ones are both preferred and top, and have the most purchases in the past 12 months. Then, they want to calculate the top 5 products overall.

In this exercise, you will create a Synapse Spark notebook to make these calculations.

### Task 1: Create notebook

Select the Data hub.

On the Linked tab, expand Azure Data Lake Storage Gen2 and the primary data lake storage account, and select the wwi-02 container. Then navigate to the top-products folder in the root of this container (If you don't see the folder, select Refresh). Finally, right-click any Parquet file, select the New notebook menu item, then select Load to DataFrame.

Select the Properties button at the top-right corner of the notebook, and enter Calculate Top 5 Products for the Name. Then click the Properties button again to hide the pane.

Attach the notebook is attached to your SparkPool01 Spark pool.

![Attach to Spark Pool](notebook-top-products-attach-pool.png)

In the Python code, replace the Parquet file name with *.parquet to select all Parquet files in the top-products folder. For example, the path should be similar to: abfss://wwi-02@asadatalakexxxxxxx.dfs.core.windows.net/top-products/*.parquet.

![File Path Highlighted](notebook-top-products-filepath.png)

Select Run all on the notebook toolbar to run the notebook.

![Cell Results Displayed](notebook-top-products-cell1results.png)

Note: The first time you run a notebook in a Spark pool, Synapse creates a new session. This can take approximately 2-3 minutes.

Create a new code cell underneath by selecting the + Code button.

Enter and execute the following in the new cell to populate a new dataframe called topPurchases, create a new temporary view named top_purchases, and show the first 100 rows:

[topPurchases code]

The output should look similar to the following:

[table]

Run the following in a new code cell to create a new DataFrame to hold only top preferred products where both IsTopProduct and IsPreferredProduct are true:

[from pyspark.sql.functions import * code]

![Top Preferred DataFrame](notebook-top-products-top-preferred-df.png)

Run the following in a new code cell to create a new temporary view by using SQL:

[%%sql code]

Run the following in a new code cell to create and display a new DataFrame that stores the results of the top_5_products temporary view you created in the previous cell:

[top5Products code]

![Top Five Preferred Products](notebook-top-products-top-5-preferred-output.png)

Run the following in a new code cell to compare the number of top preferred products to the top five preferred products per customer:

[print code]

Run the following in a new code cell to calculate the top five products overall, based on those that are both preferred by customers and purchased the most

[top5ProductsOverall code]

We are going to execute this notebook from a pipeline. We want to pass in a parameter that sets a runId variable value that will be used to name the Parquet file. Run the following in a new code cell:

[import uuid code]

We are using the uuid library that comes with Spark to generate a random GUID. We want to override the runId variable with a parameter passed in by the pipeline. To do this, we need to toggle this as a parameter cell.

Select the actions ellipses (...) in the mini toolbar above the cell, then select Toggle parameter cell.

![Toggle Parameter Cell](toggle-parameter-cell.png)

Add the following code to a new code cell to use the runId variable as the Parquet filename in the /top5-products/ path in the primary data lake account. Replace SUFFIX in the path with the unique suffix of your primary data lake account - you'll find this in Cell 1 at the top of the page. When you've updated the code, run the cell.

[%%pyspark code]

Verify that the file was written to the data lake. In the Data hub, select the Linked tab. Expand the primary data lake storage account and select the wwi-02 container. Navigate to the top5-products folder (refresh the folders in the root of the container of necessary). You should see a folder for the Parquet file in the directory with a GUID as the file name.

Return to the notebook. Select Stop session on the upper-right of the notebook, and confirm you want to stop the session now when prompted. We want to stop the session to free up the compute resources for when we run the notebook inside the pipeline in the next section.

### Task 2: Add the Notebook to the pipeline

Tailwind Traders wants to execute this notebook after the Mapping Data Flow runs as part of their orchestration process. To do this, we will add this notebook to our pipeline as a new Notebook activity.

Return to the Calculate Top 5 Products notebook.

Select the Add to pipeline button at the top-right corner of the notebook, then select Existing pipeline.

Select the User Profiles to Datalake pipeline, then select Add.

Synapse Studio adds the Notebook activity to the pipeline. Rearrange the Notebook activity so it sits to the right of the Data flow activity. Select the Data flow activity and drag a Success activity pipeline connection green box to the Notebook activity.

The Success activity arrow instructs the pipeline to execute the Notebook activity after the Data flow activity successfully runs.

Select the Notebook activity, select the Settings tab, expand Base parameters, and select + New. Enter runId in the Name field. Set the the Type to String and the Value to Add dynamic content.

In the Add dynamic content pane, expand System variables, and select Pipeline run ID. This adds @pipeline().RunId to the dynamic content box. Then click OK to close the dialog.

![Pipeline Run ID](pipeline-run-id.png)

Select Publish all then Publish to save your changes.

### Task 3: Run the updated pipeline

Note: The updated pipeline can take 10 minutes or more to run!

After publishing is complete, select Add trigger, then Trigger now to run the updated pipeline.

Select OK to run the trigger.

Navigate to the Monitor hub.

Select Pipeline runs and wait for the pipeline run to successfully complete. You may need to refresh the view.

It can take over 10 minutes for the run to complete with the addition of the notebook activity.

Select the name of the pipeline (User profiles to Datalake) to view the pipeline's activity runs.

This time, we see both the Data flow activity, and the new Notebook activity. Make note of the Pipeline run ID value. We will compare this to the Parquet file name generated by the notebook. Select the Calculate Top 5 Products notebook name to view its details.

Here we see the notebook run details. You can select the Playback button to watch a playback of the progress through the jobs. At the bottom, you can view the Diagnostics and Logs with different filter options. Hover over a stage to view its details, such as the duration, total tasks, data details, etc. Select the View details link on the stage to view its details.

![Spark Stage Details](spark-stage-details.png)

The Spark application UI opens in a new tab where we can see the stage details. Expand the DAG Visualization to view the stage details.

Close the Spark details tab, and in Synapse Studio, navigate back to the Data hub.

Select the Linked tab, select the wwi-02 container on the primary data lake storage account, navigate to the top5-products folder, and verify that a folder exists for the Parquet file whose name matches the Pipeline run ID.

As you can see, we have a file whose name matches the Pipeline run ID we noted earlier.

These values match because we passed in the Pipeline run ID to the runId parameter on the Notebook activity.

---

*Completed this lab as part of the Azure Data Engineering Portfolio. All steps followed the provided instructions, and the pipelines ran successfully.*
