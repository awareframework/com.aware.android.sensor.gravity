# AWARE Gravity

[![jitpack-badge](https://jitpack.io/v/awareframework/com.aware.android.sensor.gravity.svg)](https://jitpack.io/#awareframework/com.aware.android.sensor.gravity)

The gravity sensor measures the force of gravity applied to the sensor built-in into the device and provides a three dimensional vector indicating the direction and magnitude of gravity (in m/s²). When a device is at rest, the gravity sensor should measure equally as the accelerometer.

![Sensor axes](http://www.awareframework.com/wp-content/uploads/2015/01/axis_device.png)

The coordinate-system is defined relative to the screen of the phone in its default orientation (facing the user). The axis are not swapped when the device’s screen orientation changes. The X axis is horizontal and points to the right, the Y axis is vertical and points up and the Z axis points towards the outside of the front face of the screen. In this system, coordinates behind the screen have negative Z axis. Also, the natural orientation of a device is not always portrait, as the natural orientation for many tablet devices is landscape. For more information, check the official [Android’s Sensor Coordinate System][3] documentation.

## Public functions

### GravitySensor

+ `start(context: Context, config: GravitySensor.Config?)`: Starts the gravity sensor with the optional configuration.
+ `stop(context: Context)`: Stops the service.
+ `currentInterval`: Data collection rate per second. (e.g. 5 samples per second)

### GravitySensor.Config

Class to hold the configuration of the sensor.

#### Fields

+ `sensorObserver: GravitySensor.Observer`: Callback for live data updates.
+ `interval: Int`: Data samples to collect per second. (default = 5)
+ `period: Float`: Period to save data in minutes. (default = 1)
+ `threshold: Double`: If set, do not record consecutive points if change in value is less than the set value.
+ `enabled: Boolean` Sensor is enabled or not. (default = `false`)
+ `debug: Boolean` enable/disable logging to `Logcat`. (default = `false`)
+ `label: String` Label for the data. (default = "")
+ `deviceId: String` Id of the device that will be associated with the events and the sensor. (default = "")
+ `dbEncryptionKey` Encryption key for the database. (default = `null`)
+ `dbType: Engine` Which db engine to use for saving data. (default = `Engine.DatabaseType.NONE`)
+ `dbPath: String` Path of the database. (default = "aware_gravity")
+ `dbHost: String` Host for syncing the database. (default = `null`)

## Broadcasts

### Fired Broadcasts

+ `GravitySensor.ACTION_AWARE_GRAVITY` fired when gravity saved data to db after the period ends.

### Received Broadcasts

+ `GravitySensor.ACTION_AWARE_GRAVITY_START`: received broadcast to start the sensor.
+ `GravitySensor.ACTION_AWARE_GRAVITY_STOP`: received broadcast to stop the sensor.
+ `GravitySensor.ACTION_AWARE_GRAVITY_SYNC`: received broadcast to send sync attempt to the host.
+ `GravitySensor.ACTION_AWARE_GRAVITY_SET_LABEL`: received broadcast to set the data label. Label is expected in the `GravitySensor.EXTRA_LABEL` field of the intent extras.

## Data Representations

### Gravity Sensor

Contains the hardware sensor capabilities in the mobile device.

| Field      | Type   | Description                                                     |
| ---------- | ------ | --------------------------------------------------------------- |
| maxRange   | Float  | Maximum sensor value possible                                   |
| minDelay   | Float  | Minimum sampling delay in microseconds                          |
| name       | String | Sensor’s name                                                  |
| power      | Float  | Sensor’s power drain in mA                                     |
| resolution | Float  | Sensor’s resolution in sensor’s units                         |
| type       | String | Sensor’s type                                                  |
| vendor     | String | Sensor’s vendor                                                |
| version    | String | Sensor’s version                                               |
| deviceId   | String | AWARE device UUID                                               |
| label      | String | Customizable label. Useful for data calibration or traceability |
| timestamp  | Long   | unixtime milliseconds since 1970                                |
| timezone   | Int    | [Raw timezone offset][1] of the device                          |
| os         | String | Operating system of the device (ex. android)                    |

### Gravity Data

Contains the raw sensor data.

| Field     | Type   | Description                                                     |
| --------- | ------ | --------------------------------------------------------------- |
| x         | Float  | value of X axis                                                 |
| y         | Float  | value of Y axis                                                 |
| z         | Float  | value of Z axis                                                 |
| accuracy  | Int    | Sensor’s accuracy level (see [SensorManager][2])               |
| label     | String | Customizable label. Useful for data calibration or traceability |
| deviceId  | String | AWARE device UUID                                               |
| label     | String | Customizable label. Useful for data calibration or traceability |
| timestamp | Long   | unixtime milliseconds since 1970                                |
| timezone  | Int    | [Raw timezone offset][1] of the device                          |
| os        | String | Operating system of the device (ex. android)                    |

## Example usage

```kotlin
// To start the service.
GravitySensor.start(appContext, GravitySensor.Config().apply {
    sensorObserver = object : GravitySensor.Observer {
        override fun onDataChanged(data: GravityData) {
            // your code here...
        }
    }
    dbType = Engine.DatabaseType.ROOM
    debug = true
    // more configuration...
})

// To stop the service
GravitySensor.stop(appContext)
```

## License

Copyright (c) 2018 AWARE Mobile Context Instrumentation Middleware/Framework (http://www.awareframework.com)

Licensed under the Apache License, Version 2.0 (the "License"); you may not use this file except in compliance with the License. You may obtain a copy of the License at

http://www.apache.org/licenses/LICENSE-2.0
Unless required by applicable law or agreed to in writing, software distributed under the License is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the License for the specific language governing permissions and limitations under the License.

[1]: https://developer.android.com/reference/java/util/TimeZone#getRawOffset()
[2]: http://developer.android.com/reference/android/hardware/SensorManager.html
[3]: http://developer.android.com/guide/topics/sensors/sensors_overview.html#sensors-coords