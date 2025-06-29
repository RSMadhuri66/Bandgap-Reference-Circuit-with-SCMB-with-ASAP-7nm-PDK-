# Designing a Bandgap reference circuit with SBCM configuration using ASAP 7nm PDk
## Contents 
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

#### Bandgap reference circuit with SBCM 
The BGR is essential for providing a stable reference voltage that remains consistent regardless of temperature, supply voltage, and process variations. A Self-Biased Current Mirror (SBCM)-based BGR is used , which improves upon traditional designs that combine Proportional-to-Absolute-Temperature (PTAT) and Complementary-to-Absolute-Temperature (CTAT) voltages. The SBCM architecture enhances scalability and is particularly advantageous for low-power, high-precision applications. The SBCM design negates the need for external biasing through an internal feedback mechanism. Key challenges include managing temperature sensitivity and supply voltage sensitivity in the 7 nm process. The SBCM architecture mitigates temperature dependence by effectively balancing PTAT and CTAT voltages while addressing issues related to low-voltage operation.

#### ASAP 7nm PDK 

The ASAP 7nm Process Design Kit (PDK) is a comprehensive suite of design resources tailored for the 7nm technology node, developed to facilitate the design and fabrication of integrated circuits (ICs) at this advanced scale. This PDK provides a detailed set of design rules, models, and guidelines that enable engineers to optimize their designs for performance, power efficiency, and area (PPA). ASAP 7nm supports a range of design flows, ensuring compatibility with various Electronic Design Automation (EDA) tools. Additionally, the PDK includes corner cases, design-for-manufacturability (DFM) features, and process variation models, which are critical for ensuring that designs yield successfully during fabrication.

Incorporating cutting-edge features, the ASAP 7nm PDK is designed to leverage the benefits of FinFET technology, allowing for significant improvements in transistor performance and reduced leakage current compared to traditional planar devices. This PDK also emphasizes robust power management capabilities, providing designers with tools to optimize dynamic and static power consumption. Furthermore, ASAP 7nm is aligned with industry standards, enabling designers to achieve rapid time-to-market while ensuring that their designs meet the stringent requirements of modern semiconductor applications, such as high-performance computing, mobile devices, and artificial intelligence.



### Week 2 - Characterization of CMOS VTC 

#### Nfet Id and Vd Characteristics 

Nfet spice file 

```
** sch_path: /home/maddy/asap_7nm_Xschem/nfet_char.sch
**.subckt nfet_char
V1 nfet_in GND 0
V2 vdd GND 3
R1 vdd nfet_out 1k m=1
Xnfet2 nfet_out nfet_in GND GND asap_7nm_nfet l=7e-009 nfin=14
**** begin user architecture code


.dc v1 0 0.7 1m v2 0 0.7 0.2
.control
run
set xbrushwidth=3
let vd = vdd - nfet_out
let id  = vd/1000
plot id
.endc


**** end user architecture code
**.ends
.GLOBAL GND
**** begin user architecture code

.subckt asap_7nm_nfet S G D B l=7e-009 nfin=14
	nnmos_finfet S G D B BSIMCMG_osdi_N l=7e-009 nfin=14
.ends asap_7nm_nfet

.model BSIMCMG_osdi_N BSIMCMG_va (
+ TYPE = 1
************************************************************
*                         general                          *
************************************************************
+version = 107             bulkmod = 1               igcmod  = 1               igbmod  = 0
+gidlmod = 1               iimod   = 0               geomod  = 1               rdsmod  = 0
+rgatemod= 0               rgeomod = 0               shmod   = 0               nqsmod  = 0
+coremod = 0               cgeomod = 0               capmod  = 0               tnom    = 25
+eot     = 1e-009          eotbox  = 1.4e-007        eotacc  = 1e-010          tfin    = 6.5e-009
+toxp    = 2.1e-009        nbody   = 1e+022          phig    = 4.2466          epsrox  = 3.9
+epsrsub = 11.9            easub   = 4.05            ni0sub  = 1.1e+016        bg0sub  = 1.17
+nc0sub  = 2.86e+025       nsd     = 2e+026          ngate   = 0               nseg    = 5
+l       = 2.1e-008        xl      = 1e-009          lint    = -2e-009         dlc     = 0
+dlbin   = 0               hfin    = 3.2e-008        deltaw  = 0               deltawcv= 0
+sdterm  = 0               epsrsp  = 3.9             nfin    = 1
+toxg    = 1.80e-009
************************************************************
*                            dc                            *
************************************************************
+cit     = 0               cdsc    = 0.01            cdscd   = 0.01            dvt0    = 0.05
+dvt1    = 0.47            phin    = 0.05            eta0    = 0.07            dsub    = 0.35
+k1rsce  = 0               lpe0    = 0               dvtshift= 0               qmfactor= 2.5
+etaqm   = 0.54            qm0     = 0.001           pqm     = 0.66            u0      = 0.0303
+etamob  = 2               up      = 0               ua      = 0.55            eu      = 1.2
+ud      = 0               ucs     = 1               rdswmin = 0               rdsw    = 200
+wr      = 1               rswmin  = 0               rdwmin  = 0               rshs    = 0
+rshd    = 0               vsat    = 70000           deltavsat= 0.2             ksativ  = 2
+mexp    = 4               ptwg    = 30              pclm    = 0.05            pclmg   = 0
+pdibl1  = 0               pdibl2  = 0.002           drout   = 1               pvag    = 0
+fpitch  = 2.7e-008        rth0    = 0.225           cth0    = 1.243e-006      wth0    = 2.6e-007
+lcdscd  = 5e-005          lcdscdr = 5e-005          lrdsw   = 0.2             lvsat   = 0
************************************************************
*                         leakage                          *
************************************************************
+aigc    = 0.014           bigc    = 0.005           cigc    = 0.25            dlcigs  = 1e-009
+dlcigd  = 1e-009          aigs    = 0.0115          aigd    = 0.0115          bigs    = 0.00332
+bigd    = 0.00332         cigs    = 0.35            cigd    = 0.35            poxedge = 1.1
+agidl   = 1e-012          agisl   = 1e-012          bgidl   = 10000000        bgisl   = 10000000
+egidl   = 0.35            egisl   = 0.35
************************************************************
*                            rf                            *
************************************************************
************************************************************
*                         junction                         *
************************************************************
************************************************************
*                       capacitance                        *
************************************************************
+cfs     = 0               cfd     = 0               cgso    = 1.6e-010        cgdo    = 1.6e-010
+cgsl    = 0               cgdl    = 0               ckappas = 0.6             ckappad = 0.6
+cgbo    = 0               cgbl    = 0
************************************************************
*                       temperature                        *
************************************************************
+tbgasub = 0.000473        tbgbsub = 636             kt1     = 0               kt1l    = 0
+ute     = -0.7            utl     = 0               ua1     = 0.001032        ud1     = 0
+ucste   = -0.004775       at      = 0.001           ptwgt   = 0.004           tmexp   = 0
+prt     = 0               tgidl   = -0.007          igt     = 2.5
************************************************************
*                          noise                           *
************************************************************
**)
.control
pre_osdi /home/maddy/asap_7nm_Xschem/bsimcmg_arch64.osdi
.endc


**** end user architecture code
.end

```
<picture>
<img alt = "Id vs Nfet_in" src="idnfetin.png">
</picture>


