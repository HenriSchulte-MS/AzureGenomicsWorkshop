# Exercise 2 - Work with Data in Azure

## Contents
- Exercise 2 - Work with Data in Azure
    - [Prerequisites](#prerequisites)
    - [Task 1: Provision an Azure Storage Account](#task-1-provision-an-azure-storage-account)
    - [Task 2: Create a blob container](#task-2-create-a-blob-container)
    - [Task 3: Provision an Azure Data Factory](#task-3-provision-an-azure-data-factory)
    - [Task 4: Create a linked service](#task-4-create-a-linked-service)
    - [Task 5: Create a linked service to an external source](#task-5-create-a-linked-service-to-an-external-source)
    - [Task 6: Create datasets](#task-6-create-datasets)


## Prerequisites
Before you start this hands-on lab, make sure that you have completed the steps outlines in [Prerequisites - Before the hands-on lab](/Prerequisites%20-%20Before%20the%20hands-on%20lab.md).


## Task 1: Provision an Azure Storage Account
An Azure storage account contains all of your Azure Storage data objects: blobs, files, queues, tables, and disks. The storage account provides a unique namespace for your Azure Storage data that is accessible from anywhere in the world over HTTP or HTTPS. Data in your Azure storage account is durable and highly available, secure, and massively scalable.

### Create a storage account

Every storage account must belong to an Azure resource group. A resource group is a logical container for grouping your Azure services. When you create a storage account, you have the option to either create a new resource group, or use an existing resource group. This article shows how to create a new resource group.

A **general-purpose v2** storage account provides access to all of the Azure Storage services: blobs, files, queues, tables, and disks. The steps outlined here create a general-purpose v2 storage account, but the steps to create any type of storage account are similar. For more information about types of storage accounts and other storage account settings, see [Azure storage account overview](https://docs.microsoft.com/en-us/azure/storage/common/storage-account-overview).

To create a general-purpose v2 storage account in the Azure portal, follow these steps:

1. On the Azure portal menu, select **Create a resource**. Type **Storage Account** into the search bar. As you begin typing, you receive suggestions. Click on the suggested **Storage account**.
1. On the **Storage account** window that appears, choose **Create**.
1. On the **Basics** tab, select the subscription in which to create the storage account.
1. Under the **Resource group** field, select the resource group you used in Exercise 1, or create a new resource group.  For more information on Azure resource groups, see [Azure Resource Manager overview](https://docs.microsoft.com/en-us/azure/azure-resource-manager/management/overview).
1. Next, enter a name for your storage account. The name you choose must be unique across Azure. The name also must be between 3 and 24 characters in length, and may include only numbers and lowercase letters. Note this name for later.
1. Select a location for your storage account (e.g. West Europe), or use the default location.
1. Select a performance tier. The default tier is *Standard*.
1. Set the **Account kind** field to *Storage V2 (general-purpose v2)*.
1. Specify how the storage account will be replicated. You can leave this setting at the default option: *Read-access geo-redundant storage (RA-GRS)*. For more information about available replication options, see [Azure Storage redundancy](https://docs.microsoft.com/en-us/azure/storage/common/storage-redundancy).
1. Additional options are available on the **Networking**, **Data protection**, **Advanced**, and **Tags** tabs. Skip these tabs and navigate straight to **Review + Create** to review your storage account settings and create the account.
![Create storage account](https://docs.microsoft.com/en-us/azure/includes/media/storage-create-account-portal-include/account-create-portal.png)
1. Select **Create**. Your Storage Account is now being deployed and will complete in a short while.
1. Once deployment is complete, select **Go to resource** to navigate to the storage account page.
![Deployment complete](/img/2.1_deployment.png)


## Task 2: Create a blob container
Azure Blob storage is Microsoft's object storage solution for the cloud. Blob storage is optimized for storing massive amounts of unstructured data. Unstructured data is data that doesn't adhere to a particular data model or definition, such as text or binary data.

### Create a blob container

In this section, you create a blob container named **adftutorial** in Azure Blob storage.

1. From the storage account page, select **Overview** > **Containers**.
![Containers](/img/2.2_containers.png)
1. On the *\<Account name>* | **Containers** page's toolbar, select **+ Container**.
1. In the **New container** dialog box, enter **project1** for the name, and then select **Create**. The *\<Account name>* | **Containers** page is updated to include **project1** in the list of containers.
1. Repeat steps 2. and 3. to create two additional containers named **project2** and **project3**.


## Task 3: Provision an Azure Data Factory

1. Launch **Microsoft Edge** or **Google Chrome** web browser. Currently, Data Factory UI is supported only in Microsoft Edge and Google Chrome web browsers.
1. Go to the [Azure portal](https://portal.azure.com). 
1. From the Azure portal menu, select **Create a resource**.
1. Select **Integration**, and then select **Data Factory**. 
   
   ![Data Factory selection in the "New" pane](https://docs.microsoft.com/en-us/azure/data-factory/media/doc-common-process/new-azure-data-factory-menu.png)
1. On the **Create Data Factory** page, under **Basics** tab, select your Azure **Subscription** in which you want to create the data factory.
1. For **Resource Group**, select the resource group you created previously.

1. For **Region**, select the location for the data factory (e.g. West Europe).
 
1. For **Name**, enter a *globally unique* name. If you see the following error, change the name of the data factory (for example, **&lt;yourname&gt;DataFactory**) and try creating again. For naming rules for Data Factory artifacts, see the [Data Factory - naming rules](https://docs.microsoft.com/en-us/azure/data-factory/naming-rules) article.
  
   ![Error when a name is not available](https://docs.microsoft.com/en-us/azure/data-factory/media/doc-common-process/name-not-available-error.png)

1. For **Version**, select **V2**.

1. Select **Next: Git configuration**, and then select **Configure Git later** check box.

1. Select **Review + create**, and select **Create** after the validation is passed. After the creation is complete, select **Go to resource** to navigate to the **Data Factory** page. 

1. Select the **Author & Monitor** tile to start the Azure Data Factory user interface (UI) application on a separate browser tab.
   
   ![Home page for the data factory, with the "Author & Monitor" tile](https://docs.microsoft.com/en-us/azure/data-factory/media/doc-common-process/data-factory-home-page.png)
   
   > If you see that the web browser is stuck at "Authorizing", clear the **Block third-party cookies and site data** check box. Or keep it selected, create an exception for **login.microsoftonline.com**, and then try to open the app again.


## Task 4: Create a linked service
In this procedure, you create a linked service to link your Azure Storage account to the data factory. The linked service has the connection information that the Data Factory service uses at runtime to connect to it.

1. On the Azure Data Factory UI page, open [**Manage**](https://docs.microsoft.com/en-us/azure/data-factory/author-management-hub) tab from the left pane.

1. On the Linked services page, select **+New** to create a new linked service.

   ![New linked service](https://docs.microsoft.com/en-us/azure/data-factory/media/doc-common-process/new-linked-service.png)
   
1. On the **New Linked Service** page, select **Azure Blob Storage**, and then select **Continue**. 

1. On the New Linked Service (Azure Blob Storage) page, complete the following steps: 

   a. For **Name**, enter **AzureStorageLinkedService**.

   b. For **Storage account name**, select the name of your Azure Storage account.

   c. Select **Test connection** to confirm that the Data Factory service can connect to the storage account. 

   d. Select **Create** to save the linked service. 

      ![Linked service](https://docs.microsoft.com/en-us/azure/data-factory/media/quickstart-create-data-factory-portal/linked-service.png)


## Task 5: Create a linked service to an external source
In the previous task, you have created a linked service to a storage account in your own subscription. Now, you are setting up a connection to an external data source. This workshop is using an AWS S3 bucket for this purpose, however, this could also be another source on the internet or on-premise.

1. On the Azure Data Factory UI page, open [**Manage**](https://docs.microsoft.com/en-us/azure/data-factory/author-management-hub) tab from the left pane.

1. On the Linked services page, select **+New** to create a new linked service.

   ![New linked service](https://docs.microsoft.com/en-us/azure/data-factory/media/doc-common-process/new-linked-service.png)
   
1. On the **New Linked Service** page, select **Amazon S3**, and then select **Continue**. 

1. On the New Linked Service (Amazon S3) page, complete the following steps: 

   a. For **Name**, enter **S3LinkedService**.

   b. For **Access key ID**, enter **AKIA43YCVZVZCMWZO5QB**.

   c. For **Secret access key**, paste the key that your instructor has provided to you.

   d. Select **Test connection** to confirm that the Data Factory service can connect to the storage account. 

   e. Select **Create** to save the linked service. 

      ![Linked service](img/2.5_s3.png)


## Task 6: Create datasets
In this procedure, you create two datasets: **InputDataset** and **OutputDataset**. These datasets are of type **Amazon S3** and **AzureBlob**. They refer to the linked services that you created in the previous task. 

In the linked service settings, you specified the S3 bucket that contains the source data. In the source dataset settings, you can specify where exactly the source data resides (bucket, folder, and file). In the sink dataset settings, you can specify where the data is copied to (blob container, folder, and file). 

1. Select **Author** tab from the left pane.

1. Select the **+** (plus) button, and then select **Dataset**.

   ![Menu for creating a dataset](https://docs.microsoft.com/en-us/azure/data-factory/media/quickstart-create-data-factory-portal/new-dataset-menu.png)

1. On the **New Dataset** page, select **Amazon S3**, and then select **Continue**. 

1. On the **Select Format** page, choose the format type of your data, and then select **Continue**. In this case, select **Binary** when copy files as-is without parsing the content.

   ![Select format](https://docs.microsoft.com/en-us/azure/data-factory/media/quickstart-create-data-factory-portal/select-format.png)
   
1. On the **Set Properties** page, complete following steps:

    a. Under **Name**, enter **InputDataset**. 

    b. For **Linked service**, select **S3LinkedService**.

    c. For **File path**, select the **Browse** button.

    d. In the **Choose a file or folder** window, choose **genomics-external-source**, and then select **OK**.
    
    e. Select **OK**.   

    ![Set properties for InputDataset](/img/2.6_s3.png)
1. Repeat the steps to create the output dataset:  

    a. Select the **+** (plus) button, and then select **Dataset**.

    b. On the **New Dataset** page, select **Azure Blob Storage**, and then select **Continue**.

    c. On the **Select Format** page, choose **Binary**, and then select **Continue**.

    d. On the **Set Properties** page, specify **OutputDataset** for the name. Select **AzureStorageLinkedService** as linked service.

    e. Leave **File path** unspecified for now. It will be determined dynamically later.

    f. Select **OK**.   

1. In order to output our data to the correct container, you need to specify the file path. However, the correct container will only be determined at runtime. Therefore, instead of specifying a container now, you can provide the dataset with a parameter instead. Select **OutputDataset** on the left. 
1. Select **Parameters** to open the parameter tab.
1. Select **+New** to create a new parameter.
1. For **Name**, enter **container**.

![Create parameter](/img/2.6_outparam.png)