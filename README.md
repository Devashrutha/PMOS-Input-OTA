
<a name="readme-top"></a>

<!-- PROJECT LOGO -->
<br />
<div align="center">

  <h3 align="center">90nm PMOS input OTA</h3>
  <p align="center">
    Schematic and Layout design using Cadence Virtuoso at 90nm technology
    <br />
    <a href="https://github.com/Devashrutha/PMOS-Input-OTA/tree/main"><strong>Explore the docs »</strong></a>
    <br />
    <br />
  </p>
   <img src = "/Pictures/Layout/pmos_ota_lay_logo.png">
</div>



<!-- TABLE OF CONTENTS -->
<details>
  <summary>INDEX</summary>
  <ol>
    <li>
      <a href="#introduction">Introduction</a>
      <ul>
        <li><a href="#specifications">Specifications</a></li>
      </ul>
    </li>
    <li>
      <a href="#getting-started">Getting Started</a>
    </li>
    <li><a href="#adjusted-width-and-length-ratios">Adjusted Width and Length Ratios</a></li>
    <li><a href="#layout-design">Layout Design</a></li>
    <li><a href="#simulation-results">Simulation Results</a></li>
  </ol>
</details>



<!-- ABOUT THE PROJECT -->
## Introduction

The most popular approach for CMOS OPAMPs has been the two-stage architecture. It consists of a differential input stage followed by a second gain stage. In some cases where the output is a resistive load, a buffer is used. OPAMPs usually have low output impedance as they are used as amplifiers so that maximum power is transferred to the load. This design includes an NMOS input differential amplifier with an NMOS current mirror and PMOS active load followed by a common source amplifier. There are several advantages to choosing NMOS input MOSFETs instead of PMOS. 

Here's why:
* NMOS input OPAMPS typically have low input impedance, which means they are less prone to input capacitance problems.
* They offer high-speed operation for use in DSP circuits.
* They consume less power than their counterpart making them more power-efficient.
* Better noise performance.

Of course, the choice between NMOS input and PMOS input depends on the application and requirements.


<p align="right">(<a href="#readme-top">back to top</a>)</p>



### Specifications

This section gives the required specifications that the final OPAMP design must satisfy:

* DC gain : `47dB`
* Gain Bandwidth Product (GBP) : `30MHz`
* Phase Margin (PM) : `> 60 degrees`
* Slew Rate : `20V/us`
* VDD : `1.8V`
* Input Common Mode Randge High (ICMR+) : `1.6V`
* Input Common Mode Randge Low (ICMR-) : `0.8V`
* Load Capacitance (CL) : `2pF`
* Power Consumption <= `200uW`
* Coupling Capacitor (Cc) >= `0.22*CL`



<p align="right">(<a href="#readme-top">back to top</a>)</p>



<!-- GETTING STARTED -->
## Getting Started

Using Cadence Virtuoso with gpdk90 technology we shall first use the above specifications to find the (W/L) ratios for the various MOSFETs. All the manual calculations are included in the project directory.

<div align ="center">
  <img src = "/Pictures/Schematic/unCox.png">
</div>





<!-- Adjusted (W/L) Ratios -->
## Adjusted Width and Length Ratios


The final widths and lengths for the MOSFETs are:


| MOSFET       | WIDTH (W)      | LENGTH (L)    | (W/L) RATIO  |
|    :---:     |     :---:      |     :---:     |   :---:      |
| `M1 & M2`    | 720n           | 240n          | `3`          |
| `M3 & M4`    | 2.16u          | 240n          | `9`          |
| `M5 & M8`    | 2.4u           | 800n          | `3`          |
| `M6`         | 15.35u         | 240n          | `64`         |
| `M7`         | 9.6u           | 800n          | `12`         |

Further improvements can be made to increase the gain, GBP, and PM of the OPAMP by simulating and re-adjusting the ratios.
<p align="right">(<a href="#readme-top">back to top</a>)</p>


<!-- Layout Design -->
## Layout Design

The layout design below shows the PMOS input OTA. 

<div align="center">
  <img src= "/Pictures/Layout/nmos_opamp_lay.png">
</div>


* The floorplan is based on `Constructive Placement` following the `min-cut` algorithm. Where the devices are placed such that there are minimum connection edges across the boundaries.
* All the metal layers are modified to have equal width and T structures are avoided wherever possible. This reduces the chances of electromigration effects (Hillocks or Voids).
* M1 in `Blue`, M2 in `Red`, M3 in `Light Green`, Poly in `Dark Green`
* The critical device being the differential pair is placed such that it results in minimum trace length between other devices and itself.
* The PMOS devices are placed in their `nwells` since the PMOSCAP does not share the same body voltage as the other PMOSs it resides in its nwell.
* A `PMOSCAP` was used instead of a `mimcap` to save space. We know that the gate capacitance of a MOSFET can mimic a capacitor when all the other terminals (D, S, B) are shorted.
* The `guard rings` are basically `large taps` that help to isolate devices from each other creating a low resistance well. It `prevents any charge buildup` and `noise` by other devices from affecting the operation of the guarded group.
* The pins `Ibias`, `V+`, `V-`, and `Vout` are brought up to M3 while the VSS and VDD pins are left at M1. This really depends on the overall design in which this OPAMP might sit.
* The total area occupied by this 11 MOSFET OPAMP is `170pm^2`.
  
<p align="right">(<a href="#readme-top">back to top</a>)</p>


<!-- Simulation Results -->
## Simulation Results

The following results are the obtained values after post-layout simulation.

* Achieved Gain     : `46.42dB` and `47.54dB` at 1.6v and 0.8v respectively.
* Achieved GBP                : `27.528MHz` and `27.187MHz` at 1.6v and 0.8v respectively.
* Achieved PM               : `65.9 degrees` and `65.9 degrees` at 1.6v and 0.8v respectively.
* Power consumption : `197.394uW` and `194.372uW` at 1.6v and 0.8v respectively.

`Clubbed simulation output for AC Gain and Phase`:
<div align="center">
  <img src= "/Pictures/Results/Clubbed Output (LVS AV_ext).png">

</div>

`DRC check`:
<div align="center">
    <img src= "/Pictures/Results/DRC.png">

</div>

`LVS check`:
<div align="center">
    <img src= "/Pictures/Results/LVS.png">

</div>

`PWR consumption at 1.6V`:
<div align="center">
    <img src= "/Pictures/Results/PWR_1.6.png">

</div>

`PWR consumption at 0.8V`:
<div align="center">
    <img src= "/Pictures/Results/PWR_0.8.png">
</div>

<p align="right">(<a href="#readme-top">back to top</a>)</p>

