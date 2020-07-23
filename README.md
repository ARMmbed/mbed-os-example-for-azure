![](./resources/official_armmbed_example_badge.png)

# Azure IoT Hub example on Mbed OS

The example project is part of the [Arm Mbed OS Official Examples](https://os.mbed.com/code/). It contains an application that uses the [Microsoft Azure SDK](https://github.com/Azure/azure-iot-sdk-c) to connect to an IoT Hub instance using the MQTT protocol.

You can build the project with all supported [Mbed OS build tools](https://os.mbed.com/docs/mbed-os/latest/tools/index.html). However, this example project specifically refers to the command-line interface tool [Arm Mbed CLI](https://github.com/ARMmbed/mbed-cli#installing-mbed-cli).
(Note: To see a rendered example you can import into the Arm Online Compiler, please see our [import quick start](https://os.mbed.com/docs/mbed-os/latest/quick-start/online-with-the-online-compiler.html#importing-the-code).)

## Setting up an Azure IoT Hub account

Follow Azure IoT Hub's official documentation to

1. Create a new hub on the Azure portal ([documentation](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-create-through-portal#create-an-iot-hub)). You will need a [_Standard_ tier](https://azure.microsoft.com/en-gb/pricing/details/iot-hub/) hub to enable cloud-to-device messages for this example, and a free option is available in this tier.
1. Register a new device to the hub you have created ([documentation](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-create-through-portal#register-a-new-device-in-the-iot-hub)). Make a copy of the "Primary Connection String" of the device.

Now the IoT Hub is ready for use in this example.

## Building this example

To fetch the example,

1. Fetch the project and its dependencies
    ```
    mbed import mbed-os-example-azure
    ```
    Or if you fetched the example with `git clone`, run `mbed deploy` inside the cloned repository.
1. In the example repository you fetched, open [`mbed_app.json`](./mbed_app.json) and set the value of `iothub_connection_string` to the Primary Connection String you [previously copied](#setting-up-the-cloud), keeping any escaped quotes (`\"`).
1. (If you want to use WiFi) set `nsapi.default-wifi-ssid` to your WiFi name and `nsapi.default-wifi-password` to your WiFi password, keeping any escaped quotes (`\"`).

To compile and run the example,

1. Connect your development board to your PC with a USB cable.
1. (If you want to use Ethernet) connect the board to an Ethernet cable of your network.
1. Compile, flash and run the example
    ```
    mbed compile -m <TARGET> -t <TOOLCHAIN> -f --sterm --baud 115200
    ```
    For example, `<TARGET>` can be `DISCO_L475VG_IOT01A` and `<TOOLCHAIN>` can be `GCC_ARM` if you want to use this combination.

## Expected output

The example starts by connecting your network interface:
```
Info: Connecting to the network
Info: Connection success, MAC: <your MAC>
```

Then it fetches time from an NTP server, because Azure IoT Hub's Share Access Key requires correct timestamps for connections:
```
Info: Getting time from the NTP server
Info: Time: Wed Jul22 15:9:41 2020

Info: RTC reports Wed Jul22 15:9:41 2020
```
**Note**: If it fails to fetch time or the reported time makes no sense (e.g. year 2100), the NTP server might be unreachable for a moment. It is usually enough to reset the board to rerun the example.

Now it  initializes the Azure SDK and starts sending one message every second:
```
Info: Sending: "10 messages left to send, or until we receive a reply"
-> 15:9:43 CONNECT | VER: 4 | KEEPALIVE: 240 | FLAGS: 192 | USERNAME: <your Hub>.azure-devices.net/<your device>/?api-version=2017-11-08-preview&DeviceClientType=iothubclient%2f1.3.8%20(native%3b%20mbedOS5%3b%20undefined) | PWD: XXXX | CLEAN: 0
<- 15:9:43 CONNACK | SESSION_PRESENT: true | RETURN_CODE: 0x0
Info: Connected to IoT Hub
-> 15:9:43 SUBSCRIBE | PACKET_ID: 2 | TOPIC_NAME: devices/<your device>/messages/devicebound/# | QOS: 1
-> 15:9:43 PUBLISH | IS_DUP: false | RETAIN: 0 | QOS: DELIVER_AT_LEAST_ONCE | TOPIC_NAME: devices/<your device>/messages/events/ | PACKET_ID: 3 | PAYLOAD_LEN: 53
<- 15:9:43 SUBACK | PACKET_ID: 2 | RETURN_CODE: 1
<- 15:9:43 PUBACK | PACKET_ID: 3
Info: Message sent successfully
Info: Sending: "9 messages left to send, or until we receive a reply"
-> 15:9:44 PUBLISH | IS_DUP: false | RETAIN: 0 | QOS: DELIVER_AT_LEAST_ONCE | TOPIC_NAME: devices/<your device>/messages/events/ | PACKET_ID: 4 | PAYLOAD_LEN: 52
<- 15:9:44 PUBACK | PACKET_ID: 4
Info: Message sent successfully
...
```

## Sending cloud-to-device messages

On the Azure portal, go to the page of your device (i.e. where you [copied the Primary Connection String](#setting-up-the-cloud)), you can send a message to your device using "Message to Device".

Once the message is received by your device, it prints:
```
Info: Message received from IoT Hub
Info: Message body: <YOUR MESSAGE>
```

## Troubleshooting
If you have problems, you can review the [documentation](https://os.mbed.com/docs/latest/tutorials/debugging.html) for suggestions on what could be wrong and how to fix it.

## Related links
* [Mbed boards](https://os.mbed.com/platforms/)
* [Mbed OS Configuration](https://os.mbed.com/docs/latest/reference/configuration.html).
* [Mbed OS Serial Communication](https://os.mbed.com/docs/latest/tutorials/serial-communication.html).
* [Azure IoT Hub](https://azure.microsoft.com/en-gb/services/iot-hub/)
* [Microsoft Azure IoT device SDK for C](https://github.com/Azure/azure-iot-sdk-c/tree/master/iothub_client)
