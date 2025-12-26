# Automatic-Plant-Watering-System
ATmega16-based Soil Moisture Control System

Overview

This project implements an automatic plant watering system using an ATmega16 microcontroller.
The system monitors soil moisture levels and activates a water pump via relay when the soil is below a predefined moisture threshold.

The design emphasizes deterministic control, electrical safety, and robustness under real-world constraints such as sensor noise, relay switching transients, and pump inrush current.

System Architecture
Hardware Components

ATmega16

Main controller (bare-metal, no OS)

ADC for soil moisture sensor

GPIO control for relay

Soil Moisture Sensor

Resistive type

Subject to noise, corrosion, and drift

Relay Module

Electrically isolates MCU from pump

Water Pump

AC or DC pump (depending on configuration)

Logical Block Diagram
[Soil Moisture Sensor] ──> ATmega16 ──> Relay ──> Water Pump

Control Strategy
Control Philosophy

A threshold-based control with hysteresis is used.
This approach is preferred over complex control due to:

Slow soil dynamics

Binary actuator (pump ON/OFF)

High safety requirement

State Machine
IDLE
 ├─> WATERING
 └─> LOCKOUT


IDLE
System monitors soil moisture.

WATERING
Pump is activated for a fixed duration.

LOCKOUT
Enforces minimum delay between watering cycles.

Timing & Execution

Main loop rate: ~5 Hz

ADC sampling: averaged over multiple samples

Pump control: time-limited activation using timer-based logic

Blocking delays are avoided during pump activation to maintain control determinism.

Sensor Processing
Soil Moisture Sensor

Resistive sensor output connected to ADC

Noise and contact instability expected

Processed using:

Moving average filtering

Hysteresis threshold to prevent relay chatter

Example:

Moisture ON threshold: < 35%

Moisture OFF threshold: > 45%

Power & Electrical Safety

Relay provides electrical isolation between MCU and pump

Pump causes:

Inrush current

Voltage transient

Mitigation:

Flyback diode (DC pump)

Snubber circuit (AC pump)

Separate power supply for pump and MCU

Common ground (if DC)

Reliability & Safety Measures

Pump ON time is strictly limited

Relay switching frequency minimized

Default state on startup: PUMP OFF

No continuous pumping even if sensor fails

Calibration
Soil Moisture Sensor

Manual calibration:

Dry soil reference

Fully wet soil reference

ADC values mapped to relative moisture percentage

Calibration constants stored in firmware

Known Limitations

Soil moisture sensor prone to corrosion

No flow sensor to confirm water delivery

No feedback on actual soil saturation depth

No watchdog timer

These limitations are explicitly acknowledged.

Possible Improvements

Replace resistive sensor with capacitive sensor

Add flow sensor to detect pump failure

Implement hardware watchdog

Add rain sensor for outdoor use

Add RTC-based watering schedule

Project Goals

This project demonstrates a real-world embedded control system with:

Slow dynamics control

High-power actuator interfacing

Electrical isolation and safety considerations

Deterministic firmware behavior

Author

Abdullah Khalid Fadillah
Computer Systems Engineering
Focus: Embedded Systems & Robotics
