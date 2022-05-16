## Overview

Platform provides the ability to create and manage alarms related to your entities: devices, assets, customers, etc. For example, you may configure to automatically create an alarm when the temperature sensor reading is above a certain threshold. Of course, this is a very simplified case, and real scenarios can be much more complex.

## Main concepts

Let’s review the main concepts of the alarm below:

### Originator

Alarm originator is an entity that causes the alarm. For instance, Device A is an originator of the alarm if Platform receives a temperature reading from it and raise the “HighTemperature” alarm because the reading exceeds the threshold.

### Type

Alarm type helps to identify the root cause of the alarm. For example, “HighTemperature” and “LowHumidity” are two different alarms.

### Severity

Each alarm has severity which is either Critical, Major, Minor, Warning, or Indeterminate (sorted by priority in descending order).

### Lifecycle

Alarm may be active or cleared. When Platform creates an alarm it persists the **start** and **end time** of the alarm. By default, the start time and the end time are the same. If the alarm trigger condition repeats, the platform updates the end time. Platform can automatically clear the alarm when an event occurs that matches an alarm clearing condition. Alarm clear condition is optional. A user can clear the alarm manually.

Besides active and cleared alarm state, Platform also keeps track of whether someone has acknowledged the alarm. Alarm acknowledgment is possible via the dashboard widget, or an entity details tab.

To summarize, there are 4 possible values of the “**status**” field:

- Active unacknowledged (ACTIVE_UNACK) - alarm is not cleared and not acknowledged yet;
- Active acknowledged(ACTIVE_ACK) - alarm is not cleared, but already acknowledged;
- Cleared unacknowledged(CLEARED_UNACK) - alarm was already cleared, but not yet acknowledged;
- Cleared acknowledged(CLEARED_ACK) - alarm was already cleared and acknowledged;

### Alarm uniqueness

Platform identifies alarm using a combination of originator, type, and start time. Thus, at a single point in time, there is only one active alarm with the same originator, type, and start time.

Let’s assume you have provisioned alarm rules to create a “HighTemperature” alarm when the temperature is greater than 20. And you also provisioned alarm rules to clear the “HighTemperature” alarm when the temperature is less than or equal to 20.

Assuming the following sequence of events:

- 12:00 - temperature equals 18
- 12:30 - temperature equals 22
- 13:00 - temperature equals 25
- 13:30 - temperature equals 18

Hence, you will see a single “HighTemperature” alarm with start time = 12:30 and end time = 13:00.



