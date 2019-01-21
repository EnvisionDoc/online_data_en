# AI Data Aggregation Process
This guide intends to help you learn how to process AI data with the AI aggregation template.

## Prerequisites
- EnOS developer account
- Authorization to the Streaming module
- Device connection is configured with data uploading to the cloud

## Procedure
The procedure of processing data with AI aggregation template is as follows:

1. Design and create AI processing pipeline

2. Save and publish the pipeline

3. Start the pipeline

4. Monitor the running and results of the pipeline

## Goal and Data Preparation
**Goal**

The goal of this guide is to get the maximum value of the *test_raw* input point within every 5 minutes and output the value to the output point *test_5min*.

**Data Preparation**
- Model configuration: Detailed information about the *testModel* for this guide is as follows:

.. list-table::
   :widths: 20 15 15 15 20 10

   * - Feature Type
     - Name
     - Identifier
     - Point Type
     - Data Type
     - Unit
   * - Measure Point
     - test_raw
     - test_raw
     - AI
     - DOUBLE
     - ...
   * - Measure Point
     - test_5min
     - test_5min
     - AI
     - DOUBLE
     - ...

.. note:: - In this example, *test_raw* is the input point, and *test_5min* is the output point.
        - Ensure that both the input point and the output point are of AI type.


- Storage configuration:  Configuring the input point *test_raw* as AI raw data and the output point *test_5min* as minute-level normalized AI data. For more information, see [Storage Strategy Configuration](https://docs.envisioniot.com/docs/data-asset/en/latest/storage_strategy_overview.html).  
- Data ingestion: For information about data ingestion of input point *test_raw*, see [Device Connection](https://docs.envisioniot.com/docs/device-connection/zh_CN/latest/gettingstarted_device_connection.html).


## Step 1. Designing AI Aggregation Pipeline

1. Log in EnOS Console and click **Streaming** > **Pipeline Designer** to view all the pipelines that have been created within the organization on the **Pipeline Management** page. You can double-click a pipeline to view and edit its configuration.

2. Click the **+** icon above the pipeline list to create a new pipeline. Enter the name and description of the pipeline and select *Window Aggregation for AI* as the template. Optionally, you can choose to import the configuration file of an existing pipeline to complete the configuration quickly.

3. Configure the window strategy of the pipeline

   - Window Type: Select **Tumbling Window** type, which has a fixed size and does not overlap.
   - Latency Setting: Select an allowed lateness for data arriving late. *0 second* indicates that data arriving late will be dropped.

4. Configure the processing strategy of the pipeline. Click **New processing strategy** to add a data processing record. Description of each field is as follows:

   - **Input point**: Select the measure point of AI raw data. In this example, select the *test_raw* point of the *testModel*.
   - **Threshold**: Specify the threshold for filtering raw data before processing.
   - **Interpolation**: Select the interpolation algorithm that is used to process the input data that exceed the threshold. Currently, interpolation strategy supports abandon only.
   - **Aggregation**: Select the function to compute valid data in the window. EnOS Streaming System currently supports functions like max, min, avg, sum, and cnt.
   - **Window size**: Select the size of time window, that is the amount of data to be computed in a single window.
   - **Output point**: Select the point to receive the processed result. In this example, select the *test_5min* point.

## Step 2. Save and Release the Pipeline

When the pipeline configuration is completed, you can save and release the AI aggregation pipeline. Then, you can click **Streaming** > **Pipeline Monitor** to view the released pipeline.

## Step 3. Start the Pipeline

On the **Pipeline Monitoring** page, find the released pipeline in the table, and click the **Start** icon |start_icon| for the pipeline in the **Operation** column to start running the pipeline.

## Step 4. View the Running Results of the Pipeline

On the **Pipeline Monitoring** page, find the running pipeline in the table, and click the pipeline name to open the **Pipeline Details** page. You can view the following information about the pipeline:

- **Summary**: View the summary of the running pipeline, such as the overall data processing records and the data aggregation records in a specific period.
- **Log**: Click the **View Logs** icon on the upper right corner to check the running log of the pipeline.
- **Results**: Call the *getAssetsAINormalizedData* API to get the minute-level normalized data in the output point. For more information, see [Calling EnOS REST APIs](https://docs.envisioniot.com/docs/app-development/en/latest/call_enos_api.html).

.. |start_icon| image:: media/start_icon.png

<!--end-->
