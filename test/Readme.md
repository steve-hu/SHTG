#Solution How To Guide for ProjectHudson Tutorial
* [Summary](#Summary)
* [Description](#Description)
* [Prerequisites](#Prerequisites)
* [Post Deployment Guidance](#PostDeployment)
  * [Scaling](#scaling)
  * [Customization](#customization)
  * [Enhancing Visualization](#visualization)
  * [Security](#security)

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
