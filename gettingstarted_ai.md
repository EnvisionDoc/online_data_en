# AI Data Aggregation Process
This guide intends to help you learn how to process AI data with the AI aggregation template.

## Prerequisites
- EnOS developer account
- Authorization to the Streaming module
- Device connection is configured with data uploading to the cloud

## Procedure
The procedure of processing data with AI aggregation template is as follows:

1. Design and create AI processing stream

2. Save and release the stream

3. Start the stream

4. Monitor the running and results of the stream

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


- Storage configuration:  Configuring the input point *test_raw* as AI raw data and the output point *test_5min* as minute-level normalized AI data. For more information, see [Storage Strategy Configuration](https://www.envisioniot.com/docs/data-asset/en/latest/storage_strategy_overview.html).  
- Data ingestion: For information about data ingestion of input point *test_raw*, see [Device Connection](https://www.envisioniot.com/docs/device-connection/en/latest/gettingstarted_device_connection.html).


## Step 1. Designing AI Aggregation Stream

1. Log in EnOS Console and click **Stream Data Processing** > **Stream Development** to view all the streams created within the organization. You can double-click a stream to view and edit its configuration.

2. Click the **+** icon above the stream list to create a stream. Enter the name and description of the stream and select *Window Aggregation for AI* as the template. Optionally, you can choose to import the configuration file of an existing stream to complete the configuration quickly.

3. Configure the window strategy of the stream

   - Window Type: Select **Tumbling Window** type, which has a fixed size and does not overlap.
   - Latency Setting: Select an allowed lateness for data arriving late. *0 second* indicates that data arriving late will be dropped.

4. Configure the data processing strategy of the stream. Click **New strategy** to add a data processing strategy. Description of each field is as follows:

   - **Input point**: Select the measure point of AI raw data. In this example, select the *test_raw* point of the *testModel*.
   - **Threshold**: Specify the threshold for filtering raw data before processing.
   - **Interpolation**: Select the interpolation algorithm that is used to process the input data that exceed the threshold. Currently, interpolation strategy supports abandon only.
   - **Aggregation**: Select the function to compute valid data in the window. EnOS Streaming System currently supports functions like max, min, avg, sum, and cnt.
   - **Window Size**: Select the size of each time window, that is the amount of data to be computed in a single window.
   - **Output Point**: Select the point to receive the processed result. In this example, select the *test_5min* point.

## Step 2. Save and Release the Stream

When the stream configuration is completed, you can save and release the AI aggregation stream. Then, you can click **Stream Operation** to view the released stream.

## Step 3. Start the Stream

On the **Stream Operation** page, find the released stream in the table, and click the **Start** icon |start_icon| for the stream in the **Operations** column to start running the stream.

## Step 4. View the Running Results of the Stream

On the **Stream Operation** page, find the running stream in the table, and click the stream name to open the **Stream Details** page. You can view the following information about the stream:

- **Summary**: View the summary of the running stream, such as the overall data processing records and the data aggregation records in a specific period.
- **Log**: Click the **View Logs** icon on the upper right corner to check the running log of the stream.
- **Results**: Call the *getAssetsAINormalizedData* API to get the minute-level normalized data in the output point. For more information, see [Calling EnOS REST APIs](https://www.envisioniot.com/docs/app-development/en/latest/call_enos_api.html).

.. |start_icon| image:: media/start_icon.png

<!--end-->
