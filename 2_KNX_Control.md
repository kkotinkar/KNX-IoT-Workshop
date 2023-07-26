# Chapter 2 - Test and Use the KNX IoT System

### Step 1: Verify the Thread network operation

#### A) KNX IoT Light Switch Sensor / nRF5340-DK

After flashing the nRF5340-DK, reset the board or turn it on if has been turned off previously. Your device should automatically start the Thread network interface, the software is configure to either create or join the Thread network with the following configuration:

| Parameter | Value |
| ------------- | ------------- |
| PAN ID  | 0xabcd  |
| Channel  | 11  |
| Network name | KNX  |
| Extended PAN ID | dead00beef00cafe    |
| Network key | 00112233445566778899aabbccddeeff  |

The software samples has the OpenThread Command Line Interface (CLI) enabled. You can access the OT CLI via the UART shell with command ``ot``.

Wait and verify that the device is acting as Thread leader.
```
ot state
```

Expect:

```
leader
Done
```

Double check that the Thread network configuration is as expected (see Table above).

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