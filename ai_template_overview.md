# AI Data Aggregation Template Overview
EnOS Streaming System provides a unified AI data aggregation template to process the AI data ingested from a measure point and assign the processed data to another measure point defined for the same device, thus enabling developers to process real-time AI data easily and quickly.  

## Features
1. Supporting event-time-based AI data aggregation.

2. Providing rich aggregation algorithm, including  max, min, avg, sum, and cnt.

3. Supporting time window latency. When window latency is set, an intermediate output will be generated (the computed output value of the current window when the next window starts) . The intermediate output will be saved in TSDB but will not be archived.

4. Supporting various threshold ranges. Input data that exceed the threshold will be processed by the interpolation algorithm.

## Aggregation Pipeline Configuration
The configuration method of the aggregation pipeline depends on the type of selected time window. Time window is a useful data processing mechanism of streaming computing. Windowing is simply the notion of taking a data source and chopping it up along temporal boundaries into finite chunks for processing (such as sum). EnOS Streaming system supports tumbling window and sliding window.

### Tumbling Window
Tumbling windows have a fixed size and do not overlap. Data belonging to a window will be aggregated with the specified method. For example, if you specify a tumbling window with a size of 5 minutes, the current window will be evaluated, and a new window will be started every five minutes. See the example in the following figure.

.. image:: media/window_type.png
   :width: 780px

**Event time**

In general, event time (time when an event occurs) is used in AI data aggregation. More precisely, every event has a corresponding timestamp, and the timestamp is part of the data record. Event time is actually a timestamp. When event time is used to define time windows, the streaming system can deal with disorderly time flow and variable event time deviation, and compute meaningful results based on the actual time of events.

**Window latency**

Generally, when a window closes, the data processing in the window should have been completed, and a new window will be started. However, device data transfer might be delayed because of various factors like device failure and transmission efficiency. Latency setting is introduced to specify the extended validity time of the window after it closes. Late data arriving within the allowed lateness will be added to the window and computed again. Late data arriving after the allowed lateness will be dropped. In the following example, window size is 5 minutes, and window2 contains data falling in the time range of 11:00 - 11:05. If latency is not enabled, window2 will be closed at 11:05, and data1 and data2 that are arriving late will be dropped. If a latency of 5 minutes is enabled, data1 will be added to window2 for computing again, but data2 will be dropped.

.. image:: media/latency_setting.png
   :width: 780px

**Processing strategy**

EnOS integrates asset templates to normalize all asset input data, so the streaming system can apply the same processing strategy for asset data of the same model. Each processing strategy defines the input point, output point, threshold, interpolation, window size, and aggregation.

 + **Input point**: The model point that provides input data to be processed.

   > AI data aggregation template can process measure point data of AI type only.

 + **Output point**: After the input data is processed, the processed result is transferred to the output point, and an output record is generated. The timestamp of the output record is the start time of the time window.

   > 1. The output point must be a measure point of AI type.
   > 2. Ensure that the input point and output point belong to the same model.
   > 3. Avoid designing loop pipelines in the Streaming system, like a -> b -> c -> a.

 + **Threshold**: Before processing, data are filtered by the specified threshold. Data exceeding the threshold will be processed by interpolation algorithm.

   > If the data of the input point is not raw data and the interpolation strategy is "Abandon", the specified threshold will not take effect.

 + **Interpolation**: Interpolation algorithm that is used to revise the input data. Currently, interpolation strategy supports abandon only. Data that exceed the threshold will not be included in the processing.

 + **Window size**: Specifies the amount of data to be computed in a single window.

   > In sequential aggregation pipelines like "a -> b -> c", the window size of  pipeline "b -> c" should be the same as that of pipeline "a -> b" because pipeline "a -> b" will generate an intermediate output. 

 + **Aggregation**: The function that is selected to compute data in the window. EnOS Streaming System currently supports functions like max, min, avg, sum, and cnt.

   -  max: Compare all valid record values in the time window and output the maximum value.
   -  min: Compare all valid record values in the time window and output the minimum value.
   -  avg: Calculate and output the average value of all valid record values in the time window.
   -  sum: Calculate and output the sum value of all valid record values in the time window.
   -  cnt: Count all valid record values in the time window and output the total number.

**Processing strategy management**

You can perform the following general operations for processing strategies:

- **Add**: Click **New processing strategy** to add a new entry of processing strategy. The pipeline can be saved only when all configuration is completed.
- **Copy**: Click the **Copy** icon to copy an existing strategy and configure a new one quickly.
- **Edit**: Each processing strategy can be edited if needed.
- **Delete**: Click the **Delete** icon to remove a processing strategy if needed.
