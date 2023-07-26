# Chapter 2 - Test and Use the KNX IoT System

### Step 1: Verify the Thread network operation

#### A) KNX IoT Light Switch Sensor / nRF5340-DK

After flashing the nRF5340-DK, reset the board or turn it on if has been turned off previously. Your device should automatically start the Thread network interface. The software is configured to either create or join the Thread network with the following configuration:

| Parameter | Value |
| ------------- | ------------- |
| PAN ID  | 0xabcd  |
| Channel  | 11  |
| Network name | KNX  |
| Extended PAN ID | dead00beef00cafe    |
| Network key | 00112233445566778899aabbccddeeff  |

By default the samples have the OpenThread Command Line Interface (CLI) enabled to control the Thread stack. <br>
You can access the OT CLI via the UART shell with command ``ot``.

Wait and verify that the device is acting as Thread leader.
```
ot state
```

Expect:

```
leader
Done
```

Double check that the Thread network configuration is as expected (compare to Table above).

```
ot dataset active
```

Expect:

```
Active Timestamp: 0
Channel: 11
Channel Mask: 0x07fff800
Ext PAN ID: dead00beef00cafe
Mesh Local Prefix: fdde:ad00:beef:0::/64
Network Key: 00112233445566778899aabbccddeeff
Network Name: KNX
PAN ID: 0xabcd
PSKc: <...>
Security Policy: 672 onrc
Done
```

#### B) KNX IoT Light Switch Actuator / Thingy:53

Repeat the process for the Thingy:53. It should act as Thread router or Thread child device.

### Step 2: Setup the KNX IoT device configuration

The KNX device and group object configuration can be set via the UART shell, using command syntax ``uart:~$ knx``.

#### A) KNX IoT Light Switch Sensor / nRF5340-DK

Set the KNX device installation identifier (iid) and the device individual address (ia):

```
knx dev iid 1
```

```
knx dev ia 1
```

#### B) KNX IoT Light Switch Actuator / Thingy:53

Set the **same KNX device installation identifier (iid)**, but a **different the device individual address (ia)**:

```
knx dev iid 1
```

```
knx dev ia 2
```

> **Note**
> You can read back the configuration via ``knx dev ia/iid``.

### Step 3: Setup the KNX IoT Bindings / Group Object Table

We will use all four buttons on the nRF5340-DK to setup a specific LED control configuration for the Thingy:53. You may change this according to personal preference.<br>
The configuration allows to create different colors, e.g. by turning on LED2 and LED3, you will achieve a light blue/cyan color (green+blue).

| Buttons (nRF5340-DK) | Controlled LEDs (Thingy:53) |
| ------------- | ------------- |
| BUTTON1  | LED1 (RED)  |
| BUTTON2  | LED2 (GREEN) |
| BUTTON3  | LED3 (BLUE)  |
| BUTTON4  | LED1, LED2, LED3 (RGB WHITE) |

#### A) KNX IoT Light Switch Sensor / nRF5340-DK

Configure the sensor device by adding entries to the KNX group object table for the 4 buttons on the nRF5340 DK.<br>
The datapoint path is ``/p/<id>`` with ``id`` ranging from 1 to 4 representing the buttons. More documentation can be found [here](https://nordicplayground.github.io/nrf-knx-iot/testing_samples/knxiot_testing.html#connecting-light-switch-sensor-to-light-switch-actuator).

```
knx got add 1 /p/1 22 [1]
knx got add 2 /p/2 22 [2]
knx got add 3 /p/3 22 [3]
knx got add 4 /p/4 22 [4]
```

#### B) KNX IoT Light Switch Actuator / Thingy:53

Configure the actuator device by adding entries to the KNX group object table for the 3 LEDs on the Thingy:53.<br>
We want the LEDs to toggle based on a specific button pressed (here called group address).<br>
The datapoint path is ``/p/<id>`` where ``id`` ranges from 1 to 3 representing the LEDs on the Thingy:53. More documentation can be found [here](https://nordicplayground.github.io/nrf-knx-iot/testing_samples/knxiot_testing.html#connecting-light-switch-sensor-to-light-switch-actuator).

LED1 (red) shall only toggle using BUTTON 1, and with all other LEDs using BUTTON 4 (Note the last parameter, the group address list):

```
knx got add 1 /p/1 22 [1,4]
```

Apply the KNX configuration accordingly for the remaining LEDs:

```
knx got add 2 /p/2 22 [2,4]
```

```
knx got add 3 /p/3 22 [3,4]
```

### Step 4: Test the KNX IoT System

Use the buttons on the nRF5340-DK to control the RGB LED of the Thingy:53.<br>
You may also read back the KNX bindings/group object table configuration via:
```
knx got print
```
