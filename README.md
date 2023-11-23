
<a name="readme-top"></a>

<!-- PROJECT LOGO -->
<br />
<div align="center">

  <h3 align="center">90nm PMOS input OTA</h3>
  <p align="center">
    Schematic and Layout design using Cadence Virtuoso
    <br />
    <a href="https://github.com/Devashrutha/PMOS-Input-OTA/tree/main"><strong>Explore the docs »</strong></a>
    <br />
    <br />
  </p>
   <img src = "/Pictures/Layout/PMOS_Input_OTA_Lyt_Logo.png">
</div>



<!-- TABLE OF CONTENTS -->
<details>
  <summary>INDEX</summary>
  <ol>
    <li>
      <a href="#introduction">Introduction</a>
    </li>
    <li>
      <a href="#schematic-design">Schematic Design</a>
    </li>
    <li><a href="#adjusted-width-and-length-ratios">Adjusted Width and Length Ratios</a></li>
    <li><a href="#layout-design">Layout Design</a></li>
    <li><a href="#simulation-results">Simulation Results</a></li>
  </ol>
</details>



<!-- ABOUT THE PROJECT -->
## Introduction

In the past, analogue filters were designed using OPAMPs whose bandwidth (BW) was limited by the gain-
bandwidth product which limited the ability to get the ideal filter response. This technology process also
consumed more power. Therefore, OTAs are used in place of OPAMPs. OTA’s are also used as Low Noise
Amplifiers (LNA) which have high gain and less noise (good Singal to Noise Ratio (SNR)). Therefore, OTA’s
play an integral role in the analogue front end of low-power systems.

An OTA is a Differential Voltage Controlled Current Source (DVCCS) which converts the differential input
voltage to an output current. 
<p align="right">(<a href="#readme-top">back to top</a>)</p>



<!-- GETTING STARTED -->
## Schematic Design

An Integrated Circuit (IC) contains analogue and digital cells which operate on the same supply voltage range.
To make the analogue circuits work in this range it is operated in the subthreshold region or weak inversion
region.
* The input stage of the schematic is similar to a PMOS differential pair circuit consisting of a PMOS
current mirror which sets the Ibias (MP5 and MP6) and 2 PMOS differential inputs V- and V+ (MP1 and
MP2) with an amplification stage formed by MP4 and MN4.
* The V+ and V- are converted into a differential current signal I1 and I2.
* MOSFET’s M3 and M4 are active load transistors for the differential current signals I1 and I2 which
results in lower operating current, resulting in lower gm and increased linearity.
* Therefore, the gain of the OTA is given by:

  ```math
  A_v=\frac{\beta.gm_{M1}}{gds_{M8}+gds_{M10}}
  ```
* Where gds is the conductance between the drain and source. From the above equation, we can see
that the high gain requirement leads to a multistage design with long channel MOSFETs biased at low
current levels this degrades the frequency of operation but can be easily avoided using partial
feedback between MP1 and MP2 (not done in this design) to enhance the output impedance of the
amplifier.
* As shown in Figure 2, MP1 and MP2 operate in the subthreshold voltage region, the differential
current output is given as:

```math
\begin{gathered}
I_{\text {out }}=\beta\left(I_{D, M2}-I_{D, M1}\right)= \beta\left(I_2-I_1\right)=\beta I_{B I A S}\left(\frac{\frac{I_2}{I_1}-1}{\frac{I_2}{I_1}+1}\right) \\
\text { OR } \\
I_{\text {out }}=g_m V_{\text {in }} \\
\text { Where } \frac{I_2}{I_1}=e^{\frac{-V_{\text {in }}}{n V_T}} \text { and } V_{\text {in }}=(V+)-(V-)
\end{gathered}
```
* Based on the above equations we can define the transconductance of the OTA as:

