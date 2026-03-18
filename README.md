# 150W Isolated Cart-Pole Control Board (STM32)

![Platform](https://img.shields.io/badge/Platform-STM32F103C8T6-blue)
![Power](https://img.shields.io/badge/Power-150W-red)
![PWM](https://img.shields.io/badge/PWM-20kHz-green)

## 📌 Project Overview
This project presents the design and implementation of a specialized control board for an Inverted Pendulum on a Cart (Cart-Pole) system. It serves as a benchmark for testing cascaded control strategies in highly non-linear, unstable, and under-actuated environments.

## 🛠 Hardware Architecture

### 1. Multi-Stage Power Management
The system operates on a primary **24V DC** input with hierarchical regulation to ensure signal stability and efficiency:
* **12V Rail**: Regulated by the **L7812** for MOSFET gate drivers.
* **5V Auxiliary Rail**: Stepped down via a high-efficiency **TPS5430 Buck Converter** (~90% efficiency) to power encoders and sensors.
* **Isolated 3.3V Logic**: Utilizes a **B0505S** isolated DC-DC converter and an **AMS1117-3.3** LDO to shield the MCU from power-stage inductive switching noise.

### 2. Motor Drive Stage (150W H-Bridge)
* **MOSFET Selection**: Features four **IRF3205S** N-channel MOSFETs with an exceptionally low $R_{DS(on)} \approx 8 m\Omega$ to minimize conduction losses.
* **Gate Driving**: Employs **IR2104** drivers with bootstrap circuitry to maintain high-side gate saturation.
* **Safety Logic**: Integrated **520ns hardware dead-time** prevents "shoot-through" short circuits.

### 3. Galvanic Isolation
* **High-Speed Paths**: **6N137 optocouplers** (10Mbps) are dedicated to PWM and Encoder signals to prevent propagation delay in the real-time control loop.
* **Isolation Barrier**: Complete physical partitioning between the Power Zone and Clean Logic Zone via the optocoupler bridge.

## 💻 Firmware & Control Logic
Powered by the **STM32F103C8T6** (ARM Cortex-M3) at **72MHz**:
* **Encoder Processing**: Utilizes hardware Timer Encoder Mode for high-resolution quadrature decoding without CPU overhead.
* **PWM Configuration**: Frequency set at **20kHz** to eliminate audible jitter and provide granular torque control:
  $$f_{PWM} = \frac{f_{CLK}}{(PSC+1) \times (ARR+1)} = \frac{72,000,000}{36 \times 100} = 20,000 \text{ Hz}$$
* **Control Algorithm**: Implements a **Cascaded PID** structure where the outer loop manages cart position and the inner loop stabilizes the pendulum angle.

## 🔍 Failure Analysis (Root Cause)
During integration, a hardware breakdown occurred in the H-bridge. Forensic analysis identified a missing connection between the **VS pin** (Pin 6) of the IR2104 drivers and the **source terminals** of the high-side MOSFETs.This led to a "floating gate" state, causing a direct short-circuit from the 24V rail to Ground.

---
**Author**: Nguyễn Huỳnh Khôi 
**Supervised by**: PhD. Võ Lâm Chương 
**University**: Ho Chi Minh City University of Technology and Engineering (HCMUTE)
