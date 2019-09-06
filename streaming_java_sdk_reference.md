# SDK reference

## `AbstractInstance` Class

- Package: `com.envision.energy.sdk.eos.calculate.data.service`
- Class description: Abstract type of master data objects
- Attributes:  NA
- Methods: NA

## `AttributeKey` Class

- Package: `com.envision.energy.sdk.eos.calculate.data.service`
- Class description: Enumeration of master data attributes
- Attributes:  NA
- Methods: NA

## `CalculateDevice` class

- Package: `com.envision.energy.sdk.eos.calculate`
- Class description: Device-level computing interface
- Attributes:  NA
- Methods:
  * `List<VirtualPoint> calculate(final DeviceCal device)``

## `CalculateDeviceWithDataService` class

- Package: `com.envision.energy.sdk.eos.calculate`
- Class description: A device-level computing interface that provides access to master data / dimension data
- Attributes:  NA
- Methods:
  * `List<VirtualPoint> calculate(final DeviceCal device, final IEosDataService dataService)``

## `CalculateSite` class

- Package: com.envision.energy.sdk.eos.calculate
- Class description: A site-level computing interface
- Attributes:  NA
- Methods:
  * `List<VirtualPoint> calculate(final SiteCal site)``

## `CalculateSiteWithDateService` class

- Package: `com.envision.energy.sdk.eos.calculate`
- Class description: A site-level computing interface that provides access to master data / dimension data
- Attributes:  NA
- Methods:
  * `List<VirtualPoint> calculate(final SiteCal site, final IEosDataService handler)`

## `CIMAttributeSpec` class

- Package: `com.envision.energy.sdk.eos.calculate.data.service`
- Class description: Specifications of master data attributes
- Attributes:  NAs: `key`, `valueType`, `valueTypeArgs`, `displayId`
- Methods: NA

## `CIMAttributeSpecAndValue` class

- Package: `com.envision.energy.sdk.eos.calculate.data.service`
- Class description: Instantiation of master data attribute specifications and values
- Attributes: `value`
- Methods: NA

## `CIMAttributeValues` class

- Package: `com.envision.energy.sdk.eos.calculate.data.service`
- Class description: The master data attribute values
- Attributes:  NA
- Methods: NA

## `DeviceCal` class

- Package: `com.envision.energy.sdk.eos.calculate.data`
- Class description: The device data
- Attributes:  NA
- Methods: NA

## `IEosDataService` class

- Package: `com.envision.energy.sdk.eos.calculate.data`
- Class description: Service broker interface of master data and dimension data
- Attributes:  NA
- Methods: see the following table

.. list-table::
   :widths: auto

   * - Method signature
     - Return value
     - Description
   * - getInstanceAttributeValue(String uuid, String attrKey)
     - String
     - Gets a specific master data attribute
   * - getInstanceAttributeValues(String uuid, List attrKeys)
     - Map<String,String >
     - Gets master data attributes in the input list
   * - getInstanceAttributeValues(String uuid)
     - Map<String,String >
     - Gets all master data attributes
   * - getSpaceIdByUUID(String uuid)
     - String
     - Gets the SpaceId for corresponding id
   * - findByKey(String mdmID, String tableName, String key)
     - Map<String, Object>
     - Gets the dimension data
   * - findByKey(String mdmID, String tableName, List keys)
     - Map<String, Object>
     - Gets the dimension data
   * - isPointMapped(String uuid, String pointName)
     - boolean
     - Determines whether the provided point is configured with a map


## `ImplementInstance`

- Package: com.envision.energy.sdk.eos.calculate.data.service
- Class description: Master data
- Attributes:  NA
- Methods: NA

## `OrgnizationInstance` class

- Package: com.envision.energy.sdk.eos.calculate.data.service
- Class description: Master data related to the organizational structure
- Attributes:  NA
- Methods: NA

## `PointCal` class

- Package: com.envision.energy.sdk.eos.calculate.data
- Class description: Point data
- Attributes:  NA
- Methods: NA

## `ProviderInfo` class

- Package: com.envision.energy.sdk.eos.calculate.data.annotation
- Class description: Information about the application to which the computation package belongs
- Attributes:  NA
- Methods: NA

## `SiteCal` class

- Package: com.envision.energy.sdk.eos.calculate.data
- Class description: Site data
- Attributes:  NA
- Methods: NA

## `VirtualPoint` class

- Package: com.envision.energy.sdk.eos.calculate.data
- Class description: Calculation points
- Attributes:  NA
- Methods: NA

## Log query

Streaming log management provides a variety of log query functions to help developer locating problems quickly.

### Log printing

The stream computing runtime system can collect logs printed by users in the stream computing tasks.
The following code is an example for log writing:

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
