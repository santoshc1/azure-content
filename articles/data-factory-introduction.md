<properties 
	pageTitle="Introduction to Azure Data Factory" 
	description="Learn how you can use the Azure Data Factory service to compose data processing, data storage and data movement services to create pipelines that produce trusted information." 
	services="data-factory" 
	documentationCenter="" 
	authors="spelluru" 
	manager="jhubbard" 
	editor="monicar"/>

<tags 
	ms.service="data-factory" 
	ms.workload="data-services" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="02/20/2015" 
	ms.author="spelluru"/>

# Introduction to Azure Data Factory Service
The **Azure Data Factory** service is a fully managed service for composing data storage, processing, and movement services into streamlined, scalable, and reliable data production pipelines.  Developers can use Data Factory to transform semi-structured, unstructured and structured data from on-premises and cloud sources into trusted information. Developers build data-driven workflows (pipelines) that join, aggregate and transform data sourced from their on-premises, cloud-based and internet services, and set up complex data processing through simple JSON scripting. The Azure Data Factory service provides monitoring and management of these pipelines at a glance with a rich visual experience offered through the Azure Preview Portal. The information produced by pipelines can be easily consumed using BI and analytics tools, and other applications to reliably drive key business insights and decisions.

This article provides an overview of the Azure Data Factory service, the value it provides, and the scenarios it supports. 

##Azure Data Factory Overview
Traditionally, data integration projects have revolved around creating Extract-Transform-Load (ETL) processes that extract data from various data sources within an organization, transform the data to conform to the target schema of an Enterprise Data Warehouse (EDW), and load the data into an EDW as shown in the following image. The EDW is then accessed as the single source of truth for BI analytics solutions. 

![Traditional ETL][image-data-factory-introduction-traditional-ETL]

Today’s data landscape for enterprises continues to grow exponentially in volume, variety, and complexity as shown in the following image.  It is more diverse than ever with on-premises and cloud-born data of different forms and velocities.  Data processing must happen across geographic locations, and includes a combination of open source software, commercial solutions and custom processing services and that are expensive, and hard to integrate and maintain.  The agility needed to adapt to today’s changing Big Data landscape is an opportunity to merge the traditional EDW with capabilities required for a modern information production system.  The Azure Data Factory service is the composition platform to work across traditional EDWs and the changing data landscape to empower enterprises to leverage all data that is available to them for data-driven decision making.

![Todays Diverse Processing Landscape][image-data-factory-introduction-todays-diverse-processing-landspace]


The **Azure Data Factory** service empowers enterprises to harness this diversity by providing a platform to **compose data processing, storage and movement services into information production pipelines, and manage trusted data assets**.

The Azure Data Factory service lets you:

- **Easily work with diverse data storage and processing systems.** 
Data Factory enables you to use data where it is stored and compose services for storage, processing, and data movement.  For example, you can compose information production pipelines to process on-premises data like SQL Server, together with cloud data like Azure SQL Database, Blobs, and Tables.  
- **Transform data into trusted information.** 
Combining and shaping complex data can take more than one try to get right, and changing data models can be costly and time consuming.  Using Data Factory you can focus on transformative analytics while the service takes care of the plumbing.  Data factory supports Hive, Pig and C# processing, along with key processing features such as automatic Hadoop (HDInsight) cluster management, re-tries for transient failures, configurable timeout policies, and alerting.  
- **Monitor data pipelines in one place.** 
With a diverse data portfolio, it’s important to have a reliable and complete view of your storage, processing, and data movement services.  Data Factory helps you quickly assess end-to-end data pipeline health, pinpoint issues, and take corrective action if needed. Visually track data lineage and the relationships between your data across any of your sources. See a full historical accounting of job execution, system health, and dependencies from a single monitoring dashboard
- **Get rich insights from transformed data**
Adapt to the constantly changing questions that your organization needs to answer, and stay on top of when your data production is ready to go.  Improve your ability to drive better business insights by producing timely and trusted information for consumption. Use data pipelines to deliver transformed data from the cloud to on-premises sources like SQL Server, or keep it in your cloud storage sources for consumption by BI and analytics tools and other applications.

Today, to take advantage of the benefits of Data Factory, developers interact directly with individual data pipelines, storage services, and compute services.  As the Data Factory service evolves over time, we will introduce additional storage and processing services, and new mechanisms of grouping compute and storage services and data pipelines together into ‘Hubs’.  We describe Hubs here in our introduction, as this nascent concept appears throughout the service as a precursor for future releases.

