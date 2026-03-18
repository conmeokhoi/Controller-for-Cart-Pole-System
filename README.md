# Controller for Cart-Pole System
Application: High-performance motor control board specifically designed for Cart-Pole (Inverted Pendulum) systems, operating on a 24V DC input to drive motors up to 24V.
Communication & Feedback: Integrated UART and I2C interfaces for high-level controller interfacing, along with high-speed isolated encoder inputs using 6N137 optocouplers to ensure precise, noise-free position feedback.
Power Architecture: Hierarchical regulation to 12V/5V/3.3V via TPS5430 Buck Converters (~90% efficiency) for optimal thermal management and stable logic supply.
Galvanic Isolation: Full decoupling of power and logic stages using 6N137 (10Mbps) and B0505S DC-DC modules to eliminate ground loops and EMI in high-frequency switching environments.
Power Stage: 150W H-Bridge featuring IRF3205S MOSFETs (RDS(on)​≈8mΩ) to maximize current density and minimize conduction losses.
Gate Drive Logic: IR2104 drivers with bootstrap circuitry and 520ns hardware dead-time to prevent shoot-through and ensure reliable high-side switching.
Circuit Protection: Integrated overcurrent protection via a slow-blow (T-type) main fuse, designed to handle motor inrush currents while providing fail-safe isolation during short-circuit faults.
