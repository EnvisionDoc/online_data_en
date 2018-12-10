## Monitoring an AI Pipeline

After the pipeline is released online, you can manage the pipeline through various operations on the **Pipeline Monitoring** page, such as previewing, starting, and stopping the pipeline, checking its running status, and viewing pipeline configuration.

- **Preview pipeline**: Before starting the pipeline, you can preview it. On the **Pipeline Monitoring** page, search for the pipeline by its ID or name, or filter pipelines with the pipeline owner. Click the pipeline name to open the pipeline details page, and then click the **Preview** button on the upper right corner. Complete the preview configuration and run the preview. The preview will intercept and process the latest real-time data, and the processed data will not be saved in the DB. With the preview results, you can debug and tune the pipeline. If the preview results meet the expectation, you can start the pipeline directly.

- **Start pipeline**: Click the **Start** button in the **Operation** column of the pipeline list to start the pipeline. Then the pipeline will run continuously in the Streaming system.

- **Stop pipeline**: To stop a running pipeline, click the **Stop** button in the **Operation** column.

- **Check running status**: Click the name of a running pipeline to view its running status, including its running result summary and error logs.

- **View configuration**: Click the **View Configuration** button in the **Operation** column to view the pipeline configuration.

- **Export configuration**: Click the **Export Configuration** button in the **Operation** column to export the pipeline configuration.

  

## Viewing pipeline running results

When the pipeline is started, it will process the asset data of the current organization by default. The processed data results for a specific asset can be viewed on the **Device Management** page.