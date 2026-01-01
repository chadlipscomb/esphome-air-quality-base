
## ESPHome Air Quality Base – Repository Structure

This repository is intentionally structured to keep **hardware**, **devices**, **behavior**, and **wiring** separate.
The goal is long‑term maintainability, easy reuse, and minimal breakage as the project evolves.

---

## `/packages/hw` — Hardware Abstractions

What goes here:
Bus definitions such as I2C, UART, SPI
LEDs, buttons, displays (pure hardware)
Pin usage only

Rules:
No sensors
No logic
No references to meaning (PM, CO₂, status, etc)

Think of this as the pinout layer.

---

## `/packages/devices` — Physical Devices

What goes here:
Concrete hardware devices
Raw sensor data
Device‑specific calculations only

Rules:
Can define sensor IDs
Can define text sensors derived only from that device
Must not reference LEDs, UI, alerts, or other devices

Devices describe what exists, not what it means.

---

## `/packages/features` — Behavior & Meaning

What goes here:
Logic that represents meaning or behavior
LED patterns
UI logic
Alerts, thresholds, interpretations

Rules:
Must not reference concrete device IDs
Must not assume a specific sensor exists
Only consume abstracted inputs

Features define what the system does, not where data comes from.

---

## `/packages/glue` — Wiring / Integration

What goes here:
The only place allowed to reference both devices and features
Routing and mapping logic

Rules:
No sensors defined here
No business logic
Pure wiring

Glue answers: which thing feeds which other thing.

---

## `/packages/profiles` — Device Builds

What goes here:
Substitutions (pins, tuning)
Package enable or disable lists

Rules:
No logic
No sensors
No features

Profiles define what is included for a given board.

---

## `/packages/common` — Cross‑Cutting Infrastructure

What goes here:
Logging
WiFi
OTA
Status and diagnostics

Rules:
No hardware pins
No sensor meaning
Safe to include everywhere

---

## Layer Interaction

hw → devices → glue → features
profiles choose what is active

---

Design principle:

Devices produce facts.
Features consume meaning.
Glue decides who talks to whom.