The plot Id vs Vd

<picture>
<img alt = "Id vs Vd" src="idvd.png">
</picture>

The schematic of Nfet is given below. It's created in Xschem. 

<picture>
<img alt = "Schematic" src="nfetschem.png">
</picture>


### CMOS Inverter_vtc Characteristics 
##### Inverter_vtc.spice File

```

** sch_path: /home/maddy/asap_7nm_Xschem/inverter_vtc.sch
**.subckt inverter_vtc
Xpfet1 nfet_out nfet_in vdd vdd asap_7nm_pfet l=7e-009 nfin=14
Xnfet1 nfet_out nfet_in GND GND asap_7nm_nfet l=7e-009 nfin=14
V1 nfet_in GND pulse(0 0.7 20p 10p 10p 20p 500p 1)
V2 vdd GND 0.7
**** begin user architecture code


.dc v1 0 0.7 1m
*.tran 1e-12 100e-12

.control
    * First run DC
    dc v1 0 0.7 1m
    run

    * DC measurements
    meas dc v_th when nfet_out = nfet_in
    plot nfet_out nfet_in
    
    let gain_av = abs(deriv(nfet_out))
    meas dc max_gain max gain_av
    let gain_target = max_gain * 0.999
    meas dc vil find nfet_in when gain_av = gain_target cross=1
    meas dc voh find nfet_out when gain_av = gain_target cross=1
    meas dc vih find nfet_in when gain_av = gain_target cross=2
    meas dc vol find nfet_out when gain_av = gain_target cross=2
    let nmh = voh - vih
    let nml = vil - vol
    print v_th max_gain vil voh vih vol nmh nml
    
    *Transconductance
    
    let id = v2#branch
    let gm = real(deriv(id, nfet_in))
    meas dc gm_max MAX gm
    plot gm
    let r_out= deriv(nfet_out,id)
    plot r_out
    plot id
    
    * Transient measurements
    tran 1e-12 100e-12
    meas tran tpr when nfet_in = 0.35 rise = 1
    meas tran tpf when nfet_out = 0.35 fall = 1
    let tp = (tpr + tpf) / 2
    let trans_current = v2#branch
    meas tran id_pwr integ trans_current from=2e-11 to=6e-11
    let pwr = id_pwr * 0.7
    let power = abs(pwr / 40e-12)
    print tpr tpf tp id_pwr pwr power
  
    tran 0.1 100p                         
    meas tran tr when nfet_in=0.07 RISE=1  
    meas tran tf when nfet_out=0.63 FALL=1 
    let t_delay = tr + tf                  
    print t_delay                         
    let f = 1/t_delay                     
    print f                              

    

.endc




**** end user architecture code
**.ends
.GLOBAL GND
**** begin user architecture code

.subckt asap_7nm_pfet S G D B l=7e-009 nfin=14
	npmos_finfet S G D B BSIMCMG_osdi_P l=7e-009 nfin=14
.ends asap_7nm_pfet

.model BSIMCMG_osdi_P BSIMCMG_va (
+ TYPE = 0

************************************************************
*                         general                          *
************************************************************
+version = 107             bulkmod = 1               igcmod  = 1               igbmod  = 0
+gidlmod = 1               iimod   = 0               geomod  = 1               rdsmod  = 0
+rgatemod= 0               rgeomod = 0               shmod   = 0               nqsmod  = 0
+coremod = 0               cgeomod = 0               capmod  = 0               tnom    = 25
+eot     = 1e-009          eotbox  = 1.4e-007        eotacc  = 3e-010          tfin    = 6.5e-009
+toxp    = 2.1e-009        nbody   = 1e+022          phig    = 4.9278          epsrox  = 3.9
+epsrsub = 11.9            easub   = 4.05            ni0sub  = 1.1e+016        bg0sub  = 1.17
+nc0sub  = 2.86e+025       nsd     = 2e+026          ngate   = 0               nseg    = 5
+l       = 2.1e-008        xl      = 1e-009          lint    = -2.5e-009       dlc     = 0
+dlbin   = 0               hfin    = 3.2e-008        deltaw  = 0               deltawcv= 0
+sdterm  = 0               epsrsp  = 3.9             nfin    = 1
+toxg    = 1.8e-009
************************************************************
*                            dc                            *
************************************************************
+cit     = 0               cdsc    = 0.003469        cdscd   = 0.001486        dvt0    = 0.05
+dvt1    = 0.36            phin    = 0.05            eta0    = 0.094           dsub    = 0.24
+k1rsce  = 0               lpe0    = 0               dvtshift= 0               qmfactor= 0
+etaqm   = 0.54            qm0     = 2.183e-012      pqm     = 0.66            u0      = 0.0237
+etamob  = 4               up      = 0               ua      = 1.133           eu      = 0.05
+ud      = 0.0105          ucs     = 0.2672          rdswmin = 0               rdsw    = 200
+wr      = 1               rswmin  = 0               rdwmin  = 0               rshs    = 0
+rshd    = 0               vsat    = 60000           deltavsat= 0.17            ksativ  = 1.592
+mexp    = 2.491           ptwg    = 25              pclm    = 0.01            pclmg   = 1
+pdibl1  = 800             pdibl2  = 0.005704        drout   = 4.97            pvag    = 200
+fpitch  = 2.7e-008        rth0    = 0.15            cth0    = 1.243e-006      wth0    = 2.6e-007
+lcdscd  = 0               lcdscdr = 0               lrdsw   = 1.3             lvsat   = 1441
************************************************************
*                         leakage                          *
************************************************************
+aigc    = 0.007           bigc    = 0.0015          cigc    = 1               dlcigs  = 5e-009
+dlcigd  = 5e-009          aigs    = 0.006           aigd    = 0.006           bigs    = 0.001944
+bigd    = 0.001944        cigs    = 1               cigd    = 1               poxedge = 1.152
+agidl   = 2e-012          agisl   = 2e-012          bgidl   = 1.5e+008        bgisl   = 1.5e+008
+egidl   = 1.142           egisl   = 1.142
************************************************************
*                            rf                            *
************************************************************
************************************************************
*                         junction                         *
************************************************************
************************************************************
*                       capacitance                        *
************************************************************
+cfs     = 0               cfd     = 0               cgso    = 1.6e-010        cgdo    = 1.6e-010
+cgsl    = 0               cgdl    = 0               ckappas = 0.6             ckappad = 0.6
+cgbo    = 0               cgbl    = 0
************************************************************
*                       temperature                        *
************************************************************
+tbgasub = 0.000473        tbgbsub = 636             kt1     = 0               kt1l    = 0
+ute     = -1.2            utl     = 0               ua1     = 0.001032        ud1     = 0
+ucste   = -0.004775       at      = 0.001           ptwgt   = 0.004           tmexp   = 0
+prt     = 0               tgidl   = -0.007          igt     = 2.5
************************************************************
*                          noise                           *
************************************************************
**)
.control
pre_osdi /home/maddy/asap_7nm_Xschem/bsimcmg_arch64.osdi
.endc



.subckt asap_7nm_nfet S G D B l=7e-009 nfin=14
	nnmos_finfet S G D B BSIMCMG_osdi_N l=7e-009 nfin=14
.ends asap_7nm_nfet

.model BSIMCMG_osdi_N BSIMCMG_va (
+ TYPE = 1
************************************************************
*                         general                          *
************************************************************
+version = 107             bulkmod = 1               igcmod  = 1               igbmod  = 0
+gidlmod = 1               iimod   = 0               geomod  = 1               rdsmod  = 0
+rgatemod= 0               rgeomod = 0               shmod   = 0               nqsmod  = 0
+coremod = 0               cgeomod = 0               capmod  = 0               tnom    = 25
+eot     = 1e-009          eotbox  = 1.4e-007        eotacc  = 1e-010          tfin    = 6.5e-009
+toxp    = 2.1e-009        nbody   = 1e+022          phig    = 4.2466          epsrox  = 3.9
+epsrsub = 11.9            easub   = 4.05            ni0sub  = 1.1e+016        bg0sub  = 1.17
+nc0sub  = 2.86e+025       nsd     = 2e+026          ngate   = 0               nseg    = 5
+l       = 2.1e-008        xl      = 1e-009          lint    = -2e-009         dlc     = 0
+dlbin   = 0               hfin    = 3.2e-008        deltaw  = 0               deltawcv= 0
+sdterm  = 0               epsrsp  = 3.9             nfin    = 1
+toxg    = 1.80e-009
************************************************************
*                            dc                            *
************************************************************
+cit     = 0               cdsc    = 0.01            cdscd   = 0.01            dvt0    = 0.05
+dvt1    = 0.47            phin    = 0.05            eta0    = 0.07            dsub    = 0.35
+k1rsce  = 0               lpe0    = 0               dvtshift= 0               qmfactor= 2.5
+etaqm   = 0.54            qm0     = 0.001           pqm     = 0.66            u0      = 0.0303
+etamob  = 2               up      = 0               ua      = 0.55            eu      = 1.2
+ud      = 0               ucs     = 1               rdswmin = 0               rdsw    = 200
+wr      = 1               rswmin  = 0               rdwmin  = 0               rshs    = 0
+rshd    = 0               vsat    = 70000           deltavsat= 0.2             ksativ  = 2
+mexp    = 4               ptwg    = 30              pclm    = 0.05            pclmg   = 0
+pdibl1  = 0               pdibl2  = 0.002           drout   = 1               pvag    = 0
+fpitch  = 2.7e-008        rth0    = 0.225           cth0    = 1.243e-006      wth0    = 2.6e-007
+lcdscd  = 5e-005          lcdscdr = 5e-005          lrdsw   = 0.2             lvsat   = 0
************************************************************
*                         leakage                          *
************************************************************
+aigc    = 0.014           bigc    = 0.005           cigc    = 0.25            dlcigs  = 1e-009
+dlcigd  = 1e-009          aigs    = 0.0115          aigd    = 0.0115          bigs    = 0.00332
+bigd    = 0.00332         cigs    = 0.35            cigd    = 0.35            poxedge = 1.1
+agidl   = 1e-012          agisl   = 1e-012          bgidl   = 10000000        bgisl   = 10000000
+egidl   = 0.35            egisl   = 0.35
************************************************************
*                            rf                            *
************************************************************
************************************************************
*                         junction                         *
************************************************************
************************************************************
*                       capacitance                        *
************************************************************
+cfs     = 0               cfd     = 0               cgso    = 1.6e-010        cgdo    = 1.6e-010
+cgsl    = 0               cgdl    = 0               ckappas = 0.6             ckappad = 0.6
+cgbo    = 0               cgbl    = 0
************************************************************
*                       temperature                        *
************************************************************
+tbgasub = 0.000473        tbgbsub = 636             kt1     = 0               kt1l    = 0
+ute     = -0.7            utl     = 0               ua1     = 0.001032        ud1     = 0
+ucste   = -0.004775       at      = 0.001           ptwgt   = 0.004           tmexp   = 0
+prt     = 0               tgidl   = -0.007          igt     = 2.5
************************************************************
*                          noise                           *
************************************************************
**)
.control
pre_osdi /home/maddy/asap_7nm_Xschem/bsimcmg_arch64.osdi
.endc


**** end user architecture code
.end

``` 