An Azure Data Factory Hub is a container for storage and compute services (both referred to as Linked services), as well as for the data pipelines that use and run on those resources. The Hub container allows the Data Factory to be divided into logical or domain specific groupings.  For example, an enterprise may have a “West US Azure Hub” which manages all of the Linked services and pipelines focused in the West US data center, or a “Sales EDW Hub” which manages all the Linked services and pipelines associated with populating and processing data for the Sales EDW.  An important characteristic of Hubs is that a pipeline runs on a single hub. This means that when defining a pipeline, all of the Linked services referenced by tables or activities within that pipeline must have the same Hub name as the pipeline itself.

Hubs will help to encapsulate storage and compute in a way where pipelines can reference only a Hub rather than the specific services and tables it uses. The Hub can then use policies to decide where to run a pipeline. This will have several important impacts. One is that it will provide easier scale-up as more Linked services can be added to a Hub, and pipelines can be load-balanced across these new Linked services. Another is that it will reuse of pipeline definitions on different Hubs.

## Application Model
The following diagram illustrates the application model supported by Azure Data Factory.

![Application Model][image-data-factory-application-model]

There are three information production stages in an Azure Data Factory:


- **Connect & Collect**. In this stage, data from various data sources is imported into data hubs.
- **Transform & Enrich**. In this stage, the collected data is processed.
- **Publish**. In this stage, the data is published so that it can be consumed by BI tools, analytics tools, and other applications.

Data Factories enable developers to create **pipelines** which are groups of data movement and/or processing **activities** that accept one or more input **datasets** and produce one or more output datasets.  Pipelines can be executed once or on a flexible range of schedules (hourly, daily, weekly, etc…).  A **dataset** is a named view of data. The data being described can vary from simple bytes, semi-structured data like CSV files all the way to **tables** or **models**.

**Pipelines** comprised of data movement **activities** (for example: Copy Activity) are often used to import/export data from all the **data sources** (databases, files, SaaS services, etc…) used by the organization into a **data hub**.
    
Once data is in a **hub**, **pipelines** hosted by the compute services of the hub, are used to transform data into a form suitable for consumption (by BI tools, applications, customers, etc.).  
  
Finally, **pipelines** can be chained (as shown in the diagram) such that the output **dataset(s)** of one are the input(s) of another.  This allows complex data flows to be factored into **pipelines** that run within a data hub or span multiple hubs.  Using **pipelines** in this way provides organizations the building blocks to compose the best of breed on-premises, cloud and Software-as-a-Service (SaaS) services all through the lens of a single, easily managed data factory. 


##Terminology
This section introduces you to the terminology related to Azure Data Factory.

### Data Factory

**Azure Data Factory** is a fully managed service for composing data storage, processing, and movement services into streamlined, scalable, and reliable data production pipelines. 

The Data Factory service consumes, produces, manages and publishes **datasets**. An Azure subscription may have one or more Azure Data Factory instances. An Azure data factory can be linked to one or more storage or compute services (called Linked Services).
 
An Azure data factory may have one or more pipelines that process data in linked storages by using linked compute services such as Azure HDInsight. An Azure data factory does not contain the data within it. The data is rather stored outside of the data factory, in a user’s existing storage system.


### Linked Service
A linked service is a service that is linked to an Azure data factory. A linked service can be one of the following:


- A data store such as Azure Storage, Azure SQL Database and on-premises SQL Server database
- A compute service such as Azure HDInsight and Azure Machine Learning. 

A data store is a container of data or datasets and Azure HDInsight is the only compute service supported at this time. You first create a linked service to point to a data store and then define data sets to represent the data from that data store. 

### Data Set
A named view of data. The data being described can vary from simple bytes, semi-structured data like CSV files all the way up to relational tables or even models. A **table** is a data set that has a schema and is rectangular.

###Pipeline
A **pipeline** in an Azure data factory processes data in linked storage services by using linked compute services. It contains a sequence of activities where each activity performing a specific processing operation. For example, a Copy activity copies data from a source storage to a destination storage and Hive/Pig activities use a Azure HDInsight cluster to process data using Hive queries or Pig scripts.

Typical steps for creating an Azure Data Factory instance are:

1. Create a data factory
2. Create a linked service for each data store or compute service
3. Create input and output datasets
4. Create a pipeline 

###Activity
A data processing step in a pipeline that takes one or more input datasets and produces one or more output datasets.  Activities run in an execution environment (for example: Azure HDInsight cluster) and read/write data to a data store associated with the Azure Data Factory instance. 

Azure Data Factory supports the following activities in a pipeline: 

