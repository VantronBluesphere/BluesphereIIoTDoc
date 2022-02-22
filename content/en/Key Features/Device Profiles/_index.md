---
resources:
  - name: Add-Device-Profile-Rule-Chain
    src: "Add-Device-Profile-Rule-Chain.png"
    title: 
  - name: Rule-Chains-list
    src: "Rule-Chains-list.png"
    title: 
  - name: Rule-Chain-Detail
    src: "Rule-Chain-Detail.png"
    title: 
  - name: Add-Device-Profile-MQTT-transport-type-protobuf
    src: "Add-Device-Profile-MQTT-transport-type-protobuf.png"
    title: 
  - name: Device-Profiles-Alarm-Rule-Condition-Add-Key-Filter-1
    src: "Device-Profiles-Alarm-Rule-Condition-Add-Key-Filter-1.png"
    title: 
  - name: Device-Profiles-Alarm-Rule-Condition-Add-Key-Filter-2
    src: "Device-Profiles-Alarm-Rule-Condition-Add-Key-Filter-2.png"
    title: 
  - name: Device-Profiles-Alarm-Rule-Condition-Add-Key-Filter-3
    src: "Device-Profiles-Alarm-Rule-Condition-Add-Key-Filter-3.png"
    title: 
  - name: Upload-Temperature-Value
    src: "Upload-Temperature-Value.png"
    title: 
  - name: Simple-Alarm-Conditions
    src: "Simple-Alarm-Conditions.png"
    title: 
  - name: Device-Profiles-Edit-Alarm-Rule-Condition-Duration
    src: "Device-Profiles-Edit-Alarm-Rule-Condition-Duration.png"
    title: 
  - name: Upload-Temperature-Value-Time-Interval-1-Minute
    src: "Upload-Temperature-Value-Time-Interval-1-Minute.png"
    title: 
  - name: Alarm-Condition-With-A-Duration
    src: "Alarm-Condition-With-A-Duration.png"
    title: 
  - name: Upload-Temperature-Value-Three-Time
    src: "Upload-Temperature-Value-Three-Time.png"
    title: 
  - name: Repeating-Alarm-Condition
    src: "Repeating-Alarm-Condition.png"
    title: 
  - name: Device-Profiles-Add-Clear-Condition
    src: "Device-Profiles-Add-Clear-Condition.png"
    title: 
  - name: Device-Profiles-Add-Clear-Condition-Add-Key-Filter
    src: "Device-Profiles-Add-Clear-Condition-Add-Key-Filter.png"
    title: 
  - name: Upload-Temperature-Value-On-Example-4
    src: "Upload-Temperature-Value-On-Example-4.png"
    title: 
  - name: Example4-Alarms-Event-1
    src: "Example4-Alarms-Event-1.png"
    title: 
  - name: Example4-Upload-Temperature-Value-2
    src: "Example4-Upload-Temperature-Value-2.png"
    title: 
  - name: Example4-Alarms-Event-2
    src: "Example4-Alarms-Event-2.png"
    title: 
  - name: Example5-Edit-Alarm-Rule-1
    src: "Example5-Edit-Alarm-Rule-1.png"
    title: 
  - name: Example5-Edit-Alarm-Rule-2
    src: "Example5-Edit-Alarm-Rule-2.png"
    title: 
  - name: Example6-Edit-Alarm-Rule-1
    src: "Example6-Edit-Alarm-Rule-1.png"
    title: 
  - name: Example6-Edit-Alarm-Rule-2
    src: "Example6-Edit-Alarm-Rule-2.png"
    title: 
  - name: Example6-Edit-Alarm-Rule-3
    src: "Example6-Edit-Alarm-Rule-3.png"
    title: 
  - name: Example6-Edit-Alarm-Rule-4
    src: "Example6-Edit-Alarm-Rule-4.png"
    title: 
  - name: Example6-Add-Server-Attributes-1
    src: "Example6-Add-Server-Attributes-1.png"
    title: 
  - name: Example6-Upload-Temperature-Value-1
    src: "Example6-Upload-Temperature-Value-1.png"
    title: 
  - name: Example6-Alarms-Event-1
    src: "Example6-Alarms-Event-1.png"
    title: 
  - name: Example6-Clean-Alarms-Event-1
    src: "Example6-Clean-Alarms-Event-1.png"
    title: 
  - name: Example6-Edit-Server-Attributes-1
    src: "Example6-Edit-Server-Attributes-1.png"
    title: 
  - name: Device-Profile-Rule-Node-1
    src: "Device-Profile-Rule-Node-1.png"
    title: 
  - name: Device-Profile-Rule-Node-2
    src: "Device-Profile-Rule-Node-2.png"
    title: 
  - name: Notifications-About-Alarms-1
    src: "Notifications-About-Alarms-1.png"
    title: 