##### 2.1 W/L RATIO

The week 2 session starts with calculation of W/L ratio which is important in the CMOS because it has a direct impact on the transistor's electrical properties, such as its current drive capability, switching speed, and power consumption. This ratio essentially dictates the overall performance and efficiency of the entire circuit design. For 7nm PDK we would be varying only ```nfins``` for npmos and nnmos in the spice circuit. The length is going to be constant which is 7nm. We start with varying the nfins value and calculating the other parameters 

##### 2.2 SWITCHING THRESHOLD VOLTAGE  

The switching threshold, denoted as ```V_th```, is defined as the point where the input voltage ```nfet_in``` equals the output voltage ```nfet_out```. The ratio of the relative driving strengths of the PMOS and NMOS transistors determines this switching threshold. To raise ```V_th```, a larger ratio is needed, which involves widening the PMOS transistor. Conversely, enhancing the strength of the NMOS transistor will shift the switching threshold closer to ground (GND).

Spice command for calculating V_th 
``` meas dc v_th when nfet_out=nfet_in``` 

<picture>
<img alt = "CMOS VTC" src="vtc.png">
</picture>

##### 2.3 DRAIN CURRENT (Id) 

Drain current (𝐼𝐷) is the current that flows from the drain terminal to the source terminal in a field-effect transistor (FET). It is primarily controlled by the voltage applied at the gate terminal, which creates an electric field that modulates the conductivity of the channel between the drain and source. As the gate voltage increases, the channel becomes more conductive, allowing more current to flow from the drain to the source. In the saturation region, the drain current reaches a constant value, independent of the drain-source voltage, while in the triode region, it increases linearly with the drain-source voltage. The drain current is a key parameter in determining the performance of transistors in various applications, such as amplifiers and digital circuits. 

