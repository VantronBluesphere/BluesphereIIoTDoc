---
resources:
  - name: Generate-Alarms-Based-On-Collected-Data-1
    src: "Generate-Alarms-Based-On-Collected-Data-1.png"
    title: 
  - name: Generate-Alarms-Based-On-Collected-Data-2
    src: "Generate-Alarms-Based-On-Collected-Data-2.png"
    title: 
  - name: Generate-Alarms-Based-On-Collected-Data-3
    src: "Generate-Alarms-Based-On-Collected-Data-3.png"
    title: 
  - name: Generate-Alarms-Based-On-Collected-Data-4
    src: "Generate-Alarms-Based-On-Collected-Data-4.png"
    title: 
  - name: Forward-Data-To-External-Systems-1
    src: "Forward-Data-To-External-Systems-1.png"
    title: 
  - name: Other-Features-Using-Rule-Engine-1
    src: "Other-Features-Using-Rule-Engine-1.png"
    title: 
  - name: Other-Features-Using-Rule-Engine-2
    src: "Other-Features-Using-Rule-Engine-2.png"
    title: 
  - name: Other-Features-Using-Rule-Engine-3
    src: "Other-Features-Using-Rule-Engine-3.png"
    title: 
  - name: Other-Features-Using-Rule-Engine-4
    src: "Other-Features-Using-Rule-Engine-4.png"
    title: 
  - name: Other-Features-Using-Rule-Engine-5
    src: "Other-Features-Using-Rule-Engine-5.png"
    title: 
  - name: Other-Features-Using-Rule-Engine-6
    src: "Other-Features-Using-Rule-Engine-6.png"
    title: 
---

{{< toc >}}

## Overview

