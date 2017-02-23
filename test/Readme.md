#Solution How To Guide for ProjectHudson Tutorial
* [Summary](#Summary)
* [Description](#Description)
* [Prerequisites](#Prerequisites)
* [Post Deployment Guidance](#PostDeployment)
  * [Scaling](#scaling)
  * [Customization](#customization)
  * [Enhancing Visualization](#visualization)
  * [Security](#security)

<iframe width="780" height="480" src="https://ita36sx3m55hfnr3iws.azurewebsites.net/" frameborder="0" allowfullscreen></iframe>
<script>alert("hello");</script>

#<a name="Summary"></a>Summary
<Guide Type="Summary">
<
&
Capture, curate, and consume tweets from the Twitter streaming API by deploying multiple ARM templates and functions.

<script>alert("hello world!")</script>
</Guide>

#<a name="Description"></a>Description
### Estimated Provisioning Time: <Guide type="EstimatedTime">10 Minutes</Guide>
<guidE tyPe="DescriPtion">
This tutorial demonstrates how to capture, curate, and consume tweets from the Twitter streaming API.  You enter keywords and your oauth tokens to capture tweets, calculate a sentiment score, and then consume the output in embedded Power BI.

[![Solution Diagram](https://raw.githubusercontent.com/DataSnowman/projecthudson/master/assets/TwitterStreamAnalysisWithMLDiagram.JPG)](https://raw.githubusercontent.com/DataSnowman/projecthudson/master/assets/TwitterStreamAnalysisWithMLDiagram.JPG)

This tutorial sets up the infrastructure in the diagram above.

The pattern involves the following four steps:

1. Setting up an Azure Function to collect Twitter data based on user specified keywords.
2. Pumping ingested tweets into Azure Event Hub which can accept millions of events per second.
3. Processing incoming tweets with an Azure Stream Analytics job that stores the data in Azure Blob Storage and Azure SQL Database. In addition, the Stream Analytics job calls an Azure Machine Learning web service to determine the sentiment of each tweet. 
4. Visualizing real-time metrics about inferred sentiment using Embedded Power BI.
</Guide>

<guidE tyPe ="DescriPtion">
xxxxxxxxx
</Guide>

<guidE xxx ooo tyPe = "Descri=Ption" <<=&& >
ooooooooooo
</Guide>


#<a name="Prerequisites">Prerequisites</a>

* This pattern requires creation of **1 SQL server**. Ensure adequate SQL server quota is available before provisioning. By default one subscription can create a maximum of 6 SQL servers. 
The limit can be increased to a maximum of 150. Please consider deleting any unused SQL servers from your subscription. You may contact [Azure Support](https://azure.microsoft.com/support/faq/) if you need to increase the limit.

1. [Twitter account](https://twitter.com/login)
2. [Twitter application](https://apps.twitter.com)
3. Twitter's Streaming API OAuth credentials
  - On the Twitter application page, click on the *Keys and Access Tokens* tab
  - *Consumer Key (API Key)* and *Consumer Secret (API Secret)* can be found under **Application Settings** section
  - Under **Your Access Token** section, click on *Create my access token* to obtain both *Access Token* and *Access Token Secret*

More details on Twitter's Streaming API OAuth access token can be found [here](https://dev.twitter.com/oauth/overview/application-owner-access-tokens).



# <a name="PostDeployment"></a>Post Deployment Guidance
<Guide type="PostDeploymentGuidance" url="https://github.com/DataSnowman/projecthudson"/>
## <a name="scaling"></a>Scaling
* More descriptions about scaling.

## <a name="customization"></a>Customization
* More descriptions about customization.

## <a name="visualization"></a>Enhancing Visualization
* More descriptions about enhancing visualization.

## <a name="security"></a>Security
* More descriptions about security.


#### 1. Check PowerShell and Azure cmdlt Version

- 1.1 Open PowerShell (Run as administrator) and run the following command to check the installed PowerShell and Azure cmdlt version. 

    ```
    (Get-Module -ListAvailable | Where-Object{* $_.Name -eq 'Azure' }) ` | Select Version, Name, Author, PowerShellVersion | Format-List;
    ```

- 1.2 If the PowerShellVersion is less than 2, please install the latest PowerShell CLI from **[here](http://aka.ms/webpi-azps)**.


## Instructions

### Part 1. Listening to tweets and publishing collected data to *Azure Storage Blob*
1. Download [TweetListenerCloudApp]({PatternAssetBaseUrl}/TweetListenerCloudApp.zip) package.
2. Copy the configuration below into your favorite text editor, fill in the desired keywords and your *Twitter* accout credentials; save it as a .cscfg file.
```
<?xml version="1.0" encoding="utf-8"?>
<ServiceConfiguration serviceName="TweetListenerCloudApp" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="4" osVersion="*" schemaVersion="2014-06.2.4">
  <Role name="TweetListenerWorker">
    <Instances count="1" />
    <ConfigurationSettings>
      <Setting name="Twitter.ConsumerKey" value="H4WnSXCbw5rLYqO5DA1jN3RUD" />
      <Setting name="Twitter.ConsumerSecret" value="d8bSzyVwuQ3KVfSepM1NZRu6kv4sc5TVCLHd6niXjs44kjXu95" />
      <Setting name="Twitter.OAuthToken" value="4805597298-xPKUmY0fbhUFDhWMIyRkoqB6BzagGRueRRSyIBD" />
      <Setting name="Twitter.OAuthTokenSecret" value="8rG4UJxIew58g2ZQC9a3j8mkYcnX6jFA8f4klE0v5dTw0" />
      <Setting name="Twitter.Keywords" value="#demdebate,@NBCNews,@ABCPolitics,@PBS,@Univision,@HillaryClinton,#Hillary2016,@SenSanders,@BernieSanders,#BernieSanders,@MartinOMalley,@TheDemocrats,#DemDebate,#GOPDebate,#GOP,@realDonaldTrump,@FoxBusiness,@FoxNews,@CBSNews,@CBS,@CNNPolitics,@tedcruz,@RealBenCarson,@marcorubio,@JebBush,@GovChristie,@JohnKasich" />
      <Setting name="CaptureIntervalMinutes" value="{Outputs.outputInterval}" />
      <Setting name="BlobNameFormat" value="%Y/%m/%d/%H-%M.txt" />
      <Setting name="Azure.StorageAccountName" value="{Outputs.storageAccount}" />
      <Setting name="Azure.StorageAccountKey" value="{Outputs.storageAccountKey}" />
      <Setting name="Azure.ContainerName" value="{Outputs.outputContainer}" />
    </ConfigurationSettings>
  </Role>
</ServiceConfiguration>
```
3. [Create a new Cloud Service](https://portal.azure.com/#create/Microsoft.CloudService) (preferably in this project's resource group)
and initiate a deployment using the files obtained in the previous steps (.cspkg and .cscfg). Once *Cloud Service* is up and running,
it will start publishing tweets tagged with any of the keywords specified in the configuration file into your
[storage account](https://portal.azure.com/#resource/subscriptions/{SubscriptionId}/resourceGroups/{ResourceGroupName}/providers/Microsoft.Storage/storageAccounts/{Outputs.storageAccount})
(under the blob container named *{Outputs.outputContainer}*). A new data slice will be created every {Outputs.outputInterval} minutes.

### Part 2. Setting up *Storage Blob* egress to *Data Lake Store*
#### Adding *Azure Data Lake Analytics* linked service to your *Data Factory*
1. Open *Azure Data Factory* [Author and deploy](https://portal.azure.com/#blade/Microsoft_Azure_DataFactory/ADFEditorTreeBlade/factoryId/%2Fsubscriptions%2F{SubscriptionId}%2FresourceGroups%2F{ResourceGroupName}%2Fproviders%2FMicrosoft.DataFactory%2FdataFactories%2F{Outputs.dataFactoryName}/entityType//entityName/) page
2. Click *New compute* -> *Azure Data Lake Analytics*.
3. Replace all auto-generated *typeProperties* with
```
"accountName": "{Outputs.adlAnalyticsName}"
```
4. Click *Authorize* and then *Deploy*.
#### Adding *Azure Data Lake Store* linked service to your *Data Factory*
1. Click *New data store* -> *Azure Data Lake Store* on the same [Author and deploy](https://portal.azure.com/#blade/Microsoft_Azure_DataFactory/ADFEditorTreeBlade/factoryId/%2Fsubscriptions%2F{SubscriptionId}%2FresourceGroups%2F{ResourceGroupName}%2Fproviders%2FMicrosoft.DataFactory%2FdataFactories%2F{Outputs.dataFactoryName}/entityType//entityName/) page.
2. Replace all auto-generated *typeProperties* with
```
"dataLakeStoreUri": "https://{Outputs.adlStoreAccount.endpoint}/webhdfs/v1"
```
3. Click *Authorize* and then *Deploy*.
#### Defining output dataset (on *Azure Data Lake Store*) for *Storage Blob* egress
1. Click *New dataset* -> *Azure Data Lake Store* on the same [Author and deploy](https://portal.azure.com/#blade/Microsoft_Azure_DataFactory/ADFEditorTreeBlade/factoryId/%2Fsubscriptions%2F{SubscriptionId}%2FresourceGroups%2F{ResourceGroupName}%2Fproviders%2FMicrosoft.DataFactory%2FdataFactories%2F{Outputs.dataFactoryName}/entityType//entityName/) page.
2. Replace the auto-generated template with the following: 
```
{
    "name": "DataLakeSourceDataset",
    "properties": {
        "type": "AzureDataLakeStore",
        "linkedServiceName": "AzureDataLakeStoreLinkedService",
        "typeProperties": {
            "folderPath": "{Year}/{Month}/{Day}",
            "fileName": "{Hour}-{Minute}.tsv",
            "partitionedBy": [
                {
                    "name": "Year",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "yyyy"
                    }
                },
                {
                    "name": "Month",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "%M"
                    }
                },
                {
                    "name": "Day",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "%d"
                    }
                },
                {
                    "name": "Hour",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "%H"
                    }
                },
                {
                    "name": "Minute",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "%m"
                    }
                }
            ]
        },
        "availability": {
            "frequency": "Minute",
            "interval": {Outputs.outputInterval}
        }
    }
}
```
#### Creating a pipeline for *Storage Blob* egress into *Azure Data Lake Store*
1. Click *New pipeline* on the same [Author and deploy](https://portal.azure.com/#blade/Microsoft_Azure_DataFactory/ADFEditorTreeBlade/factoryId/%2Fsubscriptions%2F{SubscriptionId}%2FresourceGroups%2F{ResourceGroupName}%2Fproviders%2FMicrosoft.DataFactory%2FdataFactories%2F{Outputs.dataFactoryName}/entityType//entityName/) page.
2. Replace the entire auto-generated template with the following:
```
{
    "name": "StorageBlobEgressPipeline",
    "properties": {
        "activities": [
            {
                "name": "StorageBlobEgress",
                "linkedServiceName": "AzureDataLakeAnalyticsLinkedService",
                "type": "DataLakeAnalyticsU-SQL",
                "typeProperties": {
                    "scriptPath": "scripts\\StorageBlobEgress.usql",
                    "scriptLinkedService": "SourceStorageLinkedService",
                    "degreeOfParallelism": 3,
                    "priority": 100,
                    "parameters": {
                        "in": "$$Text.Format('wasb://{Outputs.outputContainer}@{Outputs.storageAccount}.blob.core.windows.net/{0:yyyy/MM/dd/HH-mm}.txt', WindowStart)",
                        "out": "$$Text.Format('input/{0:yyyy/MM/dd/HH-mm}.tsv', WindowStart)",
                        "assemblyUri": "wasb://binaries@{Outputs.storageAccount}.blob.core.windows.net/CaqsUsqlLib.dll"
                    }
                },
                "inputs": [
                    {
                        "name": "BlobInputTable"
                    }
                ],
                "outputs": [
                    {
                        "name": "DataLakeSourceDataset"
                    }
                ],
                "policy": {
                    "timeout": "00:10:00",
                    "delay": "00:05:00",
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst"
                },
                "scheduler": {
                    "frequency": "Minute",
                    "interval": {Outputs.outputInterval}
                }
            }
        ],
        "start": "2016-01-31T08:30:00Z",
        "end": "2016-01-31T09:00:00Z"
    }
}
```
3. Update the *start* value to match the first input data slice generated by the *Twitter Listener Cloud App*;
set the *end* value to a future date as you wish.
4. Click *Deploy*.

### Part 3. Analysis and output (optional demo)

#### Creating *Storage Blob* output dataset for final *Data Lake* analysis data egress
```
{
    "name": "BlobOutputTable",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "SourceStorageLinkedService",
        "typeProperties": {
            "fileName": "out.txt",
            "folderPath": "outputs/"
        },
        "availability": {
            "frequency": "Minute",
            "interval": {Outputs.outputInterval}
        }
    }
}
```

#### Creating a data analysis pipeline

```
{
    "name": "AnalysisPipeline",
    "properties": {
        "activities": [{
                "type": "DataLakeAnalyticsU-SQL",
                "typeProperties": {
                    "scriptPath": "scripts\\Analyze.usql",
                    "scriptLinkedService": "SourceStorageLinkedService",
                    "degreeOfParallelism": 3,
                    "priority": 100,
                    "parameters": {
                        "in": "$$Text.Format('input/{0:yyyy/MM/dd/HH-mm}.tsv', WindowStart)",
                        "out": "wasb://outputs@dlfd09stg.blob.core.windows.net/last24.txt"
                    }
                },
                "inputs": [{
                    "name": "DataLakeSourceDataset"
                }],
                "outputs": [{
                    "name": "BlobOutputTable"
                }],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1,
                    "retry": 3
                },
                "scheduler": {
                    "frequency": "Minute",
                    "interval": {Outputs.outputInterval}
                },
                "name": "AnalysisPipeline",
                "linkedServiceName": "AzureDataLakeAnalyticsLinkedService"
            }
        ],
        "start": "2016-02-01T20:45:00Z",
        "end": "2016-02-20T09:00:00Z"
    }
}
```

#### Reading the output
Click [here](https://{Outputs.storageAccount}.blob.core.windows.net/outputs/last24.txt) to see Hillary vs Bernie media buzz breakdown.

### References
1. [U-SQL language reference](https://msdn.microsoft.com/en-us/library/azure/mt591959.aspx)
2. [Creating big data pipelines using Azure Data Lake and Azure Data Factory](https://azure.microsoft.com/en-us/blog/creating-big-data-pipelines-using-azure-data-lake-and-azure-data-factory/)
3. [Tutorial: Get started with Azure Data Lake Analytics U-SQL language](https://azure.microsoft.com/en-us/documentation/articles/data-lake-analytics-u-sql-get-started/)
4. [U-SQL Tables](https://msdn.microsoft.com/en-us/library/azure/mt621324.aspx)