Here the drain current is calculated using ```v2#branch current ```. We can get this by using ``` display ``` in NGSPICE. 
The commands that were used to plot Id  
```
let id=v2#branch
plot id

```
<picture>
<img alt = "Drain Current" src="id.png">
</picture>

##### 2.4 Gate Capacitance 

##### 2.5 Power Consumption 

Power Consumption is the product of integral of transient current and Vdd divided by the time period. 

Spice commands: 

```
   let trans_current = v2#branch
    meas tran id_pwr integ trans_current from=20e-12 to=60e-12
    let pwr = id_pwr *0.7
    let power = abs(pwr/40)
    print power

```


#### 2.6 Propagation Delay (Tp) 

The propagation delay of a logic gate e.g. inverter is the difference in time (calculated at 50% of input-output transition), when output switches, after application of input. Rise time (tr) is the time, during transition, when output switches from 10% to 90% of the maximum value. Fall time (tf) is the time, during transition, when output switches from 90% to 10% of the maximum value. Many designs could also prefer 30% to 70% for rise time and 70% to 30% for fall time. It could vary upto different designs.

Spice Commands 
```
meas tran tpr when nfet_in=0.35 RISE=1 : Measures the rise time (tpr) when the input voltage reaches 0.35V
meas tran tpf when nfet_out=0.35 FALL=1 : Measures the fall time (tpf) when the output voltage reaches 0.35V. 
let tp = (tpf + tpr)/2 : Calculates the average propagation delay (tp) as the mean of the rise and fall times.
print tp : prints the Propagation Delay

```

##### 2.7 Gain (Av) 

Gain is defined as Change in output voltage to that of input voltage. 
Spice Commands to calculate gain 
```
let gain_av=deriv(nfet_out) : this gives out negative gain
** to get abs gain we can use

let gain_av=abs(deriv(nfet_out)) : Gives the absolute value of gain.
plot gain
```

##### 2.8 Noise Margin 

Noise margin in a CMOS voltage transfer characteristic (VTC) refers to the tolerance a digital circuit has to noise before signal integrity is compromised. It is defined by two key metrics: Noise Margin High (NMH) and Noise Margin Low (NML). NMH is the difference between the minimum output high voltage (VOH) and the minimum input high voltage (VIH) required to recognize a logic high signal. NML is the difference between the maximum output low voltage (VOL) and the maximum input low voltage (VIL) required to recognize a logic low signal. Larger noise margins ensure the circuit is more resistant to voltage fluctuations or noise, maintaining proper logic level detection.

Spice Commands: 

```
meas dc vil find nfet_in when nfet_out=gain CROSS=1   : Measures the (vil, the "input low voltage") when (nfet_out) crosses the gain value for the first time (CROSS=1).
meas dc voh find nfet_out when nfet_out=gain CROSS=1  : Measures the (voh, the "output high voltage") at the same crossing point as vil. 
meas dc vih find nfet_in when nfet_out=gain CROSS=2   : Measures the (vih, the "input high voltage") when the output voltage crosses the gain value for the second time (CROSS=2). 
meas dc vol find nfet_out when nfet_out=gain CROSS=2  : Measures the (vol, the "output low voltage") at the second crossing point. This represents the low state of the output.

let nmh=voh-vih                                       : Calculates the noise margin high (nmh)
print nmh                                             : Prints the calculated noise margin high.
let nml=vil-vol                                       : Calculates the noise margin low (nml)
print nml                                             : Prints the calculated noise margin low.

```

##### 2.9 Transconductance (Gm) 

Transconductance is defined as the ratio of change in drain current and change in Vgs (Gate-source Voltage). In our case, we already have drain current and VGS is input voltage which is ```nfet_in```. 

