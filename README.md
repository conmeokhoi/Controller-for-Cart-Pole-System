# 150W Isolated Cart-Pole Control Board (STM32)

![Platform](https://img.shields.io/badge/Platform-STM32F103C8T6-blue)
![Power](https://img.shields.io/badge/Power-150W-red)
![PWM](https://img.shields.io/badge/PWM-20kHz-green)

## 📌 Project Overview
[cite_start]This project presents the design and implementation of a specialized control board for an Inverted Pendulum on a Cart (Cart-Pole) system[cite: 3, 4, 37]. [cite_start]It serves as a benchmark for testing cascaded control strategies in highly non-linear, unstable, and under-actuated environments[cite: 38].

## 🛠 Hardware Architecture

### 1. Multi-Stage Power Management
[cite_start]The system operates on a primary **24V DC** input with hierarchical regulation to ensure signal stability and efficiency[cite: 54, 311, 496]:
* [cite_start]**12V Rail**: Regulated by the **L7812** for MOSFET gate drivers[cite: 320, 527].
* [cite_start]**5V Auxiliary Rail**: Stepped down via a high-efficiency **TPS5430 Buck Converter** (~90% efficiency) to power encoders and sensors[cite: 354, 357, 520].
* [cite_start]**Isolated 3.3V Logic**: Utilizes a **B0505S** isolated DC-DC converter and an **AMS1117-3.3** LDO to shield the MCU from power-stage inductive switching noise[cite: 372, 380, 563].

### 2. Motor Drive Stage (150W H-Bridge)
* [cite_start]**MOSFET Selection**: Features four **IRF3205S** N-channel MOSFETs with an exceptionally low $R_{DS(on)} \approx 8 m\Omega$ to minimize conduction losses[cite: 396, 397, 717].
* [cite_start]**Gate Driving**: Employs **IR2104** drivers with bootstrap circuitry to maintain high-side gate saturation[cite: 411, 414, 718].
* [cite_start]**Safety Logic**: Integrated **520ns hardware dead-time** prevents "shoot-through" short circuits[cite: 413, 720].

### 3. Galvanic Isolation
* [cite_start]**High-Speed Paths**: **6N137 optocouplers** (10Mbps) are dedicated to PWM and Encoder signals to prevent propagation delay in the real-time control loop[cite: 444, 445, 781].
* [cite_start]**Isolation Barrier**: Complete physical partitioning between the Power Zone and Clean Logic Zone via the optocoupler bridge[cite: 791, 792].

## 💻 Firmware & Control Logic
[cite_start]Powered by the **STM32F103C8T6** (ARM Cortex-M3) at **72MHz**[cite: 435, 964]:
* [cite_start]**Encoder Processing**: Utilizes hardware Timer Encoder Mode for high-resolution quadrature decoding without CPU overhead[cite: 436, 971].
* [cite_start]**PWM Configuration**: Frequency set at **20kHz** to eliminate audible jitter and provide granular torque control[cite: 1026, 1031]:
  $$f_{PWM} = \frac{f_{CLK}}{(PSC+1) \times (ARR+1)} = \frac{72,000,000}{36 \times 100} = 20,000 \text{ Hz}$$
* [cite_start]**Control Algorithm**: Implements a **Cascaded PID** structure where the outer loop manages cart position and the inner loop stabilizes the pendulum angle[cite: 168, 170, 172].

## 🔍 Failure Analysis (Root Cause)
[cite_start]During integration, a hardware breakdown occurred in the H-bridge[cite: 1212]. [cite_start]Forensic analysis identified a missing connection between the **VS pin** (Pin 6) of the IR2104 drivers and the **source terminals** of the high-side MOSFETs[cite: 1225, 1226]. [cite_start]This led to a "floating gate" state, causing a direct short-circuit from the 24V rail to Ground[cite: 1229, 1231].

---
[cite_start]**Author**: Nguyễn Huỳnh Khôi [cite: 6]
**Supervised by**: PhD. [cite_start]Võ Lâm Chương [cite: 5]
[cite_start]**University**: Ho Chi Minh City University of Technology and Engineering (HCMUTE) [cite: 1, 2]
