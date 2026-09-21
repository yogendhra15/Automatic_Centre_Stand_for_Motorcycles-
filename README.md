# Automatic Centre Stand for Motorcycles

An Arduino-based electromechanical system that automates the deployment and retraction of a motorcycle's centre stand — replacing manual lifting with a one-touch, sensor-protected actuation sequence.

> Mechatronics and Instrumentation Lab Project, Indian Institute of Technology Indore
> Course Instructor: Prof. I. A. Palani

## Team

| Name | Roll No. |
|---|---|
| Banoth Samithlal | 230003013 |
| Mardana Naveen | 230003042 |
| Ramavath Manthru Naik | 230003055 |
| Reddi Yogendhra Babu | 230003056 |
| Sajja Dora Raja Praveen | 230003063 |
| Yadavalli Akshay Kumar | 230003087 |

## Overview

Traditional centre stands require the rider to lift 40–60% of a motorcycle's weight, which is difficult for elderly riders, women, and anyone with physical limitations. This project replaces that manual effort with a high-torque electromechanical actuator controlled by an Arduino Nano. With a single button press, the actuator lowers the stand, lifts the bike into a stable upright position, and automatically locks — with limit switches, an H-bridge motor driver, and safety cutoffs preventing overload or over-travel

## Key Features

- **One-touch operation** — no manual lifting or physical technique required
- **Automatic motor cutoff** via limit switches at both travel extremes, preventing motor stall and overheating
- **H-bridge relay control** for bidirectional motor operation (extend/retract) without a dedicated driver IC
- **Compact, retrofittable design** — modular linear actuator built from low-cost components
- **Anti-theft benefit** — actuator-driven mechanism resists manual tampering

## System Architecture

### Mechanical Design
- **Drive Unit:** TT DC gear motor (high torque, low RPM), rigidly mounted to the chassis
- **Transmission Assembly:** Lead-screw ("rod-in-a-rod" telescoping) mechanism
  - M6 threaded rod as the lead screw, coupled to the motor via a 3D-printed connector
  - M6 T-nut travels along the rod inside a sliding actuator tube
  - Fixed outer cover tube guides motion and protects internal components
- **Mechanical Sensing:** Two micro-switch push buttons define the extend/retract travel limits

### Electronics
- **Controller:** Arduino Nano (ATmega328P)
- **Power:** TT motor runs at ~9 V (prototype used a 9 V battery; production target is the motorcycle's 12 V line) stepped down to a regulated 5 V logic supply, with filtering/protection against back-EMF and reverse polarity
- **Inputs:** Two user levers (extend/retract) and two limit-switch stop buttons, using pull-up resistors for clean logic states
- **Motor Driver:** Two 5 V relays in an H-bridge configuration to reverse motor polarity
  - Forward → Deploy centre stand
  - Reverse → Retract centre stand

### Pin Mapping (Arduino Nano)

| Function | Pin |
|---|---|
| Lever Forward (extend) | D2 |
| Lever Reverse (retract) | D3 |
| Stop Button – Forward limit | D4 |
| Stop Button – Reverse limit | D5 |
| Relay – Forward | D6 |
| Relay – Reverse | D7 |

## Control Logic

The firmware runs as a simple state machine (`STOPPED`, `FORWARD`, `REVERSE`):

1. Stop buttons always take priority — pressing either immediately halts the motor.
2. A single lever press (falling edge) starts the motor in the corresponding direction.
3. While running, the Arduino continuously polls the limit switches.
4. On reaching a limit, both relays are deactivated instantly, cutting power and preventing stalling or over-travel.

See [`firmware/`](./firmware) for the full Arduino sketch.

## Advantages

- Removes the physical effort barrier to using a centre stand, improving accessibility for elderly and physically limited riders
- Reduces motorcycle parking footprint compared to a side stand
- Improves parking stability
- Modular and retrofittable onto most motorcycles without permanent modification

## Limitations

- The TT gear motor's torque suits lightweight/medium motorcycles, not heavy models
- Adds a small continuous load on the motorcycle battery with frequent use
- Actuator assembly is not yet water/dust sealed
- Motor speed is slow, resulting in longer deployment/retraction times than commercial actuators

## Future Work

- **Variable speed control:** Replace relays with an L298N H-bridge driver for PWM-based soft start/stop
- **Closed-loop positional control:** Add a rotary encoder or potentiometer for precise position commands (e.g. "extend 75 mm")
- **Overcurrent / jam protection:** Integrate an ACS712 current sensor to detect and respond to motor jams
- Waterproof housing and sealed enclosures
- Wireless control (Bluetooth / mobile app / remote key)
- Additional interlocks: ground-level detection, tilt sensors, motion-lock to prevent accidental deployment

## Repository Structure

```
.
├── firmware/          # Arduino Nano control sketch
├── hardware/          # CAD models, wiring diagrams, BOM
├── docs/              # Full lab report and supporting documentation
└── README.md
```

## Conclusion

This project demonstrates a low-cost, safety-conscious automation of the traditionally manual motorcycle centre stand, combining mechanical design with embedded control to improve accessibility and parking safety. With a stronger actuator and the planned safety and control upgrades, the design has a clear path toward a commercial-grade product.
