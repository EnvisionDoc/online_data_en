# Developer guide

EnOS System sends the data acquired to Kafka in the cloud. The stream computing engine processes the data received in real time and outputs it to the downstream sql database, real-time database and message queue according to certain strategies. Users can access real-time data through the Restful interface provided by EnOS API.

### Programming guide

- Introduction to common computational scenarios
    Some commonly used computational scenarios are listed below for reference purposes
    + Computation of device state
      In normal business scenarios, it is necessary to obtain some state parameters of a device for confirming whether the device works normally or not. When the original acquisition point may not have a corresponding acquisition point, or when the business scenario is relatively complex and the values of multiple acquisition points need to be considered comprehensively, programming can be considered at this time
    + Aggregation and computation of site information
- Notes
    + All types in the computation package must be serializable, otherwise the computation package will fail to be loaded
    + The stream computing framework will maintain the state of devices and sites internally and update it in real time based on the acquired data. The state update includes the update of the latest acquisition point values and the update of device connection state, and the specific update frequency depends on the acquisition and upload frequency of acquisition side
    + If a device is not connected, it means that the acquisition point of the device will not be updated
    + At present, the frequency of device-level computing and site-level computing is 5 seconds, that is, even if the user's data is refreshed every second, the computation package is always triggered once every 5 seconds. In future version, a customizable computing frequency will be provided
    + Computation is triggered only if the computation package has appropriate data access rights

### Problem diagnosis

After the user finishes writing Java computing program codes, he or she needs to put them into a jar package and submit to corresponding environment through the job server. The computation results of the user can be checked and verified by checking corresponding computation points through the EnOS API interface. The latest real-time results can be queried directly through API /domainService/getmdmidspoints.