- **Copy Activity** copies the data from a data store to another data store. See [Copy data with Azure Data Factory][copy-data-with-adf] for details about what data stores the Copy Activity supports. 
- **HDInsight Activity** processes data by running Hive/Pig scripts or MapReduce programs on an HDInsight cluster. See [Use Pig and Hive with Data Factory][use-pig-hive] and [Invoke MapReduce Programs from Data Factory][run-map-reduce] for details. 
- **Azure Machine Learning Batch Scoring Activity** invokes the Azure Machine Learning batch scoring API. See [Create Predictive Pipelines using Azure Data Factory and Azure Machine Learning][azure-ml-adf] for details. 
- **Stored Procedure Activity** invokes a stored procedure in an Azure SQL Database. See the [Stored Procedure Activity][msdn-stored-procedure-activity] on MSDN Library for details.   
 
###Data Hub
An Azure Data Hub is a container for data storage and compute services. For example, a Hadoop cluster with HDFS as storage and Hive/Pig as compute (processing) is a data hub. Similarly, an enterprise data warehouse (EDW) can be modeled as a data hub (database as storage, stored procedures and/or ETL tool as compute services).  Pipelines use data stores and run on compute resources in a data hub. Only HDInsight hub is supported at this moment.

The Data Hub allows a data factory to be divided into logical or domain specific groupings, such as the “West US Azure Hub” which manages all of the linked services (data stores and compute) and pipelines focused in the West US data center, or the “Sales EDW Hub” which manages all the linked services and pipelines concerned with populating and processing data for the Sales Enterprise Data Warehouse.

An important characteristic of Hub is that a pipeline runs on a single hub. This means that when defining a pipeline, all of the linked services referenced by tables or activities within that pipeline must have the same hub name as the pipeline itself. If the HubName property is not specified for a linked service, the linked service is placed in the “Default” Hub.

###Slice
A slice is a logical time based partitioning of the data produced by one or more activity runs. A table in an Azure data factory is composed of slices over the time axis. The width of a slice is determined by the schedule – hourly/daily. When the schedule is “hourly”, a slice is produced hourly with the start time and end time of a pipeline and so on.  

Slices provide the ability for IT Professionals to work with a subset of overall data for a specific time window (for example: the slice that is produced for the duration (hour): 1:00 PM to 2:00 PM). They can also view all the downstream data slices for a given time internal and rerun a slice in case of a failure.

The run is a unit of processing for a slice. There could be one or more runs for a slice in case of retries or if you rerun your slice in case of failures. A slice is identified by its start time. 

###Data Management Gateway
Microsoft Data Management Gateway is software that connects on-premises data sources to cloud services for consumption. You must have at least one gateway installed in your corporate environment and register it with the Azure Data Factory portal before adding on-premises data sources as linked services.


##Next Steps

1. [Get started with Data Factory][datafactory-getstarted]. This article provides an end-to-end tutorial that shows you how to create a sample Azure data factory that copies data from an Azure blob to an Azure SQL database.
2. [Tutorial: Move and process log files using Data Factory][adf-tutorial]. This article provides an **end-to-end walkthrough** that shows how to implement a **real world scenario** using Azure Data Factory to transform data from log files into insights.
3. [Common scenarios for using Azure Data Factory][adf-common-scenarios]. This article describes a few common scenarios for using the Azure Data Factory service.

[Power-Query-Azure-Table]: http://office.microsoft.com/en-001/excel-help/connect-to-microsoft-azuretable-storage-HA104122607.aspx
[Power-Query-Azure-Blob]: http://office.microsoft.com/en-001/excel-help/connect-to-microsoft-azure-blob-storage-HA104113447.aspx
[Power-Query-Azure-SQL]: http://office.microsoft.com/en-001/excel-help/connect-to-a-microsoft-azure-sql-database-HA104019809.aspx
[Power-Query-OnPrem-SQL]: http://office.microsoft.com/en-001/excel-help/connect-to-a-sql-server-database-HA104019808.aspx

[copy-data-with-adf]: ../data-factory-copy-activity
[use-pig-hive]: ../data-factory-pig-hive-activities/
[run-map-reduce]: ../data-factory-map-reduce/
[azure-ml-adf]: ../data-factory-create-predictive-pipelines/
[adf-common-scenarios]: ../data-factory-common-scenarios

[msdn-stored-procedure-activity]: https://msdn.microsoft.com/library/dn912649.aspx

[adf-tutorial]: ../data-factory-tutorial
[datafactory-getstarted]: ../data-factory-get-started/

[image-data-factory-introduction-traditional-ETL]: ./media/data-factory-introduction/TraditionalETL.PNG

[image-data-factory-introduction-todays-diverse-processing-landspace]:./media/data-factory-introduction/TodaysDiverseDataProcessingLandscape.PNG

[image-data-factory-application-model]:./media/data-factory-introduction/DataFactoryApplicationModel.png

[image-data-factory-data-flow]:./media/data-factory-introduction/DataFactoryDataFlow.png



