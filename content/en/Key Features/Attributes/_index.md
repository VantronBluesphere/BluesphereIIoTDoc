---
resources:
  - name: Attributes-Types-Server-Side-Attributes-Administration-UI-1
    src: "Attributes-Types-Server-Side-Attributes-Administration-UI-1.png"
    title: 
  - name: Attributes-Types-Server-Side-Attributes-Administration-UI-2
    src: "Attributes-Types-Server-Side-Attributes-Administration-UI-2.png"
    title: 
  - name: Attributes-Types-Shared-Attributes-Administration-UI-1
    src: "Attributes-Types-Shared-Attributes-Administration-UI-1.png"
    title: 
  - name: Attributes-Types-Shared-Attributes-Administration-UI-2
    src: "Attributes-Types-Shared-Attributes-Administration-UI-2.png"
    title: 
  - name: Attributes-Types-Client-Attributes-Administration-UI-1
    src: "Attributes-Types-Client-Attributes-Administration-UI-1.png"
    title: 
---

{{< toc >}}

## Overview

Our platform provides the ability to assign custom attributes to your entities and manage these attributes. Those attributes are stored in the database and may be used for data visualization and data processing.

Attributes are treated as key-value pairs. Flexibility and simplicity of the key-value format allow easy and seamless integration with almost any IoT device on the market. Key is always a string and is basically an attribute name, while the attribute value can be either string, boolean, double, integer or JSON. For example:

```json
{
 "firmwareVersion":"v2.3.1", 
 "booleanParameter":true, 
 "doubleParameter":42.0, 
 "longParameter":73, 
 "configuration": {
    "someNumber": 42,
    "someArray": [1,2,3],
    "someNestedObject": {"key": "value"}
 }
}
```

## Attribute names

As a platform user, you can define any attribute name. However, we recommend to use [camelCase](https://en.wikipedia.org/wiki/Camel_case). This make it easy to write custom JS functions for data processing and visualization.

## Attribute types

There are three types of attributes. Let’s review them with examples:

### Server-side attributes

This type of attribute is supported by almost any platform entity: Device, Asset, Customer, Tenant, User, etc. Server-side attributes are the ones that you may configure via Administration UI or REST API. The device firmware can’t access the server-side attribute.

Each device has five default server-side attributes:

- **active** - represents current device state, either true or false;
- **lastConnectTime** - represents the last time device was connected to platform, number of milliseconds since January 1, 1970, 00:00:00 GMT;
- **lastDisconnectTime** - represents the last time device was disconnected from platform, number of milliseconds since January 1, 1970, 00:00:00 GMT;
- **lastActivityTime** - represents the last time device pushed telemetry, attribute update, or RPC command, number of milliseconds since January 1, 1970, 00:00:00 GMT;
- **inactivityAlarmTime** - represents the last time inactivity event was triggered, number of milliseconds since January 1, 1970, 00:00:00 GMT.

Let’s assume you would like to build a building monitoring solution and review few examples:

1. The *latitude*, *longitude* and *address* are good examples of server-side attribute you may assign to assets that represent building or other real estate. You may use this attributes on the Map Widget in your dashboard to visualize location of the buildings.
2. The *floorPlanImage* may contain a URL to the image. You may use this attribute to visualize floor plan on the Image Map Widget.
3. The *maxTemperatureThreshold* and *temperatureAlarmEnabled* may be used to configure and enable/disable alarms for a certain device or asset.

#### Administration UI

- Select Devices. Click on the particular device row to open device details. Select "Attributes" tab. Choose "Server attributes" scope. Click "+" Icon.

  {{< img name="Attributes-Types-Server-Side-Attributes-Administration-UI-1" size="large" lazy=false >}} 

- Input new attribute name. Select attribute value type and input attribute value.

  {{< img name="Attributes-Types-Server-Side-Attributes-Administration-UI-2" size="large" lazy=false >}} 

- Sort using "Last update time" to quickly locate the newly created attribute.

  

### Shared attributes

This type of attributes is available only for Devices. It is similar to the Server-side attributes but has one important difference. The device firmware/application may request the value of the shared attribute(s) or subscribe to the updates of the attribute(s). The devices which communicate over MQTT or other bi-directional communication protocols may subscribe to attribute updates and receive notifications in real-time. The devices which communicate over HTTP or other request-response communication protocols may periodically request the value of shared attribute.

The most common use case of shared attributes is to store device settings. Let’s assume the same building monitoring solution and review few examples:

1. The *targetFirmwareVersion* attribute may be used to store the firmware version for particular Device.
2. The *maxTemperature* attribute may be used to automatically enable HVAC if it is too hot in the room.

The user may change the attribute via UI.

#### Administration UI

- Select Devices. Click on the particular device row to open device details. Select "Attributes" tab. Choose "Shared attributes" scope. Click "+" Icon.

  {{< img name="Attributes-Types-Shared-Attributes-Administration-UI-1" size="large" lazy=false >}} 

- Input new attribute name. Select attribute value type and input attribute value.

  {{< img name="Attributes-Types-Shared-Attributes-Administration-UI-2" size="large" lazy=false >}} 

- Sort using "Last update time" to quickly locate the newly created attribute.

### Client-side attributes

This type of attributes is available only for Devices. It is used to report various semi-static data from Device (Client) to Platform (Server). It is similar to shared attributes, but has one important difference. The device firmware/application may send the value of the attributes from device to the platform.

The most common use case of client attributes is to report device state. Let’s assume the same building monitoring solution and review few examples:

1. The *currentFirmwareVersion* attribute may be used to report the installed firmware/application version for the device to the platform.
2. The *currentConfiguration* attribute may be used to report current firmware/application configuration to the platform.
3. The *currentState* may be used to persist and restore current firmware/application state via network, if device does not have the persistent storage.

The user and server-side applications may browser the client-side attributes via UI but they are not able to change them. Basically, the value of the client-side attribute is read-only for the UI.

{{< img name="Attributes-Types-Client-Attributes-Administration-UI-1" size="large" lazy=false >}} 

  