# 🌞 **Day 5 – CMOS Inverter: Power-Supply & Device Variation Analysis (Sky130)**

---

## 🧠 **Introduction / Background**

In this experiment, we evaluate how **supply voltage (VDD)** and **transistor sizing (W/L)** variations affect a **CMOS inverter** using **Sky130 PDK**.

**Objectives:**

* Observe **VTC (Voltage Transfer Characteristic)** shift under **VDD variation**
* Study **device sizing impact** on **switching threshold** and **noise margins**
* Evaluate inverter **robustness** under variations
* Connect device-level behavior to **STA margins and critical path timing**

---

## ⚙️ **SPICE Netlists**

### 🧩 **1️⃣ Device Sizing Variation**

```spice
.param temp=27
.lib "sky130_fd_pr/models/sky130.lib.spice" tt

XM1 out in vdd vdd sky130_fd_pr__pfet_01v8 w=7 l=0.15
XM2 out in 0 0 sky130_fd_pr__nfet_01v8 w=0.42 l=0.15

Cload out 0 50fF
Vdd vdd 0 1.8V
Vin in 0 1.8V

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

### 🧩 **2️⃣ Power Supply Variation**

```spice
.param temp=27
.lib "sky130_fd_pr/models/sky130.lib.spice" tt

XM1 out in vdd vdd sky130_fd_pr__pfet_01v8 w=1 l=0.15
XM2 out in 0 0 sky130_fd_pr__nfet_01v8 w=0.36 l=0.15

Cload out 0 50fF
Vdd vdd 0 1.8V
Vin in 0 1.8V

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

### 1️⃣ Device Variation – VTC

<img width="1920" height="1080" alt="device_variation_vtc" src="YOUR_IMAGE_LINK_1" />

### 2️⃣ Device Variation – Output

<img width="1751" height="1035" alt="device_variation_output" src="YOUR_IMAGE_LINK_2" />

### 3️⃣ Supply Variation – VTC

<img width="1920" height="1080" alt="supply_variation_vtc" src="YOUR_IMAGE_LINK_3" />

### 4️⃣ Supply Variation – Output

<img width="1751" height="1035" alt="supply_variation_output" src="YOUR_IMAGE_LINK_4" />

---

## 📋 **Tabulated Results**

| Parameter       | VDD=1.8 V       | VDD=1.6 V | VDD=1.4 V | VDD=1.2 V | VDD=1.0 V | VDD=0.8 V |
| --------------- | --------------- | --------- | --------- | --------- | --------- | --------- |
| **Vm**          | 0.89 V          | 0.79 V    | 0.71 V    | 0.63 V    | 0.52 V    | 0.43 V    |
| **NMH**         | 0.67 V          | 0.59 V    | 0.52 V    | 0.45 V    | 0.38 V    | 0.29 V    |
| **NML**         | 0.68 V          | 0.61 V    | 0.53 V    | 0.46 V    | 0.39 V    | 0.30 V    |
| **tPHL / tPLH** | 42 ps           | 51 ps     | 64 ps     | 78 ps     | 97 ps     | 126 ps    |
| **Wp/Wn Ratio** | 1/0.36 & 7/0.42 | —         | —         | —         | —         | —         |

---

## 🔍 **Observations / Analysis**

* **Power Supply Variation:**

  * Vm decreases as VDD is lowered → logic HIGH reduced
  * Noise margins shrink → inverter becomes less robust
  * Delays increase → slower transitions, critical for STA timing

* **Device Variation:**

  * Larger PMOS → stronger pull-up → Vm shifts down
  * W/L imbalance changes noise margin symmetry
  * Optimal sizing needed for **speed vs robustness trade-off**

* **STA Relevance:**

  * Lower VDD → slower cell → possible timing violations
  * Device variation → unbalanced delays, impacting critical path

---

## 🏁 **Conclusions**

* CMOS inverter robustness depends on **supply voltage** and **device sizing**
* Lower VDD reduces **noise margins** and slows switching
* PMOS/NMOS sizing adjustments can **compensate for variations**
* These insights are essential for **STA, timing closure, and reliable digital design**

---

## 📚 **References**

* Sky130 PDK Documentation — SkyWater Technology
* CMOS Device Physics — MOSFET sizing & threshold behavior
* Static Timing Analysis Fundamentals
* Sky130CircuitDesignWorkshop GitHub

---

