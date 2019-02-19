# Designing a Data Aggregation Stream
The general process of designing a data aggregation stream is as follows:

.. image:: media/managing_stream.png
   :width: 600px

## Creating a Stream
EnOS Streaming System supports creating a data processing stream and importing the configuration file of an existing stream.
- **Creating a stream**:  You can create a stream from scratch by entering the stream name and description and selecting a template for data aggregation.
- **Importing configuration**: You can import the configuration file of an existing stream and enter the new name and description to create a stream quickly.

## Configuring the Stream
EnOS Streaming System provides templates to enable developers configure the following streams:
- Window Aggregation for AI template: Supports AI data aggregation for a measure point and assign the processed data to another measure point on the same device. For details, see [AI Data Aggregation](ai_template_overview).  
- Multiple Data Streams Aggregation template: Supports expression processing of multiple measure points on a device.

## Releasing the Stream
After the stream configuration is completed, you can release it online. Before that, other processing of the stream can be performed, such as saving the stream configuration, setting alarms, reverting the configuration to the online version, and exporting the configuration.
- **Save**: After the stream configuration is completed, click **Save** to save the configuration, so that the stream can be released online.
- **Release**: After the stream configuration is saved, click **Release** to release the stream online.
  .. note:: Before releasing the stream online, ensure that the stream does not have an online running version.
- **Export**: If you want to reuse the stream configuration, click **Export** to export the configuration file to a local directory. When creating more streams, you can import the saved configuration file for reference.
- **Revert to Online Version**: When you are in the progress of editing the stream configuration of an online version and get unsatisfied with the changes, you can revert the configuration to the online version with one click.
- **Alarm Setting**: For each stream, an alarm can be set to report errors through email or SMS. When errors are reported for running streams, the owner of the stream will receive notification.
