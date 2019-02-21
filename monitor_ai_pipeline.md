# Monitoring the Data Aggregation Stream

After the data aggregation stream is released online, you can manage the stream through various operations on the **Stream Operation** page, such as previewing, starting, and stopping the stream, checking its running status, and viewing stream configuration.

 - **Preview stream**: Before starting the stream, you can preview it. On the **Stream Operation** page, search for the stream by its ID or name. Click the stream name to open the stream details page, and then click the **Preview** button on the upper right corner. Complete the preview configuration and run the preview. The preview will intercept and process the latest real-time data, and the processed data will not be saved in the DB. With the preview results, you can debug and tune the stream. If the preview results meet the expectation, you can start the stream directly.

 - **Start stream**: Click the **Start** icon in the **Operations** column of the stream list to start the stream. Then the stream will run continuously in the Streaming system.

 - **Stop stream**: To stop a running stream, click the **Stop** icon in the **Operations** column.

 - **Check running status**: Click the name of a running stream to view its running status, including its running result summary and error logs.

 - **View configuration**: Click the **View** icon in the **Operations** column to view the stream configuration details.

 - **Export configuration**: Click the **Export** icon in the **Operations** column to export the stream configuration file.


 ## Viewing stream running results

When the stream is started, it will process the asset data of the current organization by default. The processed data results for a specific asset can be viewed on the **Device Management** page.