A **time series** is a series of [data points](https://en.wikipedia.org/wiki/Data_point) indexed (or listed or graphed) in time order. We provides a rich set of features related to time-series data. Assume you have already pushed time-series data to platform, you can:

- **Visualize** time series data using configurable and highly customizable widgets and [dashboards](https://thingsboard.io/docs/pe/user-guide/dashboards/);
- **Generate [alarms](https://thingsboard.io/docs/pe/user-guide/alarms/)** based on collected data;
- **Forward** data to external systems using [External Rule Nodes](https://thingsboard.io/docs/pe/user-guide/rule-engine-2-0/external-nodes/) (e.g. Kafka or RabbitMQ Rule Nodes).

## Data visualization

You can visualize data in your dashboards. We recommend [dashboards overview](https://thingsboard.io/docs/pe/user-guide/dashboards/) to get started. Once you are familiar how to create dashboards and configure data sources, you may use widgets to visualize either latest values or real-time changes and historical values. Good examples of widgets that visualize latest values are [digital](https://thingsboard.io/docs/pe/user-guide/ui/widget-library/#digital-gauges) and [analog](https://thingsboard.io/docs/pe/user-guide/ui/widget-library/#analog-gauges) gauges, or [cards](https://thingsboard.io/docs/pe/user-guide/ui/widget-library/#cards). [Charts](https://thingsboard.io/docs/pe/user-guide/ui/widget-library/#charts) are used to visualize historical and real-time values and [maps](https://thingsboard.io/docs/pe/user-guide/ui/widget-library/#maps-widgets) to visualize movement of devices and assets.

You may also use [input widgets](https://thingsboard.io/docs/pe/user-guide/ui/widget-library/#input-widgets) to allow dashboard users to input new time-series values using the dashboards.

## Generate alarms based on collected data

You can use [alarm rules](https://thingsboard.io/docs/pe/user-guide/device-profiles/#alarm-rules) of Device Profile to configure most common alarm conditions via UI or use [filter nodes](https://thingsboard.io/docs/pe/user-guide/rule-engine-2-0/filter-nodes/) to configure more specific use cases via custom JS functions.

### Example1: Use the filter node "script" to customize the filter criteria and send alarm emails

Assume that you created a alarm rule for "temperature greater than 10", and you only want to accept a email when the temperature greater than 20 degrees. You can do this using the filter node.

- Step1. Select a filter node "script" in the rule engine and write custom JS functions.

  {{< img name="Generate-Alarms-Based-On-Collected-Data-1" size="large" lazy=false >}} 

  {{< img name="Generate-Alarms-Based-On-Collected-Data-2" size="large" lazy=false >}} 

- Step2. Connect the "device profile" node to this node.

  {{< img name="Generate-Alarms-Based-On-Collected-Data-3" size="large" lazy=false >}} 

- Step3. Connect this node to "send email" node.

  {{< img name="Generate-Alarms-Based-On-Collected-Data-4" size="large" lazy=false >}} 
  
  
  

## Forward data to external systems

Maybe you want to forward you data to your Kafka and then analyze it using Kafka Streams or other advanced analytics, such as machine learning, predictive analytics, etc. You can easy complete it using External Rule Nodes of Rule Engine. 

{{< img name="Forward-Data-To-External-Systems-1" size="large" lazy=false >}} 

## Other features using Rule Engine

### Modify incoming time-series data before they are stored in the database

Use [message type switch](https://thingsboard.io/docs/pe/user-guide/rule-engine-2-0/filter-nodes/#message-type-switch-node) rule node to filter messages that contain “Post telemetry” request. Then, use [transformation rule nodes](https://thingsboard.io/docs/pe/user-guide/rule-engine-2-0/transformation-nodes/) to modify a particular message.

#### Example1: Copy time-series data into higher level entity

All incoming Messages have originator field that identifies an entity that submits Message. It could be a Device, Asset, Customer, Tenant, etc.

The node "change originator" is used in cases when a submitted message should be processed as a message from another entity. For example, Device submits telemetry and telemetry should be copied into higher level Asset or to a Customer. In this case, Administrator should add this node before [**Save Timeseries**](https://thingsboard.io/docs/pe/user-guide/rule-engine-2-0/action-nodes/#save-timeseries-node) Node.

- Step1. Select a transformation node "change originator" in the rule engine

  {{< img name="Other-Features-Using-Rule-Engine-1" size="large" lazy=false >}} 

- Step2. Choose the Originator source.

  {{< img name="Other-Features-Using-Rule-Engine-2" size="large" lazy=false >}} 

- Step3. Add this node before [**Save Timeseries**](https://thingsboard.io/docs/pe/user-guide/rule-engine-2-0/action-nodes/#save-timeseries-node) Node.

  {{< img name="Other-Features-Using-Rule-Engine-3" size="large" lazy=false >}} 

### Calculate delta between previous and current time-series value

Use [calculate delta](https://thingsboard.io/docs/pe/user-guide/rule-engine-2-0/enrichment-nodes/#calculate-delta) rule node to calculate power, water and other consumption based on smart-meter readings.

#### Example1: Calculate water consumption every day

The node "calculate delta" can calculates ‘delta’ based on the previous time-series reading and current reading and adds it to the message. Useful for smart-metering use case. For example, when the water metering device reports the absolute value of the pulse counter once per day. To find out the consumption for the current day you need to compare value for the previous day with the value for current day.

- Step1. Select a enrichment node "calculate delta" in the rule engine

  {{< img name="Other-Features-Using-Rule-Engine-4" size="large" lazy=false >}} 

- Step2. Configure parameters.

  Let's explain these parameters separately:

  - Input value key (‘pulseCounter’ by default) - specifies the key that will be used to calculate the delta.
  - Output value key (‘delta’ by default) - specifies the key that will store the delta value in the enriched message.
  - Decimals - precision of the delta calculation.
  - Use cache for latest value (‘enabled’ by default) - enables caching of the latest values in memory.
  - Tell ‘Failure’ if delta is negative (‘enabled’ by default) - forces failure of message processing if delta value is negative.
  - Add period between messages (‘disabled’ by default) - adds value of the period between current and previous message.

  {{< img name="Other-Features-Using-Rule-Engine-5" size="large" lazy=false >}} 

- Step3. Add this node before [**Save Timeseries**](https://thingsboard.io/docs/pe/user-guide/rule-engine-2-0/action-nodes/#save-timeseries-node) Node.

  {{< img name="Other-Features-Using-Rule-Engine-6" size="large" lazy=false >}} 

Let’s assume next messages originated by the same device and arrive to the rule node in the sequence they are listed:

```
msg: {"pulseCounter": 42}, metadata: {"ts": "1616510425000"}
msg: {"pulseCounter": 73}, metadata: {"ts": "1616510485000"}
msg: {"temperature": 22}, metadata: {"ts": "1616510486000"}
msg: {"pulseCounter": 42}, metadata: {"ts": "1616510487000"}
```

The output will be the following:

```
msg: {"pulseCounter": 42, "delta": 0, "periodInMs": 0}, metadata: {"ts": "1616510425000"}, relation: Success
msg: {"pulseCounter": 73, "delta": 31, "periodInMs": 60000}, metadata: {"ts": "1616510485000"}, relation: Success
msg: {"temperature": 22}, metadata: {"ts": "1616510486000"}, relation: Other
msg: {"pulseCounter": 42}, metadata: {"ts": "1616510487000"}, relation: Failure
```

### Fetch previous time-series values to analyze incoming telemetry from device

Use [originator telemetry](https://thingsboard.io/docs/pe/user-guide/rule-engine-2-0/enrichment-nodes/#originator-telemetry) rule node to enrich incoming time-series data message with previous time-series data of the device.

### Fetch attribute values to analyze incoming telemetry from device

Use [enrichment](https://thingsboard.io/docs/pe/user-guide/rule-engine-2-0/enrichment-nodes/) rule nodes to enrich incoming telemetry message with attributes of the device, related asset, customer or tenant. This is extremely powerful technique that allows to modify processing logic and parameters based on settings stored in the attributes.

