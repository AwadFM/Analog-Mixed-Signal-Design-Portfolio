# Multi-Stage Analog Audio Effects Pedal (Voice Distorter)

## Overview
This project features the design, simulation, and hardware prototyping of a multi-stage analog signal conditioning circuit optimized for processing human voice frequencies ($250\text{ Hz}$ to $4\text{ kHz}$). Built under tight hardware and power constraints, the circuit processes raw audio signals, introduces controlled nonlinear harmonic distortion, and outputs a conditioned waveform ready for downstream amplification or observation.

---

## System Architecture & Design Constraints

### 1. Single-Supply Power Subsystem
* **Power Source:** Designed entirely around a single $9\text{V}$ battery rail ($V_{CC} = 9\text{V}$, $V_{EE} = 0\text{V}$), eliminating the need for a bulky dual-rail laboratory bench power supply.
* **Virtual Ground Biasing:** Because an AC audio signal naturally swings above and below a $0\text{V}$ reference, operating on a single supply risks clipping the entire negative half-cycle of the wave. To prevent this loss of signal information, a resistive voltage divider network establishes a stable $4.5\text{V}$ "virtual ground" reference. This bias offsets the input signal into the optimal linear operating region of the operational amplifiers.

### 2. Signal Conditioning & Distortion Pipeline
* **Preamplification & Active Filtering:** An LM358 operational amplifier acts as an initial gain stage while establishing a bandpass filter network. The frequency response is tightly constrained to the $250\text{ Hz}$ to $4\text{ kHz}$ voice band to focus harmonic distortion solely on human speech characteristics.
* **Nonlinear Distortion Network:** Incorporates a diode-clipping feedback loop within the op-amp stages. By utilizing the nonlinear threshold characteristics of standard silicon diodes, the circuit cleanly clips the signal peaks, introducing rich harmonic saturation (distortion) to the voice profile.
* **AC Output Coupling (DC Restoration):** At the final stage, a high-pass capacitive filter acts as a DC-blocking network. This strips the $4.5\text{V}$ virtual ground bias away, cleanly shifting the fully distorted audio signal back down to a true $0\text{V}$ baseline for safe integration with external speakers or recording equipment.

---

## Simulation & Verification

The circuit's performance was iteratively modeled, simulated, and verified using **LTspice** before moving to physical construction.

### Simulation Methods
* **Transient Analysis (`.tran`):** Used to observe the precise clipping thresholds of the diode networks and verify that the $4.5\text{V}$ bias prevents clipping at the power rails.
* **AC Sweep (`.ac`):** Used to plot the frequency response and verify the roll-off characteristics of the active bandpass filters, ensuring out-of-band noise below $250\text{ Hz}$ and above $4\text{ kHz}$ is heavily attenuated.

---

## Hardware Implementation
* **Prototyping:** Assembled and benchmarked on a physical breadboard using discrete resistors, capacitors, clipping diodes, and LM358 operational amplifiers.
* **Testing Hardware:** Characterized real-world circuit performance by injecting test signals via a function generator and analyzing output distortion, stage gains, and DC offsets using a digital oscilloscope.

---

## Repository Structure

```text
Voice-Distorter-Pedal/
├── voice_distorter_pedal.asc   # Master LTspice schematic and simulation configuration
├── schematic_capture.png       # High-resolution export of the circuit schematic
└── README.md                   # Technical documentation and project overview