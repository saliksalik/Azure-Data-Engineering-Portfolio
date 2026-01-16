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

## Lab Details

**Lab Title:** Lab environment setup with a pre-installed virtual machine  
**Module:** Module 0

### Module 0 - Lab environment setup with a pre-installed virtual machine
The following instructions enable learners to prepare their lab environments for the modules that follow. Please run through these instructions prior to starting Module 1.

**Time to complete:** ~5 minutes to perform the steps below and initiate the automated setup scripts. The scripts may take an hour or more to complete.

**Note:** These instructions are designed to be used in the pre-installed virtual machine provided for the course.

### Requirements
Before starting setup, you will need an Azure Account with the ability to create an Azure Synapse Workspace.

**Important note if using an Azure Pass subscription**

If you are using an account for which you have previously redeemed an Azure Pass subscription that has expired, your account may be associated with multiple Azure subscriptions with the same name (Azure Pass - Sponsorship). Before starting the setup steps, ensure that only the most recent active subscription of this name is enabled by following these steps:

1. Open the Azure portal at https://portal.azure.com and sign in using the account associated with your subscription.
2. On the portal toolbar at the top of the page, select the Directories and Subscriptions button.
3. In the Default subscriptions filter drop-down list, de-select any (Disabled) Azure Pass - Sponsorship subscriptions, and ensure that only the active Azure Pass - Sponsorship subscription that you want to use is selected.

### Setup Steps
Perform the following tasks to prepare your environment for the labs.

1. Use the Windows Search box to search for Windows PowerShell, and then run it as an administrator.
	- **Note:** Make sure you run Windows Powershell, not Windows PowerShell ISE; and be sure to run it as Administrator.
2. In Windows PowerShell, run the following commands to download the required course files. This may take a few minutes:
	```powershell
	mkdir c:\dp-203
	cd c:\dp-203
	git clone https://github.com/microsoftlearning/dp-203-data-engineer.git data-engineering-ilt-deployment
	```
3. In Windows PowerShell, run the following command to set the execution policy so you can run a local PowerShell script file:
	```powershell
	Set-ExecutionPolicy Unrestricted
	```
	- If you receive a prompt that you are installing the module from an untrusted repository, enter `A` to select the Yes to All option.
4. Change directories to the folder containing the automation scripts:
	```powershell
	cd C:\dp-203\data-engineering-ilt-deployment\Allfiles\00\artifacts\environment-setup\automation\
	```
5. Run the setup script:
	```powershell
	.\dp-203-setup-Part01.ps1
	```
	- **Warning:** The original script creates resources in regions like "australiaeast", "northeurope", etc. If you encounter issues, change the region list in the file `dp-203-setup-Part01.ps1` at line 110 to a safe region (e.g., "eastasia", "northcentralus") before executing the script.

	- This script creates and prepares most of the Azure accounts needed (except for Cosmos DB). It takes around 10-15 minutes to complete.

	- When prompted to sign into Azure, use your credentials. After signing in, you can close the browser and return to PowerShell.

	- If you have more than one Azure subscription, select the one you want to use in the labs by entering its number in the list of subscriptions.

	- When prompted, enter a suitably complex password for the SQL Database (make a note of this password).

	- If the script appears to "stall" (no new information for 10 minutes), press ENTER and check for error messages. If errors occur (e.g., resource name in use, capacity constraints), delete the `data-engineering-synapse-xxxxxx` resource group in the Azure portal and re-run the script.

	- If you see an error about supplying the tenant ID for your Azure pass subscription, ensure you have enabled only the Azure Pass subscription you want to use (see Requirements above).

You will have 2 additional script files to run in later labs.
