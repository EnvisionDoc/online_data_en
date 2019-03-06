# Getting Started with Stream Computing Service

This topic illustrates how to use the stream computing service and print computation logs through the Java SDKs provides by EnOS.

## System Requirements

Java SDK version 1.6 or 1.7

## Before You Start

- Stream computing is based on the real-time data collected from devices that are connected to the EnOS cloud. For how to connect devices, see [EnOS Device connection](https://www.envisioniot.com/docs/device-connection/en/latest/deviceconnection_overview.html).

- Download the EnOS stream computing and log SDKs.

- To use the stream computing service, a computation package must carry a corresponding application key, which is obtained through the application management service. For more information, see [Getting started with application registration](https://www.envisioniot.com/docs/app-development/en/latest/app_mgmt/getting_started_app_management.html).


- Add the EnOS stream computing package into the `pom.xml` file for the project:

  ``` xml
      <dependency>
        <groupId>com.envisioniot</groupId>
        <artifactId>enos-calculate</artifactId>
        <version>1.2.2</version>
        <scope>provided</scope>
      </dependency>
  ```

- EnOS provides the log SDK. To use the log service, add the following dependency into the `pom.xml` file:
  ``` xml
      <dependency>
        <groupId>com.envisioniot</groupId>
        <artifactId>enos-logsdk-logger</artifactId>
        <version>1.2</version>
      </dependency>
  ```

- As the computation package is loaded via Java's [ServiceLoader](https://docs.oracle.com/javase/7/docs/api/java/util/ServiceLoader.html), you must create a new file under the `META-INFF/services` folder. This file should be named with the service API name, and the contents of the file is the Java code that implements the service as shown in the following section.

## Sample Codes

### Sample 1: Device-level Computation

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

### Sample 2: Site-level Computation

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

## Step 1:  Programming Java Code to Perform Stream Computing and Print Logs

You can use the following code as a reference:

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

## Step 2: Deploy the Stream Computing Package on EnOS

The finished Java codes need to be packed into a `jar` file and uploaded to EnOS for the computation to run.

1. Click **Streaming Service** from the left navigation panel of EnOS Console.

2. Click **Upload Steaming package** and provide the following settings in the **Upload streaming computing package** window:

   - Click **Upload**, browse to and select your stream computing package with a `.jar` file extension.
   - Select the application in the name of which to run stream computing.
   - Enter the file version.

## Step 3: Query the Logs of Stream Computing Jobs

1. Click **Monitoring and Logs > Log Management** from the left navigation panel of EnOS Console.

2. Select the target application and time range that you want to query logs for, and when needed, enter the keywords to return relevant logs.

For more information, see [Querying logs of stream computing jobs](querying_logs).
