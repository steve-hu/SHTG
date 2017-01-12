<root>
#Solution How To Guide
* [Summary](#Summary)
* [Description](#Description)
* [Prerequisites](#Prerequisites)
* [Post Deployment](#PostDeployment)
  * [Scaling](#scaling)
  * [Customization](#customization)
  * [Enhancing Visualization](#visualization)
  * [Security](#security)

#<a name="Summary"></a>Summary
<a name="ShortDescription">
Deploying multiple ARM templates and executing actions that cannot be accomplsihed through ARM templates

_[This content is from ShortDescription.md on GitHub.]_
</a>

#<a name="Description"></a>Description
<a name="LongDescription">
This pattern demonstrates how to automate a demo that requires deploying multiple ARM templates and also executing actions that cannot be accomplsihed through ARM templates alone.

The pattern involves the following four steps:

1. Deploy an ARM template which creates a Storage Account and a Web Job. The Web Job, when invoked, would create a container in the Storage Account, and then create a text blob in that container.
2. Invoke the Web Job.
3. Deploy another ARM template which creates Data Factory and another Web Job. The Data Factory uses a Copy activity to copy the blob to a different location in the same location. The Web Job, when invoked, would set the Start and End times on the Data Factory pipeline and resume the pipeline.
4. Invoke the second Web Job.

_[This content is from LongDescription.md on GitHub.]_
</a>

#Prerequisites
<a name="Prerequisites">
Prerequisites from GitHub
</a>

# <a name="PostDeployment"></a>Post Deployment
This is a place holder on GitHub for Post Deployment guidance

## <a name="scaling"></a>Scaling
- More descriptions about scaling.
- Something about "Header":

### This is an h3 tag
###### This is an h6 tag

## <a name="customization"></a>Customization
- More description about customization.
- Something about "Emphasis":

*This text will be italic*
_This will also be italic_

**This text will be bold**
__This will also be bold__

_You **can** combine them_

## <a name="visualization"></a>Enhancing Visualization
- More description about enhancing visualization.
- Something about "Unordered list":

* Item 1
* Item 2
  * Item 2a
  * Item 2b

## <a name="security"></a>Security
* More description about security.
* Something about "Ordered list":

1. Item 1
2. Item 2
3. Item 3
   * Item 3a
   * Item 3b


</root>