---

{{< toc >}}

## Overview

The Tenant administrator is able to configure common settings for multiple devices using Device Profiles. Each Device has one and only profile at a single point in time.

There has a default device profile which you can choice to configure the device when you creating the device, if you haven't created other device profiles.

Let’s take a look at the settings available in the device profile one by one.



## Device Profile settings

### Rule Chain

By default, the [Root Rule Chain](https://thingsboard.io/docs/pe/user-guide/rule-engine-2-0/overview/#rule-chain) processes all incoming and outcoming messages and events for any device. And you can specify a custom root Rule Chain for your devices here. The new Rule Chain will receive all telemetry, device activity(Active/Inactive), device lifecycle(Created/Updated/Deleted) events, and send messages to device. This setting is available in the Device Profile wizard and in the Device Profile details. 

You can navigate to "Management->Rule chains" to create your custom root Rule Chain or do it like below:

{{< img name="Add-Device-Profile-Rule-Chain" size="large" lazy=false >}}

and navigate to "Management->Rule chains", you will see a new rule chain is created:

{{< img name="Rule-Chains-list" size="large" lazy=false >}}

{{< img name="Rule-Chain-Detail" size="large" lazy=false >}} 

### Queue Name

The queue connects your devices and rule engine like a pipeline. All incoming messages from the devices will be transmitted to the rule engine through the queue. For multiple use cases, you might want to use different queues for different devices. For example, you might want to isolate data processing for Fire Alarm/Smoke Detector sensors and other devices. This way, even if your system has a peak load produced by millions of water meters, whenever the Fire Alarm is reported, it will be processed without delay.

There are **three types** of queues to choice from:

- **Main**: Default queue, all messages from devices are submitted to the rule chains in the order they arrive. And the messages processed by the rule engine with errors will simply be ignored.
- **HighPriority**: This queue may be used for delivery of alarms or other critical processing steps. It will retry all failed messages processed by rule engine and timed-out messages until success.
- **SequentialByOriginator**: This queue is important if you would like to make sure that messages are processed in correct order. Messages from the same entity will be processed by rule engine with the order they arrive to the queue. 

### Transport configuration

The platform supports three transport types: **Default, MQTT and CoAP**.

#### Default transport type

With the Default transport type, you can continue to use the platform’s default [MQTT](https://thingsboard.io/docs/pe/reference/mqtt-api/), [HTTP](https://thingsboard.io/docs/pe/reference/http-api/), [CoAP](https://thingsboard.io/docs/pe/reference/mqtt-api/) and [LwM2M](https://thingsboard.io/docs/pe/reference/lwm2m-api/) APIs to connect your devices. There is no specific configuration setting for the default transport type.

#### MQTT transport type

The MQTT transport type enables advanced MQTT transport settings. Now you are able to specify custom MQTT topics filters for time-series data and attribute updates.

**The MQTT transport type has the following settings:**

##### MQTT device topic filters

Custom MQTT topic filters support single-level ‘+’ and multi-level ‘#’ wildcards and allow you to connect to almost any MQTT based device that sends a payload using JSON or Protobuf.

{{< hint info >}}

**single-level wildcard ‘+’**

As the name suggests, a single-level wildcard replaces one topic level. The plus symbol represents a single-level wildcard in a topic. For example a subscription to *myhome/groundfloor/+/temperature* can produce the following results:

- myhome/groundfloor/livingroom/temperature
- myhome/groundfloor/kitchen/temperature

{{< /hint >}}

{{< hint info >}}

**multi-level wildcard ‘#’**

The multi-level wildcard covers many topic levels. The hash symbol represents the multi-level wild card in the topic. For the broker to determine which topics match, the multi-level wildcard must be placed as the last character in the topic and preceded by a forward slash. For example a subscription to *myhome/groundfloor/#* can produce the following results:

- myhome/groundfloor/livingroom/temperature
- myhome/groundfloor/kitchen/temperature
- myhome/groundfloor/kitchen/brightness

{{< /hint >}}

##### MQTT device payload

By default, the platform expects devices to send data via JSON. However, it is also possible to send data via [Protocol Buffers](https://developers.google.com/protocol-buffers). Now we supports customizable proto schemas for [telemetry upload](https://thingsboard.io/docs/pe/reference/mqtt-api/#telemetry-upload-api) and [attribute upload](https://thingsboard.io/docs/pe/reference/mqtt-api/#publish-attribute-update-to-the-server).

{{< hint info >}}

Protocol Buffers, or Protobuf, is a language- and a platform-neutral way of serializing structured data. It is convenient to minimize the size of transmitted data.

{{< /hint >}}

You can copy your proto schema into the text box:

{{< img name="Add-Device-Profile-MQTT-transport-type-protobuf" size="large" lazy=false >}} 

#### CoAP transport type

The CoAP transport type enables advanced CoAP transport settings. With the CoAP transport type, you have the ability to select the CoAP device type.

##### CoAP device type

By default CoAP device type **Default** have CoAP device payload set to JSON that supports basic [CoAP API](https://thingsboard.io/docs/pe/reference/coap-api/) same as for [Default transport type](https://thingsboard.io/docs/pe/user-guide/device-profiles/#default-transport-type). However, it is also possible to send data via [Protocol Buffers](https://developers.google.com/protocol-buffers) by changing the parameter CoAP device payload to Protobuf.

If you select device type:**Efento NB-IoT**, platform supports integration with next Efento NB-IoT sensors:

- temperature,
- humidity,
- air pressure,
- differential pressure,
- open / close,
- leakage,
- I/O.

FW version: 06.02 or newer.

### Alarm Rules

When the messages from a device is transmitted to the rule engine, it checks the alarm rules you configured here to create alarm, clean alarm and so on. Alarm rules is important and useful for most use cases.

Alarm Rule consists of the following properties:

- **Alarm Type** - a type of Alarm. Alarm type must be unique within the device profile alarm rules;
- **Create Conditions** - defines the criteria when the Alarm will be created/updated. The condition consists of the following properties:
  - Severity - will be used to create/update an alarm. Platform verifies Create Conditions in the descending order of the severity. For example, if a condition with Critical severity is true, the platform will raise alarm with Critical severity, and “Major”, “Minor” or “Warning” conditions will not be evaluated. Severity must be unique per alarm rule (e.g., two conditions created within the same alarm rule can’t have the same severity);
  - Key Filters - list of logical expressions against attributes or telemetry values. For example, *“(temperature < 0 OR temperature > 20) AND softwareVersion = ‘2.5.5’“*;
  - Condition Type - either simple, duration, or repeating. For example, *3 times in a row* or *during 5 minutes*. The simple condition will raise an alarm once the first matching event occurrs;
  - Schedule - defines the time interval during which the rule is active. Either “active all the time”, “active at specific time” or “custom”;
  - Details - the alarm details template supports substitution of the telemetry and/or attribute values using ${attributeName} syntax;
- **Clear condition** - defines criteria when the Alarm will be cleared;
- **Advanced settings** - defines alarm propagation to related assets, customers, tenant, or other entities.

Let’s learn how to use the Alarm Rules with an example. Let’s assume we would like to keep track of the temperature inside of the fridge with valuable goods.
We also assume that we have already created a device profile called “Temperature Sensors”, and provisioned our device "Fridge Temperature Sensors" with the temperature sensor.

#### Example 1. Simple alarm conditions

We would like to create a **Critical** alarm when the temperature is greater than 10 degrees.

- Step 1. Open the device profile and toggle edit mode.

- Step 2. Click the "Add alarm rule" button.

- Step 3. Input Alarm Type and click on the red "+" sign.

- Step 4. Click the "Add Key Filter" button.

  {{< img name="Device-Profiles-Alarm-Rule-Condition-Add-Key-Filter-1" size="large" lazy=false >}} 

- Step 5. Select the "Timeseries" key type. Input the "temperature" key name. Change "Value type" to "Numeric". Click the "Add" button.

  {{< img name="Device-Profiles-Alarm-Rule-Condition-Add-Key-Filter-2" size="large" lazy=false >}} 

- Step 6. Select the "greater than" operation and input the threshold value. Click "Add".

  {{< img name="Device-Profiles-Alarm-Rule-Condition-Add-Key-Filter-3" size="large" lazy=false >}} 

- Step 7. Click the "Save" button.

- Step 8. Finally, apply changes.

- Step 9. Use MQTTX to upload a temperature value to platform.

  {{< img name="Upload-Temperature-Value" size="large" lazy=false >}} 

- Step 10. Navigate to "Management->Devices", click the device named "Fridge Temperature Sensors" and click "Alarms" tag, we can see the alarm event.

  {{< img name="Simple-Alarm-Conditions" size="large" lazy=false >}} 

#### Example 2. Alarm condition with a duration

Let’s assume that we would like to modify Example 1 and raise alarms only if the temperature exceeds a certain threshold for 1 minute.

For this purpose, we need to edit the alarm condition and modify the condition type from “Simple” to “Duration”. We should also specify the duration value and unit.

- Step 1. Edit the alarm condition and change the condition type to "Duration". Specify duration value and unit. Save the condition.

  {{< img name="Device-Profiles-Edit-Alarm-Rule-Condition-Duration" size="large" lazy=false >}} 

- Step 2. Apply changes.

- Step 3. Use MQTTX to upload a temperature value to platform at 1 minute intervals.

  {{< img name="Upload-Temperature-Value-Time-Interval-1-Minute" size="large" lazy=false >}} 

- Step 4. Navigate to "Management->Devices", click the device named "Fridge Temperature Sensors" and click "Alarms" tag, we can see the alarm event.

  {{< img name="Alarm-Condition-With-A-Duration" size="large" lazy=false >}} 

#### Example 3. Repeating alarm condition

Let’s assume we would like to modify Example 1 and raise alarms only if the sensor reports a temperature that exceeds the threshold 3 times in a row.

For this purpose, we need to edit the alarm condition and modify the condition type from “Simple” to “Repeating”. We should also specify 3 as ‘Count of events’.

- Step 1. Edit the alarm condition and change the condition type to "Repeating". Specify 3 as "Count of events". Save the condition.

- Step 2. Apply changes.

- Step 3. Use MQTTX to upload a temperature value to platform three times.

  {{< img name="Upload-Temperature-Value-Three-Time" size="large" lazy=false >}} 

- Step 4. Navigate to "Management->Devices", click the device named "Fridge Temperature Sensors" and click "Alarms" tag, we can see the alarm event.

  {{< img name="Repeating-Alarm-Condition" size="large" lazy=false >}} 


#### Example 4. Clear alarm rule

Let’s assume we would like to automatically clear the alarm if the temperature in the fridge goes back to normal.

- Step 1. Open the device profile and toggle edit mode. Click the "Add clear condition" button.

  {{< img name="Device-Profiles-Add-Clear-Condition" size="large" lazy=false >}} 

- Step 2. Click on the red "+" sign.

- Step 3. Add Key Filter.

  {{< img name="Device-Profiles-Add-Clear-Condition-Add-Key-Filter" size="large" lazy=false >}} 

- Step 4. Finally, apply changes.

- Step 5. Use MQTTX to upload a temperature value to platform.

  {{< img name="Upload-Temperature-Value-On-Example-4" size="large" lazy=false >}} 

- Step 6. Navigate to "Management->Devices", click the device named "Fridge Temperature Sensors" and click "Alarms" tag, we can see a alarm event with status "Active Unacknowledged".

  {{< img name="Example4-Alarms-Event-1" size="large" lazy=false >}} 

- Step 7. Use MQTTX to upload a temperature value 8 to platform.

  {{< img name="Example4-Upload-Temperature-Value-2" size="large" lazy=false >}} 

- Step 8. Navigate to "Management->Devices", click the device named "Fridge Temperature Sensors" and click "Alarms" tag, we can see the status of the alarm event  created before has been modified to "Cleared Unacknowledged".

  {{< img name="Example4-Alarms-Event-2" size="large" lazy=false >}} 

#### Example 5. Define alarm rule schedule

Let’s assume we would like an alarm rule to evaluate alarms only during working hours.

- Step 1. Edit the schedule of the alarm rule.

  {{< img name="Example5-Edit-Alarm-Rule-1" size="large" lazy=false >}} 

- Step 2. Select timezone, days, time interval, and click "Save".

  {{< img name="Example5-Edit-Alarm-Rule-2" size="large" lazy=false >}} 

- Step 3. Finally, apply changes.

#### Example 6. Advanced thresholds

Let’s assume we would like our users to be able to overwrite the thresholds from Dashboard UI. We can also add the flag to enable or disable certain alarms for each device. For this, we will use dynamic values in the alarm rule condition. We will use two attributes: the boolean *temperatureAlarmFlag*, and the numeric *temperatureAlarmThreshold*. Our goal is to trigger an alarm creation when “*temperatureAlarmFlag* = True AND *temperature* is greater than *temperatureAlarmThreshold*”.

- Step 1. Modify the temperature key filter and change the value type to dynamic.

  {{< img name="Example6-Edit-Alarm-Rule-1" size="large" lazy=false >}} 

- Step 2. Select a dynamic source type and input the *temperatureAlarmThreshold*, then click "Update". You may optionally check "Inherit from owner". Inheritance allows to take the threshold value from customer if it is not set on the device level. If the attribute value is not set on both device and customer levels, rule will take the value from the tenant attributes.

  {{< img name="Example6-Edit-Alarm-Rule-2" size="large" lazy=false >}} 

- Step 3. Add another key filter for the *temperatureAlarmFlag*, then click "Add".

  {{< img name="Example6-Edit-Alarm-Rule-3" size="large" lazy=false >}} 

  {{< img name="Example6-Edit-Alarm-Rule-4" size="large" lazy=false >}} 

- Step 4. Finally, click "Save" and apply changes.

- Step 5. Provision device attributes either manually or via the script.

  {{< img name="Example6-Add-Server-Attributes-1" size="large" lazy=false >}} 

- Step 6. Use MQTTX to upload a temperature value to platform.

  {{< img name="Example6-Upload-Temperature-Value-1" size="large" lazy=false >}} 

- Step 7. Navigate to "Management->Devices", click the device named "Fridge Temperature Sensors" and click "Alarms" tag, we can see the alarm event.

  {{< img name="Example6-Alarms-Event-1" size="large" lazy=false >}} 

- Step 8. Clean the alarm event created in Step 7.

  {{< img name="Example6-Clean-Alarms-Event-1" size="large" lazy=false >}} 

- Step 9. Now we change the server attribute *temperatureAlarmFlag* to *false*, and repeate Step 7.

  {{< img name="Example6-Edit-Server-Attributes-1" size="large" lazy=false >}} 

- Step 10. Navigate to "Management->Devices", click the device named "Fridge Temperature Sensors" and click "Alarms" tag, as we can see, no new events were created.

#### Device profile rule node

Device Profile rule node creates and clears alarms based on the alarm rules defined in the device profile. By default, this is the first rule node in the chain of processing. The rule node processes all incoming messages and reacts to the attributes and telemetry values. There are two important settings in the rule node:

**Persist state of alarm rules** - forces the rule node to store the state of processing. Disabled by default. This setting is useful if you have duration or repeating conditions. Let’s assume you have a condition “Temperature is greater than 50 for 1 hour”, and the first event with a temperature greater than 50 was reported at 1 pm. At 2 pm you should receive the alarm (assuming temperature conditions will not change). However, if you will restart the server after 1 pm and before 2 pm, the rule node needs to lookup the state from DB. Basically, if you enable this and the ‘Fetch state of alarm rules’ option, the rule node will be able to raise the alarm. If you leave it disabled, the rule node will not generate the alarm. We disable this setting by default for performance reasons. If enabled, and if the incoming message matches at least one of the alarm conditions, it will cause additional write operation to persist in the state.

**Fetch state of alarm rules** - forces rule node to restore the state of processing on initialization. Disabled by default. This setting is useful if you have duration or repeating conditions. It should work in tandem with the ‘Persist state of alarm rules’ option, but on rare occasions, you may want to disable this setting while the ‘Persist state of alarm rules’ option is enabled. Assuming you have many devices that send data frequently or constantly, you can avoid loading the state from the DB on initialization. The Rule Node will fetch the state from the database when the first message from a specific device arrives.

{{< img name="Device-Profile-Rule-Node-1" size="large" lazy=false >}} 

{{< img name="Device-Profile-Rule-Node-2" size="large" lazy=false >}} 

#### Notifications about alarms

Assuming you have configured alarm rules you may also want to receive a notification when platform creates or updates the alarm. The device profile rule node has three main outbound relation types that you can use: ‘Alarm Created’, ‘Alarm Severity Updated’, and ‘Alarm Cleared’. See the example rule chain below. Please make sure that the system administrator has configured the SMS/email providers before you proceed or configure your own settings in the rule nodes.

You may also use existing guides: [Send email on alarm](https://thingsboard.io/docs/user-guide/rule-engine-2-0/tutorials/send-email/) (Use part which explains ‘to email’ and ‘send email’ nodes) or [Telegram notifications](https://thingsboard.io/docs/user-guide/rule-engine-2-0/tutorials/integration-with-telegram-bot/). There is also an additional ‘Alarm Updated’ relation type that should be ignored in most cases to avoid duplicate notifications.

{{< img name="Notifications-About-Alarms-1" size="large" lazy=false >}} 



  

