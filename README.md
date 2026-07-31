# Real-Time Fire & Smoke Detection System (STM32F746 + FreeRTOS)

A real-time embedded hazard detection system built on the STM32F746 microcontroller using FreeRTOS. The system runs independent sensor-monitoring tasks concurrently and triggers a centralized alert response when fire or smoke is detected — a course project for EE-423: Embedded Systems Design.

**Team:** Bushra Rehman, Sabika Fatima, Noor Fatima

## Overview

Two sensor tasks run independently and continuously:
- **Fire Task** — reads a digital GPIO input to detect flame presence
- **Smoke Task** — reads an analog (ADC) input and compares it against a threshold

When either task detects a hazard, it pushes an alert through a FreeRTOS queue to a centralized **Alert Task**, which is the only task with control over the LED alarm output. This queue-based design avoids race conditions on shared hardware and keeps sensor logic decoupled from the alarm response.

## System Architecture

**Hardware:**
- STM32F746 development board
- Digital fire sensor
- Analog (ADC) smoke sensor
- LED visual alarm

**Software:**
- STM32 HAL
- FreeRTOS kernel (tasks + queues)

## FreeRTOS Task Design

| Task | Function |
|---|---|
| Fire Task | Polls digital GPIO for flame detection; sends alert on trigger |
| Smoke Task | Reads ADC value, compares to threshold; sends alert on exceedance |
| Alert Task | Highest priority; receives queued alerts and drives the LED for a fixed duration |

Inter-task communication is handled via a FreeRTOS queue, ensuring only the Alert Task ever writes to the LED, regardless of which sensor task triggered the alert.

## Testing & Validation

The system was tested under three conditions — fire-only, smoke-only, and simultaneous fire + smoke — with the LED alarm activating correctly and no missed detections across test runs, confirming reliable task scheduling under concurrent sensor input.



## Report

Full lab report is included in this repository: `LabReport_FireSmoke.docx`

## Conclusion

The project demonstrates effective use of FreeRTOS on STM32F746 for real-time, concurrent hazard monitoring, with queue-based inter-task communication providing fast, reliable alert response.
