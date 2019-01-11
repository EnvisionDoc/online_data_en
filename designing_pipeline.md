# Designing Data Aggregation Pipeline
The general process of designing a data aggregation pipeline is as follows:

.. image:: media/2019-01-02-21-17-10.png
   :width: 700px

## Creating a Pipeline
EnOS Streaming System supports creating a pipeline and importing the configuration file of an existing pipeline.
- Creating a pipeline:  You can create a pipeline from scratch by entering the pipeline name and description and selecting a template for data aggregation.
- Importing configuration: You can import the configuration file of an existing pipeline and enter the new name and description to create a pipeline quickly.

## Configuring the Pipeline
EnOS Streaming System provides templates to enable developers configure the following pipelines:
- Window Aggregation for AI template: Supports AI data aggregation for a measure point and assign the processed data to another measure point on the same device. For details, see [AI Data Aggregation Template Overview](ai_template_overview).  
- Multi-channel Processing template: Supports expression processing of multiple measure points on a device.

## Releasing the Pipeline
After the pipeline configuration is completed, you can release it online. Before that, other processing of pipeline can be performed, such as saving the pipeline configuration, setting alarms, reverting the configuration to the online version, and exporting the configuration.
- **Save**: After the pipeline configuration is completed, click **Save** to save the configuration, so that the pipeline can be released online.
- **Release**: After the pipeline configuration is saved, click **Release** to release the pipeline online.
  .. note:: before releasing the pipeline online, ensure that the pipeline does not have an online running version.
- **Export**: If you want to reuse the pipeline configuration, click **Export** to export the configuration file to local directory. When creating more pipelines, you can import the saved configuration file for reference.
- **Revert to online version**: When you are in the progress of editing the pipeline configuration of an online version and get unsatisfied with the changes, you can revert the configuration to the online version with one click.
- **Alarm setting**: For each pipeline, an alarm can be set to report errors through email or SMS. When errors are reported for running pipelines, the owner of the pipeline will receive notification.

## Monitoring the Pipeline
After the pipeline is released online, you can manage the pipeline through various operations on the **Pipeline Monitoring** page, such as previewing, starting and stopping the pipeline, checking its running status, and viewing pipeline configuration.
- **Preview pipeline**: Before starting the pipeline, you can preview it. On the **Pipeline Monitoring** page, search for the pipeline by its ID or name, or filter pipelines with the pipeline owner. Click the pipeline name to open the pipeline details page, and then click the **Preview** icon in the upper right corner of the page. Complete the preview configuration and run the preview. The preview will intercept and process the latest real-time data, and the processed data will not be saved in the DB. With the preview results, you can debug and tune the pipeline. If the preview results meet the expectation, you can start the pipeline directly.
- **Start pipeline**: Click the **Start** button in the **Operation** column of the pipeline list to start the pipeline. Then the pipeline will run continuously in the Streaming system.
- **Stop pipeline**: To stop a running pipeline, click the **Stop** button in the **Operation** column.
- **Check running status**: Click the name of a running pipeline to view its running status, including its running result summary and error logs.
- **View configuration**: Click the **View Configuration** button in the **Operation** column to view the pipeline configuration.
- **Export configuration**: Click the **Export Configuration** button in the **Operation** column to export the pipeline configuration.