Spice commands 
```
let gm = real(deriv(id, nfet_in)) : Calculates the transconductance (gm) as the derivative of the branch current with respect to the input voltage.
meas dc gm_max MAX gm: Measures the maximum transconductance (gm_max).
plot gm

````
The below graph is not the absolute value, for positive results please use abs command. 

<picture>
<img alt = "Transconductance" src="gm.png">
</picture>

##### 2.10 Frequency (f) 

In this case, the maximum signal frequency was calculated, using delay time. As disscussed above about rise time (tr) and fall time (tf). So the frequency would be 1/(tr+tf) 
Spice commands used for this : 
```
tran 0.1 100p                          : Performs a transient analysis with a time step of 0.1ns and a total simulation time of 100ps.
meas tran tr when nfet_in=0.07 RISE=1  : Measures the rise time (tr) when the input voltage reaches 0.07V.
meas tran tf when nfet_out=0.63 FALL=1 : Measures the fall time (tf) when the output voltage reaches 0.63V.
let t_delay = tr + tf                  : Calculates the total delay time as the sum of rise and fall times.
print t_delay                          : Prints the total delay time.
let f = 1/t_delay                      : Calculates the frequency (f) as the reciprocal of the total delay time.
print f                                : Prints the frequency.

```

##### 2.11 Output Resistance  

The output resistance is defined as the ratio of output node voltage and the change in drain current. So here we are taking derivative of output voltage and derivative of the drain current. 
Spice commands: 

```
let r_out= deriv(nfet_out,id)   : Calculates the output resistance by taking the derivative of the output voltage with respect to the branch current.
plot r_out                      : Plots the output resistance.

```
<picture>
<img alt = "Output Resistance" src="r_out.png">
</picture>

Schematic of the Inverter 

<picture>
<img alt = "Inverter_VTC" src="invsch.png">
</picture>

##### Characteristics Table 

| Sr. No | W (Width) pmos | L (Length) pmos | (W/L Ratio) pmos | W (Width) nmos | L (Length) nmos | (W/L Ratio) nmos | Switching Threshold Voltage (VTC) -mV | Drain Current (Id) (μA) | Power Consumption (P) | Propagation Delay (t_pd) (ps) | Gain (Av) | Noise Margin (NM) | Transconductance (gm) X10^-3 | Frequency (f) (GHz) | Output Resistance (Ro) |
|--------|-----------------|------------------|------------------|----------------|------------------|------------------|-------------------------------------|--------------------------|-----------------------|------------------------------|-----------|-------------------|----------------------------|---------------------|------------------------|
| 1      | 10              | 7 nm             | 1.43             | 14             | 7 nm             | 2                | 321.54                              | 191.1                    | 2.674 × 10^-17        | 0.2511                      | 6.418     | NMH=0.2355V, NML=0.1346V | 9.695                      | 22.592              | 6.4122                 |
| 2      | 11              | 7 nm             | 1.57             | 15             | 7 nm             | 2.14             | 323.35                              | 208.112                  | 2.908 × 10^-17        | 0.2513                      | 6.4734    | NMH=0.2334V, NML=0.1361V | 1.059                      | 22.584              | 6.4736                 |
| 3      | 16              | 7 nm             | 2.29             | 15             | 7 nm             | 2.14             | 349.25                              | 249.45                   | 3.525 × 10^-17        | 0.2538                      | 6.4268    | NMH=0.1870V, NML=0.1761V | 1.384                      | 22.429              | 6.4265                 |
| 4      | 17              | 7 nm             | 2.43             | 15             | 7 nm             | 2.14             | 353.43                              | 256.272                  | 3.622 × 10^-17        | 0.25409                     | 6.4292    | NMH=0.1811V, NML=0.185V | 1.441                      | 22.393              | 6.4292                 |
| 5      | 17              | 7 nm             | 2.43             | 16             | 7 nm             | 2.29             | 348.98                              | 265.612                  | 3.754 × 10^-17        | 0.2537                      | 6.427     | NMH=0.1877V, NML=0.1761V | 1.472                      | 22.431              | 6.4271                 |
| 6      | 17              | 7 nm             | 2.43             | 17             | 7 nm             | 2.43             | 344.78                              | 274.515                  | 3.877 × 10^-17        | 0.2347                      | 6.4285    | NMH=0.1958V, NML=0.1685V | 1.5                        | 22.463              | 6.4284                 |
| 7      | 16              | 7 nm             | 2.29             | 17             | 7 nm             | 2.43             | 340.58                              | 266.85                   | 3.763 × 10^-17        | 0.2531                      | 6.43115   | NMH=0.2029V, NML=0.1619V | 1.438                      | 22.492              | 6.4311                 |
| 8      | 17              | 7 nm             | 2.43             | 14             | 7 nm             | 2                | 351.19                              | 246.447                  | 3.4822 × 10^-17       | 0.2543                      | 6.4339    | NMH=0.1730V, NML=0.1960V | 1.408                      | 22.349              | 6.4339                 |
| 9      | 16              | 7 nm             | 2.29             | 14             | 7 nm             | 2                | 354.01                              | 240.063                  | 3.39 × 10^-17         | 0.2541                      | 6.42865   | NMH=0.17988V, NML=0.18383V | 1.3533                    | 22.388              | 6.42865                |
| 10     | 16              | 7 nm             | 2.29             | 15             | 7 nm             | 2.14             | 349.25                              | 249.4516                 | 3.53 × 10^-17         | 0.2538                      | 6.42678   | NMH=0.1870V, NML=0.1768V | 1.384                      | 22.429              | 6.42678                |
| 11     | 16              | 7 nm             | 2.29             | 16             | 7 nm             | 2.29             | 354.01                              | 240.064                  | 3.39 × 10^-17         | 0.2541                      | 6.42865   | NMH=0.1798V, NML=0.1838V | 1.3533                    | 22.388              | 6.42865                |
| 12     | 15              | 7 nm             | 2.14             | 14             | 7 nm             | 2                | 349.5                               | 241.65                   | 3.29 × 10^-17         | 0.2538                      | 6.4272    | NMH=0.1874V, NML=0.1764V | 1.295                      | 22.426              | 6.4275                 |
| 13     | 15              | 7 nm             | 2.14             | 15             | 7 nm             | 2.14             | 344.78                              | 242.21                   | 3.42 × 10^-17         | 0.2534                      | 6.4283    | NMH=0.1958V, NML=0.16851V | 1.32                       | 22.463              | 6.4285                 |
| 14     | 14              | 7 nm             | 2                | 15             | 7 nm             | 2.14             | 340.41                              | 234.53                   | 3.307 × 10^-17        | 0.25307                     | 6.4628    | NMH=0.2033V, NML=0.1617V | 1.26                       | 22.496              | 6.4326                 |
| 15     | 13              | 7 nm             | 1.86             | 14             | 7 nm             | 2                | 339.65                              | 218.36                   | 3.07 × 10^-17         | 0.25304                     | 6.4323    | NMH=0.2038V, NML=0.1611V | 1.173                      | 22.498              | 6.4324                 |
| 16     | 14              | 7 nm             | 2                | 13             | 7 nm             | 1.86             | 349.913                             | 217.122                  | 3.06 × 10^-17         | 0.2538                      | 6.4279    | NMH=0.186V, NML=0.176V  | 1.207                      | 22.423              | 6.4279                 |
| 17     | 15              | 7 nm             | 2.14             | 13             | 7 nm             | 1.86             | 354.52                              | 223.85                   | 3.16 × 10^-17         | 0.2541                      | 6.4299    | NMH=0.178V, NML=0.185V  | 1.26                       | 22.382              | 6.4299                 |
| 18     | 15              | 7 nm             | 2.14             | 12             | 7 nm             | 1.71             | 341.52                              | 216.62                   | 3.04 × 10^-17         | 0.2537                      | 6.4273    | NMH=0.1854V, NML=0.1766V | 1.208                      | 22.406              | 6.4273                 |
| 19     | 14              | 7 nm             | 2                | 12             | 7 nm             | 1.71             | 339.1                               | 203.1                    | 2.95 × 10^-17         | 0.2542                      | 6.4326    | NMH=0.174V, NML=0.1771V  | 1.097                      | 22.492              | 6.4324                 |
| 20     | 12              | 7 nm             | 1.71             | 12             | 7 nm             | 1.71             | 340.62                              | 200.85                   | 2.92 × 10^-17         | 0.2542                      | 6.4298    | NMH=0.1724V, NML=0.178V  | 1.095                      | 22.496              | 6.4313                 |
| 21     | 13              | 7 nm             | 1.86             | 12             | 7 nm             | 1.71             | 342.21                              | 196.31                   | 2.89 × 10^-17         | 0.2541                      | 6.4268    | NMH=0.1714V, NML=0.179V  | 1.098                      | 22.512              | 6.4267                 |
| 22     | 12              | 7 nm             | 1.71             | 12             | 7 nm             | 1.71             | 339.22                              | 195.92                   | 2.86 × 10^-17         | 0.2542                      | 6.4299    | NMH=0.169V, NML=0.1793V  | 1.093                      | 22.498              | 6.4268                 |


### Design of BandGap Reference Circuit with Xschem. 

#### Design criteria taken into consideration 

```
Supply Voltage (DC)  - 1.75V
Reference Resistance - 50K ohms
Resistance near Ptat circuit - 33k ohms
temparature range - -45C to 125C
Output voltahe - 1.6V
Inaccuracy - 2%

