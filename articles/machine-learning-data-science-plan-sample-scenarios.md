<properties title="Cloud Data Science Process Walkthroughs" pageTitle="Cloud Data Science Process Walkthroughs | Azure" description="Cloud Data Science Process Walkthroughs" metaKeywords="" services="data-science-process" solutions="" documentationCenter="" authors="msolhab" manager="paulettm" editor="" videoId="" scriptId="" />

<tags ms.service="data-science-process" ms.workload="data-services" ms.tgt_pltfrm="na" ms.devlang="na" ms.topic="article" ms.date="02/19/2015" ms.author="msolhab" /> 

Cloud Data Science Process in Azure Machine Learning
========================================

This article walks through the Cloud Data Science Process map and some sample data source and target scenarios. It illustrates some of the option choices and processing sequences based on data characteristics, source location, and target repositories in Azure. The Cloud Data Science Process flow for the sample scenarios is summarized in the last section.

#### For all scenarios, you need:

   [Create a storage account](http://azure.microsoft.com/en-us/documentation/articles/storage-create-storage-account/)

  [Create an Azure ML workspace](http://azure.microsoft.com/en-us/documentation/articles/machine-learning-create-workspace/)

**The following sections present a few sample scenarios. For each scenario, a possible Data Science flow and supporting Azure resources are listed.**

- [Scenario \#1: Small to medium tabular dataset in a local files](#smalllocal)
- [Scenario \#2: Small to medium dataset in a local files, requiring processing](#smalllocalprocess)
- [Scenario \#3: Large dataset in a local files, target Azure blobs](#largelocal)
- [Scenario \#4: Small to medium dataset in a local files, target SQL Server in Azure VM](#smalllocaltodb)
- [Scenario \#5: Large dataset in a local files, target SQL Server in Azure VM](#largelocaltodb)
- [Scenario \#6: Large dataset in a SQL Server database on-prem, target SQL Server in Azure VM](#largedbtodb)

## <a name="smalllocal"></a>Scenario \#1: Small to medium tabular dataset in a local files

![Small to medium local files][1]

#### Additional Azure resources: None

1.  Sign in to the [Azure Machine Learning Studio](https://studio.azureml.net/).

2.  Upload a dataset.

3.  Build an Azure Machine Learning experiment flow starting with uploaded dataset(s).

## <a name="smalllocalprocess"></a>Scenario \#2: Small to medium dataset of local files that require processing

![Small to medium local files with processing][2]

#### Additional Azure resources: Azure Virtual Machine (IPython Notebook server)

1.  Create an Azure Virtual Machine running IPython Notebook.

2.  Upload data to an Azure storage container.

3.  Pre-process and clean data in IPython Notebook, accessing data from Azure
    storage container.

4.  Transform data to cleaned, tabular form.

5.  Save transformed data in Azure blobs.

6.  Sign in to the [Azure Machine Learning Studio](https://studio.azureml.net/).

7.  Read the data from Azure blobs using the Reader module.

8. Build an Azure Machine Learning experiment flow starting with ingested dataset(s).

## <a name="largelocal"></a>Scenario \#3: Large dataset of local files, targeting Azure Blobs

![Large local files][3]

#### Additional Azure resources: Azure Virtual Machine (IPython Notebook server)

1.  Create an Azure Virtual Machine running IPython Notebook.

2.  Upload data to an Azure storage container.

3.  Pre-process and clean data in IPython Notebook, accessing data from Azure blobs.

4.  Transform data to cleaned, tabular form, if needed.

5.  Explore data, and create features as needed.

6.  Extract a small-to-medium data sample.

7.  Save the sampled data in Azure blobs.

8. Sign in to the [Azure Machine Learning Studio](https://studio.azureml.net/).

9. Read the data from Azure blobs using the Reader module.

10. Build Azure ML experiment flow starting with ingested dataset(s).


## <a name="smalllocaltodb"></a>Scenario \#4: Small to medium dataset of local files, targeting SQL Server in an Azure Virtal Machine

![Small to medium local files to SQL DB in Azure][4]

#### Additional Azure resources: Azure Virtual Machine (SQL Server / IPython Notebook server)

1.  Create an Azure Virtual Machine running SQL Server + IPython Notebook.

2.  Upload data to an Azure storage container.

3.  Pre-process and clean data in Azure storage container using IPython Notebook.

4.  Transform data to cleaned, tabular form, if needed.

5.  Save data to VM-local files (IPython Notebook is running on VM, local drives refer to VM drives).

6.  Load data to SQL Server database running on an Azure VM.

    a.  Option \#1: Using SQL Server Management Studio.

		i.  Login to SQL Server VM
        ii. Run SQL Server Management Studio.
        iii. Create database and target tables.
        iv. Use one of the bulk import methods to load the data from VM-local files.

    b.  Option \#2: Using IPython Notebook – not advisable for medium and larger
        datasets

        i.  Use ODBC connection string to access SQL Server on VM.
        ii. Create database and target tables.
        iii. Use one of the bulk import methods to load the data from VM-local files.

7.  Explore data, create features as needed. Note that the features do not need to be materialized in the database tables. Only note the necessary query to create them.

8. Decide on a data sample size, if needed and/or desired.

9. Sign in to the [Azure Machine Learning Studio](https://studio.azureml.net/).

10. Read the data directly from the SQL Server using the Reader module. Paste the necessary query which extracts fields, creates features, and samples data if needed directly in the Reader query.

11. Build Azure ML experiment flow starting with ingested dataset(s).

## <a name="largelocaltodb"></a>Scenario \#5: Large dataset in a local files, target SQL Server in Azure VM

![Large local files to SQL DB in Azure][5]

#### Additional Azure resources: Azure Virtual Machine (SQL Server / IPython Notebook server)

1.  Create an Azure Virtual Machine running SQL Server and IPython Notebook server.

2.  Upload data to an Azure storage container.

3.  (Optional) Pre-process and clean data.

    a.  Pre-process and clean data in IPython Notebook, accessing data from Azure
        blobs.

    b.  Transform data to cleaned, tabular form, if needed.

    c.  Save data to VM-local files (IPython Notebook is running on VM, local drives refer to VM drives).

4.  Load data to SQL Server database running on an Azure VM.

    a.  Login to SQL Server VM.

    b.  If data not saved already, download data files from Azure
        storage container to local-VM folder.

    c.  Run SQL Server Management Studio.

    d.  Create database and target tables.

	e.  Use one of the bulk import methods to load the data.

    f.  If table joins are required, create indexes to expedite joins.

 > [AZURE.NOTE] For faster loading of large data sizes, it is recommended to create partitioned tables and to bulk import the data in parallel. For more information, see [Parallel Data Import to SQL Partitioned Tables](http://azure.microsoft.com/en-us/documentation/articles/machine-learning-data-science-parallel-load-sql-partitioned-tables/).

5.  Explore data, create features as needed. Note that the features do not need to be materialized in the database tables. Only note the necessary query to create them.

6.  Decide on a data sample size, if needed and/or desired.

7.  Sign in to the [Azure Machine Learning Studio](https://studio.azureml.net/).

8. Read the data directly from the SQL Server using the Reader module. Paste the necessary query which extracts fields, creates features, and samples data if needed directly in the Reader query.

9. Simple Azure ML experiment flow starting with uploaded dataset

## <a name="largedbtodb"></a>Scenario \#6: Large dataset in a SQL Server database on-prem, targeting SQL Server in an Azure Virtual Machine

![Large SQL DB on-prem to SQL DB in Azure][6]

#### Additional Azure resources: Azure Virtual Machine (SQL Server / IPython Notebook server)

1.  Create an Azure Virtual Machine running SQL Server and IPython Notebook server.

2.  Use one of the data export methods to export the data from SQL Server to dump files.

    a.  Note: If you decide to move all data from the on-prem database,
        an alternate (faster) method to move the full database to the
        SQL Server instance in Azure. Skip the steps to export data,
        create database, and load/import data to the target database and
        follow the alternate method.

3.  Upload dump files to Azure storage container.

4.  Load the data to a SQL Server database running on an Azure Virtual Machine.

    a.  Login to the SQL Server VM.

    b.  Download data files from an Azure storage container to the local-VM folder.

    c.  Run SQL Server Management Studio.

    d.  Create database and target tables.

	e.  Use one of the bulk import methods to load the data.

	f.  If table joins are required, create indexes to expedite joins.

> [AZURE.NOTE] For faster loading of large data sizes, create partitioned tables and to bulk import the data in parallel. For more information, see [Parallel Data Import to SQL Partitioned Tables](http://azure.microsoft.com/en-us/documentation/articles/machine-learning-data-science-parallel-load-sql-partitioned-tables/).

5.  Explore data, create features as needed. Note that the features do not need to be materialized in the database tables. Only note the necessary query to create them.

6.  Decide on a data sample size, if needed and/or desired.

7.  Sign in to the [Azure Machine Learning Studio](https://studio.azureml.net/).

8. Read the data directly from the SQL Server using the Reader module. Paste the necessary query which extracts fields, creates features, and samples data if needed directly in the Reader query.

9. Simple Azure ML experiment flow starting with uploaded dataset.

### Alternate method to copy a full database from an on-premises  SQL Server to Azure SQL Database

![Detach local DB and attach to SQL DB in Azure][7]

#### Additional Azure resources: Azure Virtual Machine (SQL Server / IPython Notebook server)

To replicate the entire SQL Server database in your SQL Server VM, you should copy a database from one location/server to another, assuming that the database can be taken temporarily offline. You do this in the SQL Server Management Studio Object Explorer GUI, or using the equivalent Transact-SQL commands.

1. Detach the database at the source location. For more information, see [Detach a database](https://technet.microsoft.com/en-us/library/ms191491.aspx).
2. In Windows Explorer or Windows Command Prompt window, copy the detached database file or files and log file or files to the target location on the SQL Server VM in Azure.
3. Attach the copied files to the target SQL Server instance. For more information, see [Attach a Database](https://technet.microsoft.com/en-us/library/ms190209.aspx). 

[Move a Database Using Detach and Attach (Transact-SQL)](https://technet.microsoft.com/en-us/library/ms187858.aspx)

Sample Scenarios Summary
------------------------

The following diagram summarizes the scenarios described above and the Cloud Data Science Process choices made in each scenario. Note that data processing, exploration, feature engineering, and sampling may take place in one or
more method/environment -- at the source, intermediate, and/or target environment – and may proceed iteratively as needed. The diagram serves as an illustration of some of possible flows only.

![Sample DS process walkthrough scenarios][8]

### Azure Data Science in Action Example

For an end-to-end walkthrough example of the Azure Data Science Process using a public dataset, see [Azure Data Science Process in Action](http://azure.microsoft.com/en-us/documentation/articles/machine-learning-data-science-process-sql-walkthrough/).

[1]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-small-in-aml.png
[2]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-local-with-processing.png
[3]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-local-large.png
[4]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-local-to-db.png
[5]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-large-to-db.png
[6]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-db-to-db.png
[7]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-attach-db.png
[8]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-sample-scenarios.png