```math
g_m=\frac{\partial I_{\text {out }}}{\partial V_{\text {in }}}=\frac{\beta I_{\text {BIAS }}}{2 n V_T}
```
* We can conclude that the transconductance (𝒈𝒎) of the OTA is controlled by the setup current 𝑰𝑩𝑰𝑨𝑺
which means the output current 𝑰𝒐𝒖𝒕 is proportional to the input voltage 𝑽𝒊𝒏 .
* M5 and M6 make up the Ibias circuit (current mirror) having the same (W/L) ratio. Thus, the current
set at M5 is mirrored and outputted at the drain of M6. They have the largest (W/L) ratio as the
mobility of PMOS is lesser than that of NMOS and a sufficient amount of current needs to flow
through for the entire circuit to operate.
* The differential input PMOS (M1 and M2) have twice the width of the NMOS active loads (M3
and M4) as the mobility of charge carriers of PMOS is lower than that of NMOS charge
carriers. PMOS is preferred as it has better noise performance.
* The number of fingers for each of the MOSFETs is discussed later in the layout section.
* We can divide the OTA schematic into 3 main sections:
1. Ibias current mirror circuit (M5 and M6).
2. Differential input stage with NMOS active loads (M1 and M2, M3 and M4).
3. NMOS Common Source Amplification Stage (M10 (Amplifying MOS), M8 and M7(PMOS
current mirror active load)).

<div align ="center">
  <img src = "/Pictures/Schematic/PMOS_Input_OTA_Sch.png">
</div>

We use the following test bench to simulate the OTA to validate the AC gain and phase.

<div align ="center">
  <img src = "/Pictures/Schematic/PMOS_Input_OTA_Tb.png">
</div>

<!-- Adjusted (W/L) Ratios -->
## Adjusted Width and Length Ratios


The final widths and lengths for the MOSFETs are:


| MOSFET       | WIDTH (W)      | LENGTH (L)    | (W/L) RATIO  |
|    :---:     |     :---:      |     :---:     |   :---:      |
| `M1 & M2`    | 15u            | 1.05u         | `14.28`      |
| `M3 & M4`    | 7.5u           | 1.05u         | `7.14`       |
| `M5 & M6`    | 20u            | 1.05u         | `19.05`      |
| `M7 & M8`    | 15u            | 1.05u         | `14.28`      |
| `M9 & M10`   | 7.5u           | 1.05u         | `7.14`       |

Further improvements can be made to increase the gain, PM of the OTA by simulating and re-adjusting the ratios.
<p align="right">(<a href="#readme-top">back to top</a>)</p>


<!-- Layout Design -->
## Layout Design

The layout design below shows the PMOS input OTA. 

<div align="center">
  <img src= "/Pictures/Layout/PMOS_Input_OTA_Lyt.png">
</div>

* The `PMOS current mirrors`, `PMOS active load`, and the `guard ring` are placed at an `equal distance` from each other horizontally to avoid `Shallow Trench Isolation` effects. This is done as the number of fingers is odd and this configuration does not result in the source terminals coming on either end of the PMOS devices. 
* The floorplan is based on `Constructive Placement` following the `min-cut` algorithm. Where the devices are placed such that there are minimum connection edges across the boundaries.
* All the metal layers are modified to have equal width and T structures are avoided wherever possible. This reduces the chances of electromigration effects (Hillocks or Voids).
* M1 in `Blue`, M2 in `Red`, M3 in `Light Green`, Poly in `Dark Green`
* The critical device being the differential pair is placed such that it results in minimum trace length between other devices and itself.
* The PMOS devices are placed in their `nwells`.
* Since the differential input stage `does not share the same body voltage` it is placed in its own nwell.
* The `guard rings` are basically `large taps` that help to isolate devices from each other creating a low resistance well. It `prevents any charge buildup` and `noise` by other devices from affecting the operation of the guarded group.
* The total area occupied by this 11 MOSFET OPAMP is `0.33nm^2`.
  
<p align="right">(<a href="#readme-top">back to top</a>)</p>


<!-- Simulation Results -->
## Simulation Results

The following results are the obtained values after post-layout simulation.

* Achieved Gain     : `41.136dB` at 1V.
* Achieved GBP                : `4.56MHz` at 1V.
* Achieved PM               : `77 degrees` at 1V.
* Power consumption : `14.40uW` at 1V.

`Clubbed simulation output for AC Gain and Phase(Schematic and AV Extracted)`:
<div align="center">
  <img src= "/Pictures/Results/Gain_Phase_Combined_AV_Sch.png">

</div>

`DRC check`:
<div align="center">
    <img src= "/Pictures/Results/DRC.png">

</div>

`LVS check`:
<div align="center">
    <img src= "/Pictures/Results/LVS.png">

</div>

`PWR Consumption`:
<div align="center">
    <img src= "/Pictures/Results/PWR.png">

</div>

<p align="right">(<a href="#readme-top">back to top</a>)</p>

