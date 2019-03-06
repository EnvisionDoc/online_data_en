# AI Data Aggregation Template
AI (analogy input)  data consists of physical parameters like temperature, pressure, flow, etc. The values of these parameters are collected by various sensors, converted to electric signals by a transmitter, and transferred to the analogy input of the controller.

EnOS Streaming Processing Engine provides a unified AI data aggregation template to process the AI type data ingested from a measure point and assign the processed data to another measure point defined for the same device, thus enabling developers to process real-time AI data easily and quickly.  

## Features
- Supporting event-time-based AI data aggregation.

- Providing rich aggregation algorithm, including `max`, `min`, `avg`, `sum`, and `cnt`.

- Supporting time window latency. When window latency is set, an intermediate output will be generated (the computed output value of the current window when the next window starts) . The intermediate output will be saved in TSDB but will not be archived.

- Supporting various threshold ranges. Input data that exceeds the threshold will be processed by the interpolation algorithm.

## Processing Stream Configuration
The configuration method of the AI data processing stream depends on the type of selected time window. Time window is a useful data processing mechanism of streaming computing. Windowing is simply the notion of taking a data source and chopping it up along temporal boundaries into finite chunks for processing (such as sum). EnOS Streaming Processing Engine supports tumbling window.

### Processing strategy

EnOS integrates asset templates to normalize all asset input data, so the stream processing engine can apply the same processing strategy for asset data of the same model. Each processing strategy defines the input point, output point, threshold, interpolation, window size, and aggregation.

 + **Input point**: The model point that provides input data to be processed.

   > AI data aggregation template can process measure point data of AI type only.

 + **Output point**: After the input data is processed, the processed result is transferred to the output point, and an output record is generated. The timestamp of the output record is the start time of the time window.

   > 1. The output point must be a measure point of AI type.
   > 2. Ensure that the input point and output point belong to the same model.
   > 3. Avoid designing loop streams in the stream, like a -> b -> c -> a.

 + **Threshold**: Before processing, the input data is filtered by the specified threshold. Data exceeding the threshold will be processed by interpolation algorithm.

   > If the data of the input point is not raw data and the interpolation strategy is "Ignore", the specified threshold will not take effect.

 + **Interpolation**: Interpolation algorithm that is used to revise the input data. Currently, interpolation strategy supports "Ignore" only. Data that exceeds the threshold will not be included in the processing.

 + **Window size**: Specifies the amount of data to be computed in a single window.

   > In sequential aggregation streams like "a -> b -> c", the window size of stream "b -> c" should be the same as that of stream "a -> b" because stream "a -> b" will generate an intermediate output.

 + **Aggregation**: The function that is selected to compute data in the window. EnOS Streaming Processing Engine currently supports functions like `max`, `min`, `avg`, `sum`, and `cnt`.

   -  `max`: Compare all valid record values in the time window and output the maximum value.
   -  `min`: Compare all valid record values in the time window and output the minimum value.
   -  `avg`: Calculate and output the average value of all valid record values in the time window.
   -  `sum`: Calculate and output the sum value of all valid record values in the time window.
   -  `cnt`: Count all valid record values in the time window and output the total number.

### Processing strategy management

You can perform the following general operations for processing strategies:

- **Add**: Click **New Strategy** to add a new entry of processing strategy. The stream can be saved only when all configuration is completed.
- **Copy**: Click the **Copy** icon to copy an existing strategy and configure a new one quickly.
- **Edit**: Each processing strategy can be edited if needed.
- **Delete**: Click the **Delete** icon to remove a processing strategy if needed.