For example, the following request is to query the real-time power generation of a wind turbine:
[https://eos.envisioncn.com/eeop/domainService/getmdmidspoints?appKey=your_appkey&sign=your_sign&token=your_token&mdmids=1782244a86890000&points=WWPP.APProduction](https://eos.envisioncn.com/eeop/domainService/getmdmidspoints?appKey=your_appkey&sign=your_sign&token=your_token&mdmids=1782244a86890000&points=WWPP.APProduction)


<body>
<table border="1" cellspacing="0" cellpadding="0" width="798">
  <tr>
    <td><strong>Parameter Name</strong></td>
    <td><strong>Parameter   Value</strong></td>
    <td><strong>Description</strong></td>
  </tr>
  <tr>
    <td>appKey</td>
    <td valign="top">your_appkey</td>
    <td valign="top">User-assigned   appId</td>
  </tr>
  <tr>
    <td>sign</td>
    <td valign="top">your_sign</td>
    <td valign="top">User-assigned   sign</td>
  </tr>
  <tr>
    <td>token</td>
    <td valign="top">your_token</td>
    <td valign="top">User-assigned   token</td>
  </tr>
  <tr>
    <td>mdmids</td>
    <td valign="top">1782244a8689000</td>
    <td valign="top">Device id</td>
  </tr>
  <tr>
    <td>points</td>
    <td valign="top">WWPP.APProduction</td>
    <td valign="top">List of points   to be queried</td>
  </tr>
</table>
</body>

Return value:
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

## API guide

### Device-level computing interface

The following two interfaces are provided for device -level computing: the first interface provides only the basic device object DeviceCal, which can satisfy complicated computing requirements between device points; the second interface provide a service broker for accessing master data / dimension in addition to device objects, and can be implemented when some master data attributes or dimension data need to be introduced in some scenarios
``` java
    public interface CalculateDevice extends Serializable {
        List<VirtualPoint> calculate(final DeviceCal device);
    }

    public interface CalculateDeviceWithDataService extends Serializable {
        List<VirtualPoint> calculate(final DeviceCal device,
            final IEosDataService dataService);
    }
```
### Site-level computing interface


A site-level computing interface provides the SiteCal object. In addition, as with the device-level computing interface, an interface with a master data / dimension data service broker is provided as well.
``` java
    public interface CalculateSite extends Serializable {
        List<VirtualPoint> calculate(final SiteCal site);
    }

    public interface CalculateSiteWithDataService extends Serializable {
        List<VirtualPoint> calculate(final SiteCal site,
            final IEosDataService handler);
    }
```

## SDK reference

- AbstractInstance Class
    + Package: com.envision.energy.sdk.eos.calculate.data.service
    + Class description: Abstract type of master data objects
    + Member attribute
    + Member method
- AttributeKey Class
    + Package: com.envision.energy.sdk.eos.calculate.data.service
    + Class description: Enumeration of master data attributes
    + Member attribute
    + Member method
- CalculateDevice
    + Package: com.envision.energy.sdk.eos.calculate
    + Class description: Device-level computing interface
    + Member attribute
    + Member method
        * List<VirtualPoint> calculate(final DeviceCal device)
- CalculateDeviceWithDataService
    + Package: com.envision.energy.sdk.eos.calculate
    + Class description: A device-level computing interface that provides access to master data / dimension data
    + Member attribute
    + Member method
        * List<VirtualPoint> calculate(final DeviceCal device, final IEosDataService dataService)
- CalculateSite
    + Package: com.envision.energy.sdk.eos.calculate
    + Class description: A site-level computing interface
    + Member attribute
    + Member method
        * List<VirtualPoint> calculate(final SiteCal site)
- CalculateSiteWithDateService
    + Package: com.envision.energy.sdk.eos.calculate
    + Class description: A site-level computing interface that provides access to master data / dimension data
    + Member attribute
    + Member method
        * List<VirtualPoint> calculate(final SiteCal site, final IEosDataService handler)
- CIMAttributeSpec
    + Package: com.envision.energy.sdk.eos.calculate.data.service
    + Class description: Specifications of master data attributes
    + Member attributes: key, valueType, valueTypeArgs, displayId
    + Member method
- CIMAttributeSpecAndValue
    + Package: com.envision.energy.sdk.eos.calculate.data.service
    + Class description: Instantiation type of master data attribute specifications and values

    + Member attribute: value
    + Member method
- CIMAttributeValues
    + Package: com.envision.energy.sdk.eos.calculate.data.service
    + Class description: Type of master data attribute values
    + Member attribute
    + Member method
- DeviceCal
    + Package: com.envision.energy.sdk.eos.calculate.data
    + Class description: Type of device data
    + Member attribute
    + Member method
- IEosDataService
    + Package: com.envision.energy.sdk.eos.calculate.data
    + Class description: Service broker interface of master data / dimension data
    + Member attribute
    + Member method

        <body>
<table border="1" cellspacing="0" cellpadding="0" width="741">
  <tr>
    <td nowrap valign="top"><a name="OLE_LINK4"></ a><a name="OLE_LINK3"><strong>Method signature</strong></ a></td>
    <td nowrap valign="top"><strong>Return value</strong></td>
    <td nowrap valign="top"><strong>Description</strong></td>
  </tr>
  <tr>
    <td nowrap valign="top">getInstanceAttributeValue(String uuid, String attrKey)</td>
    <td valign="top">String</td>
    <td valign="top">Get a specific master data attribute:</td>
  </tr>
  <tr>
    <td nowrap valign="top">getInstanceAttributeValues(String uuid, List&nbsp;attrKeys)</td>
    <td valign="top">Map&lt;String,String&gt;</td>
    <td valign="top">Get master data attributes in the input list:</td>
  </tr>
  <tr>
    <td nowrap valign="top">getInstanceAttributeValues(String uuid) </td>
    <td valign="top">Map&lt;String,String&gt; </td>
    <td valign="top">Get all master data attributes </td>
  </tr>
  <tr>
    <td nowrap valign="top">getSpaceIdByUUID(String uuid)</td>
    <td valign="top">String </td>
    <td valign="top">Gets the SpaceId for corresponding id </td>
  </tr>
  <tr>
    <td nowrap valign="top">findByKey(String mdmID, String tableName,   String key)</td>
    <td valign="top">Map&lt;String, Object&gt; </td>
    <td valign="top">Get the dimension data </td>
  </tr>
  <tr>
    <td nowrap valign="top">findByKey(String mdmID, String tableName, List&nbsp;keys) </td>
    <td valign="top">Map&lt;String,   Object&gt; </td>
    <td valign="top">Get   the dimension data </td>
  </tr>
  <tr>
    <td nowrap valign="top">isPointMapped(String   uuid, String pointName) </td>
    <td valign="top">boolean</td>
    <td valign="top">Determine   whether the provided point is configured with a map</td>
  </tr>
</table>
</body>

- ImplementInstance
    + Package: com.envision.energy.sdk.eos.calculate.data.service
    + Class description: Type of master data
    + Member attribute
    + Member method
- OrgnizationInstance
    + Package: com.envision.energy.sdk.eos.calculate.data.service
    + Class description: Type of master data related to organizational structure
    + Member attribute
    + Member method
- PointCal
    + Package: com.envision.energy.sdk.eos.calculate.data
    + Class description: Type of point data
    + Member attribute
    + Member method
- ProviderInfo
    + Package: com.envision.energy.sdk.eos.calculate.data.annotation
    + Class description: Note on the application to which the computation package ID belongs
    + Member attribute
    + Member method
- SiteCal
    + Package: com.envision.energy.sdk.eos.calculate.data
    + Class description: Type of site data
    + Member attribute
    + Member method
- VirtualPoint
    + Package: com.envision.energy.sdk.eos.calculate.data
    + Class description: Data type of computation points
    + Member attribute
    + Member method

## Log query
Streaming log management provides developers with a variety of log query functions to help them quickly locate problems

### Log printing
The stream computing runtime system collects logs printed by users in stream computing tasks.
The example code for log writing is as follows:
```java

public class DeviceSampelCal implements CalculateDeviceWithDataService {
    //To use the log debugging function, please use EnvisionLogger, and the parameter must be "EOSStreamingLogger"
    private final EnvisionLogger logger = EnvisionLogger.getLogger("EOSStreamingLogger");

    private final static String TEST_POINT = "DEMO_TEST_DEVICE_POINT";

    @Override
    public List<VirtualPoint> calculate(final DeviceCal device, IEosDataService handler) {
        List<VirtualPoint> pointList = new ArrayList<>();
        ...
}
```

### APP filtering
Streaming logs filter out different logs based on different APPs.

### Time range filtering
Log management supports time range filtering, which allows user to query logs for any time period

### Grammar query
1. The default keyword matching field is msg, and the grammar of other keyword fuzzy matching fields is field:value, which does not support simultaneous queries under multiple conditions;
2. In keyword search, you can use a string [key=value] to qualify search results, which is used to limit the search scope to key=value. Multiple key=value conditions must be separated by “;” (simultaneous action). For example: Xiao Er [surname = Wang; sex = male] means that the search contains records of “Xiao” and “Er”, and the surname recorded in the return result must be “Wang” and the sex must be “male”;
3. When a string is used to qualify the search results, you can add “!!” before key to keep the condition unchanged when viewing the context based on a particular result, such as: Xiao Er [!!Surname = Wang; sex = male). When viewing the context against a particular result, the surname must be “Wang”. PS: The conditions for fields timestamp and msg are ignored by default when viewing the context.

### View of context
When developers are interested in the context of a log, they may click the View Context button to view the context of the log. The default time interval of the context is the first 5s + the last 5s.

### Display of field settings
If too much content is display in a log, developers can set the fields to be displayed.