```

#### Step-by-Step Design Overview

##### Schematic Creation

1. Define the Circuit Nodes: Key nodes were defined, including power supply (VDD), ground (GND), output reference voltage (Vref), and critical internal nodes.
2. Component Selection

NFets and Pfets : The ASAP 7nm process models for both PMOS (asap_7nm_pfet) and NMOS (asap_7nm_nfet) transistors. For the performance like BJTs, Nfets Drain and Gate were shorted. This was done for accurate results. 

3. Resistors: Standard resistors were placed to set biasing currents and establish gain for temperature compensation, the values were adjusted according to the PTAT and CTAT behaviour. R1 is 33k and R2 (Rref) is 50k

4. Power Supply Configuration : Configured a 1.8V power supply (VDD) as the main source for the circuit, ensuring all components received adequate voltage for proper operation.
Current Mirrors Implementation and implemented a current mirror using PMOS transistors to provide a stable current source for the circuit. This is critical for the accurate generation of both CTAT and PTAT voltages.
The current mirror consists of two PMOS transistors, with one configured as a reference and the other mirroring the current to the output stage.
5. CTAT and PTAT Voltage Generation

```CTAT Voltage:``` Created a pathway for generating a voltage that decreases with increasing temperature, resembling the PTAT behavior. This was achieved through the arrangement of NMOS transistors connected to a resistor that sets the appropriate voltage drop.
```PTAT Voltage:``` A separate branch of the circuit was designed to produce a voltage that is proportional to the absolute temperature. This was done using a different arrangement of NMOS transistors, ensuring that the output voltage changes linearly with temperature.

6. Output Voltage (Vref) Configuration
The final output voltage (Vref) was derived from the combination of CTAT and PTAT outputs. 

After completing the schematic,  the simulation parameters were set up within Xschem to run a DC analysis. The simulation was configured to sweep the temperature from -45°C to 125°C.


The below image is taken as a reference for designing a bandgap circuit using SBCM 

<picture>
<img alt = "Bandgap_reference" src="reference.png">
</picture>


#### Design using Xschem 

<picture>
<img alt = "Xschem" src="xschem.png">
</picture>


#### The Graphs that were plotted using Xschem 

<picture>
<img alt = "Generated Graphs" src="Graphs.png">
</picture>


##### Generated sPice File 

```
** sch_path: /home/madhuri/asap_7nm_Xschem/bandgap_ref.sch
**.subckt bandgap_ref
Xnfet1 net3 net1 net7 net10 asap_7nm_nfet l=7e-009 nfin=14
Xnfet2 net2 net1 net9 net11 asap_7nm_nfet l=7e-009 nfin=14
Xpfet1 net3 net2 VDD net12 asap_7nm_pfet l=7e-009 nfin=14
Xpfet2 net2 net2 VDD net13 asap_7nm_pfet l=7e-009 nfin=14
Xpfet3 Vref net2 VDD net14 asap_7nm_pfet l=7e-009 nfin=14
Xpfet4 net4 net2 VDD net15 asap_7nm_pfet l=7e-009 nfin=14
Xpfet5 net5 net2 net4 net16 asap_7nm_pfet l=7e-009 nfin=14
Xpfet6 net1 net2 net2 net17 asap_7nm_pfet l=7e-009 nfin=14
Xnfet3 net5 net5 net6 net18 asap_7nm_nfet l=7e-009 nfin=14
Xnfet4 net6 net6 GND net19 asap_7nm_nfet l=7e-009 nfin=14
Xnfet5 net7 net7 GND net20 asap_7nm_nfet l=7e-009 nfin=14
Xnfet6 net8 net8 GND GND asap_7nm_nfet l=7e-009 nfin=14
Xnfet7 net8 net8 GND GND asap_7nm_nfet l=7e-009 nfin=14
Xnfet8 net8 net8 GND GND asap_7nm_nfet l=7e-009 nfin=14
Xnfet9 net8 net8 GND GND asap_7nm_nfet l=7e-009 nfin=14
R1 net9 net8 33k ac=1k m=1
R2 Vref Vctat 50k ac=1k m=1
Xnfet10 Vctat Vctat GND net21 asap_7nm_nfet l=7e-009 nfin=14
V2 vdd GND 1.75
**** begin user architecture code


