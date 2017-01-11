This pattern demonstrates how to automate a demo that requires deploying multiple ARM templates and also executing actions that cannot be accomplsihed through ARM templates alone.

The pattern involves the following four steps:

1. Deploy an ARM template which creates a Storage Account and a Web Job. The Web Job, when invoked, would create a container in the Storage Account, and then create a text blob in that container.
2. Invoke the Web Job.
3. Deploy another ARM template which creates Data Factory and another Web Job. The Data Factory uses a Copy activity to copy the blob to a different location in the same location. The Web Job, when invoked, would set the Start and End times on the Data Factory pipeline and resume the pipeline.
4. Invoke the second Web Job.

_[This content is from LongDescription.md on GitHub.]_
