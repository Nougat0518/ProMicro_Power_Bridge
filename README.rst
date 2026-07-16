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

Runtime Behavior
****************

The bridge validates XDS error codes and rejects power values outside the
supported range before forwarding them. A hardware watchdog resets the board
if the main bridge loop stops running.

The first XDS device that produces a valid measurement is selected for the
current boot. Use the serial command ``bind clear`` to disconnect it and allow
another sensor to be selected without rebooting. Use ``help`` to list all
serial commands.

CPS/CSCS crank revolution data is estimated by integrating the reported
cadence. It is provided for client compatibility and is not a measured crank
position or an authoritative lifetime revolution counter.

ANT calibration requests are reported as failed because this bridge cannot
forward calibration operations to the underlying XDS sensor. Calibrate the XDS
sensor using its native interface instead.

Battery Voltage Calibration
***************************

P0.04 (AIN2) samples the ``BVOLT`` divider. The fixed divider scale is
``2000/1000`` for a 100 kΩ resistor from BAT to P0.04 and a 100 kΩ resistor
from P0.04 to GND. At 4.2 V battery voltage, the ADC pin voltage is about
2.1 V. Adjust ``BATTERY_SCALE_PERMILLE`` in ``src/main.c`` only if a
multimeter comparison shows a consistent offset.

Battery voltage is printed once per second by default. The ``vbat`` command
prints one sample; ``vbat on`` and ``vbat off`` control periodic output.

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