.dc temp -45 125 5
.control
run
plot v(Vref)
plot v(Vctat)
.endc


**** end user architecture code
**.ends
.GLOBAL GND
.GLOBAL VDD
**** begin user architecture code

.subckt asap_7nm_pfet S G D B l=7e-009 nfin=14
	npmos_finfet S G D B BSIMCMG_osdi_P l=7e-009 nfin=14
.ends asap_7nm_pfet

.model BSIMCMG_osdi_P BSIMCMG_va (
+ TYPE = 0

************************************************************
*                         general                          *
************************************************************
+version = 107             bulkmod = 1               igcmod  = 1               igbmod  = 0
+gidlmod = 1               iimod   = 0               geomod  = 1               rdsmod  = 0
+rgatemod= 0               rgeomod = 0               shmod   = 0               nqsmod  = 0
+coremod = 0               cgeomod = 0               capmod  = 0               tnom    = 25
+eot     = 1e-009          eotbox  = 1.4e-007        eotacc  = 3e-010          tfin    = 6.5e-009
+toxp    = 2.1e-009        nbody   = 1e+022          phig    = 4.9278          epsrox  = 3.9
+epsrsub = 11.9            easub   = 4.05            ni0sub  = 1.1e+016        bg0sub  = 1.17
+nc0sub  = 2.86e+025       nsd     = 2e+026          ngate   = 0               nseg    = 5
+l       = 2.1e-008        xl      = 1e-009          lint    = -2.5e-009       dlc     = 0
+dlbin   = 0               hfin    = 3.2e-008        deltaw  = 0               deltawcv= 0
+sdterm  = 0               epsrsp  = 3.9             nfin    = 1
+toxg    = 1.8e-009
************************************************************
*                            dc                            *
************************************************************
+cit     = 0               cdsc    = 0.003469        cdscd   = 0.001486        dvt0    = 0.05
+dvt1    = 0.36            phin    = 0.05            eta0    = 0.094           dsub    = 0.24
+k1rsce  = 0               lpe0    = 0               dvtshift= 0               qmfactor= 0
+etaqm   = 0.54            qm0     = 2.183e-012      pqm     = 0.66            u0      = 0.0237
+etamob  = 4               up      = 0               ua      = 1.133           eu      = 0.05
+ud      = 0.0105          ucs     = 0.2672          rdswmin = 0               rdsw    = 200
+wr      = 1               rswmin  = 0               rdwmin  = 0               rshs    = 0
+rshd    = 0               vsat    = 60000           deltavsat= 0.17            ksativ  = 1.592
+mexp    = 2.491           ptwg    = 25              pclm    = 0.01            pclmg   = 1
+pdibl1  = 800             pdibl2  = 0.005704        drout   = 4.97            pvag    = 200
+fpitch  = 2.7e-008        rth0    = 0.15            cth0    = 1.243e-006      wth0    = 2.6e-007
+lcdscd  = 0               lcdscdr = 0               lrdsw   = 1.3             lvsat   = 1441
************************************************************
*                         leakage                          *
************************************************************
+aigc    = 0.007           bigc    = 0.0015          cigc    = 1               dlcigs  = 5e-009
+dlcigd  = 5e-009          aigs    = 0.006           aigd    = 0.006           bigs    = 0.001944
+bigd    = 0.001944        cigs    = 1               cigd    = 1               poxedge = 1.152
+agidl   = 2e-012          agisl   = 2e-012          bgidl   = 1.5e+008        bgisl   = 1.5e+008
+egidl   = 1.142           egisl   = 1.142
************************************************************
*                            rf                            *
************************************************************
************************************************************
*                         junction                         *
************************************************************
************************************************************
*                       capacitance                        *
************************************************************
+cfs     = 0               cfd     = 0               cgso    = 1.6e-010        cgdo    = 1.6e-010
+cgsl    = 0               cgdl    = 0               ckappas = 0.6             ckappad = 0.6
+cgbo    = 0               cgbl    = 0
************************************************************
*                       temperature                        *
************************************************************
+tbgasub = 0.000473        tbgbsub = 636             kt1     = 0               kt1l    = 0
+ute     = -1.2            utl     = 0               ua1     = 0.001032        ud1     = 0
+ucste   = -0.004775       at      = 0.001           ptwgt   = 0.004           tmexp   = 0
+prt     = 0               tgidl   = -0.007          igt     = 2.5
************************************************************
*                          noise                           *
************************************************************
**)
.control
pre_osdi /home/madhuri/asap_7nm_Xschem/bsimcmg_arch64.osdi
.endc



.subckt asap_7nm_nfet S G D B l=7e-009 nfin=14
	nnmos_finfet S G D B BSIMCMG_osdi_N l=7e-009 nfin=14
.ends asap_7nm_nfet

