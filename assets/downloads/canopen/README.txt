CVP5C CANopen object dictionary
===============================

This folder contains the CANopen object dictionary in standard exchange
formats:

- CVP5C_CANopen_ObjectDictionary.eds
- CVP5C_CANopen_ObjectDictionary.xdd
- CVP5C_CANopen_ObjectDictionary.xml

Source:

- Firmware/GE_VALVE_CAN/ObjectDictionary_CAN_Backups/2024_May

Important integration note:

The object dictionary contains entries for firmware variants and future
configurations. The distributor integration manual describes the current
delivered configuration: EV1 only, no digital outputs, and no external analog
input. For this configuration, implement the PDO and SDO objects marked as
active in the manual.

