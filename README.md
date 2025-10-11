# CMOS-Circuit-Design-Spice-Simulations-VSDIAT


> This repository documents a hands-on CMOS circuit design workshop using the open-source SKY130 technology node. Over 10 intensive days, participants explore transistor-level modeling, SPICE-based simulations, and robustness analysis across process and voltage variations. Each module combines concise theory, guided simulations, and real-world design exercises, producing reproducible plots and verified design insights. The materials cover MOSFET physics, inverter characterization, noise-margin optimization, dynamic performance, and integration with open-source digital flows like OpenLane and VSDFlow, preparing learners for silicon-ready design.

---
![CMOS Workshop Banner](images/cmos_banner.png)

## 📚 Table of Contents
1. [Mosfet Fundamental & Spice Setup](#mosfet-fundamental--spice-setup)
2. [Velocity saturation & CMOS Inverter Basics](#velocity-saturation--cmos-inverter-basics)
3. [Switching Threshold & Dynamic Behaviour](#switching-threshold--dynamic-behaviour)
4. [Noise-Margin Robustness  Analysis](#noise-margin-robustness-analysis)
5. [Power Supply & Process Variation Evaluation](#power-supply--process-variation-evaluation)

## 🛠️ Tools Used
## 🛠️ Tools Used

![Spice Simulator: LTspice](https://img.shields.io/badge/Spice%20Analysis-LTspice-blue?style=for-the-badge)<br>
![PDK: SKY130](https://img.shields.io/badge/PDK-SKY130%20open%20source-blue?style=for-the-badge)<br>
![Environment: Linux/Windows](https://img.shields.io/badge/Environment-Linux%20/%20Windows-lightgrey?style=for-the-badge)<br>
![Waveform Analysis: LTspice, Python](https://img.shields.io/badge/Waveform%20Analysis-LTspice%2C%20Python-green?style=for-the-badge)<br>
![Documentation: Markdown & GitHub](https://img.shields.io/badge/Documentation-Markdown%20%26%20GitHub-yellow?style=for-the-badge)

## 1️⃣ Mosfet Fundamental & Spice Setup

<details>
  <summary>Click to expand theory and setup details</summary>

### Theory
MOSFET fundamentals, DC characterization, Id-Vds/Vgs curves, channel-length modulation…

### SPICE Setup
- LTspice or NGSPICE simulator  
- NMOS transistor model  
- Voltage sweep: 0-5V

### Observations
- Drain current increases with Vgs  
- Saturation occurs when Vds > Vgs - Vth

</details>





