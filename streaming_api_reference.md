# API reference

## Device-level computing API

The following APIs are provided for device-level computing:

- `CalculateDevice`

  The `CalculateDevice` API provides access to basic device objects and can satisfy complex computing requirements between device points.

  ``` java
      public interface CalculateDevice extends Serializable {
          List<VirtualPoint> calculate(final DeviceCal device);
      }
  ```

- `CalculateDeviceWithDataService`

  The `CalculateDeviceWithDataService` API provides a service broker for accessing master data and dimension data in addition to the device objects.

  ``` java
      public interface CalculateDeviceWithDataService extends Serializable {
          List<VirtualPoint> calculate(final DeviceCal device,
              final IEosDataService dataService);
      }
```

## Site-level computing API

Similar to device-level APIs, EnOS provides the following types of site-level computing APIs:

- `CalculateSite`

  The `CalculateSite` API provides access to the basic site objects.

  ``` java
        public interface CalculateSite extends Serializable {
            List<VirtualPoint> calculate(final SiteCal site);
        }
  ```
- `CalculateSiteWithDataService`

  The `CalculateSiteWithDataService` API provides access to the master data and dimension data through a service broker in addition to CalculateSite interface.

  ```java
        public interface CalculateSiteWithDataService extends Serializable {
            List<VirtualPoint> calculate(final SiteCal site,
                final IEosDataService handler);
        }
    ```
