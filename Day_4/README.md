# 🌞 **Day 4 – CMOS Inverter Noise Margin / Robustness Analysis (Sky130)**

---

## 🧠 **Introduction / Background**

In this experiment, we evaluate **noise margins (NML, NMH)** of a **CMOS inverter** using the **Sky130 PDK**.

Goals:

* Determine the inverter **logic levels**: VIL, VIH, VOL, VOH
* Compute **Noise Margins**:

  * NML = VIL − VOL
  * NMH = VOH − VIH
* Analyze **circuit robustness** under input variations
* Understand the relationship to **STA timing and reliability**

---

## ⚙️ **SPICE Netlist**

```spice
*******************************************************
* 📁 File: inverter_noise_margin_day4.spice
* 📗 Purpose: CMOS Inverter Noise Margin / Robustness Analysis
* 📚 PDK: sky130_fd_pr (1.8 V devices)
*******************************************************

* === Model Description ===
.param temp=27

* === Include Sky130 model files ===
.lib "sky130_fd_pr/models/sky130.lib.spice" tt

* === Netlist Description ===
XM1 out in vdd vdd sky130_fd_pr__pfet_01v8 w=1 l=0.15
XM2 out in 0 0 sky130_fd_pr__nfet_01v8 w=0.36 l=0.15

Cload out 0 50fF

Vdd vdd 0 1.8V
Vin in 0 1.8V

* === Simulation Commands ===
.op
.dc Vin 0 1.8 0.01

.control
run
setplot dc1
display
.endc

.end
```

> 🔹 This DC sweep generates the **VTC (Vout vs Vin)** needed for noise margin extraction.

---

## 📊 **Plots & Figures**

* **Vout vs Vin** (VTC curve)
* Identify the points:

  * **VIL** → Maximum input considered logic LOW (slope = −1 lower region)
  * **VIH** → Minimum input considered logic HIGH (slope = −1 upper region)
  * **VOL** → Output LOW at Vin = VIL
  * **VOH** → Output HIGH at Vin = VIH

<img width="1920" height="1080" alt="day_4" src="https://github.com/user-attachments/assets/b104adec-3f28-4a01-a57b-c59f73499368" />
<img width="1751" height="1035" alt="noisemargin" src="https://github.com/user-attachments/assets/cc48faf1-7780-442d-847a-d4559d93d19f" />




---

## 📋 **Tabulated Results**

| Parameter | Description        | Value          |
| --------- | ------------------ | -------------- |
| **VIL**   | Max input LOW      | [your value] V |
| **VIH**   | Min input HIGH     | [your value] V |
| **VOL**   | Output LOW at VIL  | [your value] V |
| **VOH**   | Output HIGH at VIH | [your value] V |
| **NML**   | Noise Margin Low   | [your value] V |
| **NMH**   | Noise Margin High  | [your value] V |

*(Replace `[your value]` with simulation results.)*

---

## 🔍 **Observations / Analysis**

* NML and NMH quantify **logic robustness** against input noise.
* Larger noise margins → **more tolerant inverter**, less prone to misinterpretation.
* **Transistor sizing (Wp/Wn)** affects Vm → shifts VIH/VIL → changes noise margins.
* Useful for **STA and design verification**, ensuring reliable operation under variations.

---

## 🏁 **Conclusions**

* Noise margins are key to **digital inverter reliability**.
* Proper VTC analysis ensures **robust logic levels** and **stable operation**.
* W/L ratio adjustments can improve **robustness** and help meet **STA timing and noise specifications**.

---

## 📚 **References**

* Sky130 PDK Documentation
* CMOS & MOSFET textbooks
* GitHub: [Sky130CircuitDesignWorkshop](https://github.com/kunalg123/sky130CircuitDesignWorkshop)

---

Do you want me to do that?
