# Grid-Forming Inverter System: Droop Control and Power Management

![Grid-Forming System Architecture](Docs/LaTeX_Source/Images/Full_Sys.svg)

## 🚀 Overview
This repository contains the mathematical modeling, control design, and MATLAB/Simulink simulation of a **Grid-Forming Voltage Source Inverter (GFM-VSI)**. Unlike traditional grid-following inverters that inject current into a stiff grid, grid-forming inverters act as controlled AC voltage sources that independently establish the local grid voltage and frequency.

This project implements a robust **Droop Control** strategy ($P-\omega$ and $Q-V$) paired with a cascaded multi-loop architecture in the $dq0$ synchronous reference frame, demonstrating the inverter's readiness for isolated microgrid integration and decentralized active/reactive power sharing.

## 🧠 Advanced Control Strategy
The control structure mimics the physical dynamics of a traditional Synchronous Generator (SG) without relying on a conventional Phase-Locked Loop (PLL) in steady-state operation.

### 1. Power Calculation and Droop Control
*   Instantaneous active ($P$) and reactive ($Q$) powers are calculated and passed through first-order low-pass filters ($30 / (s + 30)$).
*   **Frequency Droop ($P-\omega$):** Adjusts the internal angular frequency based on active power variations ($\omega = \omega^* - m_p (P - P^*)$). Integrating this frequency generates the internal phase angle ($\theta$) for all coordinate transformations.
*   **Voltage Droop ($Q-V$):** Generates the internal voltage magnitude reference based on reactive power demand ($V_{ref} = V^* - n_q (Q)$).

### 2. Cascaded Voltage & Current Loops ($dq0$ Frame)
*   **Outer Voltage Loop:** A PI controller regulates the filter capacitor voltage to track the generated $V_{ref}$, outputting reference currents ($I_d^*, I_q^*$) for the inner loop.
*   **Inner Current Loop:** A fast PI controller regulates the inverter output currents. To ensure independent control of the $d$ and $q$ axes, **Feedforward Decoupling Terms** ($\pm \omega L \cdot i$) are implemented.
*   **SPWM Generation:** The decoupled signals are transformed back to the $abc$ frame and compared against a $5\text{ kHz}$ carrier wave to generate IGBT gate pulses.

## ⚙️ System Performance Analysis
The simulation validates the Grid-Forming Inverter under dynamic load sharing conditions:

*   **Autonomous Power Tracking:** The droop controller successfully drives the inverter to track and settle at the designated active power setpoint ($10\text{ kW}$) with zero steady-state error and excellent transient damping.
*   **Voltage Quality:** A third-order LCL filter effectively suppresses the $5\text{ kHz}$ switching harmonics. The cascaded inner loops ensure the generation of a stiff, harmonic-free sinusoidal output voltage regardless of load fluctuations.

### 📊 Dynamic Response Highlight
| Active Power Output Tracking | Zoomed-in Harmonic-Free Waveforms |
| :---: | :---: |
| ![P Scope](Docs/LaTeX_Source/Images/Scope1.jpg) | ![Waveforms](Docs/LaTeX_Source/Images/Scope_Zoomed.jpg) |

## 📂 Repository Structure
*   `Simulation/`: Contains the modular MATLAB/Simulink model (`.slx`) divided into clearly defined subsystems (Power & Droop Control, Inner Control Loops, Signal Transformation).
*   `Docs/`: Contains the comprehensive project report (`grid-forming-inverter-control.pdf`) detailing the equations, control theory, and waveform analyses.
*   `Docs/LaTeX_Source/`: Contains the LaTeX source code and the `Images/` subfolder with all high-resolution vector diagrams and scope plots.

## 👨‍💻 Author
**Abd El-Rahman Muhammad Saad Muhammad**
*   **University:** Alexandria University
*   **Department:** Electrical Engineering
