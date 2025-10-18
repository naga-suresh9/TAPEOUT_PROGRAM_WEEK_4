# 🌞 **Day 2 — Sky130 NMOS Device Characterization: Id–Vds & Id–Vgs Analysis** ⚡

---

## 🧠 **Introduction / Background**

In this experiment, we analyze the electrical behavior of the **NMOS transistor** using **Sky130 1.8 V models**.
The goal is to explore how the **drain current (Id)** responds to changes in both **drain-source voltage (Vds)** and **gate-source voltage (Vgs)**.

This helps us to:

* Identify **linear** and **saturation** regions in operation.
* Estimate the **threshold voltage (Vth)** from the transfer curve.
* Understand how transistor-level parameters affect **timing, delay**, and **switching speed** in CMOS logic circuits.

---

## ⚙️ **SPICE Netlists**

### 🧩 **1️⃣ Id–Vds Characteristics Simulation**

```spice
*******************************************************
* 📁 File: nfet_idvds.spice
* 📗 Purpose: NMOS Id–Vds characteristics simulation
* 📚 PDK: Sky130_fd_pr (1.8 V NMOS)
*******************************************************

* === Device Parameters ===
.param temp = 27

* === Include Sky130 model ===
.lib "sky130_fd_pr/models/sky130.lib.spice" tt

* === Circuit Setup ===
XM1 vdd n1 0 0 sky130_fd_pr__nfet_01v8 w=0.39 l=0.15
R1 n1 in 55
Vdd vdd 0 1.8V
Vin in 0 1.8V

* === Simulation Commands ===
.op
.dc Vdd 0 1.8 0.1 Vin 0 1.8 0.2

.control
run
display
setplot dc1
.endc

.end
```

---

### 🧩 **2️⃣ Id–Vgs Characteristics (Threshold Extraction)**

```spice
*******************************************************
* 📁 File: nfet_idvgs.spice
* 📗 Purpose: NMOS Id–Vgs characteristics and Vth extraction
* 📚 PDK: Sky130_fd_pr (1.8 V NMOS)
*******************************************************

* === Device Parameters ===
.param temp = 27

* === Include Sky130 model ===
.lib "sky130_fd_pr/models/sky130.lib.spice" tt

* === Circuit Setup ===
XM1 vdd n1 0 0 sky130_fd_pr__nfet_01v8 w=0.39 l=0.15
R1 n1 in 55
Vdd vdd 0 1.8V
Vin in 0 1.8V

* === Simulation Commands ===
.op
.dc Vin 0 1.8 0.1

.control
run
display
setplot dc1
.endc

.end
```

---

## 📊 **Plots & Figures**

* **Id–Vds** curves for multiple **Vgs** values show two clear regions:

  * **Linear Region:** Current rises almost linearly with Vds.
  * **Saturation Region:** Current becomes constant after Vds ≈ Vgs − Vth.

<img width="1920" height="1080" alt="day2_nfet_idvds_L015_W039 spice" src="https://github.com/user-attachments/assets/1fdca394-a7c6-43a2-9d66-11d8c6621c10" />
<img width="1681" height="995" alt="day2_nfet_idvds_output" src="https://github.com/user-attachments/assets/fa15968b-11e0-4b1e-aab8-92b82cff9c05" />
* **Id–Vgs** curve helps in **Vth extraction** using the slope–intercept method.
<img width="1920" height="1080" alt="day2_nfet_idvgs_L015_W039 spice" src="https://github.com/user-attachments/assets/7622cc24-ecdf-47d6-b2e5-3cdec80eb6ec" />
<img width="1681" height="995" alt="day2_nfet_idvgs_output" src="https://github.com/user-attachments/assets/b8937eb3-33e5-473b-8092-1a9119ab7ef5" />




---

## 📐 **Tabulated Results**

| ⚙️ Parameter | 🧩 Description                   | 📏 Value        |
| ------------ | -------------------------------- | --------------- |
| **Vth**      | Threshold voltage (from Id–Vgs)  | [your value] V  |
| **Id_sat**   | Saturation current (Vgs = 1.8 V) | [your value] µA |
| **Vds(sat)** | Saturation onset (≈ Vgs − Vth)   | [your value] V  |

*(Insert your simulation data here!)*

---

## 🔍 **Observations / Analysis**

✅ When **Vds** is small → NMOS operates in **linear region**, and Id ∝ Vds.
✅ As **Vds** approaches (Vgs − Vth) → channel pinches off → **saturation region** begins.
✅ Increasing **Vgs** enhances channel charge → higher Id.
✅ **Vth** determines when transistor begins strong conduction.
✅ These behaviors directly influence **logic threshold** and **timing delay** in digital circuits.

---

## 🧩 **Connection to STA Concepts**

📈 In **Static Timing Analysis (STA)**:

* **Threshold variation (Vth)** → causes delay uncertainty.
* **Drive current (Id_sat)** → affects signal transition time.
* **Device geometry** and **supply voltage** → change switching speed & critical path delay.

---

## 🏁 **Conclusions**

⭐ The experiment clearly demonstrates NMOS transistor behavior under different biasing conditions.
⭐ **Id–Vds** curves reveal how the device transitions from linear to saturation.
⭐ **Vth extraction** helps estimate device switching properties.
⭐ These transistor-level insights form the foundation for analyzing **timing**, **power**, and **performance** in CMOS logic design.

---

## 📚 **References**

* 📘 *Sky130 PDK Documentation*
* ⚙️ *MOSFET Device Theory & SPICE Simulations*
* 💻 *Sky130 Circuit Design Workshop*
* 🧠 *VLSI Design Fundamentals – CMOS Theory Notes*

---

