# AGENTS.hardware.md — Hardware Capability Routing

## Purpose

This fragment is injected into AGENTS.md when the `hardware` capability is active. It routes agents to the correct specialists for hardware interface development and device integration.

## Activate when

- Working with hardware interfaces, firmware, or device drivers
- Integrating serial (RS-232/RS-485), USB-HID, Bluetooth, or NFC/RFID devices
- Analyzing embedded system behavior or smart card protocols
- Designing experiment isolation for hardware interactions
- Probing or characterizing unknown hardware devices

## Skills in this capability

| Skill | Trigger |
|---|---|
| `embedded-specialist` | Embedded systems design, firmware interfaces, microcontroller integration |
| `hardware-analyst` | Device probing, capability discovery, hardware characterization |
| `smart-card-specialist` | ISO 7816 smart cards, APDU commands, PKI card integration |
| `serial-specialist` | RS-232/RS-485 serial communication, baud rate negotiation, framing protocols |
| `usb-hid-specialist` | USB Human Interface Device protocol, descriptor parsing, HID report formats |
| `bluetooth-specialist` | BLE and Classic Bluetooth, GATT profiles, pairing, service discovery |
| `nfc-rfid-specialist` | NFC tags, RFID readers, ISO 14443/15693, NDEF records |

## Device Probing Protocol

When encountering an unknown hardware device:

```
hardware-analyst (identify device type and capabilities)
  → relevant specialist (serial / usb-hid / bluetooth / nfc-rfid / smart-card)
    → embedded-specialist (if firmware-level interaction needed)
```

## Experiment Isolation Mandate

All hardware experiments that require elevated privileges or direct device access MUST be isolated in a Docker container with explicit device passthrough (`--device` flag). Never run hardware experiments directly on the host without documenting the justification in an ADR.

## Hardware ADR Mandate

Any decision to adopt a new hardware communication protocol, add a device driver dependency, or change the device interaction model requires an ADR via `adr-writer` (from `common/`).
