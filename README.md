# CMOS-Circuit-Design-Spice-Simulations-VSDIAT


> This repository documents a hands-on CMOS circuit design workshop using the open-source SKY130 technology node. Over 10 intensive days, participants explore transistor-level modeling, SPICE-based simulations, and robustness analysis across process and voltage variations. Each module combines concise theory, guided simulations, and real-world design exercises, producing reproducible plots and verified design insights. The materials cover MOSFET physics, inverter characterization, noise-margin optimization, dynamic performance, and integration with open-source digital flows like OpenLane and VSDFlow, preparing learners for silicon-ready design.

---
![CMOS Workshop Banner](images/cmos_banner.png)

## 📚 Table of Contents
1. [Mosfet Fundamental & Spice Setup](#mosfet-fundamental--spice-setup)
2. [Velocity saturation & CMOS Inverter Basics](#velocity-saturation--cmos-inverter-basics)
3. [Switching Threshold & Dynamic Behaviour](#switching-threshold--dynamic-behaviour)
4. [Noise-Margin Robustness  Analysis](#noise-margin-robustness-analysis)
5. [Power Supply & Process Variation Evaluation](#power-supply--pr0cess-variation-evaluation)

## 🛠️ Tools Used
## 🛠️ Tools Used

![Spice Simulator: LTspice](https://img.shields.io/badge/Spice%20Analysis-LTspice-blue?style=for-the-badge)<br>
![PDK: SKY130](https://img.shields.io/badge/PDK-SKY130%20open%20source-blue?style=for-the-badge)<br>
![Environment: Linux/Windows](https://img.shields.io/badge/Environment-Linux%20/%20Windows-lightgrey?style=for-the-badge)<br>
![Waveform Analysis: LTspice, Python](https://img.shields.io/badge/Waveform%20Analysis-LTspice%2C%20Python-green?style=for-the-badge)<br>
![Documentation: Markdown & GitHub](https://img.shields.io/badge/Documentation-Markdown%20%26%20GitHub-yellow?style=for-the-badge)

## 1️⃣ Mosfet Fundamental & Spice Setup



### NgspiceSky130_D1SK1 - Introduction to Circuit Design and SPICE Simulations

<details>
<summary><b>L1 - Why do we need SPICE Simulations?</b></summary>

SPICE simulations are used to analyze and verify circuit behavior before fabrication by applying input signals and observing outputs. They help designers evaluate parameters like voltage, current, delay, power, and noise margins, reducing design errors and improving circuit reliability.

SPICE simulations help evaluate circuit performance across a large number of operating points and conditions. They provide accurate values for parameters such as voltage, current, delay, and power, which would be difficult and time-consuming to calculate manually.

</details>

<details>
<summary><b>L2-Introduction to basic element in circuit design-NMOS</b></summary>



<p align="center">
  <img src="L2_1.png" width="800">
</p>

At first we will keep the Vgs=0, means source and drain terminal both are grounded. Body is also grounded. P-substrate and n+ act as PN junction diode, so there is a high resistance. There is no channel formation. For nmos we apply a positive Gate to source voltage. When the gate voltage is zero, no inversion channel exists between the source and drain. Both source-substrate and drain-substrate junctions remain unbiased, resulting in very high resistance between source and drain.

<p align="center">
  <img src="L2_2.png" width="800">
</p>

As the gate voltage (VGS) increases, electrons begin to accumulate beneath the gate oxide. At a particular gate voltage, the semiconductor surface under the gate inverts from p-type to n-type, resulting in the formation of an inversion channel between the source and drain. This minimum gate voltage required to create the conducting channel is called the threshold voltage (VTH). The formation of this inversion layer is known as strong inversion.

<p align="center">
  <img src="L2_3.png" width="800">
</p>


Further increase in gate voltage beyond the threshold voltage strengthens the inversion layer and forms a continuous n-channel between source and drain. The conductivity of this channel is controlled by the applied gate voltage.

</details>

<details>
<summary><b>L3 - Strong inversion and threshold voltage</b></summary>
  <p align="center">
  <img src="L2_3.png" width="800">
</p>
  
Once the inversion channel is formed, any further increase in the gate voltage (VGS) attracts more electrons towards the gate-substrate interface, strengthening the n-type inversion layer. The region beneath the gate, which was originally part of the p-type substrate, effectively behaves as an n-type channel, providing a continuous conductive path between the source and drain. However, the formation of the channel alone does not result in current flow. A drain-to-source voltage (VDS) must be applied to create an electric field that drives electrons from the source towards the drain. Thus, VGS forms the channel, while VDS enables current conduction through the channel.

</details>

<details>
<summary><b>L4 - Threshold voltage with positive substrate potential</b></summary>

<p align="center">
  <img src="L4_1.png" width="800">
</p>

When VSB = 0, the inversion channel forms uniformly along the gate region once the gate voltage reaches the threshold voltage (VTO). As a result, channel formation occurs relatively earlier.

When a positive VSB is applied, the source-to-substrate junction becomes more reverse biased, increasing the depletion region width near the source. Due to this increased depletion charge, a higher gate voltage is required to attract sufficient electrons and create the inversion layer. Consequently, the threshold voltage increases from VTO to VTO + V1.

This phenomenon is known as the Body Effect, where the threshold voltage of a MOSFET increases with increasing source-to-body voltage (VSB).

<p align="center">
  <img src="L4_2.png" width="800">
</p>

The threshold voltage is no longer constant and becomes a function of VSB. The body effect equation quantifies the increase in threshold voltage caused by the applied source-to-body bias, with parameters such as body-effect coefficient (γ) and Fermi potential (ϕF) determined by the manufacturing process.



</details>

---

### NgspiceSky130_D1SK2 - NMOS Resistive and Saturation Region

<details>
<summary><b>L1 - Resistive region of operation with small drain-source voltage</b></summary>


After the gate voltage (VGS) exceeds the threshold voltage (VTH), a continuous inversion channel is formed between the source and drain. This channel acts as a path through which electrons can travel.

Now let us apply a very small drain-to-source voltage (VDS), for example 0.05 V, while keeping VGS > VTH. Since the drain voltage is very small compared to the gate voltage, the voltage drop along the channel is also very small.

At the source end of the channel, the gate-to-channel voltage is approximately:
VGS - 0, because the channel voltage is nearly zero at the source.

As we move towards the drain, the channel potential gradually increases from 0 V to VDS. Therefore, at any point x along the channel, the effective gate-to-channel voltage becomes:
VGS - V(x)
Since VDS is very small, the value of VGS − V(x) remains greater than the threshold voltage throughout the entire channel length. As a result, the inversion layer exists uniformly from source to drain.

Because the channel is continuous and almost uniform, it behaves like a resistor whose resistance is controlled by the gate voltage. Increasing VGS attracts more electrons into the channel, reducing its resistance. Decreasing VGS reduces the channel charge, increasing its resistance.

The movement of electrons occurs due to the electric field created by the small drain-to-source voltage. Since the channel behaves similarly to a resistor, the drain current increases approximately linearly with VDS.

This is why this region is called the Linear Region or Resistive Region of Operation.
### Key Observations
- A continuous inversion channel exists from source to drain.
- VDS is small compared to VGS.
- The channel charge is almost uniform throughout the channel.
- Drain current increases linearly with VDS.
- The MOSFET behaves like a voltage-controlled resistor.
- Increasing VGS reduces channel resistance and increases current flow.

### **Conclusion**

When VGS > VTH and VDS is small, the MOSFET operates in the resistive (linear) region. The channel is fully formed across the entire device length, and the drain current is approximately proportional to the applied drain voltage, making the transistor behave like a controllable resistor.

</details>

<details>
<summary><b>L2 - Drift Current Theory</b></summary>

Your notes here.

</details>

<details>
<summary><b>L3 - Drain Current Model for Linear Region</b></summary>

Your notes here.

</details>

<details>
<summary><b>L4 - SPICE Verification</b></summary>

Your notes here.

</details>

---

### NgspiceSky130_D1SK3 - Introduction to SPICE

<details>
<summary><b>L1 - Basic SPICE Setup</b></summary>

Your notes here.

</details>

<details>
<summary><b>L2 - Circuit Description in SPICE Syntax</b></summary>

Your notes here.

</details>

<details>
<summary><b>L3 - Technology Parameters</b></summary>

Your notes here.

</details>

<details>
<summary><b>L4 - First SPICE Simulation</b></summary>

Your notes here.

</details>





