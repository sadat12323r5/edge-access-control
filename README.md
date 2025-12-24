# Secure Edge Biometric Access Control System

This project implements a **secure, edge-based smart lock** that performs biometric authentication locally while enforcing safety-critical behaviour through a dedicated real-time controller. The system is designed as a **cyber-physical access device**, not a cloud-dependent IoT gadget.

Authentication is multi-modal and proximity-aware, combining computer vision, optional voice/DSP processing, and short-range RFID/NFC credentials. Security is enforced at both the software and hardware levels, with clear separation between perception, control, and actuation.

---

## System Architecture Overview

The system is split into two primary compute domains:

**Edge Compute (Linux)**
A Raspberry Pi handles camera input, computer vision, machine learning inference, identity fusion, logging, and communication with backend services. This layer is responsible for perception and decision-making but is *not* trusted with real-time safety control.

**Real-Time Controller (MCU)**
An STM32 microcontroller manages lock actuation, sensor IO, safety logic, and fail-operational behaviour. It runs independently of Linux and guarantees deterministic timing for all physical interactions.

This separation ensures that failures or delays in high-level software cannot cause unsafe physical behaviour.

---

## Core Features

The system supports on-device face recognition, optional liveness detection, short-range RFID/NFC authentication, and configurable multi-factor access policies. All authentication decisions are logged locally and can be synchronised with a backend for auditing and administration.

Safety and robustness are first-class concerns. The lock enforces a formally structured state machine, supports fail-safe and fail-operational modes, and integrates tamper detection and watchdog mechanisms. Firmware integrity and device identity can be protected using hardware-backed cryptographic primitives.

---

## Repository Structure

The repository is organised by system layer:

* `firmware/`
  STM32 firmware for real-time control, safety logic, and sensor/actuator management.

* `edge/`
  Raspberry Pi software for computer vision, ML inference, biometric fusion, and device orchestration.

* `backend/`
  API services for user management, access logs, configuration, and administration.

* `frontend/`
  Web-based dashboard for monitoring system state and managing credentials.

* `hardware/`
  Hardware design files including PCB schematics, power electronics, and RFID/NFC antenna simulations.

* `docs/`
  Architecture diagrams, design notes, threat models, and reports.

---

## Hardware Platform

The reference hardware configuration consists of:

* Raspberry Pi (Linux edge compute and camera interface)
* STM32 microcontroller (real-time control and safety logic)
* CSI camera module (face capture and liveness cues)
* Lock actuator (servo, solenoid, or motor-driven strike)
* RFID/NFC reader with custom 13.56 MHz inductive loop antenna
* Optional audio front-end for DSP-based voice authentication
* Tamper switches, limit switches, and power monitoring circuitry

---

## Security Model

The system is designed with defence in depth. Authentication decisions are local and do not depend on cloud connectivity. Communication channels are encrypted, credentials are protected at rest, and firmware integrity can be enforced using signed updates and secure boot.

Biometric spoofing resistance is addressed through liveness checks, temporal consistency analysis, and optional multi-modal fusion. Physical tampering is detected and handled explicitly by the real-time controller.

---

## Development Tools

Key tools used in this project include:

* STM32CubeIDE for firmware development
* Raspberry Pi OS with Python/C++ for edge software
* OpenCV and ONNX/TensorFlow Lite for vision and ML inference
* CST Microwave Studio for RFID/NFC antenna design
* KiCad and LTspice for hardware and power electronics
* FastAPI and PostgreSQL for backend services
* React (TypeScript) + Vite for the administrative dashboard and monitoring UI
* GitHub Actions for CI and automated testing

---

## Project Status

This project is under active development. Early iterations focus on core functionality and safety, with later iterations extending into hardware acceleration, advanced liveness defences, and behaviour-aware authentication.

---

## Disclaimer

This project is intended for educational and research purposes. It is not certified for use in safety- or security-critical environments without further validation, testing, and compliance work.

---

## Author

Developed as a systems-level engineering project exploring the intersection of embedded systems, computer vision, hardware security, and cyber-physical design.

