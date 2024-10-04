# Designing a Bandgap reference circuit with SCMB configuration using ASAP 7nm PDk
## contents 
## Introduction 

### Week 1 - Research Paper on Bandgap Circuit using ASAP 7nm PDK 

#### What is Finfet? 
The definition of a Finfet by Synopsys is " 
A FinFET is a type of field-effect transistor (FET) that has a thin vertical fin instead of being completely planar. The gate is fully “wrapped” around the channel on three sides formed between the source and the drain. The greater surface area created between the gate and channel provides better control of the electric state and reduces leakage compared to planar FETs. Using FinFETs, results in much better electrostatic control of the channel and thus better electrical characteristics than planar FETs." 
FinFETs serve as the foundation for contemporary nanoelectronic semiconductor device manufacturing. These microchips, which employ FinFET technology, were commercialized in the early 2010s and became the primary gate design for process nodes at 14 nm, 10 nm, and 7 nm. Typically, a single FinFET transistor consists of multiple fins positioned side by side, all enveloped by the same gate, allowing them to function electrically as a single unit to enhance drive strength and performance.
<picture>
<img alt = "Finfet diagram." src="finfet.png">
</picture>

#### What is Bandgap reference voltage ? 
A bandgap reference voltage is a voltage source that generates an output voltage proportional to the bandgap energy of a semiconductor. The most commonly used bandgap references are based on silicon (Si), which typically produce an output of around 1.2 V. Gallium arsenide (GaAs) can also be utilized to create a reference source with a higher output voltage due to its wider bandgap of 1.42 eV, as shown in a recent research study. In theory, any semiconductor can be employed to develop a bandgap voltage reference, provided it can be deposited on standard wafer materials.
#### Bandgap reference circuit with SCMB 
The BGR is essential for providing a stable reference voltage that remains consistent regardless of temperature, supply voltage, and process variations. A Self-Biased Current Mirror (SBCM)-based BGR is used , which improves upon traditional designs that combine Proportional-to-Absolute-Temperature (PTAT) and Complementary-to-Absolute-Temperature (CTAT) voltages. The SBCM architecture enhances scalability and is particularly advantageous for low-power, high-precision applications. The SBCM design negates the need for external biasing through an internal feedback mechanism. Key challenges include managing temperature sensitivity and supply voltage sensitivity in the 7 nm process. The SBCM architecture mitigates temperature dependence by effectively balancing PTAT and CTAT voltages while addressing issues related to low-voltage operation.

### Week 2 - Characterization of CMOS VTC 

#### Nfet Id and Vd Characteristics 

#### CMOS Inverter_vtc Characteristics 
_ ##### W/L ratio 
     

