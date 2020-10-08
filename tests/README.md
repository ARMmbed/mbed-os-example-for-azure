# Testing examples

Examples are tested using tool [htrun](https://github.com/ARMmbed/mbed-os-tools/tree/master/packages/mbed-host-tests) and templated print log.

To run the test, use following command after you build the example:
```
mbedhtrun -d /Volumes/DIS_L4IOT/ -p /dev/tty.usbmodem1432403 -m DISCO_L475VG_IOT01A -f ./BUILD/DISCO_L475VG_IOT01A/GCC_ARM/mbed-os-example-for-azure.bin --compare-log tests/azure.log --baud-rate=115200 --sync=0
```


More details about `htrun` are [here](https://github.com/ARMmbed/mbed-os-tools/tree/master/packages/mbed-host-tests#testing-mbed-os-examples).
