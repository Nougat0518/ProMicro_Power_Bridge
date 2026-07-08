ProMicro_Power_Bridge
######################

XDS BLE to ANT+ Bicycle Power and BLE CPS/CSCS bridge for nRF52840 boards with
UF2 bootloader support.

Overview
********

This firmware connects to an XDS power meter over its private BLE service,
parses the power/cadence data, and forwards it as ANT+ Bicycle Power. It also
advertises standard BLE Cycling Power and Cycling Speed/Cadence services for
phones and watches.

Building and Running
********************

Build the UF2/nice!nano style target with:

.. code-block:: console

   west build --build-dir build_nano_xds . --pristine --board promicro_nrf52840/nrf52840/uf2

The generated UF2 file is:

.. code-block:: console

   build_nano_xds/ProMicro_Power_Bridge/zephyr/zephyr.uf2

This UF2 build is linked for the S140/nice!nano UF2 bootloader application
offset at 0x26000.
