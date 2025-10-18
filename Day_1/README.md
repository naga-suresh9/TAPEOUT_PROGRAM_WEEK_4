# Day_1
# Sky130 MOSFET Characterization: NFET Id-Vds Analysis

## Introduction / Background

This experiment characterizes an NFET MOSFET using the Sky130 PDK models.  
The main objective is to analyze **Id vs Vds** characteristics at different Vgs voltages.  
These characteristics are critical for understanding device operation regions (linear and saturation) and for extracting key MOSFET parameters like threshold voltage (Vth) and saturation current.

---

## SPICE Netlist

```spice
*Model Description
.param temp=27

*Include Sky130 PDK library
.lib "sky130_fd_pr/models/sky130.lib.spice" tt

*Netlist
XM1 Vdd n1 0 0 sky130_fd_pr__nfet_01v8 w=5 l=2
R1 n1 in 55
Vdd vdd 0 1.8V
Vin in 0 1.8V

*Simulation commands
.op
.dc Vdd 0 1.8 0.1 Vin 0 1.8 0.2

.control
run
display
setplot dc1
.endc

.end
````

---

## Plots & Figures

* **Id vs Vds characteristics** for multiple Vgs (see attached screenshot).
* Annotated regions:

  * **Linear (ohmic) region:** Id increases linearly with Vds.
  * **Saturation region:** Id becomes constant beyond Vds ≈ Vgs - Vth.
* Threshold voltage and saturation onset marked on the plot.

> ![Id-Vds Plot](27319.jpg)

---

## Tabulated Results

| Parameter                 | Value           |
| ------------------------- | --------------- |
| Extracted Vth (Threshold) | [your value] V  |
| Id_sat (typical)          | [your value] µA |
| Vds at saturation onset   | [your value] V  |

> Replace `[your value]` with numbers obtained from your simulation.

---

## Observations / Analysis

* At **low Vds**, the device operates in the **linear region**, and Id increases linearly with Vds.
* At **higher Vds**, the device enters **saturation**, and Id remains almost constant.
* Increasing **Vgs** increases Id due to stronger channel formation and enhanced carrier mobility.
* The point where Vds ≈ Vgs - Vth marks the **onset of saturation**.
* These results are important for **Static Timing Analysis (STA)**: variations in Vth and Id impact circuit switching speed, output drive, and delay.

---

## Conclusions

* NFET Id-Vds characteristics help determine MOSFET behavior in circuits.
* Accurate threshold voltage extraction is essential for timing and noise margin analysis.
* These parameters feed into post-synthesis STA and design optimization.

---

## References

* Sky130 PDK models and documentation
* MOSFET theory and SPICE simulation textbooks
* Example GitHub repositories:

  * CMOS-Circuit-Design-and-SPICE-Simulation-using-SKY130-Technology
  * Inverter-design-and-analysis-using-sky130pdk
  * sky130CircuitDesignWorkshop

```

---

I can also make a **fully ready folder structure README** including:

- `schematic/`  
- `plots/`  
- `netlist/`  

so it’s **GitHub-ready with all files organized**.  

Do you want me to do that next?
```
