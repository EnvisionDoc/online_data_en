# Getting started

## Service request

For a stream computing service, it is necessary to provide corresponding appropriate App Id in the compute package and create an application in the "Application List" to obtain the App Id

## Preparation for development

Java version is required to be 1.6 or 1.7
* Currently, the stream computing service only provides Java SDK
First, you need to introduce the followingWrite the processing package into the pom .xml for the project:

``` xml
    <dependency>
      <groupId>com.envisioniot</groupId>
      <artifactId>enos-calculate</artifactId>
      <version>1.2.2</version>
      <scope>provided</scope>
    </dependency>
```
* The stream computing service provides Log SDK as follows
``` xml
    <dependency>
      <groupId>com.envisioniot</groupId>
      <artifactId>enos-logsdk-logger</artifactId>
      <version>1.2</version>
    </dependency>
```

In addition, user's computation package is loaded via Java's [ServiceLoader](https://docs.oracle.com/javase/7/docs/api/java/util/ServiceLoader.html). This approach requires the programmer writing the package to create a new file named after the service interface under the META-INFF / services folder, the content of which is the specific service implementation

### Sample 1 -- Device-level computation
``` java
package com.envision.energy.streaming.app;
import com.envision.energy.sdk.eos.calculate.CalculateDevice;
import com.envision.energy.sdk.eos.calculate.data.DeviceCal;
import com.envision.energy.sdk.eos.calculate.data.PointCal;
import com.envision.energy.sdk.eos.calculate.data.VirtualPoint;
import com.envision.logsdk.logger.EnvisionLogger;

import java.util.ArrayList;
import java.util.List;

public class DeviceCalImpl implements CalculateDevice {
    private static final long serialVersionUID = -3260058863967318463L;
    private final EnvisionLogger logger = EnvisionLogger.getLogger("EOSStreamingLogger");

    @Override
    public List<VirtualPoint> calculate(final DeviceCal device) {
        List<VirtualPoint> pointList = new ArrayList<>();
        try {
            logger.info("start device cal..." + device.getDeviceID());
            PointCal point_1 = device.getPoint("vol.4");
            PointCal point_2 = device.getPoint("vol.5");
            if (point_1 == null || point_2 == null) {
                return pointList;
            }
            Double val_a = Double.valueOf(point_1.getValue());
            Double val_b = Double.valueOf(point_2.getValue());

            String pointID = "newPoint-1";
            Double val = val_a + val_b;
            long timeStamp = point_1.getTimestamp();
            String deviceID = device.getDeviceID();
            VirtualPoint myPoint = new VirtualPoint(pointID, String.valueOf(val), timeStamp);
            pointList.add(myPoint);
        } catch (Exception e) {
            logger.error("", e);
        }
        return pointList;
    }
}
```

### Sample 2 -- Site-level computation
- Please prepare the development by refering to the sample 1;
- The code of SDK is below:
``` java
package com.envision.energy.streaming.app;
import com.envision.energy.sdk.eos.calculate.CalculateSite;
import com.envision.energy.sdk.eos.calculate.data.SiteCal;
import com.envision.energy.sdk.eos.calculate.data.VirtualPoint;
import com.envision.logsdk.logger.EnvisionLogger;

import java.util.ArrayList;
import java.util.List;

public class SiteCalImpl implements CalculateSite {
    private static final long serialVersionUID = -2347134131478433813L;
    private final EnvisionLogger logger = EnvisionLogger.getLogger("EOSStreamingLogger");

    @Override
    public List<VirtualPoint> calculate(final SiteCal site) {
        List<VirtualPoint> pointList = new ArrayList<>();

        try {
            logger.info("start site cal..." + site.getProjectID());
            String calPointID = "vol.4";
            Double val_1 = Double.valueOf(site.getDeviceCal("1")
                    .getPoint(calPointID).getValue());
            Double val_2 = Double.valueOf(site.getDeviceCal("2")
                    .getPoint(calPointID).getValue());
            String pointID = "SiteCalImpl-calPoint-1";
            Double val = val_1 + val_2 + 10000.0;
            long timeStamp =
                    site.getDeviceCal("1").getPoint(calPointID).getTimestamp();
            String deviceID =
                    site.getDeviceCal("1").getPoint(calPointID).getDeviceID();
            VirtualPoint myPoint =
                    new VirtualPoint(pointID, String.valueOf(val), timeStamp);
            pointList.add(myPoint);
        } catch (Exception e) {
            logger.error("", e);
        }
        return pointList;
    }
}
```

## Collection and query of stream computing logs

This document provides an example of how to apply for a database container and add data to a database from scratch, with most settings using either default or sample values. For more detailed configuration information, please refer to the following sections.

### Prerequisites
- EnOS system account
- A device has been connected to the EnOS system
- A computer in which development software has been installed,

### Steps
- Create an APP and perform asset authorization
- Download the stream computing SDK and log SDK.
- Write the processing logic for stream computing tasks and print logs
- Deploy stream computing packets
- Query the logs of stream computing tasks

#### Step 1:  Create an APP
- Log in to the EnOS system, and enter the APP list to create an APP

#### Step 2:  Download the stream computing SDK and log SDK.
- Log into the SDK center and download the stream computing SDK and log SDK

#### Step 3:  Write the processing logic for stream computing tasks and print logs
- The example code is as follows
```java
package com.envision.energy.stream.app.demo;

import com.envision.energy.sdk.eos.calculate.CalculateDeviceWithDataService;
import com.envision.energy.sdk.eos.calculate.data.DeviceCal;
import com.envision.energy.sdk.eos.calculate.data.IEosDataService;
import com.envision.energy.sdk.eos.calculate.data.PointCal;
import com.envision.energy.sdk.eos.calculate.data.VirtualPoint;
import com.envision.logsdk.logger.EnvisionLogger;

import java.util.ArrayList;
import java.util.List;

/**
 * Created by changyi.yuan on 2017/7/15.
 */
public class DeviceSampelCal implements CalculateDeviceWithDataService {
    //To use the log debugging function, please use EnvisionLogger and the parameter must be "EOSStreamingLogger"
    private final EnvisionLogger logger = EnvisionLogger.getLogger("EOSStreamingLogger");

    private final static String TEST_POINT = "DEMO_TEST_DEVICE_POINT";

    @Override
    public List<VirtualPoint> calculate(final DeviceCal device, IEosDataService handler) {
        List<VirtualPoint> pointList = new ArrayList<>();

        //Here, please use try / catch to facilitate troubleshooting if something goes wrong
        try {
            logger.info("start device cal..." + device.getDeviceID());

            PointCal pointA = device.getPoint("pointA");
            PointCal pointB = device.getPoint("pointB");

            VirtualPoint sumResultPoint = sum(pointA, pointB);
            pointList.add(sumResultPoint);
        } catch (Exception e) {
            logger.error("", e);
        }

        return pointList;
    }

    public VirtualPoint sum(PointCal pointA, PointCal pointB) {
        double valueA = pointA == null ? 0 : Double.valueOf(pointA.getValue());
        double valueB = pointB == null ? 0 : Double.valueOf(pointB.getValue());
        double result = valueA + valueB;
        VirtualPoint testPoint = new VirtualPoint(TEST_POINT, String.valueOf(result), System.currentTimeMillis());
        return testPoint;
    }

}

```

#### Step 4:  Deploy stream computing packets
- Log in to the EnOS system, enter the stream computing service, upload streaming computing packets and deploy them

#### Step 5:  Query the logs of stream computing tasks
- Enter the log management module
- Select the target APP and time range, enter keywords to make queries
