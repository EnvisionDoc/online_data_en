# Querying stream computing logs

You can check and verify the computation results in the **Log Management** interface of EnOS Console. You can query the real-time data throught the rest API.

## Querying logs in the EnOS Console

1. Click **Monitoring and Logs > Log Management** from the left navigation panel of EnOS Console.

2. Select the target application and time range that you want to query logs for, and when needed, enter the keywords to return relevant logs.

   When you enter keywords to filter the results, the following conventions apply:

   - When not specified, the search engine searches the keyword in the `msg` field. To perform fuzzy search in other fields, the syntax is `field:value`.Multiple search conditions in one query is not supported.

   - You can use string `[key=value]` to limit the search scope in keyword searching. Multiple `key=value` conditions must be separated by `;`. For example: `Developer [surname = Smith; sex = male]` means that searching result includes "developer" and must have "Smith" as the surname and "male" as the sex.

   - When using string `[key=value]` to limit the search scope, you can add `!!` before `key` to keep the condition unchanged when viewing the context based on a particular result. For example: `Developer [!!Surname = Smith; sex = male)` means the surname must still be “Smith” when viewing the context against a particular result.

   .. note:: The conditions for fields `timestamp` and `msg` are ignored by default when viewing the context.

## Querying real-time data through rest API

You can query the latest real-time results directly through the rest API `/domainService/getmdmidspoints`.

For example, the following request is to query the real-time power generation of a wind turbine: https://eos.envisioncn.com/eeop/domainService/getmdmidspoints?appKey=your_appkey&sign=your_sign&token=your_token&mdmids=1782244a86890000&points=WWPP.APProduction

The following table describes the parameters in the query:


.. list-table::
   :widths: auto

   * - Parameter Name
     - Description
   * - appKey
     - User-assignedappId
   * - sign
     - User-assigned sign
   * - token
     - User-assigned token
   * - mdmids
     - Device id, in this request, the value 1782244a8689000.
   * - points
     - List of points to be queried. In this request, the value WWPP.APProduction.



The example request will return the following result:
``` json
{
  "result": {
 "15a93cf5-8ec0-4ad4-a6a8-dc0192004d5e": {
"points": {
  "WWPP.APProduction": {
 "value": "4379411.0"
  }
}
 }
  },
  "status": 0,
  "msg": "success"
}
```
