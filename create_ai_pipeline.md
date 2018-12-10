## Creating an AI Pipeline

EnOS Streaming System provides unified operation interface for developers to quickly create and design AI processing pipeline, specify pipeline window strategy, configure device telemetry strategy, and window aggregation scheme. Take the following steps to create an AI processing pipeline.

1. Log in EnOS Developer Center, open the **Console**, and select **Streaming** > **Pipeline Designer**.

2. Click the **+ New Pipeline** icon and enter basic information for the new pipeline, including pipeline name, description, and template to be used. You can also import the configuration file of an existing pipeline to create a pipeline quickly.

3. After the pipeline is created, complete the design of the pipeline on the pipeline details page. 

4. From the **Window Type** and **Latency Setting** dropdown lists, select a window strategy and latency setting for the pipeline.

   - **Window Type**: Windowing is simply the notion of taking a data source and chopping it up along temporal boundaries into finite chunks for processing. Currently, only fixed windows are supported. With fixed windows, the input data are chopped into fixed-sized windows and then processed. See the following example.

   ![](D:/docs/online_data_en/media/window_type.png)

   - **Latency Setting**: Generally, when a window closes, the data processing in the window should have been completed, and a new window will open. However, device data transfer might be delayed because of various factors like device failure and transmission efficiency. Latency setting is introduced to specify the extended validity time of the window after it closes. Late data arriving within the latency will be included in the window and processed again. Late data arriving after the latency will be discarded. In the following example, window size is 5 minutes, and window2 contains data falling in the time range of 11:00 - 11:05. If latency is not enabled, window2 will be closed at 11:05, and data1 and data2 that are arriving late will be discarded. If a latency of 5 minutes is enabled, data1 will be included in window2 for processing again, and data2 will be discarded.

   ![](D:/docs/online_data_en/media/latency_setting.png)

5. In the **Data Processing** section, click **New processing strategy** to add and edit data processing strategies for the pipeline. EnOS Streaming System defines unified window aggregation scheme for the AI data of the same device model. Each data processing strategy consists of an input point, output point, threshold, interpolation strategy, algorithm, and window size.

   - **Input Point**: The model point that provides input data to be processed. Acceptable data types are int, float, and double. Note that an input point cannot be an output point in the same pipeline. Otherwise, an error will be reported.
   - **Output Point**: After the input data is processed, the processed results are transferred to the output point. The output data types can also be int, float, and double.     
   - **Threshold**: Before processing, data are filtered by the specified threshold. Data exceeding the threshold will be processed by interpolation algorithm.
   - **Interpolation Strategy**: Interpolation algorithm that is used to revise the input data. Currently, interpolation strategy supports abandon only. 
   - **Algorithm**: The function that is selected to compute data in the window, including max, min, avg, sum, and cnt. 
   - **Window Size**: Specifies the amount data to be computed in a single window. 

6. When the configuration of the pipeline is completed, you can choose to save the pipeline, export the configuration, revert it to an online version (if available), set the alarm sending method, or release the pipeline online. 