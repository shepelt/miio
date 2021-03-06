# Device types and capabilities

To make it easier to work with different devices this library normalizes
different models into types. These device types have their own API to match
what the actual device can actually do. In addition each device also has a
set of capabilities, that are used to flag that a device can do something
extra on top of its type API.

## Types

Id                    | Description                                                | Devices
----------------------|------------------------------------------------------------|---------------------
[`switch`](switch.md) | Switchable devices such as power plugs and light switches. | Mi Smart Socket Plug, Aqara Plug, Aqara Light Control
[`controller`](controller.md) | Devices that are primarily used to control something else. | Aqara Button, Aqara Cube, Aqara Light Switch
[`gateway`](gateway.md) | Mi Smart Home Gateway that pulls in sub devices of the Aqara type | Mi Smart Home Gateway 2, Mi Smart Home Gateway 3
[`air-purifier`](air-purifier.md) | Air purifiers and air filtering devices. | Mi Air Purifier, Mi Air Purifier 2 and Mi Air Purifier Pro
[`vacuum`](vacuum.md) | Robot vacuums. | Mi Robot Vacuum

## Capabilities

Id                         | Description
---------------------------|-------------
[`power`](cap-power.md)  | Device supports being switched on or off.
[`power-channels`](cap-power-channels.md) | Device has one or more channels that can be switched on or off. Used for type `switch`.

## Models

The table below indicates how well different devices are supported. The support
column can be one of the following:

* None - this device is not a miIO-device or has some quirk making it unusable
* Generic - this device is supported via the generic API but does not have a high-level API
* Untested - this device has an implementation but needs testing with a real device
* Basic - the basic functionality of the device is implemented, but more advanced features are missing
* Good - most of the functionality is available including some more advanced features such as settings
* Excellent - as close to complete support as possible

If your device:

* Is not in this list, it might still be a miIO-device and at least have generic support. See the next section for details about how to find out if that is the case.
* Needs a manual token and the table says it should not, something has probably changed in the firmware, please open an issue so the table can be adjusted.
* Is marked as Untested you can help by testing the implementation is this library and opening an issue with information about the result.

Name                          | Type                            | Auto-token | Support   | Note
------------------------------|---------------------------------|------------|-----------|--------
Mi Air Purifier 1              | [`air-purifier`](air-purifier.md) | Yes        | Untested  |
Mi Air Purifier 2              | [`air-purifier`](air-purifier.md) | Yes        | Good      |
Mi Air Purifier Pro            | [`air-purifier`](air-purifier.md) | Yes        | Basic     | Some of the new features and sensors are not supported.
Mi Flora                      | -                               | -          | None      | Communicates using Bluetooth.
Mi Lunar Smart Sleep Sensor   | -                               | Yes        | Generic   |
Mi Robot Vacuum               | [`vacuum`](vacuum.md)           | No         | Basic     | DND, timers and mapping features. are not supported
Mi Smart Socket Plug          | [`switch`](switch.md)           | Yes        | Good      |
Mi Smart Socket Plug 2        | [`switch`](switch.md)           | Yes        | Good      |
Mi Smart Home Gateway 1       | -                               | Yes        | Generic   | API used to access sub devices not supported.
Mi Smart Home Gateway 2       | [`gateway`](gateway.md)         | Yes        | Basic     | Light, sound and music features not supported.
Mi Smart Home Gateway 3       | [`gateway`](gateway.md)         | Yes        | Basic     | Light, sound and music features not supported.
Mi Smart Home Cube            | [`controller`](controller.md)   | Yes        | Excellent | Aqara device via Smart Home Gateway
Mi Smart Home Light Switch    | [`controller`](controller.md)   | Yes        | Untested  | Aqara device via Smart Home Gateway.
Mi Smart Home Light Control   | [`switch`](switch.md)           | Yes        | Untested  | Aqara device via Smart Home Gateway. Controls power to one or two lights.
Mi Smart Home Temperature and Humidity Sensor | `sensor`        | Yes        | Excellent | Aqara device via Smart Home Gateway.
Mi Smart Home Wireless Switch | [`controller`](controller.md)   | Yes        | Excellent | Aqara device via Smart Home Gateway.
Mi Smart Home Door / Window Sensor | `magnet`                   | Yes        | Untested  | Aqara device via Smart Home Gateway.
Mi Smart Home Occupancy Sensor | `motion`                       | Yes        | Untested  | Aqara device via Smart Home Gateway.
Mi Smart Home Aqara Plug      | [`switch`](switch.md)           | Yes        | Untested  | Aqara device via Smart Home Gateway.
Mi Smart Home Smoke Sensor    | -                               | Yes        | Generic   | Aqara device - unknown support
Mi Smart Home Gas Sensor      | -                               | Yes        | Generic   | Aqara device - unknown support
Mi Smart Power Strip 1        | [`switch`](switch.md)           | Unknown    | Untested  | Setting power and mode is untested.
Mi Smart Power Strip 2        | [`switch`](switch.md)           | Unknown    | Untested  | Setting power and mode is untested.

## Finding devices on your network

In certain cases your device might not be listed under models but still be a
miIO-device, for example if it's a new device or something that no one using
the library has tested yet.

The command line application can help with discovery of devices. Get started by
install the command line application:

`npm install -g miio`

Run the app in discovery mode to list devices on your network:

`miio --discover`

This will start outputting all of the devices found, with their address,
identifiers, models and tokens (if found). If your device can be supported it
will show up in this list.

If the device shows up feel free to open an issue about supporting the device.
Be sure to include the name of the device model and the model id output by the
discover command.

## Generic devices

The `generic` type is used when a device is of an unknown model. All properties
and methods of generic devices are also available for specific devices types.

### Properties

* `device.defineProperty(string)`, indicate that a property should be fetched from the device
* `device.defineProperty(string, function)`, indicate that a property should be fetched from the device and mapped with the given function.
* `device.setProperty(string, mixed)`, set the value of a property
* `device.getProperties(Array[string]): Object`, get the given properties if they are monitored
* `device.monitor()`, monitor the device for changes in defined properties
* `device.stopMonitoring()`, stop monitoring the device for changes
* `device.on('propertyChanged', function)`, receive changes to defined properties

* `device.loadProperties(Array[string])`, load properties from the device

### Methods

* `device.call(string, array)`, call a method on the device with the given arguments, returns a promise