.model BSIMCMG_osdi_N BSIMCMG_va (
+ TYPE = 1
************************************************************
*                         general                          *
************************************************************
+version = 107             bulkmod = 1               igcmod  = 1               igbmod  = 0
+gidlmod = 1               iimod   = 0               geomod  = 1               rdsmod  = 0
+rgatemod= 0               rgeomod = 0               shmod   = 0               nqsmod  = 0
+coremod = 0               cgeomod = 0               capmod  = 0               tnom    = 25
+eot     = 1e-009          eotbox  = 1.4e-007        eotacc  = 1e-010          tfin    = 6.5e-009
+toxp    = 2.1e-009        nbody   = 1e+022          phig    = 4.2466          epsrox  = 3.9
+epsrsub = 11.9            easub   = 4.05            ni0sub  = 1.1e+016        bg0sub  = 1.17
+nc0sub  = 2.86e+025       nsd     = 2e+026          ngate   = 0               nseg    = 5
+l       = 2.1e-008        xl      = 1e-009          lint    = -2e-009         dlc     = 0
+dlbin   = 0               hfin    = 3.2e-008        deltaw  = 0               deltawcv= 0
+sdterm  = 0               epsrsp  = 3.9             nfin    = 1
+toxg    = 1.80e-009
************************************************************
*                            dc                            *
************************************************************
+cit     = 0               cdsc    = 0.01            cdscd   = 0.01            dvt0    = 0.05
+dvt1    = 0.47            phin    = 0.05            eta0    = 0.07            dsub    = 0.35
+k1rsce  = 0               lpe0    = 0               dvtshift= 0               qmfactor= 2.5
+etaqm   = 0.54            qm0     = 0.001           pqm     = 0.66            u0      = 0.0303
+etamob  = 2               up      = 0               ua      = 0.55            eu      = 1.2
+ud      = 0               ucs     = 1               rdswmin = 0               rdsw    = 200
+wr      = 1               rswmin  = 0               rdwmin  = 0               rshs    = 0
+rshd    = 0               vsat    = 70000           deltavsat= 0.2             ksativ  = 2
+mexp    = 4               ptwg    = 30              pclm    = 0.05            pclmg   = 0
+pdibl1  = 0               pdibl2  = 0.002           drout   = 1               pvag    = 0
+fpitch  = 2.7e-008        rth0    = 0.225           cth0    = 1.243e-006      wth0    = 2.6e-007
+lcdscd  = 5e-005          lcdscdr = 5e-005          lrdsw   = 0.2             lvsat   = 0
************************************************************
*                         leakage                          *
************************************************************
+aigc    = 0.014           bigc    = 0.005           cigc    = 0.25            dlcigs  = 1e-009
+dlcigd  = 1e-009          aigs    = 0.0115          aigd    = 0.0115          bigs    = 0.00332
+bigd    = 0.00332         cigs    = 0.35            cigd    = 0.35            poxedge = 1.1
+agidl   = 1e-012          agisl   = 1e-012          bgidl   = 10000000        bgisl   = 10000000
+egidl   = 0.35            egisl   = 0.35
************************************************************
*                            rf                            *
************************************************************
************************************************************
*                         junction                         *
************************************************************
************************************************************
*                       capacitance                        *
************************************************************
+cfs     = 0               cfd     = 0               cgso    = 1.6e-010        cgdo    = 1.6e-010
+cgsl    = 0               cgdl    = 0               ckappas = 0.6             ckappad = 0.6
+cgbo    = 0               cgbl    = 0
************************************************************
*                       temperature                        *
************************************************************
+tbgasub = 0.000473        tbgbsub = 636             kt1     = 0               kt1l    = 0
+ute     = -0.7            utl     = 0               ua1     = 0.001032        ud1     = 0
+ucste   = -0.004775       at      = 0.001           ptwgt   = 0.004           tmexp   = 0
+prt     = 0               tgidl   = -0.007          igt     = 2.5
************************************************************
*                          noise                           *
************************************************************
**)
.control
pre_osdi /home/madhuri/asap_7nm_Xschem/bsimcmg_arch64.osdi
.endc


**** end user architecture code
.end

```


### ACKNOWLEDGEMENTS

I would like to thank Kunal Ghosh (Co-Founder of VSD), Dr Skandha Deepsita (Senior Physical Design Engineer) and Jossan Eden (Electronics Engineer) for guiding me throughout this research work on Bandgap reference circuit using ASAP 7nm PDK. 

- Kunal Ghosh [linkedin](https://www.linkedin.com/in/kunal-ghosh-vlsisystemdesign-com-28084836/) 
- Dr. Skandha Deepsita [Linkedin](https://www.linkedin.com/in/dr-skandha-deepsita-s-027433ba?utm_source=share&utm_campaign=share_via&utm_content=profile&utm_medium=ios_app)
- Jossan Eden [Linkedin](https://www.linkedin.com/in/jossan-eleazar-b-79809a25a/) 

### REFERENCES 

- Github [Bandgap Github](https://github.com/johnkustin/bandgapReferenceCircuit)
- Github [vsdopen2021_bgr](https://github.com/vsdip/vsdopen2021_bgr/tree/main)
- Github [7nm Reference Repo](https://github.com/AsahiroKenpachi/asap_7nm_Xschem)
- Github [sky130circuitdesign](https://github.com/VrushabhDamle/sky130CircuitDesignWorkshop?tab=readme-ov-file)
- 7nm Research paper [hal science research paper](https://hal.science/hal-01558775/document)
- Research paper [Layout and chractertization](https://mpedram.com/Papers/PowerDensity_7nm_FinFET-glsvsli15.pdf)
- Uc Berkeley research [Finfets](https://escholarship.org/content/qt2d46r42j/qt2d46r42j.pdf?t=q6z2kq)
- IIT labs [Finfet's research](https://vlsi-iitg.vlabs.ac.in/CMOS_theory.html#:~:text=Inverter%20Static%20Characteristics%20(VTC),logic%2Dlevels%20can%20be%20obtained)
- Michigan university classes [ Parameters claculation ](https://www.egr.msu.edu/classes/ece410/mason/files/Ch7.pdf)
- Cornell university ECE5745 [Capacitance and parameters](https://cornell-ece5745.github.io/ece5745-tut10-spice/)
- Ngspice tutorial [Tcl/Tk](https://ngspice.sourceforge.io/ngspice-control-language-tutorial.html)
- Finding Transconducatnce [Gm](https://www.tek.com/en/support/faqs/how-do-you-find-the-transconductance-of-a-mosfet)
- Ngspice Manual [Manual](https://ngspice.sourceforge.io/docs/ngspice-manual.pdf)
  









     

