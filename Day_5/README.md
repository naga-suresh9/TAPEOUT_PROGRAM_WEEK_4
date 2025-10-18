# 🌞 **Day 5 – CMOS Inverter: Power-Supply & Device Variation Analysis (Sky130)**

---

## 🧠 **Introduction / Background**

In this experiment, we analyze how **supply voltage (Vdd)** and **transistor sizing (W/L)** variations affect a **CMOS inverter**’s behavior using **Sky130 PDK**.

Goals:

* Study **VTC shift under supply voltage variation**
* Observe effects of **device sizing changes**
* Evaluate inverter **robustness** under variations
* Connect device-level variation to **STA margins and critical path timing**

---

## ⚙️ **SPICE Netlists**

### 🧩 **1️⃣ Device Variation Example**

```spice
*******************************************************
* 📁 File: inverter_device_variation.spice
* 📗 Purpose: CMOS inverter VTC with W/L variations
* 📚 PDK: sky130_fd_pr (1.8 V devices)
*******************************************************

* === Model Description ===
.param temp=27

* === Include Sky130 model files ===
.lib "sky130_fd_pr/models/sky130.lib.spice" tt

* === Netlist Description ===
XM1 out in vdd vdd sky130_fd_pr__pfet_01v8 w=7 l=0.15
XM2 out in 0 0 sky130_fd_pr__nfet_01v8 w=0.42 l=0.15

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

---

### 🧩 **2️⃣ Power-Supply Variation Sweep**

```spice
*******************************************************
* 📁 File: inverter_supply_variation.spice
* 📗 Purpose: CMOS inverter VTC under Vdd variation
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
.control
let powersupply = 1.8
alter Vdd = powersupply
let voltagesupplyvariation = 0
dowhile voltagesupplyvariation < 6
    dc Vin 0 1.8 0.01
    let powersupply = powersupply - 0.2
    alter Vdd = powersupply
    let voltagesupplyvariation = voltagesupplyvariation + 1
end

plot dc1.out vs in dc2.out vs in dc3.out vs in dc4.out vs in dc5.out vs in dc6.out vs in xlabel "Input Voltage (V)" ylabel "Output Voltage (V)" title "Inverter DC characteristics vs Supply Voltage"
.endc

.end
```

---

## 📊 **Plots & Figures**

* **VTC curves** under varying **Vdd** → observe how **Vm** shifts
* Compare **different W/L ratios** → see changes in **switching threshold** and noise margins
* Annotate:

  * Vm shift for each Vdd
  * Output HIGH/LOW changes
  * Impact on NML & NMH

<img width="1920" height="1080" alt="day_05_inv_supplyvariation_Wp1_Wn036" src="https://github.com/user-attachments/assets/d1022625-9c5a-4034-a2e2-707a06c429f4" />
<img width="1751" height="1035" alt="supplyvariation_output" src="https://github.com/user-attachments/assets/8b12b42f-a002-4af1-ac33-b1a5db975d8a" />

<img width="1920" height="1080" alt="day5_pfet_idvgs_" src="https://github.com/user-attachments/assets/4887abc9-0d3b-45cf-96b9-ab20b4a7b5ed" />
<img width="1751" height="1035" alt="pfet_output" src="https://github.com/user-attachments/assets/7a6add77-27c2-45ba-be95-5ed82e6f5e6e" />

---

## 📋 **Tabulated Results**

| Parameter | Description         | Values (Vdd variation)      |
| --------- | ------------------- | --------------------------- |
| **Vm**    | Switching threshold | [Vdd=1.8V], [Vdd=1.6V], ... |
| **VOH**   | Output HIGH         | [values]                    |
| **VOL**   | Output LOW          | [values]                    |
| **NML**   | Noise margin low    | [values]                    |
| **NMH**   | Noise margin high   | [values]                    |

*(Replace `[values]` after simulation.)*

---

## 🔍 **Observations / Analysis**

* Decreasing **Vdd** shifts **Vm lower** → logic “1” reduced
* Noise margins **decrease** with lower supply → less robust inverter
* Larger PMOS/NMOS widths → improved drive, better noise tolerance
* Highlights **STA-critical paths sensitivity** to supply and device variation

---

## 🏁 **Conclusions**

* **Supply voltage variation** has a direct impact on **inverter switching point and robustness**.
* **Transistor sizing** can be tuned to compensate for variation and improve **noise margins**.
* These insights are essential for **robust timing analysis and STA verification** in modern digital circuits.

---

## 📚 **References**

* Sky130 PDK Documentation
* CMOS Circuit Design & SPICE textbooks
* GitHub: [Sky130CircuitDesignWorkshop](https://github.com/kunalg123/sky130CircuitDesignWorkshop)

---

If you want, I can now **combine Days 1 → 5 into a single GitHub-style repo**, with:

* `netlists/` (all SPICE files)
* `README.md` (Day-wise sections with emojis, plots placeholders, and tables)

…ready for upload exactly like your Day 2 repo.

Do you want me to do that?
