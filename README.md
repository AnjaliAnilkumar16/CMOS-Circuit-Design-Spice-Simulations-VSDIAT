# CMOS-Circuit-Design-Spice-Simulations-VSDIAT


> This repository documents a hands-on CMOS circuit design workshop using the open-source SKY130 technology node. Over 10 intensive days, participants explore transistor-level modeling, SPICE-based simulations, and robustness analysis across process and voltage variations. Each module combines concise theory, guided simulations, and real-world design exercises, producing reproducible plots and verified design insights. The materials cover MOSFET physics, inverter characterization, noise-margin optimization, dynamic performance, and integration with open-source digital flows like OpenLane and VSDFlow, preparing learners for silicon-ready design.

---


<img width="1774" height="887" alt="ChatGPT Image Jun 6, 2026, 04_12_01 PM" src="https://github.com/user-attachments/assets/56a66196-37f0-4830-a5c6-e3a1bf299636" />


## 📚 Table of Contents
1. [Mosfet Fundamental & Spice Setup](#mosfet-fundamental--spice-setup)
2. [Velocity saturation & CMOS Inverter Basics](#velocity-saturation--cmos-inverter-basics)
3. [Switching Threshold & Dynamic Behaviour](#switching-threshold--dynamic-behaviour)
4. [Noise-Margin Robustness  Analysis](#noise-margin-robustness-analysis)
5. [Power Supply & Process Variation Evaluation](#power-supply--pr0cess-variation-evaluation)

## 🛠️ Tools Used


![Spice Simulator: LTspice](https://img.shields.io/badge/Spice%20Analysis-LTspice-blue?style=for-the-badge)<br>
![PDK: SKY130](https://img.shields.io/badge/PDK-SKY130%20open%20source-blue?style=for-the-badge)<br>
![Environment: Linux/Windows](https://img.shields.io/badge/Environment-Linux%20/%20Windows-lightgrey?style=for-the-badge)<br>
![Waveform Analysis: LTspice, Python](https://img.shields.io/badge/Waveform%20Analysis-LTspice%2C%20Python-green?style=for-the-badge)<br>
![Documentation: Markdown & GitHub](https://img.shields.io/badge/Documentation-Markdown%20%26%20GitHub-yellow?style=for-the-badge)

## Day 1️⃣ Mosfet Fundamental & Spice Setup



### NgspiceSky130_D1SK1 - Introduction to Circuit Design and SPICE Simulations

<details>
<summary><b>L1 - Why do we need SPICE Simulations?</b></summary>

SPICE simulations are used to analyze and verify circuit behavior before fabrication by applying input signals and observing outputs. They help designers evaluate parameters like voltage, current, delay, power, and noise margins, reducing design errors and improving circuit reliability.

SPICE simulations help evaluate circuit performance across a large number of operating points and conditions. They provide accurate values for parameters such as voltage, current, delay, and power, which would be difficult and time-consuming to calculate manually.

</details>

<details>
<summary><b>L2-Introduction to basic element in circuit design-NMOS</b></summary>

<img width="602" height="236" alt="L2_1" src="https://github.com/user-attachments/assets/13aab8c5-bbcb-43b2-b4bb-53682a0ad3d3" />


At first we will keep the Vgs=0, means source and drain terminal both are grounded. Body is also grounded. P-substrate and n+ act as PN junction diode, so there is a high resistance. There is no channel formation. For nmos we apply a positive Gate to source voltage. When the gate voltage is zero, no inversion channel exists between the source and drain. Both source-substrate and drain-substrate junctions remain unbiased, resulting in very high resistance between source and drain.


<img width="602" height="233" alt="L2_2" src="https://github.com/user-attachments/assets/4b72ed8c-c3c5-43e2-b7d7-95991ca190e1" />


As the gate voltage (VGS) increases, electrons begin to accumulate beneath the gate oxide. At a particular gate voltage, the semiconductor surface under the gate inverts from p-type to n-type, resulting in the formation of an inversion channel between the source and drain. This minimum gate voltage required to create the conducting channel is called the threshold voltage (VTH). The formation of this inversion layer is known as strong inversion.



<img width="602" height="296" alt="L2_3" src="https://github.com/user-attachments/assets/91a449be-a04f-4d2f-a81b-afef35212bca" />

Further increase in gate voltage beyond the threshold voltage strengthens the inversion layer and forms a continuous n-channel between source and drain. The conductivity of this channel is controlled by the applied gate voltage.

</details>

<details>
<summary><b>L3 - Strong inversion and threshold voltage</b></summary>
 

<img width="602" height="296" alt="L2_3" src="https://github.com/user-attachments/assets/c5eb9de0-7cf6-4a15-ae7a-f1da2a3641fc" />


  
Once the inversion channel is formed, any further increase in the gate voltage (VGS) attracts more electrons towards the gate-substrate interface, strengthening the n-type inversion layer. The region beneath the gate, which was originally part of the p-type substrate, effectively behaves as an n-type channel, providing a continuous conductive path between the source and drain. However, the formation of the channel alone does not result in current flow. A drain-to-source voltage (VDS) must be applied to create an electric field that drives electrons from the source towards the drain. Thus, VGS forms the channel, while VDS enables current conduction through the channel.

</details>

<details>
<summary><b>L4 - Threshold voltage with positive substrate potential</b></summary>


<img width="602" height="252" alt="L4_1" src="https://github.com/user-attachments/assets/2ad9f636-ffdf-43db-b13e-7d066d79495a" />


When VSB = 0, the inversion channel forms uniformly along the gate region once the gate voltage reaches the threshold voltage (VTO). As a result, channel formation occurs relatively earlier.

When a positive VSB is applied, the source-to-substrate junction becomes more reverse biased, increasing the depletion region width near the source. Due to this increased depletion charge, a higher gate voltage is required to attract sufficient electrons and create the inversion layer. Consequently, the threshold voltage increases from VTO to VTO + V1.

This phenomenon is known as the Body Effect, where the threshold voltage of a MOSFET increases with increasing source-to-body voltage (VSB).



<img width="602" height="313" alt="L4_2" src="https://github.com/user-attachments/assets/afacec99-81b7-49b8-a22b-e7c5acc2ba46" />


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



<img width="602" height="286" alt="DISK2_L1_1" src="https://github.com/user-attachments/assets/cdbd93c0-b345-4c37-9497-dc026843ae79" />




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

In the previous section, we saw that when VGS > VTH and VDS is small, a continuous inversion channel is formed between the source and drain. However, the presence of a channel alone does not guarantee current flow. For current to flow, there must be a driving force that moves electrons from one end of the channel to the other.

At any point x along the channel, the channel potential is represented by V(x). Therefore, the effective gate-to-channel voltage at that point becomes:
VGS - V(x)

At the source end:
V(x) = 0

Therefore, VGS - V(x) = VGS At the drain end: V(x) = VDS

Therefore, VGS - V(x) = VGS - VDS
This shows that the gate-to-channel voltage is not constant throughout the channel. It gradually decreases from the source side to the drain side, creating a non-uniform charge distribution in the inversion layer.



<img width="602" height="286" alt="DISK2_L2_1" src="https://github.com/user-attachments/assets/7aae110f-f600-4069-989c-07e9d9b59c57" />


**Observation:**
The effective gate-to-channel voltage varies from VGS at the source to VGS − VDS at the drain. As a result, the inversion charge density is slightly higher near the source and lower near the drain.

To quantify the amount of charge available for conduction, we analyze the inversion charge density at any point x along the channel. The inversion charge depends on the gate oxide capacitance (Cox) and the local gate-to-channel voltage.


<img width="602" height="288" alt="DISK2_L2_2" src="https://github.com/user-attachments/assets/6a818620-969b-4e72-8650-9712d36d33b8" />


**Observation:**
A thinner oxide results in a larger oxide capacitance, allowing the gate to exert stronger control over the channel charge.

Now let us understand how current actually flows.

In semiconductor devices, there are two primary current transport mechanisms:

Drift Current
Diffusion Current

In a MOSFET operating in the linear region, the dominant current component is drift current.

Drift current arises due to the electric field created by the drain-to-source voltage (VDS). Since the channel potential changes from 0 V at the source to VDS at the drain, a potential gradient exists across the channel.

This potential gradient produces an electric field:
Electric Field = dV/dx
which drives electrons from the source towards the drain


<img width="602" height="335" alt="DISK2_L2_3" src="https://github.com/user-attachments/assets/de4fd9e6-5862-444d-9412-3d3b46a03f16" />


**Observation:**
The drift current exists because of the potential difference between source and drain. Without VDS, the channel would exist but no net electron flow would occur.


## The drain current can now be understood as the combined effect of:
- The amount of charge available in the channel.
- The velocity with which those charge carriers move

Mathematically,
Drain Current = Charge Density × Carrier Velocity × Channel Width
## Thus, the drain current increases when:
- More inversion charge is induced by increasing VGS.
- Carrier velocity increases due to a stronger electric field.
- Channel width (W) is increased.

## Key Observations
- The effective gate-to-channel voltage varies along the channel.
- Inversion charge density is maximum near the source and minimum near the drain.
- Oxide capacitance determines how effectively the gate controls channel charge.
- Drift current is caused by the electric field generated by VDS.
- Drain current depends on both carrier charge and carrier velocity.
- Increasing VGS increases inversion charge and hence drain current.

**Conclusion**

Although the inversion channel is formed by VGS, the actual current flow is driven by the electric field generated by VDS. This drift mechanism forms the basis for deriving the MOSFET drain current equations used in the linear and saturation regions of operation.


</details>

<details>
<summary><b>L3 - Drain Current Model for Linear Region</b></summary>

In the previous section, we learned that a small drain-to-source voltage (VDS) creates an electric field along the channel. This electric field causes electrons to move from the source towards the drain, producing a drift current.

When VGS > VTH, an inversion channel is already formed between the source and drain. The amount of charge present in this channel is not uniform and depends on the local gate-to-channel voltage at every point along the channel.

<img width="1280" height="1042" alt="DISK2_L3_1" src="https://github.com/user-attachments/assets/82bcc334-4efd-4963-be73-bb7030e01675" />

**Observation:**

At any point x along the channel, the inversion charge density depends on:
VGS - V(x)
Since the channel voltage gradually increases from the source to the drain, the available channel charge also varies along the channel length.
To calculate the drain current, we first determine the amount of inversion charge available at any point in the channel.

**Observation:**

The gate oxide acts like a capacitor. A larger oxide capacitance allows the gate to induce more charge in the channel, resulting in higher drain current.
## Now let us derive the drain current expression. From a device physics point of view, current depends on two things:
- The amount of charge available for conduction.
- The velocity with which those charges move.
Therefore, Drain Current = Charge × Velocity
The carrier velocity is proportional to the electric field inside the channel:
Velocity = Mobility × Electric Field
Since the electric field is created by the drain voltage, the drain current becomes dependent on both VGS and VDS.

By substituting the charge and velocity expressions and integrating across the entire channel length (L), we obtain the drain current equation:



<img width="892" height="1280" alt="DISK2_L3_2" src="https://github.com/user-attachments/assets/ba12e56d-183c-4d2d-880a-17d13be9fdf7" />


</details>

<details>
<summary><b>L4 - SPICE conclusion to resistive operation</b></summary>

So far, we have derived the drain current equation for the linear region and understood the physical behavior of the MOSFET. The next step is to verify whether the theoretical equations accurately predict the device behavior.

The drain current equation in the linear region is:
ID = kn[(VGS - VTH)VDS - VDS²/2]
This equation indicates that the drain current depends on both the gate voltage (VGS) and the drain voltage (VDS). To understand the effect of these parameters, we perform SPICE simulations by varying both voltages systematically.

The transistor threshold voltage is assumed to be:
Different gate voltages are selected:
VGS = 0.5V, 1V, 1.5V, 2V, 2.5V
The corresponding overdrive voltages become:
VGS - VTH = 0.05V, 0.55V, 1.05V, 1.55V, 2.05V
**Observation:**

For a transistor to remain in the linear (resistive) region, the following condition must be satisfied:
VDS < (VGS - VTH)
Therefore, for every selected value of VGS, the drain voltage can only be swept up to the corresponding (VGS − VTH) value while maintaining linear-region operation.
The objective of the simulation is to determine how the drain current changes with increasing drain voltage for different gate voltages. For each value of VGS, VDS is swept from 0 V up to (VGS − VTH) and the corresponding drain current is calculated using SPICE.

## This allows us to:
- Verify the linear-region drain current equation.
- Observe the dependence of drain current on gate voltage.
- Understand how increasing gate voltage strengthens the channel.
- Compare theoretical calculations with simulated results.

**Why SPICE Simulation?**

Performing these calculations manually for multiple combinations of VGS and VDS is time-consuming and prone to errors. SPICE automates this process by solving the device equations numerically and generating accurate current-voltage characteristics.

SPICE simulation is used to verify the drain current behavior predicted by the linear-region MOSFET model. By sweeping VDS for different values of VGS, we can observe how the drain current varies and confirm the validity of the theoretical drain current equation in the resistive region.
</details>

<details>
<summary><b>L4 - Pinch-off region condition</b></summary> 

Imagine we have an NMOS transistor with:

VGS = 1 V. VTH = 0.45 V. Initially, let us keep VDS very small.


The effective channel voltage at any point is:


VGS - V(x)


For a channel to exist, this value must always be greater than the threshold voltage: VGS - V(x) > VTH.


As long as this condition is satisfied throughout the entire channel, electrons can travel continuously from source to drain.



<img width="602" height="306" alt="DISK2_L5_1" src="https://github.com/user-attachments/assets/7566bd29-9703-4e10-bb8d-c15c24e36317" />


At first, when VDS = 0.05 V, VGS - VDS = 1 - 0.05 = 0.95 V


Since: 0.95 V > 0.45 V, the channel exists even near the drain.


Similarly, for:


VDS = 0.15 V


VDS = 0.25 V


VDS = 0.35 V


VDS = 0.45 V


the channel voltage remains greater than the threshold voltage. Therefore, a continuous inversion layer exists from source to drain. The MOSFET continues to operate in the linear (resistive) region.


Now let us gradually increase VDS. As VDS increases, the channel voltage near the drain keeps reducing because:


Channel Voltage = VGS - VDS


Eventually we reach: VGS - VDS = VTH Substituting values:


1 - VDS = 0.45 which gives: VDS = 0.55 V




<img width="602" height="294" alt="DISK2_L5_2" src="https://github.com/user-attachments/assets/6c3c5d2e-3dfb-4bb9-aaaf-550dc70393d2" />



At this exact point, VGS - VDS = VTH 


The inversion charge at the drain end becomes almost zero. The conducting channel becomes extremely thin near the drain. This condition is called the Pinch-Off Point. The channel is just about to disappear near the drain.


If we further increase VDS beyond 0.55 V: VGS - VDS < VTH


For example:
VDS = 0.65 V. Then: VGS - VDS = 0.35 V which is less than VTH. Now there is no inversion layer near the drain side. 


<img width="602" height="288" alt="DISK2_L5_3" src="https://github.com/user-attachments/assets/c235836c-76b6-4cca-9512-4ab432ac6b5e" />


The channel is no longer continuous. A depletion region appears near the drain. This region is called the Pinch-Off Region. The condition for pinch-off is: VGS - VDS ≤ VTH



**If the channel is broken, why doesn't the current stop?**


This confuses almost everyone the first time. Imagine a river flowing through a narrow pipe. If the pipe suddenly becomes very narrow at one location, water does not stop flowing. Instead, the water accelerates through that narrow region. Something similar happens inside the MOSFET. Electrons travel from the source through the inversion channel. When they reach the pinch-off point, they enter a region having a very strong electric field created by the drain voltage. This electric field immediately sweeps the electrons into the drain.


## Therefore:
- Electrons still reach the drain.
- Current does not become zero.
- The pinch-off region simply grows larger as VDS increases.


**Why Does the Channel Voltage Become Constant?**


At the pinch-off point: VGS - V(x) = VTH


Rearranging: V(x) = VGS - VTH


Notice something interesting. 
## The right side contains only: 
- VGS
- VTH


Both are fixed values.Therefore:V(x) = Constant


This means the voltage at the pinch-off point cannot increase further. Any additional drain voltage applied beyond this point does not increase the channel voltage. Instead, it appears across the depletion region near the drain.



## As a result:
- Pinch-off point moves slightly towards the source.
- Depletion region widens.
- Drain current becomes almost independent of VDS.
This marks the beginning of the saturation region.

## Key Observations
- Channel exists only when VGS − V(x) > VTH.
- Increasing VDS reduces the channel voltage near the drain.
- Pinch-off occurs when VGS − VDS = VTH.
- Current does not stop after pinch-off.
- Electrons are swept through the depletion region by the strong electric field near the drain.
- Additional VDS increases the pinch-off region rather than increasing channel voltage.
- This is the starting point of MOSFET saturation operation.

**Conclusion**

As VDS increases, the inversion channel gradually becomes thinner near the drain. When the drain-end channel voltage falls to the threshold voltage, pinch-off occurs. Although the channel is no longer continuous, electrons continue to flow due to the strong electric field in the pinch-off region. This marks the transition from the linear region to the saturation region of MOSFET operation.

</details>

<details>
<summary><b>L6 - Drain current model for saturation region of operation</b></summary>

In the previous section, we learned that as the drain voltage (VDS) increases, the channel near the drain becomes thinner and thinner. Eventually, a point is reached where: VGS - VDS = VTH

At this condition, the inversion layer at the drain end disappears and the transistor enters the pinch-off condition. The corresponding drain voltage is: VDS(sat) = VGS - VTH


This marks the beginning of the saturation region.


<img width="602" height="316" alt="DISK2_L6_1" src="https://github.com/user-attachments/assets/76e611f4-60df-47f8-93da-abd9a9d7d9e7" />


At pinch-off, the voltage across the conductive channel becomes fixed and equal to: VGS - VTH. Any additional drain voltage does not appear across the channel. Instead, it appears across the depletion region near the drain.


<img width="602" height="309" alt="DISK2_L6_2" src="https://github.com/user-attachments/assets/ba5331ba-b003-4177-9f34-ecda6f297a07" />

The drain current is no longer a function of VDS.

## Instead, it depends only on:

- Gate voltage (VGS)
- Threshold voltage (VTH)
- Device parameters (kn)


**Channel Length Modulation**

Let us increase VDS even further beyond saturation.

The drain-substrate junction is reverse biased. As the reverse bias increases, the depletion region near the drain expands.

This enlarged depletion region occupies part of the original channel.

This is why the current appears to become constant in the saturation region.

## As a result:

- Effective channel length decreases.
- Conductive channel becomes slightly shorter.
- Channel resistance decreases.
- Drain current increases slightly.

<img width="602" height="318" alt="DISK2_L6_3" src="https://github.com/user-attachments/assets/30ed1b27-5d6b-4f14-b58a-bd2251081958" />


**Conclusion**

Once pinch-off occurs, the MOSFET enters the saturation region. The channel voltage becomes fixed at VGS − VTH, resulting in an almost constant drain current. However, due to channel length modulation, the effective channel length decreases as VDS increases, causing a small increase in drain current. This makes the practical saturation current slightly dependent on VDS, unlike the ideal MOSFET model.

</details>

---

### NgspiceSky130_D1SK3 - Introduction to SPICE

<details>
<summary><b>L1 - Basic SPICE Setup</b></summary>



<img width="602" height="328" alt="DISK3_L1_1" src="https://github.com/user-attachments/assets/7ecb52e0-496b-4d02-b264-3d91f9776b72" />


So far, we have understood how an NMOS transistor behaves physically. We studied channel formation, linear region operation, saturation, pinch-off, and channel length modulation.

Now imagine that we want to predict the behavior of a transistor before manufacturing it. Calculating every voltage and current manually would be extremely difficult, especially when a modern chip contains millions of transistors.

This is where SPICE comes into the picture.

SPICE (Simulation Program with Integrated Circuit Emphasis) acts like a virtual laboratory. Instead of fabricating the transistor and then testing it, we can simulate its behavior on a computer.

SPICE takes two important inputs:

- The circuit description (SPICE Netlist)
- Technology parameters provided by the foundry

Using these inputs, SPICE solves the transistor equations and predicts the electrical behavior of the circuit.

Think of SPICE as a calculator that already knows all the complicated transistor physics and performs the calculations automatically.

**What are Nodes in SPICE?**

<img width="602" height="303" alt="DISK3_L1_2" src="https://github.com/user-attachments/assets/1058a125-658c-4075-b288-dfcf4cf62490" />



Before SPICE can analyze a circuit, it must know how every component is connected.

For this purpose, every connection point is assigned a name called a Node.

For example:

- VDD
- VSS
- IN
- OUT
- n1

Each node represents a unique electrical connection point.

When SPICE runs a simulation, it calculates the voltage at every node and determines how current flows between them.

Without nodes, SPICE would not know how the circuit is interconnected.
</details>

<details>
<summary><b>L2 - Circuit Description in SPICE Syntax</b></summary> 

<img width="602" height="229" alt="DISK3_L2_1" src="https://github.com/user-attachments/assets/5a95551c-00df-4d96-aa39-0465b293bdfc" />


Once the nodes are defined, we must tell SPICE what components are present in the circuit.

Unlike schematic diagrams, SPICE uses text-based commands.

As shown in the figure, the transistor, resistor, and voltage source are described using simple statements.

For example:M1 vdd n1 0 0 nmos W=1.8u L=1.2u

This tells SPICE:

- M1 is an NMOS transistor
- Drain connected to vdd
- Gate connected to n1
- Source connected to ground
- Bulk connected to ground
- Width = 1.8 μm
- Length = 1.2 μm


Similarly, **R1 in n1 55** defines a resistor and **Vdd vdd 0 2.5** defines a 2.5 V supply source.

Together, these statements form the SPICE Netlist, which is simply a textual description of the circuit.

</details>

<details>
<summary><b>L3 - Technology Parameters</b></summary>

<img width="602" height="335" alt="DISK3_L3_1" src="https://github.com/user-attachments/assets/f4b30bca-8a6e-4f0c-b0c1-97807aad40d7" />


Describing the circuit connections alone is not enough.

SPICE also needs to know how the transistor behaves physically.

For example:

- What is the threshold voltage?
- What is the electron mobility?
- What is the oxide thickness?
- What is the body effect coefficient?

These values are technology-dependent and are provided by the semiconductor foundry.

You can think of this model as the transistor's DNA. It completely defines how the transistor behaves electrically.


<img width="602" height="338" alt="DISK3_L3_2" src="https://github.com/user-attachments/assets/52592bfa-0781-48c9-b7c6-17ff6972d5b2" />


Instead of writing hundreds of model parameters inside every circuit file, all these parameters are grouped into a separate model file.

As shown in the figure, the technology file may contain:

- NMOS model parameters
- PMOS model parameters
- Threshold voltage values
- Body effect coefficients
- Oxide parameters
- Mobility parameters

xxxx_025um_model.mod

Whenever SPICE runs, it reads this file and uses these parameters to accurately model transistor behavior.

These are packaged into a model file such as:

<img width="602" height="337" alt="DISK3_L3_3" src="https://github.com/user-attachments/assets/cd1fb655-c3a3-42c3-9ecf-10df0c345e38" />

Once the circuit netlist and technology file are ready, we connect them together. As shown in the above figure, the simulation file includes:

- Circuit description
- Technology model file
- Simulation commands

At this point SPICE has all the information required to start the simulation.

</details>

<details>
<summary><b>L4 - First SPICE Simulation</b></summary>

<img width="452" height="210" alt="DISK3_L4_1" src="https://github.com/user-attachments/assets/1d30d4e0-06e4-4182-98db-4eaaa9970a5d" />

Now the simulation is executed.

SPICE begins by reading the netlist and technology file.

It then solves the transistor equations internally and calculates:

- Node voltages
- Branch currents
- Device operating points

As shown in teh figure, the simulator successfully processes the circuit and displays the simulation information in the terminal.

This confirms that the circuit description and model files have been read correctl


<img width="452" height="309" alt="DISK3_L4_2" src="https://github.com/user-attachments/assets/61434c9d-035d-47da-bdb8-4fe6ac6affe9" />

After simulation, SPICE generates several output vectors.  


- Node voltages
- Drain current
- Branch currents
- Operating point values

These are the numerical results produced by the simulator.


<img width="452" height="204" alt="DISK3_L4_3" src="https://github.com/user-attachments/assets/8851d913-2480-416b-a8e8-1d1617733f21" />

Finally, SPICE plots the ID-VDS characteristics of the NMOS transistor.

The graph shows drain current versus drain voltage for multiple gate voltages.

Instead of manually calculating drain current for every VDS value, SPICE automatically sweeps the voltage and generates the complete curve.

This is one of the biggest advantages of simulation.

**Why is the Plot Command Written as -vdd#branch?**


<img width="452" height="204" alt="DISK3_L4_3" src="https://github.com/user-attachments/assets/b5a4c948-5415-47aa-99f6-3aa3297a816d" />


A common question is why the current is plotted as: -vdd#branch instead of vdd#branch 

The reason is SPICE's current sign convention. SPICE assumes that current entering the positive terminal of a voltage source is positive. However, in our NMOS circuit, current actually leaves the positive terminal of VDD and flows toward ground. Therefore, SPICE reports the current as negative. To display the drain current in the actual direction of flow, we add a negative sign:**-vdd#branch**

</details>

## Day 2️⃣ Velocity Saturation & CMOS Inverter Basics

### NgspiceSky130-Day2-Velocity saturation and basics of CMOS inverter VTC

<details>
<summary><b>L1 - L1 SPICE simulation for lower nodes?</b></summary>

After understanding how an NMOS behaves in the linear and saturation regions, the next step is to see how device dimensions affect its electrical characteristics.

To study this, we perform SPICE simulations using a lower technology node transistor. The objective is to observe how the drain current characteristics change when the transistor dimensions are modified.


<p align="center">
  <img src="Day2/D2SK1_L1_1.png" width="800">
</p>

we create an NMOS transistor in the SPICE netlist with a Width-to-Length ratio (W/L) of 2.5. The simulation sweeps the drain voltage while applying different gate voltages. Here the meaning of **.dc Vdd 0 1.8 0.1 Vin 0 1.8 0.2** means Vdd will be swept from 0 to 1.8 for each value of Vin from 0 to 1.8. That means first Vin=0, then Vdd will be 0,0.1,0.2.....,1.8. 

The SPICE simulator then calculates the drain current for each operating condition and generates the characteristic curves automatically. This allows us to study transistor behavior without performing lengthy manual calculations.

<p align="center">
  <img src="Day2/D2SK1_L1_2.png" width="800">
</p>

The output graph in the figure, shows the drain current versus drain voltage characteristics for different gate voltages.

At small drain voltages, the curves rise almost linearly because the transistor operates in the resistive region.

As the drain voltage increases further, the curves gradually flatten, indicating that the transistor has entered saturation. In this region, increasing VDS does not significantly increase the drain current.

<p align="center">
  <img src="Day2/D2SK1_L1_3.png" width="800">
</p>


<p align="center">
  <img src="Day2/D2SK1_L1_4.png" width="800">
</p>



In the previous case, the characteristics were obtained for a long-channel MOSFET. The region left of **VDS = VGS − VT** is the **Linear Region**, the region right of it is the **Saturation Region**, and the area below threshold is the **Cut-off Region**.

Now, both **W** and **L** are scaled down while keeping the **W/L ratio constant**. From the ideal drain current equation, the current should remain unchanged since it depends on **W/L**. However, practical SPICE simulations show a different behavior due to short-channel effects present in scaled technologies.

The SPICE deck below demonstrates this comparison, where only **W** and **L** are changed while all other parameters remain the same.
</details>

<details>
<summary><b>L2 - Drain current vs gate voltage for long and short channel device?</b></summary>
Up to now, the drain current equations that we studied for MOSFETs suggested that the drain current in saturation is proportional to the square of the overdrive voltage (VGS − VT). This means that if we increase the gate voltage, the drain current should increase quadratically. To verify this behavior, simulations were performed on both long-channel and short-channel devices.
 
<img width="940" height="529" alt="image" src="https://github.com/user-attachments/assets/c871d202-efc7-4c9c-b592-77ed513ef0da" />

The drain current characteristics of a long-channel MOSFET were first analyzed. From the graph, it can be observed that the drain current increases slowly at lower gate voltages and rises rapidly as the gate voltage increases further. This behavior follows the classical square-law relationship of MOSFETs. The curved nature of the graph clearly indicates this quadratic dependence.

<img width="940" height="528" alt="image" src="https://github.com/user-attachments/assets/9759c777-0376-48d0-943a-3d4b03feecdd" />

However, when we look at the short-channel device, a different behavior starts to appear. Initially, for lower values of gate voltage, the current still follows the expected quadratic relationship. But as the gate voltage continues to increase, the drain current starts becoming almost linearly dependent on VGS. This can be observed from the nearly constant spacing between adjacent current curves. Instead of increasing rapidly according to the square law, the current begins increasing at a more constant rate.

The important takeaway is:
- Long-channel MOSFET → Drain current follows quadratic dependence on VGS.
- Short-channel MOSFET → Initially quadratic, then gradually becomes linear with increasing VGS.

This difference becomes very important in modern technologies where transistor lengths are extremely small.
<img width="940" height="528" alt="image" src="https://github.com/user-attachments/assets/7601ed61-ff59-4910-9801-899aa0fa1a31" />

To observe this behavior more clearly, a simulation was set up where the drain voltage was kept constant at 2.5 V while the gate voltage was swept from 0 V to 2.5 V. The objective was to directly plot the relationship between drain current and gate voltage.

<img width="940" height="503" alt="image" src="https://github.com/user-attachments/assets/2335b3df-5ef9-4bb7-b593-763218de2ffb" />

The corresponding SPICE setup used for the simulation is shown above. The gate voltage was varied while monitoring the drain current. This allows us to visualize how the current responds to changes in gate bias.

<img width="853" height="785" alt="image" src="https://github.com/user-attachments/assets/021e0d00-b605-476e-8686-2f1ee5922a13" />

This figure shows the simulation execution and extraction of the required current parameters from NGSPICE.

<img width="460" height="329" alt="image" src="https://github.com/user-attachments/assets/65a2983c-bbb3-40b0-84af-522e536201de" /><img width="454" height="330" alt="image" src="https://github.com/user-attachments/assets/72a60ddd-468a-43a2-83ba-e6c80a5acd68" />

The resulting graph for a long-channel device clearly resembles a parabola, confirming that drain current is proportional to the square of the gate overdrive voltage. The graph is clearly quadratic and matches the long-channel MOSFET current equation.

<img width="940" height="483" alt="image" src="https://github.com/user-attachments/assets/c3b578ed-88d2-4bce-add4-04d643a44109" />

Next, the same experiment was repeated for a short-channel MOSFET. Only the device dimensions were changed to represent a short-channel transistor while keeping the same analysis procedure.

<img width="610" height="423" alt="image" src="https://github.com/user-attachments/assets/3759c8c2-393b-4b0b-be68-7122a83e5d0b" />


Unlike the previous graph, this curve is almost linear after the threshold region. Instead of rising quadratically, the drain current increases nearly proportionally with gate voltage. This observation indicates that an additional physical effect is limiting the current growth in short-channel devices.

</details>

<details>
<summary><b>L3-Velocity saturation at lower and higher electric fields</b></summary>

At this point, an important question arises.

If the drain current equation predicts quadratic growth, why does the short-channel device show almost linear behavior?

The answer lies in a phenomenon called Velocity Saturation.

To understand velocity saturation, let us recall that electrons move through the channel because of the electric field created between source and drain. Electron velocity can be expressed as:

v = μnE

where μn is the electron mobility and E is the electric field.

This equation tells us that electron velocity should increase linearly with electric field. This assumption is valid only at lower electric fields.

<img width="940" height="509" alt="image" src="https://github.com/user-attachments/assets/968de4c0-bfb3-43cb-9be0-5fc91baba58e" />

This figure compares long-channel and short-channel devices. The long-channel device continues to exhibit quadratic current growth, whereas the short-channel device begins transitioning toward linear behavior.

<img width="940" height="427" alt="image" src="https://github.com/user-attachments/assets/c15f8f4e-c852-4b66-a60d-4cf2f6175d35" />

At lower electric fields, electron velocity increases almost linearly with the electric field according to the mobility equation.

However, electrons cannot keep accelerating forever. As the electric field becomes very large, electrons experience more collisions with the silicon lattice. These collisions are called scattering effects.

<img width="940" height="404" alt="image" src="https://github.com/user-attachments/assets/2c5fdea1-83c5-4a4d-9c86-94b60d9b84f7" />

As electric field increases further, the electron velocity gradually approaches a maximum value and stops increasing significantly.

<img width="940" height="470" alt="image" src="https://github.com/user-attachments/assets/5c6557a7-642a-44f1-b6fc-b3c83c28c469" />

The graph clearly shows two regions:

- Low electric field region → Velocity increases linearly.
- High electric field region → Velocity becomes almost constant.

This constant velocity region is known as velocity saturation.

<img width="940" height="477" alt="image" src="https://github.com/user-attachments/assets/72bf0494-8e7b-42ee-8b48-d75b5519e128" />

A mathematical model is introduced to smoothly represent the transition from linear velocity behavior to saturation velocity behavior.

<img width="940" height="521" alt="image" src="https://github.com/user-attachments/assets/b838d47f-0b57-4e81-84b1-9065b2b06b27" />

This model ensures continuity between both operating regions and accurately represents carrier transport at higher electric fields.

Because short-channel devices have very small channel lengths, even moderate drain voltages can create extremely high electric fields. Therefore, electrons quickly reach their saturation velocity.

Once the velocity becomes constant, increasing gate voltage can no longer increase current according to the square-law relationship. As a result, the drain current begins to show a more linear dependence on gate voltage rather than a quadratic dependence.

This is the fundamental reason why short-channel MOSFETs deviate from classical long-channel equations.

<img width="940" height="537" alt="image" src="https://github.com/user-attachments/assets/85e6411f-8485-49e9-bee6-23dede0b0b3c" />

Using the velocity saturation expression, the drain current equation can be re-derived. The resulting equation becomes much more accurate for short-channel technologies, although it is more difficult to use for manual calculations.

<img width="940" height="472" alt="image" src="https://github.com/user-attachments/assets/d0978a09-fb2c-49a0-b352-b32586c8d7c2" />

This figure summarizes the operating regions:

- Long Channel → Cutoff, Linear, Saturation
- Short Channel → Cutoff, Linear, Velocity Saturation, Saturation

Velocity saturation becomes an additional operating behavior that is particularly important in deep submicron technologies.

## Key Observations
- Long-channel MOSFETs follow the classical square-law current model.
- Short-channel MOSFETs gradually deviate from the square-law model.
- Carrier velocity increases linearly only at lower electric fields.
- At higher electric fields, carrier velocity reaches a maximum saturation value.
- Velocity saturation limits drain current growth in modern technologies.
-This effect causes the drain current characteristics of short-channel devices to become nearly linear.
**Conclusion**

The classical MOSFET current equations accurately describe long-channel devices, where drain current varies approximately as the square of the gate overdrive voltage. However, as transistor dimensions shrink, extremely high electric fields develop inside the channel. These high fields cause electron velocity to saturate due to scattering effects. Once velocity saturation occurs, the drain current can no longer increase quadratically and instead begins showing a nearly linear dependence on gate voltage. This explains the differences observed between long-channel and short-channel MOSFET characteristics and highlights why modern technologies require more advanced current models than the traditional square-law equations.

</details>

<details>
<summary><b>L4 - Velocity saturation drain current model</b></summary>
 
In the previous section, we learned that short-channel MOSFETs no longer follow the ideal square-law relationship because the carriers eventually reach their saturation velocity. This naturally raises an important question: if the classical drain current equation is no longer accurate, how can we model the current of short-channel devices?

<img width="940" height="580" alt="image" src="https://github.com/user-attachments/assets/b1c6a15f-627e-471f-9162-ca4ecccc0ae1" />

To answer this question, a velocity saturation based drain current model is introduced. Unlike the classical MOSFET equation, this model takes into account the fact that carrier velocity cannot increase indefinitely. At lower electric fields, the current still behaves similarly to the long-channel model. However, once velocity saturation begins, the current growth slows down and becomes more linear.

<img width="940" height="560" alt="image" src="https://github.com/user-attachments/assets/4b1bfe29-9553-4c7d-b6f5-70c20e0da4fc" />


- There are three possible cases depending on which parameter is the minimum among (V_{GT}), (V_{DS}), and (V_{DSAT}).
- When (V_{GT}) is the minimum ((V_{GT} < V_{DS}) and (V_{GT} < V_{DSAT})), the minimum-function model selects (V_{GT}) as (V_{min}). Substituting (V_{min} = V_{GT}) into the drain current equation results in a current expression that becomes independent of (V_{DS}), giving a nearly constant drain current.
- Physically, saturation occurs when the drain voltage is sufficiently large such that further increases in (V_{DS}) do not significantly increase the drain current. Since (V_{DS}) is already greater than the limiting voltage (V_{GT}), the channel reaches pinch-off and the device operates in the saturation region.
- Therefore, for both long-channel and short-channel devices, the condition where (V_{GT}) is the minimum parameter indicates saturation operation, as the drain current has reached its limiting value and is no longer controlled by (VDS)

<img width="940" height="549" alt="image" src="https://github.com/user-attachments/assets/f49f1e3d-bde0-495c-837b-071c66caa88c" />

There are three possible cases depending on which parameter is the minimum among Vgt, Vds, and Vdsat.

  When Vgt is the minimum (Vgt < Vds and Vgt < Vdsat), the minimum-function model selects Vgt as Vmin. Substituting Vmin = Vgt into the drain current equation results in a current expression that becomes independent of Vds, giving a nearly constant drain current.

  Physically, saturation occurs when the drain voltage is sufficiently large such that further increases in Vds do not significantly increase the drain current. Since Vds is already greater than the limiting voltage Vgt, the channel reaches pinch-off and the device operates in the saturation region.

  Therefore, for both long-channel and short-channel devices, the condition where Vgt is the minimum parameter indicates saturation operation, as the drain current has reached its limiting value and is no longer controlled by Vds.

When Vds is the minimum parameter (Vds < Vgt and Vds < Vdsat), the minimum-function model selects Vds as Vmin. This corresponds to the condition where the drain-to-source voltage is relatively small compared to the gate overdrive voltage.

  Substituting Vmin = Vds into the drain current equation and considering low Vds, the channel-length modulation term 1 + λVds becomes approximately unity, since λVds << 1. Under this condition, the drain current varies nearly linearly with Vds.

  As a result, the current equation reduces to the familiar linear (or triode) region expression. Therefore, when Vds is the minimum parameter, the device operates in the linear region, where the channel is formed along the entire length and the drain current is controlled by both Vgs and Vds.

  <img width="940" height="459" alt="image" src="https://github.com/user-attachments/assets/93869e7f-e532-43cb-a862-cee136d08480" />

<img width="940" height="449" alt="image" src="https://github.com/user-attachments/assets/daf0d1bd-a35b-4d18-8b9f-a7070e78bb48" />

Peak current in a short-channel MOSFET is lower than that of a long-channel MOSFET due to velocity saturation.

  In a long-channel MOSFET, pinch-off occurs at a relatively higher Vds because the channel length is larger. Before pinch-off, carriers continue to accelerate as the electric field increases, resulting in a higher drain current.

  In a short-channel MOSFET, pinch-off occurs at a much lower Vds because the channel length is small. Once pinch-off occurs, channel length modulation begins, causing the effective channel length to reduce as Vds increases.

  Although the depletion region near the drain continues to widen and the electric field continues to increase with increasing Vds, the carrier velocity cannot increase indefinitely. Beyond a critical electric field, electrons reach their saturation velocity.

  As a result, further increases in Vds do not produce a proportional increase in carrier velocity. Since the drain current depends on carrier velocity, the current increase becomes limited.

  Therefore, even though channel length modulation tends to increase drain current, velocity saturation restricts the carrier velocity, resulting in a lower peak drain current in short-channel MOSFETs compared to long-channel MOSFETs.

</details>

<details>
<summary><b>L5 - Labs Sky130 Id-Vgs</b></summary>
<img width="940" height="499" alt="image" src="https://github.com/user-attachments/assets/f27deaf0-8278-49ce-95c3-5348f28bf6cc" />

Now we can show the graphs between short channel & long channel mosfets. Mosfets with width less than 250 microns are called as short channel mosfet.

<img width="940" height="595" alt="image" src="https://github.com/user-attachments/assets/6bc1e03f-0f46-4cc3-a55a-af931e16bd4d" />

<img width="940" height="449" alt="image" src="https://github.com/user-attachments/assets/36d24f17-a08a-4313-9b50-474f0751a97e" />

<img width="940" height="609" alt="image" src="https://github.com/user-attachments/assets/54288dcb-40a1-4831-99c1-e83907e37888" />

Eventhough the starting Id has a quadratic dependency on Vgs. But later it became linear. Now Vdd is kept constant at 1.8V Vgs is increased from 0 to 1.8. The graph is almost linear. 


</details>

<details>
<summary><b>L6 - Labs Sky130 Vt</b></summary>

<img width="940" height="609" alt="image" src="https://github.com/user-attachments/assets/f3339678-d3e2-4a21-8ff9-45351c6854fa" />

To get the threshold voltage, take the slope over the Vgs vs Id graph. It comes to apprx 0.77 Volts.
</details>


---


### NgspiceSky130_D2SK2 - CMOS voltage transfer characteristics (VTC)

<details>
<summary><b>L2 - MOSFET as a switch</b></summary>

<img width="940" height="564" alt="image" src="https://github.com/user-attachments/assets/82bf8bde-0745-4970-a115-4ab2a883aa22" />

Until now, MOSFETs were studied using current equations and operating regions. However, in digital circuits, MOSFETs are primarily used as switches. Understanding this switching behavior is the foundation of CMOS logic design.

Consider an NMOS transistor. When the gate voltage is below the threshold voltage, no inversion channel exists between source and drain. Since there is no conducting path, the transistor behaves like an open switch and no current flows through it.

When the gate voltage exceeds the threshold voltage, an inversion channel is formed. Electrons can now travel from source to drain, creating a conducting path. In this condition, the transistor behaves like a closed switch.

Therefore, from a digital perspective, a MOSFET can be viewed as a voltage-controlled switch:

- Gate Voltage < Threshold Voltage → Switch OFF
- Gate Voltage > Threshold Voltage → Switch ON

This simple switching property is what makes MOSFETs suitable for implementing digital logic circuits.

<img width="795" height="553" alt="image" src="https://github.com/user-attachments/assets/5c37267f-2773-42a1-8f0f-9c0bc810dd54" />


In reality, an ON transistor is not a perfect short circuit and an OFF transistor is not a perfect open circuit. However, for understanding digital logic, this approximation is sufficiently accurate.

The ON transistor can be represented as a small resistance, while the OFF transistor can be represented as a very large resistance.

The MOSFET behaves as a controllable switch whose state depends on the gate voltage. This simple switching behavior forms the basis of CMOS logic circuits and enables the implementation of digital systems.

<img width="940" height="527" alt="image" src="https://github.com/user-attachments/assets/a89a2c90-2533-481b-a758-091fea8f2fb4" />

When the PMOS is OFF, it can be modeled as an open switch because there is no conducting path between VDD and the output node.

  When the NMOS is ON, it can be modeled as a resistor because it provides a conducting path between the output node and ground.

  Since the PMOS is open and the NMOS provides a path to ground, the output node is connected to ground through the NMOS resistance.

  Therefore, the voltage drop appears across the NMOS resistor, and the output voltage is pulled close to 0 V. In an ideal MOSFET, the resistance is zero and the output is exactly 0 V. In a practical MOSFET, a small voltage may exist due to the finite ON resistance of the NMOS.
 </details>
 
 <details>
<summary><b>L2 - Introduction to standard MOS voltage current parameters</b></summary>

<img width="940" height="514" alt="image" src="https://github.com/user-attachments/assets/617dcdaf-7731-4bda-9c6f-fcda1bb6343c" />

When the PMOS is ON, it can be approximated as a resistor because it provides a conducting path between VDD and the output node.

  If the NMOS is OFF, there is no path from the output node to ground. Therefore, current does not flow through the circuit and the output voltage is pulled close to VDD.

  In reality, the PMOS is not an ideal resistor. Its resistance depends on the gate voltage, drain voltage, source voltage, and the drain current flowing through it.

  Therefore, the PMOS behaves as a non-linear resistor rather than a constant resistor. As we study MOSFET current equations in more detail, we will see that the effective resistance of the PMOS changes with its operating conditions and is a non-linear function of the drain current.

  <img width="940" height="509" alt="image" src="https://github.com/user-attachments/assets/eb131935-c669-4a29-a9f4-47da4ff00bfe" />

  This figure explains the operation of a CMOS inverter using a simple switch-resistor model.

### CMOS Inverter Structure

The left figure shows a CMOS inverter consisting of:

- PMOS connected between VDD and Vout.
- NMOS connected between Vout and VSS (Ground).
- Both gates connected together to form the input Vin.
- A load capacitance CL connected at the output node.

The load capacitance represents:
- Gate capacitances of the next stage.
- Interconnect capacitance.
- Parasitic capacitances present at the output node.

---

### Case 1: Vin = VDD (Logic 1)

In this condition:

- PMOS is OFF because VSG = 0.
- NMOS is ON because VGS > VT.

The PMOS behaves like an open switch.

The NMOS behaves like a resistor RN.

A conducting path now exists:

Vout → RN → VSS

Therefore, the output node is connected to ground through the NMOS.

The charge stored on CL is discharged through RN.

As the capacitor discharges:

- Output voltage decreases.
- Current flows from CL to ground.
- Eventually Vout approaches 0 V.

Therefore:

Vin = 1 → Vout = 0

This is why the inverter produces the complement of the input.

---

### Case 2: Vin = 0 (Logic 0)

In this condition:

- PMOS is ON because VSG > |VT|.
- NMOS is OFF because VGS = 0.

The NMOS behaves like an open switch.

The PMOS behaves like a resistor RP.

A conducting path now exists:

VDD → RP → Vout

The output capacitor CL starts charging through RP.

As the capacitor charges:

- Output voltage increases.
- Current flows from VDD into CL.
- Eventually Vout approaches VDD.

Therefore:

Vin = 0 → Vout = VDD

---

### Why is CL Important?

The capacitor CL cannot change its voltage instantaneously.

Therefore:

- During a LOW-to-HIGH transition, CL charges through RP.
- During a HIGH-to-LOW transition, CL discharges through RN.

This charging and discharging process creates the propagation delay of the inverter.

The delay depends on:

- RP
- RN
- CL

Larger capacitance means:
- Slower charging.
- Slower discharging.
- Larger delay.

---

### Important Observation

The PMOS and NMOS are shown as resistors only for simplified analysis.

In reality:

- RP is not constant.
- RN is not constant.
- Both are non-linear functions of terminal voltages and drain current.

However, this resistor model is very useful for understanding:

- CMOS inverter operation.
- Charging and discharging of load capacitance.
- Propagation delay.
- Dynamic power consumption.


We shall provide the naming convention, something like this.

<img width="940" height="471" alt="image" src="https://github.com/user-attachments/assets/eaa76a45-7c9f-49f7-ad7d-bcfbf27f15b4" />


 </details>
 
 <details>
<summary><b>L3 - PMOS/NMOS drain current vs drain voltage</b></summary>

Before constructing the Voltage Transfer Characteristics (VTC) of a CMOS inverter, it is important to understand the drain current characteristics of both NMOS and PMOS transistors individually. Since a CMOS inverter consists of both these devices connected together, understanding their individual behavior helps us understand how the inverter eventually reaches its output voltage.

<img width="940" height="505" alt="image" src="https://github.com/user-attachments/assets/61d82011-1af5-42b2-b14b-501b63997d42" />

The above figure shows the drain current versus drain voltage characteristics of an NMOS transistor. For a fixed gate voltage, when the drain voltage is initially small, the drain current increases almost linearly with drain voltage. This happens because the transistor behaves like a resistor and operates in the linear region.

As the drain voltage continues to increase, the voltage difference between gate and drain starts reducing. Eventually, the condition:

VDS = VGS − VT

is reached. At this point, pinch-off begins near the drain end of the channel. Beyond this point, the transistor enters saturation and the drain current becomes almost independent of drain voltage.

For higher values of gate voltage, a stronger inversion layer is formed inside the channel. Since more carriers are available for conduction, the drain current becomes larger. This is why multiple current curves corresponding to different gate voltages appear on the graph.

An important observation is that all curves initially rise and then gradually flatten out after entering saturation.

<img width="940" height="493" alt="image" src="https://github.com/user-attachments/assets/2fbb873c-517d-4c3c-8a98-dcb9a2ed2ced" />

Here first the IdsN & VdsN curves are first plotted. For PMOS if it has to be plotted it should be in the third quadrant since Vth, Vds & Idsp will be negative.

</details>
 
 <details>
<summary><b>L3 - Convert PMOS gate-source-voltage to Vin</b></summary>

In a standard circuits, all the node voltages is of not of much use. So everything has to be converted in terms of Vin & Vout. With respect to the equations written, first we converted VgsP into Vin, then the curve shifts to Second quadrant. 
<img width="940" height="465" alt="image" src="https://github.com/user-attachments/assets/d13f5ec3-658f-443d-891b-324d356b2650" />

- Each value of VgsP is converted into the equivalent input voltage (Vin) using the relation:

  Vin = VDD + VgsP. Since the PMOS source is connected to VDD,
  VgsP = Vin - VDD. Therefore, every value of VgsP in the PMOS current table can be directly mapped to a corresponding Vin value.
- While plotting the CMOS inverter characteristics, the horizontal axis is chosen as Vin instead of VgsP.
- For example, if: VDD = 2 V and VgsP = 0 V then: Vin = 2 V.Therefore, the point corresponding to VgsP = 0 V is plotted at Vin = 2 V on the graph.
- At the inverter output node, the PMOS drain current and NMOS drain current must be equal in magnitude and opposite in direction. Therefore:

   IdsP = -IdsN. To avoid dealing with negative current values and to maintain a single current convention, the current axis is represented using IdsN. Using:

   IdsP = -IdsN, all PMOS current values are converted into equivalent positive IdsN values.
   As a result:

   - The current axis is labeled as IdsN.
   - Negative PMOS currents are reflected as positive values.
   - Both PMOS and NMOS characteristics can be plotted on the same graph using a common current axis.

   This makes it easier to determine the operating point and construct the CMOS inverter VTC.

</details>
 
 <details>
<summary><b>L5 - Convert PMOS and NMOS drain-source-voltage to vout </b></summary>

Now Vdsp+Vdd=Vout

<img width="940" height="479" alt="image" src="https://github.com/user-attachments/assets/dd14e0b3-9319-4206-b1c9-1938c3d38c9b" />


How to interpret this curve?
- First we took pmos and converted the VdsP vs IdsP  (actually -VdsP vs -IdsP) into -VdsP vs Idn curve. In that we converted the VgsP value into Vin.
- Now its time to convert Vdsp into Vout. Vout=Vdd+Vdsp. In graph point of view a scalar quantity which is positive is added to Vdsp to obtain Vout. So the graph will shift to the right as in the picture.


<img width="930" height="774" alt="image" src="https://github.com/user-attachments/assets/2c2adccb-6eb8-4911-be69-d5f61844a98c" />

- When Vout is 0 (red dots in the image), there is a current IdsN. This means that when the capacitor is completely discharged(Vout=0), this existing current IdsN will charge the capacitor when subjected to gate voltage. 
- When Vout is at the maximum point(yellow dot in the image), since the capacitor is charged to the full voltage level there is no current flowing through the mosfet. So IdsP is zero

</details>
 
 <details>
<summary><b>L6 - Merge PMOS – NMOS load curves and plot VTC </b></summary>

<img width="913" height="985" alt="image" src="https://github.com/user-attachments/assets/223c9f11-fe13-4f10-9b23-6f8b74461613" />

This is how nmos load curve is found. Now we have to superimpose the nmos & pmos curves & for that the next figure will help.

<img width="940" height="1304" alt="image" src="https://github.com/user-attachments/assets/9aff4bd7-0592-40a2-8d72-f7c74ca3e6ee" />

- The last graph in the figure is the combination of both pmos & nmos curves. Now we will take common points in both these graphs. For example, here there are 5 distinct voltages, 0v, 0.5v, 1v, 1.5v & 2V. We need to plot Vin verses Vout plot, that’s the transfer characteristics of a cmos inverter. For that we need to find out when Vin=0V whats the Vout.
- For that take the common point between Vin=0V from both the graphs. Its marked as point A.

<img width="940" height="529" alt="image" src="https://github.com/user-attachments/assets/d47ec033-031b-4d24-92fa-625ed6fb7c27" />

</details>

---

## Day 3️⃣ Switching Threshold & Dynamic Behaviour

### Voltage transfer characteristics-SPICE simulations

 
 <details>
<summary><b>L1 - SPICE deck creation for CMOS inverter </b></summary>

<img width="273" height="250" alt="image" src="https://github.com/user-attachments/assets/bbdd7d1e-1e82-4ab5-813e-70f6e69dff35" />

We will now simulate the VTC. For that we need to create the SPICE deck. It is a connectivity information (Netlist). As there is information about substrate, the circuit is as shown below.Here M1 is PMOS and M2 is NMOS.

<img width="362" height="258" alt="image" src="https://github.com/user-attachments/assets/9e890e26-6f43-44ab-8454-ab7d10a64ec4" />

Name the nodes In model file we will mention like, 2.5V input lies between Vin and 0, similarly Vdd lies between vdd and 0.

</details>
 
 <details>
<summary><b>L2 - SPICE simulation for CMOS inverter</b></summary>


<img width="940" height="429" alt="image" src="https://github.com/user-attachments/assets/a0e1cf6f-c5ad-4f22-81da-875fffc1e198" />

Here we are supposed to define the nodes. There are 4 nodes as shown in the figure. Vin, vdd, out & Vss. We want to plot dc characteristics that is Vin vs Vout curve. For that we ned to sweep dc voltage from 0 to 2.5 in a step of 0.05. Usually since the length of transistor is 250u, the input voltage is taken as 2.5V.
The model files contains all the technological parameters regarding the nmos & pmos. Its given from foundry.
Now we need to check the same for 2 different scenarios.

<img width="940" height="125" alt="image" src="https://github.com/user-attachments/assets/ba5559b4-6f14-4dce-b343-6a9b11612cfc" />


<img width="940" height="111" alt="image" src="https://github.com/user-attachments/assets/294ca920-1a32-4625-9a68-5905de88dd96" />

In the second figure the Wp=0.9375u that is 2.5 times of Wn. Now we need to observe the results. The expected results as per the lecture is shown below. We shall simulate the same using spice & see. 

<img width="940" height="758" alt="image" src="https://github.com/user-attachments/assets/55dcb8ec-4a2d-4c80-8f0b-4bc7eae75f74" />

From this if we want to inspect something its like, when the graph is vertically divided into 2 halves through the center, the whole graph is shifted towards the left side. 

<img width="940" height="757" alt="image" src="https://github.com/user-attachments/assets/18b5588e-177b-4892-a466-44522e6051ca" />

In this case the graph passes through the center side. 

</details>
 
 <details>
<summary><b>L3 - Labs Sky130 SPICE simulation for CMOS</b></summary>

<img width="940" height="501" alt="image" src="https://github.com/user-attachments/assets/9a792d0d-6281-4e56-9f22-0529d4a0143a" />

<img width="940" height="499" alt="image" src="https://github.com/user-attachments/assets/55da2439-5af7-48fb-98c5-6be2aba3fda7" />

For this spice deck, this is the transfer characteristics. To find the switching threshold, take the point, where Vin=Vout. That point will the converging point of a 45 degree line meeting the curve.

<img width="924" height="423" alt="image" src="https://github.com/user-attachments/assets/ee92b963-0af7-4e09-bbe5-939bf4fd51d6" />

Now inorder to find the transient characteristics, we are supposed to feed a pulse signal which will changw with respect to time as gate input.

<img width="940" height="440" alt="image" src="https://github.com/user-attachments/assets/f04883d1-4b3e-4048-93ce-6a65cf9d9b63" />

<img width="940" height="530" alt="image" src="https://github.com/user-attachments/assets/cac571c4-0c8d-4ac7-b55b-c01c624479c8" />


To get the Rise Time delay: It’s the delay to which taken by the output give a logic 1, basically to rise to logic 1 when input is 0. Its calculates in such a way that, time difference between 50% of Input & 50% of the output & we got the delay as **2.48358-2.15037=0.33321ns**


<img width="928" height="319" alt="image" src="https://github.com/user-attachments/assets/ae21fcb1-66ee-4d1d-ad01-5243e99e1039" />

**Fall delay: 4.335-4.05=0.285ns**

<img width="940" height="251" alt="image" src="https://github.com/user-attachments/assets/0a107137-b153-43ff-9c0f-4a17c860859f" />


The switching threshold (VM) is the point where the PMOS and NMOS have equal current driving capability.

At this point:

- IdsP = IdsN
- Pull-up current equals pull-down current.
- Net current at the output node becomes zero.

The output node is not just a wire. It contains load capacitance and stores charge.

#### Why Does Current Balance Matter?

**Case 1: PMOS Current > NMOS Current**

- IPMOS = 80 µA
- INMOS = 20 µA

Net current:

Net Current = 80 - 20 = 60 µA

Extra charge enters the output node.

Therefore:

- Output capacitance charges.
- Vout increases.


**Case 2: PMOS Current < NMOS Current**

- IPMOS = 20 µA
- INMOS = 80 µA

Net current:

Net Current = 20 - 80 = -60 µA

Charge leaves the output node.

Therefore:

- Output capacitance discharges.
- Vout decreases.


**Case 3: PMOS Current = NMOS Current**

- IPMOS = 60 µA
- INMOS = 60 µA

Net current:

Net Current = 60 - 60 = 0

Therefore:

- No extra charge enters the node.
- No charge leaves the node.
- Output capacitance neither charges nor discharges.
- dVout/dt = 0

The output voltage stops moving.

This is the fundamental reason behind the switching threshold.

#### Why Vin = Vout at Switching Threshold?

As Vin increases:

- Vin = 0 V → PMOS dominates → Vout ≈ VDD
- Vin increases → NMOS becomes stronger → Vout starts decreasing
- Eventually → PMOS current = NMOS current

At this point:

- Net current = 0
- Output voltage is momentarily balanced

For a symmetric inverter, this balance point occurs when:

Vin = Vout

This voltage is called the switching threshold (VM).

The true definition of switching threshold is the point where PMOS current equals NMOS current. The condition Vin = Vout is a consequence of this current balance, not the definition itself.



### DC Analysis

DC analysis calculates steady-state voltages and currents by sweeping DC sources and ignoring time-dependent effects.

Applications:

- IV characteristics
- Voltage Transfer Curve (VTC)
- Switching threshold
- Noise margin calculation

Example:


.dc Vin 0 1.8 0.1


SPICE evaluates:


- Vin = 0.0 V → Solve circuit
- Vin = 0.1 V → Solve circuit
- Vin = 0.2 V → Solve circuit
- ...
- Vin = 1.8 V → Solve circuit


Each point is solved independently.

There is no concept of time.



### Transient Analysis

Transient analysis calculates the time-domain response of a circuit.

Applications:

- Switching behavior
- Propagation delay
- Rise time
- Fall time
- Dynamic performance

Example:


- Vin in 0 PULSE(0 1.8 0 0.1ns 0.1ns 2ns 4ns)
- .tran 1n 10n


SPICE evaluates:


- t = 0 ns     → Vin = 0 V
- t = 0.1 ns   → Vin rising
- t = 0.2 ns   → Vin ≈ 1.8 V
- t = 2 ns     → Vin = 1.8 V
- t = 2.1 ns   → Vin falling
- t = 4 ns     → Cycle repeats


Since Vin changes with time:

- VGS changes with time.
- MOSFET currents change with time.
- Output capacitance charges and discharges.
- Vout changes with time.



### DC Analysis vs Transient Analysis

| DC Analysis | Transient Analysis |
|------------|------------|
| No time dependence | Includes time dependence |
| Solves independent operating points | Solves continuously with time |
| Used for VTC and IV curves | Used for switching analysis |
| Ignores charging/discharging effects | Includes charging/discharging effects |
| Finds switching threshold | Finds delay and timing metrics |

### Conclusion

DC analysis evaluates a sequence of steady-state operating points and is mainly used for VTC generation and switching threshold extraction. Transient analysis tracks the actual time-domain behavior of the circuit and is used to study delays, rise/fall times, and dynamic switching performance.



</details>

---


### Static behaviour evaluation-CMOS inverter robustness-Switching Threshold

 
 <details>
<summary><b>L1 - Switching Threshold, Vm</b></summary>

<img width="940" height="458" alt="image" src="https://github.com/user-attachments/assets/19dd6126-b60a-4196-961b-1d37a814b125" />

The shape of the graph is similar in both ways. This shows the robustness of the cmos inverter. That’s why it’s the most used circuit used to create logic gates. The other factors which define or depend on robustnesss of the device is.

- Switching threshold
Switching threshold values of both inverters with Wn=Wp=0.375U & Wn=0.375, Wp=0.9375u is given below. As seen in the second figure, this value is more. This area where pmos & nmos are in saturation means they both are on, there are chances for leakage current means current might flow from Power to ground.

<img width="940" height="472" alt="image" src="https://github.com/user-attachments/assets/5425bca7-7581-42f5-87dd-a916db2e4da9" />

<img width="940" height="472" alt="image" src="https://github.com/user-attachments/assets/af117d12-08fc-4292-98dd-40102b7cf771" />

</details>

 <details>
<summary><b>L2 - Analytical expression of Vm as a function of (W/L)p and (W/L)n</b></summary>


<img width="940" height="482" alt="image" src="https://github.com/user-attachments/assets/b42aa073-29a4-4ffc-b20e-123c2233d40c" />

We are talking about the point where both transistors are in saturation mode. 

<img width="940" height="502" alt="image" src="https://github.com/user-attachments/assets/2dd5ac1d-1cc0-4dd5-bd23-0c63edb549a7" />


The things to keep in mind. The transistors are working in saturation mode. Vm is the point where Vin=Vout. Vgs of a nmos is Vin. Vgs of pmos in Vin-Vdd. The equation of Id is taken from the Lecture, L4:  Velocity saturation drain current model. After equating, IdsP & IdsN, the Vm equation is obtained. 

<img width="940" height="491" alt="image" src="https://github.com/user-attachments/assets/cdcff76d-4ccd-4bb8-9153-1c31e751ffc2" />

After taking the values from model files & width & length of transistors, Vm is obtained as 0.98V & 1.2V respectively.

</details>

 <details>
<summary><b>L3 - Analytical expression of (W/L)p and (W/L)n as a function of Vm</b></summary>

In the previous lecture, we took the value of (W/L) & found the value of Vm. Now we need to fix Vm & find out the value of (W/L).

<img width="940" height="487" alt="image" src="https://github.com/user-attachments/assets/c240b4a3-f454-44db-9ee6-a4f202f40d9f" />

Rearranging the things will be like,

<img width="598" height="129" alt="image" src="https://github.com/user-attachments/assets/bf8a61c8-e278-456c-8d17-a7e723edd91b" />

<img width="940" height="109" alt="image" src="https://github.com/user-attachments/assets/4d5fa603-d952-4f52-8037-8afe5077f6aa" />

<img width="651" height="107" alt="image" src="https://github.com/user-attachments/assets/967193e2-efa8-4ef9-89dd-9bed502d012d" />

<img width="657" height="106" alt="image" src="https://github.com/user-attachments/assets/bdba94cf-f2e7-44ff-b147-742da63d5536" />

<img width="940" height="444" alt="image" src="https://github.com/user-attachments/assets/9e65f07f-0c3d-42fd-9d8a-66e51cc51fbb" />

Now we will do some spice simulations, in such a way that we will keep Wn/Ln a constant & multiply with a constant which means, W/L ratio of pmos will be some interger times the W/L of nmos. Then w shall plot the switching thresholds & see. This will make us understand whether increasing the width of pmos will effect the switching threshold or not.


</details>

 <details>
<summary><b>L4 -  Static and dynamic simulation of CMOS inverter</b></summary>

Definition of pulse signal:

Vin in 0 0 pulse 0 2.5 0 10p 0p 1n 2n

### Understanding the PULSE Source


Vin in 0 PULSE(0 2.5 0 10p 0p 1n 2n)


This statement defines a pulse voltage source named **Vin**.

General syntax:

PULSE(Vinitial Von Tdelay Trise Tfall Ton Tperiod)


| Parameter | Value | Description |
|------------|---------|------------|
| Vinitial | 0 V | Initial voltage level (Logic 0) |
| Von | 2.5 V | Final voltage level (Logic 1) |
| Tdelay | 0 s | Delay before the first transition starts |
| Trise | 10 ps | Time taken to rise from 0 V to 2.5 V |
| Tfall | 0 ps | Time taken to fall from 2.5 V to 0 V |
| Ton | 1 ns | Duration for which the pulse remains HIGH |
| Tperiod | 2 ns | Total period of one pulse cycle |

<img width="940" height="433" alt="image" src="https://github.com/user-attachments/assets/ec1fbed3-4766-4fcc-bb1e-a6b282700c61" />

<img width="650" height="392" alt="image" src="https://github.com/user-attachments/assets/19d5013c-1b51-4ca0-8f46-825c85fb9a07" />


<img width="940" height="755" alt="image" src="https://github.com/user-attachments/assets/efa92679-3f00-44c6-90b8-57910421f844" />

Rise delay = Time at which 50% of rise output – Time at which 50% of fall input 
This mean how much extra time did the output take to rise. Actually when the input falls, then output should actually rise at the same time. But how much extra time did it take.

<img width="940" height="709" alt="image" src="https://github.com/user-attachments/assets/83c1274b-c8e8-45c4-8ad4-af7acbbce7bd" />

<img width="940" height="478" alt="image" src="https://github.com/user-attachments/assets/a16d0a01-9221-41f2-8069-aebf8cf06cb9" />

Rise delay & fall delay were calculated. Now draw a 45 degree line to the Transfer characteristics. The point where it gets intersected with the graph is the Vm=0.99V.


</details>

 <details>
<summary><b>L5 - Static and dynamic simulation of CMOS inverter with increased PMOS width</b></summary>

Now draw the same for all the combinations inside the table. So now the width of the pmos will be 2 times the width of the nmos.

<img width="717" height="494" alt="image" src="https://github.com/user-attachments/assets/db0cc953-2d71-4b2f-8a87-5bf7b0c2c2fe" />

The same change is made in dc & transient analysis.

<img width="940" height="763" alt="image" src="https://github.com/user-attachments/assets/7c9d832a-7abf-40da-952d-aaa034e330b4" />

The dc characteristics has shifted towards the right side compared to the previous curve. This is because now your pmos has become more stronger when width is increased. So now the capacitor will get charged more because of more pmos current. So for switching threshold, inorder to make Ip=In, nmos has to be equally strong. So inorder to increase drain current through nmos the only way is to increase Vin. So Vin increases, so the curve shifts towards right.


After transient analysis, we get the rise and fall delay as, 80ps & 76 ps.
<img width="940" height="436" alt="image" src="https://github.com/user-attachments/assets/ab2b3e9d-62d3-42c1-81d8-281209757bfb" />

Now Vm is calculated as 1.2V (Vin value when the intersection of Vin=Vout line with the  curve). It gets shifted towards right.

<img width="940" height="447" alt="image" src="https://github.com/user-attachments/assets/ee32cb47-9186-4a02-b1df-9633a62639ba" />

When W of pmos increases the Vm switches towards right. And moreover the rise delay(the time required to charge the capacitor) decreases. Because pmos becomes more stronger now. The switching threshold (Vm) is the input voltage at which the inverter changes state. It determines the noise margins, affects rise and fall delay symmetry, and indicates the balance between NMOS and PMOS drive strengths. Ideally, Vm is kept close to VDD/2 for robust and reliable digital operation.

<img width="940" height="377" alt="image" src="https://github.com/user-attachments/assets/fbed67fe-7b7c-423c-9a4f-8d4b25ec3615" />

</details>

 <details>
<summary><b>L6 - Applications of CMOS inverter in clock network and STA</b></summary>

<img width="940" height="480" alt="image" src="https://github.com/user-attachments/assets/b55ec136-70c5-4faf-b641-24774c5d7ac1" />

In a CMOS inverter, the rise delay and fall delay are determined by how quickly the PMOS charges the load capacitance and how quickly the NMOS discharges it. Since electron mobility is higher than hole mobility, an NMOS transistor is naturally stronger than a PMOS transistor of the same size. As a result, the output falls faster than it rises when both transistors have equal widths. To balance the rise and fall delays, the PMOS width is increased, typically by 2–3 times the NMOS width, so that the pull-up and pull-down strengths become nearly equal. This sizing technique is especially important in clock buffers, where equal rise and fall delays help maintain a 50% duty cycle, reduce clock distortion, and ensure reliable clock distribution throughout the chip.

**Effect of PMOS Width on Resistance and Delay**

The ON resistance of a MOSFET is inversely proportional to its width. Therefore, increasing the transistor width does not increase its resistance; instead, it reduces the ON resistance by providing a wider conduction path for charge carriers. In a CMOS inverter, the PMOS transistor is naturally weaker than the NMOS transistor because hole mobility is lower than electron mobility. As a result, for equal transistor sizes, the PMOS exhibits a higher ON resistance, causing the output rise delay to be larger than the fall delay. To compensate for this, the PMOS width is increased, which reduces its ON resistance and increases its drive strength. The PMOS is typically sized 2–3 times wider than the NMOS so that the pull-up and pull-down resistances become approximately equal. This helps achieve nearly equal rise and fall delays, which is especially important in clock buffers to preserve duty cycle, minimize clock distortion, and ensure reliable clock distribution throughout the chip.

These are used in clock buffers.

<img width="940" height="525" alt="image" src="https://github.com/user-attachments/assets/7c867435-e754-4cd9-a832-cc2aa8531ec4" />

As seen in the figure, for a clock source to provide clock to the destination cells, buffers are used. When we use 2 cmos inverters in a serial manner, a buffer is formed. But we can have an inverter with many combinations of W/L for the pmos & nmos. But when a combination of (W/L)Pmos = x(W/L)Nmos is made where rise delay & fall delay are almost equal(in the table second row), then we can expect a symmetrical clock waveform.

Conclusion:


**Effect of Switching Threshold on Robustness**

The switching threshold (Vm) determines the input voltage at which a CMOS inverter changes its output state and therefore plays a key role in its robustness. Ideally, Vm is designed close to VDD/2 so that both logic HIGH and logic LOW have nearly equal noise margins. Consider a CMOS inverter operating at VDD = 1.8 V. If the switching threshold is shifted too high, for example Vm = 1.3 V, the inverter requires a much higher input voltage to confidently recognize a logic HIGH. In this case, if a logic HIGH signal of 1.8 V experiences noise or voltage drop and reduces to 1.5 V, the safety margin becomes only 1.5 V − 1.3 V = 0.2 V. This means even a small additional disturbance can cause the signal to be misinterpreted. On the other hand, if Vm is 0.9 V, the same 1.5 V signal still has a margin of 0.6 V before reaching the switching threshold, making it much more tolerant to noise. Therefore, a switching threshold close to the middle of the supply voltage provides balanced noise margins and improves the robustness and reliability of the CMOS inverter.


</details>


## Day 4️⃣ Noise Margin & Robustness Analysis



### Static behavior evaluation – CMOS inverter robustness – Switching Threshold



<details>
<summary><b>L1 - Introduction to Noise Margin</b></summary>

Next step to identify the robustness of a cmos inverter is the noise margin. 

The first one is the ideal characteristics of a cmos. 

<img width="907" height="463" alt="image" src="https://github.com/user-attachments/assets/024be3c4-01a3-4887-b52e-b0b90e0e0adf" />

This means that wherever there is a change in Vin we can expect correct & sudden change in Vout. This makes the gain & infinity. But this is not possible always. As we have seen in out previous spice simulation results there is a gradual change in the output of a cmos inverter as shown in the second graph.

To fully characterize the logic levels of a CMOS inverter, four important voltage points must be identified. These voltages define the valid logic "0" and logic "1" ranges at both the input and output.

The four voltages are:

- VIL : Maximum input voltage recognized as Logic 0.
- VIH : Minimum input voltage recognized as Logic 1.
- VOL : Maximum output voltage corresponding to Logic 0.
- VOH : Minimum output voltage corresponding to Logic 1.

<img width="940" height="607" alt="image" src="https://github.com/user-attachments/assets/7e22397f-7367-4c41-9750-a283b03a50f0" />

 
<img width="940" height="572" alt="image" src="https://github.com/user-attachments/assets/784ae75f-0cec-4f68-829c-7a55119ad5a6" />


<img width="940" height="646" alt="image" src="https://github.com/user-attachments/assets/01404819-df53-43d6-9809-1b6ca8467410" />


<img width="940" height="593" alt="image" src="https://github.com/user-attachments/assets/237fd41f-017e-4cf7-9227-1d6e8366bdf1" />

</details>

<details>
<summary><b>L2 - Noise margin voltage parameters</b></summary>

<img width="940" height="551" alt="image" src="https://github.com/user-attachments/assets/b482e44d-be6d-4f9e-9439-250a76e24006" />

To determine the noise margins of a CMOS inverter, the most important points to inspect are those near the transition region of the Voltage Transfer Curve (VTC). These points define the boundaries between valid logic levels and the region where the inverter is highly sensitive to input variations.

Before the transition begins, consider the case where:


Vin = 0 V


The PMOS is ON and the NMOS is OFF.

As a result:

- The output capacitor is fully charged.
- Vout ≈ VDD.
- The output represents a valid Logic 1.

In this region, the slope of the VTC is very small:


|dVout/dVin| < 1


This is a desirable condition because any small noise appearing at the input is attenuated at the output.

For example, if a noise voltage ΔVin appears at the input:


ΔVout = (dVout/dVin) × ΔVin


Since:


|dVout/dVin| < 1


the output disturbance is smaller than the input disturbance.

Therefore:

- Noise is attenuated.
- Logic levels are restored.
- Cascaded logic gates operate reliably.

A similar condition exists near:


Vin ≈ VDD


where the output is close to Logic 0 and the magnitude of the gain is again less than one.

However, in the transition region, both PMOS and NMOS conduct simultaneously.

A small change in Vin now produces a large change in Vout.

Therefore:


|dVout/dVin| > 1


In this region:

- Noise can be amplified.
- The inverter becomes highly sensitive to input variations.
- The output rapidly transitions from VDD to 0 V.

Because the transition region is the most critical region for noise analysis, the boundaries of the valid logic levels are defined at the points where:


|dVout/dVin| = 1


These two points determine:

- VIL (Maximum Input Logic 0)
- VIH (Minimum Input Logic 1)

Graphically, they correspond to the locations on the VTC where the slope is equal to -1.

Therefore, the search for VIL and VIH begins by identifying the two points on the VTC where:


|dVout/dVin| = 1


Everything outside these points represents stable logic regions where noise is attenuated, while the region between them corresponds to the inverter transition region where the output changes rapidly from Logic 1 to Logic 0.

Now we need to compare this al most ideal curve with the actual graph we got as part of spice simulations. The corresponding VIL,VIH,VOH,VOL are marked. 
When the input is to be considered as a logic “0”, it should be below the VIL. At that time when the output is logic “1” it should be above VOH. This output might get connected to another logic as input as logic “1”. When input must be considered as logic “1”, it has to be above VIH. So VOH which might be sometimes become input to another logic also must satisfy VIH condition. So VOH should be always higher than VIH.

Like that when input is logic “1” it should be higher than VIH. When output is considered as logic “0” it should be less than VOL. When VOL when connected as an input to some other logic as logic “0” should satisfy the VIL condition.
VOL<VIL
VOH>VIH
So 
VOH>VIH>VIL>VOL


</details>

<details>
<summary><b>L3 - Noise margin equation and summary</b></summary>

<img width="940" height="620" alt="image" src="https://github.com/user-attachments/assets/16941290-9dce-4fc9-8a93-41988b493357" />

We should calculate high & low noise margins like this.

<img width="940" height="664" alt="image" src="https://github.com/user-attachments/assets/5465e850-baef-4908-b448-d0e47c67dd74" />

The undefined range should the range between high & low noise margins & this should be as minimum as possible. 
<img width="940" height="567" alt="image" src="https://github.com/user-attachments/assets/ffc62a20-917b-406a-9a04-d070d083ce5e" />

<img width="940" height="632" alt="image" src="https://github.com/user-attachments/assets/a57588d8-cf02-4318-aa6f-c34a4fc6033d" />

Basically if any bump in the input or output voltage reaches this undefined region, the transistor will get confused to take it under logic "1" or "0".

</details>

<details>
<summary><b>L4 - Noise margin variation with respect to PMOS width</b></summary>

There are many regions where gain=1, gain>1 & gain <1.
As an inverter, during the normal state, where a logic “0” should stay in the logic “0” state even during the addition of noise & for logic “1” for the same, gain<1. Why because that time when gain >1, means change in Vout is more than change in Vin, the input with noise addition will get amplified. This should not happen. But during the switching time, we can have more gain, because that’s the expected outcome. When Vdd is 1.8V, after VIL, the output VOH, should have a sweep atleast till Vdd itself. So gain is more than 1. 

<img width="940" height="504" alt="image" src="https://github.com/user-attachments/assets/6a396744-6b43-4742-b4a1-1d4359cd95d3" />


So when a cmos inverter is immune to noise, it should have a broad spectrum of high & low noise margin. So that the noise region becomes low. Now we need to see the noise margin variations with respect to nmos/pmos device parameters.  Pmos is responsible for holding the capacitor charge. So when the size of pmos increases, eventually the noise margin also increases.

<img width="940" height="482" alt="image" src="https://github.com/user-attachments/assets/f1926d63-8e73-460d-a545-43236121b2a3" />

But when as pmos size increases, nmos becomes weaker, so the ability of nmos to take out the current from output capacitor decreases.

<img width="940" height="466" alt="image" src="https://github.com/user-attachments/assets/2e0d605b-3ef2-42d9-bfb2-4bfe709e1e8f" />

After a certain limit. The noise margins will become constant.

<img width="940" height="418" alt="image" src="https://github.com/user-attachments/assets/e9f5adc7-a615-4208-ab06-7f0102820040" />


<img width="940" height="566" alt="image" src="https://github.com/user-attachments/assets/ea024b02-82dd-4306-a421-b3a738a3220a" />

This region can be used for digital design because digital design is all about considering Logic 1 & 0.

<img width="940" height="528" alt="image" src="https://github.com/user-attachments/assets/da3a8567-224f-46ff-8b16-e58f632d993b" />

</details>

<details>
<summary><b>L5 - Noise margin labs</b></summary>

<img width="940" height="489" alt="image" src="https://github.com/user-attachments/assets/f612e60d-745e-4c60-ac1d-9811d8e2518c" />

<img width="940" height="742" alt="image" src="https://github.com/user-attachments/assets/0b784723-a0d1-4acb-8c23-0732c3156937" />

**NMh= VOH-VIH=703mV**
**NMl=VIL-VOL=667Mv**

</details>


## Day 5️⃣: Power Supply & Process Variation Evaluation

### Static behavior evaluation – CMOS inverter robustness – Power supply variation

<details>
<summary><b>L1 - Smart SPICE simulation for power supply variations</b></summary>

<img width="940" height="323" alt="image" src="https://github.com/user-attachments/assets/9631c156-1318-41b3-9daa-7833711d21f3" />

To evaluate the robustness of a CMOS inverter, it is often necessary to analyze its behavior under different supply voltages (VDD). Parameters such as switching threshold, noise margin, delay, and power consumption can vary significantly with supply voltage.

One straightforward approach is to create multiple SPICE files, where each file uses a different value of VDD and runs the same simulation.

For example:


File 1 → VDD = 1.8 V
File 2 → VDD = 2.0 V
File 3 → VDD = 2.2 V
File 4 → VDD = 2.5 V


However, this approach becomes tedious because:

- Multiple SPICE files must be maintained.
- Any circuit modification must be updated in every file.
- Simulation setup becomes repetitive and error-prone.

To simplify the process, TCL scripting can be embedded inside the SPICE control block. Instead of creating multiple netlists, a single SPICE file can automatically sweep through different VDD values and execute the required simulations.

The TCL script can:

- Change the VDD value automatically.
- Run the simulation for each supply voltage.
- Store the output data.
- Generate plots for comparison.

This approach provides:

- Better automation.
- Reduced manual effort.
- Easier robustness analysis.
- Faster evaluation across multiple operating conditions.

Therefore, instead of maintaining "n" separate SPICE files for "n" different supply voltages, a single SPICE netlist combined with TCL scripting can be used to automate the entire VDD sweep and robustness analysis process.


<img width="814" height="376" alt="image" src="https://github.com/user-attachments/assets/fe911b76-984b-4009-96f5-8c7678d77007" />

### Purpose of this Script

This script automatically runs the inverter DC sweep for multiple values of VDD without creating separate SPICE files.

Instead of manually simulating:

```text
VDD = 2.5 V
VDD = 2.0 V
VDD = 1.5 V
VDD = 1.0 V
VDD = 0.5 V
```

the script changes VDD automatically and runs the simulation repeatedly.

---

### Step 1: Enter Control Mode

```spice
.control
```

Everything inside `.control` and `.endc` is interpreted as NGSPICE commands rather than circuit descriptions.

---

### Step 2: Initialize VDD

```spice
let powerSupply = 2.5
alter Vdd = powerSupply
```

Creates a variable called:

```text
powerSupply = 2.5 V
```

and assigns this value to the voltage source named `Vdd`.

Initially:

```text
VDD = 2.5 V
```

---

### Step 3: Create a Loop Counter

```spice
let voltageSupplyVariation = 0
```

This variable keeps track of how many simulations have been completed.

Initially:

```text
voltageSupplyVariation = 0
```

---

### Step 4: Start the Loop

```spice
dowhile voltageSupplyVariation < 5
```

Run the commands inside the loop while:

```text
voltageSupplyVariation < 5
```

Therefore the loop executes:

```text
0
1
2
3
4
```

Total:

```text
5 simulations
```

---

### Step 5: Run DC Sweep

```spice
dc Vin 0 2.5 0.01
```

Perform a DC sweep of the inverter input.

SPICE solves the circuit for:

```text
Vin = 0.00 V
Vin = 0.01 V
Vin = 0.02 V
...
Vin = 2.50 V
```

This generates one complete VTC curve for the current VDD value.

---

### Step 6: Reduce Supply Voltage

```spice
let powerSupply = powerSupply - 0.5
```

After one simulation finishes:

```text
2.5 → 2.0
2.0 → 1.5
1.5 → 1.0
1.0 → 0.5
0.5 → 0.0
```

The supply voltage is reduced by:

```text
0.5 V
```

each iteration.

---

### Step 7: Update the Circuit

```spice
alter Vdd = powerSupply
```

Changes the actual voltage source value inside the circuit.

For example:

```text
First run  → VDD = 2.5 V
Second run → VDD = 2.0 V
Third run  → VDD = 1.5 V
Fourth run → VDD = 1.0 V
Fifth run  → VDD = 0.5 V
```

---

### Step 8: Increment Counter

```spice
let voltageSupplyVariation = voltageSupplyVariation + 1
```

Counter becomes:

```text
0 → 1
1 → 2
2 → 3
3 → 4
4 → 5
```

---

### Step 9: End Loop

```spice
end
```

When:

```text
voltageSupplyVariation = 5
```

the condition becomes false and the loop stops.

---

### Step 10: Plot All Curves Together

```spice
plot dc1.out vs in dc2.out vs in dc3.out vs in dc4.out vs in dc5.out vs in
```

Plot the output voltage versus input voltage for all five simulations on the same graph.

Therefore we obtain:

```text
VTC for VDD = 2.5 V
VTC for VDD = 2.0 V
VTC for VDD = 1.5 V
VTC for VDD = 1.0 V
VTC for VDD = 0.5 V
```

on a single plot.

---

### Axis Labels

```spice
xlabel "input voltage [V]"
ylabel "output voltage [V]"
```

Sets:

```text
X-axis → Input Voltage
Y-axis → Output Voltage
```

---

### Plot Title

```spice
title "Inverter dc characteristics as a function of supply voltage"
```

Adds a title to the graph.

---

### Exit NGSPICE

```spice
quit
.endc
```

Terminates the simulation and exits NGSPICE.

---

### In One Sentence

This script automatically sweeps the inverter input voltage for five different supply voltages (2.5 V, 2.0 V, 1.5 V, 1.0 V, and 0.5 V), plots all the VTC curves on the same graph, and helps evaluate how robust the inverter is against VDD variations.


<img width="791" height="644" alt="image" src="https://github.com/user-attachments/assets/bf157be9-71c7-4bfe-9750-4b1bbb7ec273" />

This is the output for the piece of code.
</details>

<details>
<summary><b>L2 - Advantages and disadvantages using low supply voltage</b></summary>

<img width="940" height="487" alt="image" src="https://github.com/user-attachments/assets/f38814b3-7183-451a-9f2e-f70dce6f5d61" />


After seeing the curves I can refer to the point that when Vdd is less, gain=Change in Vout/Change in Vin is more in inverters with low value of Vdd.

The term 1/2 CV_DD^2represents the energy stored in a load capacitor when it is charged from 0 V to V_DD. During charging, the power supply delivers CV_DD^2energy, of which half is stored in the capacitor and half is dissipated as heat in the PMOS transistor. When the capacitor discharges through the NMOS, the stored energy is also dissipated as heat. This charging and discharging of capacitances is the primary source of dynamic power consumption in CMOS circuits. Therefore energy loss will be less for CMOS inverters with less Vdd.

<img width="940" height="395" alt="image" src="https://github.com/user-attachments/assets/38b2045f-0ba3-4199-b2ba-5cf98739c50a" />

**Disadvantages of low Vdd**

<img width="940" height="513" alt="image" src="https://github.com/user-attachments/assets/fadf3799-2960-4f02-b0f2-2c359d6b9b2e" />

Rise and fall delay is very high for cmos with low Vdd. Means it cant charge the capacitor fully upto the Vdss & it cant discharge the capacitor fully. It will be something like this. Then it will effect the speed of the circuit & noise margin also.

<img width="940" height="522" alt="image" src="https://github.com/user-attachments/assets/efd7b26a-6387-4f01-bdbc-254549794f49" />

</details>

<details>
<summary><b>L3 - Supply Variation Labs</b></summary>

<img width="940" height="392" alt="image" src="https://github.com/user-attachments/assets/3df9909b-3259-4e1f-8cfc-76cf725307f4" />

<img width="940" height="749" alt="image" src="https://github.com/user-attachments/assets/0c8e36ad-3123-4b15-89b4-aeee8498e9ea" />

<img width="940" height="497" alt="image" src="https://github.com/user-attachments/assets/e04a6acc-9bab-43b5-8159-f63eb240b832" />

<img width="940" height="765" alt="image" src="https://github.com/user-attachments/assets/e48dbf31-4db8-4326-938b-986a78a74858" />

**For 1.8V transistor,
Gain= (1.69303-0.102326)/(0.779775-0.980899) = 7.92Db**

<img width="940" height="651" alt="image" src="https://github.com/user-attachments/assets/2b7adc56-3a0c-4e61-9262-163170afde57" />

**For 1V transistor, Gain = (0.939535-0.534884)/(0.498876-0.576484)=-5.21Db**

Why gain reduced? As discussed the supply voltage was not enough to drive pmos to charge the capacitor. 


</details>

---

## Static behavior evaluation – CMOS inverter robustness – Device variation

<details>
<summary><b>L1 - Sources of variation – Etching process</b></summary>

Now we need to understand how sources of variations effect the robustness of CMOS. Sources of variations means the factors such as length, width, oxide thickness etc (physical factors). We need to check whether device variations will effect cmos inverters or not?


1.	Etching

<img width="940" height="396" alt="image" src="https://github.com/user-attachments/assets/f2fe2e14-1bd8-4684-a9e5-dbb3c897f3c1" />

This is the layout of a cmos inverter looks like. AS in the figure, we can see there will be p & n diffusion layers. Poly gate where the common input for both pmos & nmos are also present. These physical layers during fabrication are etched on the substrate. When the layers are not properly etched or has some process variations it can effect the performance of of the inverter. One among the factor is W & L. We know that L (channel length) defines the technology node itself. Width, which is the common area of the gate & diffusion also when not fabricated correctly, will effect the drain current.

<img width="940" height="358" alt="image" src="https://github.com/user-attachments/assets/ed22cf26-889f-4ddc-af26-891f92463dc0" />

</details>
<details>
<summary><b>L2 - Sources of variation - Oxide thickness</b></summary>

2.	Oxide thickness

<img width="940" height="495" alt="image" src="https://github.com/user-attachments/assets/c4375d45-0b19-46d0-8390-c5a016057260" />

As you can see in the figure, when the cross sectional view of nmos is taken, the idea oxide is expected to be fabricated with a thickness of tox. But practical situation might contain some distortions as shown in the figure.

<img width="940" height="424" alt="image" src="https://github.com/user-attachments/assets/f13c5aa8-94ec-4988-b536-f846237ae2a6" />

Since drain current is also dependent on the thickness of the oxide, it can be effected.

</details>

<details>
<summary><b>L3 - Smart SPICE simulation for device variations</b></summary>

<img width="940" height="300" alt="image" src="https://github.com/user-attachments/assets/52792ae9-86d7-4c25-9a7b-61b02a076e8e" />

Now inorder to check that, let us assume that, there are 2 possible situations with (strong pmos & weak nmos,) & (weak pmos & strong nmos). Let us assume that these are the maximum & minimum  possible widths & lenghths pf nmos & pmos. Now lets simulate & see how reactive the cmos transfer characteristics are with respect to the device variations.

<img width="842" height="509" alt="image" src="https://github.com/user-attachments/assets/c9fec2d1-cfb4-4802-901c-8258e2fa8a43" />

This is the spice deck.


<img width="784" height="635" alt="image" src="https://github.com/user-attachments/assets/dd41c6c8-6fac-4b7b-9173-ccb8236912d2" />

</details>

<details>
<summary><b>L4 - Conclusion</b></summary>

<img width="940" height="576" alt="image" src="https://github.com/user-attachments/assets/4df8655f-3e43-4b97-9033-b2a7c96121bb" />

After testing the switching threshold got sgifted little only. It didn’t effect the switching threshold of the same.

<img width="940" height="462" alt="image" src="https://github.com/user-attachments/assets/51bf9788-3993-40ec-9b6b-2e15e6028252" />

Noise margin is not variable enough. Its also good enough. Just 300Mv variation.

<img width="466" height="170" alt="image" src="https://github.com/user-attachments/assets/73f3a23e-5129-4364-aedb-8cfcb62e6963" />

</details>

<details>
<summary><b>L5 - Sky130 device variations labs</b></summary>

<img width="940" height="500" alt="image" src="https://github.com/user-attachments/assets/33303c17-8db7-4811-9a7a-5c190d3160ab" />


Here the pmos width is very high when compared to nmos.

<img width="940" height="779" alt="image" src="https://github.com/user-attachments/assets/e882c072-626d-445d-bed2-6a63251726fc" />

<img width="940" height="506" alt="image" src="https://github.com/user-attachments/assets/6fe3d1b8-6cf0-4cba-a2b9-089d81be8247" />

As we can see the holding area of vdd (output been 1) which is decided by the pmos is larger(left side) when compared to the discharging area (output been zero). This is because the drive strength of pmos is very high when compared to nmos.

<img width="807" height="964" alt="image" src="https://github.com/user-attachments/assets/e31c7633-aa3f-47ea-be9b-13a5da70d87e" />


Switching threshold calculation: 0.9-0.989415
Approximately 80Mv
