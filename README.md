# Near Real Time Analytics with Azure Synapse Link for Azure Cosmos DB

In this workshop, we will go through some of the [Synapse Spark sample notebooks](https://aka.ms/cosmosdb-synapselink-samples). This should provide a starting point for you to dive into the official samples: https://aka.ms/cosmosdb-synapselink-samples. By the way, we encourage feedback and welcome contributions!

You will learn about:

- [Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/?WT.mc_id=data-11340-abhishgu) and how it integrates with [Azure Synapse Analytics](https://docs.microsoft.com/azure/synapse-analytics/?WT.mc_id=data-11340-abhishgu) using [Azure Synapse Link](https://docs.microsoft.com/azure/cosmos-db/synapse-link?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&WT.mc_id=data-11340-abhishgu).
- Basic operations such as setting up Azure Cosmos DB and Azure Synapse Analytics. Creating Azure Cosmos DB containers, setting up Linked Service.
- Scenarios with examples
    - Batch Data Ingestion leveraging Synapse Link for Azure Cosmos DB and performing operations across Azure Cosmos DB containers
    - Streaming ingestion into Azure Cosmos DB collection using Structured Streaming
    - Getting started with Azure Cosmos DB's API for MongoDB and Synapse Link


![](https://docs.microsoft.com/en-us/azure/cosmos-db/media/synapse-link/synapse-analytics-cosmos-db-architecture.png)

## Learning materials and resources

If you want to go back and learn some of the foundational concepts, the following resources might be helpful:

- Key [Azure Cosmos DB concepts](https://docs.microsoft.com/azure/cosmos-db/account-databases-containers-items?WT.mc_id=data-11340-abhishgu) and [how to choose the appropriate API for Azure Cosmos DB](https://docs.microsoft.com/learn/modules/choose-api-for-cosmos-db/?WT.mc_id=data-11340-abhishgu)
- [Basic terminology associated with Azure Synapse Analytics](https://docs.microsoft.com/azure/synapse-analytics/overview-terminology?WT.mc_id=data-11340-abhishgu) and [the features, components that Azure Synapse Analytics provides.](https://docs.microsoft.com/learn/modules/introduction-azure-synapse-analytics/?WT.mc_id=data-11340-abhishgu)
- [An overview of Azure Synapse Link for Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/synapse-link?WT.mc_id=data-11340-abhishgu) and [how to configure and enable Azure Synapse Link to interact with Azure Cosmos DB.](https://docs.microsoft.com/learn/modules/configure-azure-synapse-link-with-azure-cosmos-db/?WT.mc_id=data-11340-abhishgu)

## Pre-requisites

> The steps outlined in this section have completed in advance to save time. The following resources are already available: Azure Cosmos DB accounts (Core SQL API, Mongo DB API), Azure Synapse Analytics workspace along with a Apache Spark pool.

Start by creating a free [Microsoft Azure Account](https://aka.ms/azure-account-free)

**Azure Cosmos DB**

- Create an [Azure Cosmos DB SQL (CORE) API account](https://docs.microsoft.com/en-us/azure/cosmos-db/create-cosmosdb-resources-portal?WT.mc_id=data-11340-abhishgu#create-an-azure-cosmos-db-account)
    - Enable [Synapse Link for Azure Cosmos DB](https://docs.microsoft.com/en-us/azure/cosmos-db/configure-synapse-link?WT.mc_id=data-11340-abhishgu#enable-synapse-link)
    - [Configure IP firewall in Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/how-to-configure-firewall?WT.mc_id=data-11340-abhishgu). Go to the Azure Cosmos DB SQL (CORE) API account page and select **Firewall and virtual networks** on the navigation menu. Change **Allow access** value to **All networks**, and then select **Save**
- Create an [Azure Cosmos DB API for MongoDB account](https://docs.microsoft.com/azure/cosmos-db/create-mongodb-dotnet?WT.mc_id=data-11340-abhishgu#create-a-database-account)

**Azure Synapse Analytics**

- Create an [Azure Synapse Workspace](https://docs.microsoft.com/azure/synapse-analytics/quickstart-create-workspace?WT.mc_id=data-11340-abhishgu)
- Create an [Azure Synapse Analytics Spark Pool](https://docs.microsoft.com/azure/synapse-analytics/quickstart-create-apache-spark-pool-portal?WT.mc_id=data-11340-abhishgu)

Using the Azure Portal, go to the **Access Control (IAM)** tab, click on the **+Add** and **Add a role assignment** links and add yourself to the **Contributor** role. This will allow you to create databases and tables within from your Azure Synapse Spark Pool.

## Workshop setup

> This will be covered during the workshop

Create Azure Cosmos DB database (named **RetailSalesDemoDB**) and three containers (**StoreDemoGraphics**, **RetailSales**, and **Products**). Please make sure to:

- Set the database throughput to `Autoscale` and set the limit to `4000` instead of `400`, this will speed-up the loading process of the data, scaling down the database when it is not in use. For more information, [check the documentation](https://docs.microsoft.com/azure/cosmos-db/provision-throughput-autoscale?WT.mc_id=data-11340-abhishgu).
- Use **/id** as the Partition key for all 3 containers.
- **Analytical store** is set to **On** for all 3 containers.

> [Detailed steps are outlined in the documentation](https://docs.microsoft.com/azure/cosmos-db/configure-synapse-link?WT.mc_id=reactor-3reg-reactor&WT.mc_id=data-11340-abhishgu#create-analytical-ttl)

For Azure Synapse Analytics:

- [Create an Azure Cosmos DB "Linked Service" in Azure Synapse workspace](https://docs.microsoft.com/azure/synapse-analytics/synapse-link/how-to-connect-synapse-link-cosmos-db?toc=/azure/cosmos-db/toc.json&bc=/azure/cosmos-db/breadcrumb/toc.json&WT.mc_id=data-11340-abhishgu#connect-an-azure-cosmos-db-database-to-an-azure-synapse-workspace)
- Load batch data in the [Azure Data Lake Storage Gen2 account](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-introduction?WT.mc_id=data-11340-abhishgu) associated with your Azure Synapse Analytics workspace. Create a `RetailData` folder within the root directory of the storage account. Download [these csv files](https://github.com/Azure-Samples/Synapse/tree/master/Notebooks/PySpark/Synapse%20Link%20for%20Cosmos%20DB%20samples/Retail/RetailData) to your local machine and upload them to the  `RetailData` folder you just created.

You're all set to try out the Notebooks!

## Scenarios

**Batch Data Ingestion leveraging Synapse Link for Azure Cosmos DB**

We will use [this notebook](https://github.com/Azure-Samples/Synapse/blob/master/Notebooks/PySpark/Synapse%20Link%20for%20Cosmos%20DB%20samples/Retail/spark-notebooks/pyspark/1CosmoDBSynapseSparkBatchIngestion.ipynb) to ingest batch data into Azure Cosmos DB using using Synapse. Apache Spark (with PySpark) is used for this purposes. Read up more on [the use cases for Apache Spark in Azure Synapse Analytics](https://docs.microsoft.com/azure/synapse-analytics/spark/apache-spark-overview?WT.mc_id=data-11340-abhishgu#apache-spark-in-azure-synapse-analytics-use-cases), including Data Engineering, Machine Learning etc.

Clone or download the content from the [samples repo](https://github.com/Azure-Samples/Synapse), navigate to the `Synapse/Notebooks/PySpark/Synapse Link for Cosmos DB samples/Retail/spark-notebooks/pyspark` directory and import the `1CosmoDBSynapseSparkBatchIngestion.ipynb` file into your Azure Synapse workspace

> To learn more, read up on how to [Create, develop, and maintain Synapse Studio notebooks in Azure Synapse Analytics](https://docs.microsoft.com/azure/synapse-analytics/spark/apache-spark-development-using-notebooks?tabs=classical&WT.mc_id=data-11340-abhishgu)

**Streaming ingestion into Azure Cosmos DB collection using Structured Streaming**

We will explore [this notebook](https://github.com/Azure-Samples/Synapse/blob/master/Notebooks/PySpark/Synapse%20Link%20for%20Cosmos%20DB%20samples/IoT/spark-notebooks/pyspark/01-CosmosDBSynapseStreamIngestion.ipynb) to get an overview of how to work with streaming data using Spark.

Clone or download the content from the [samples repo](https://github.com/Azure-Samples/Synapse), navigate to the `Synapse/Notebooks/PySpark/Synapse Link for Cosmos DB samples/Retail/spark-notebooks/pyspark` directory and navigate to the `Synapse/Notebooks/PySpark/Synapse Link for Cosmos DB samples/IoT/spark-notebooks/pyspark` directory and import the `01-CosmosDBSynapseStreamIngestion.ipynb` file into your Azure Synapse workspace

**Getting started with Azure Cosmos DB's API for MongoDB and Synapse Link**

We will explore [this notebook](https://github.com/Azure-Samples/Synapse/blob/master/Notebooks/PySpark/Synapse%20Link%20for%20Cosmos%20DB%20samples/IoT/spark-notebooks/pyspark/01-CosmosDBSynapseStreamIngestion.ipynb) to get an overview of how to work with streaming data using Spark.

Clone or download the content from the [samples repo](https://github.com/Azure-Samples/Synapse), navigate to the `Synapse/Notebooks/PySpark/Synapse Link for Cosmos DB samples/Retail/spark-notebooks/pyspark` directory and navigate to the `Synapse/Notebooks/PySpark/Synapse Link for Cosmos DB samples/IoT/spark-notebooks/pyspark` directory and import the `01-CosmosDBSynapseStreamIngestion.ipynb` file into your Azure Synapse workspace.

## Learn at your own pace

- Learn how to [enrich data in Spark tables with new machine learning models that you train using AutoML in Azure Machine Learning](https://docs.microsoft.com/azure/synapse-analytics/machine-learning/tutorial-automl?WT.mc_id=data-11340-abhishgu)
- A dedicated, multi-module **Learning Path** to guide you through how to [Perform data engineering with Azure Synapse Apache Spark Pools](https://docs.microsoft.com/learn/paths/perform-data-engineering-with-azure-synapse-apache-spark-pools/?WT.mc_id=data-11340-abhishgu)
- [Use this tutorial](https://docs.microsoft.com/azure/synapse-analytics/quickstart-sql-on-demand?WT.mc_id=data-11340-abhishgu) to try out how to query data in CSV, Apache Parquet, and JSON files using SQL Serverless pool in Azure Synapse Analytics.