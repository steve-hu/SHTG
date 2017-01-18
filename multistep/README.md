#Solution How To Guide for multistep solution
* [Summary](#Summary)
* [Description](#Description)
* [Prerequisites](#Prerequisites)
* [Post Deployment Guidance](#PostDeployment)
  * [Scaling](#scaling)
  * [Customization](#customization)
  * [Enhancing Visualization](#visualization)
  * [Security](#security)

#<a name="Summary"></a>Summary
<Guide type="ShortDescription">
Deploying multiple ARM templates and executing actions that cannot be accomplsihed through ARM templates.
</Guide>

#<a name="Description"></a>Description
### Estimated Provisioning Time: <Guide type="EstimatedTime">5 Minutes</Guide>
<Guide type="LongDescription">
This pattern demonstrates how to automate a demo that requires deploying multiple ARM templates and also executing actions that cannot be accomplsihed through ARM templates alone.

The pattern involves the following four steps:

1. Deploy an ARM template which creates a Storage Account and a Web Job. The Web Job, when invoked, would create a container in the Storage Account, and then create a text blob in that container.
2. Invoke the Web Job.
3. Deploy another ARM template which creates Data Factory and another Web Job. The Data Factory uses a Copy activity to copy the blob to a different location in the same location. The Web Job, when invoked, would set the Start and End times on the Data Factory pipeline and resume the pipeline.
4. Invoke the second Web Job.
</Guide>

#<a name="Prerequisites">Prerequisites</a>
<Guide type="Prerequisites">
No Prerequisites for this pattern.

You can avoid showing this section in CIQS by removing the &lt;Prerequisites&gt; tag.
</Guide>

# <a name="PostDeployment"></a>Post Deployment Guidance
<Guide type="PostDeployment" value="https://github.com/steve-hu/SHTG/tree/master/multistep"/>
## <a name="scaling"></a>Scaling
* More descriptions about scaling.

## <a name="customization"></a>Customization
* More descriptions about customization.

## <a name="visualization"></a>Enhancing Visualization
* More descriptions about enhancing visualization.

## <a name="security"></a>Security
* More descriptions about security.
