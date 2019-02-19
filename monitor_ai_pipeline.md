# Monitoring an AI pipeline

After the pipeline is released online, you can manage the pipeline through various operations on the **Pipeline Operation** page, such as previewing, starting, and stopping the pipeline, checking its running status, and viewing pipeline configuration.

 - **Preview pipeline**: Before starting the pipeline, you can preview it. On the **Pipeline Operation** page, search for the pipeline by its ID or name. Click the pipeline name to open the pipeline details page, and then click the **Preview** button on the upper right corner. Complete the preview configuration and run the preview. The preview will intercept and process the latest real-time data, and the processed data will not be saved in the DB. With the preview results, you can debug and tune the pipeline. If the preview results meet the expectation, you can start the pipeline directly.

 - **Start pipeline**: Click the **Start** icon in the **Operations** column of the pipeline list to start the pipeline. Then the pipeline will run continuously in the Streaming system.

 - **Stop pipeline**: To stop a running pipeline, click the **Stop** icon in the **Operations** column.

 - **Check running status**: Click the name of a running pipeline to view its running status, including its running result summary and error logs.

 - **View configuration**: Click the **View** icon in the **Operations** column to view the pipeline configuration details.

 - **Export configuration**: Click the **Export** icon in the **Operations** column to export the pipeline configuration file.


 ## Viewing pipeline running results

When the pipeline is started, it will process the asset data of the current organization by default. The processed data results for a specific asset can be viewed on the **Device Management** page.