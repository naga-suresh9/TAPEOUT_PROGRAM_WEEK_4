# 🌞 **Day 2 — Sky130 NMOS Device Characterization: Id–Vds & Id–Vgs Analysis**

---

## 🧠 **Introduction / Background**

This module focuses on characterizing an **NMOS transistor** using the **Sky130 1.8 V PDK models**, analyzing both **Id–Vds** and **Id–Vgs** behavior.

At short channel lengths, electric fields in the channel can become significant, affecting carrier velocity and current saturation. Understanding these effects is critical for:

* Drain current characteristics (**Id–Vds** and **Id–Vgs** curves)
* Threshold voltage estimation (**Vth**)
* Switching speed and transistor-level timing
* Impacts on CMOS logic behavior and STA analysis

### **Key Concepts Covered**

* NMOS Id–Vds & Id–Vgs characteristics
* Threshold voltage extraction from transfer curves
* Identification of linear vs. saturation regions
* Device behavior impact on timing and inverter switching

---

## 🧪 **Part A — Id–Vds Characteristics**

### **A1. SPICE Netlist — Id–Vds Sweep**

```spice
*******************************************************
* 📁 File: nfet_idvds.spice
* 📗 Purpose: NMOS Id–Vds characteristics
* 📚 PDK: Sky130_fd_pr (1.8 V)
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

### **A2. Id–Vds Plot**

<img width="1920" height="1080" alt="day2_nfet_idvds_L015_W039 spice" src="https://github.com/user-attachments/assets/1fdca394-a7c6-43a2-9d66-11d8c6621c10" />
<img width="1681" height="995" alt="day2_nfet_idvds_output" src="https://github.com/user-attachments/assets/fa15968b-11e0-4b1e-aab8-92b82cff9c05" />

* **Observation:**

  * For small **Vds**, Id rises linearly → *Linear Region*
  * For Vds ≈ Vgs − Vth, Id saturates → *Saturation Region*

---

## 🧪 **Part B — Id–Vgs Characteristics & Vth Extraction**

### **B1. SPICE Netlist — Id–Vgs Sweep**

```spice
*******************************************************
* 📁 File: nfet_idvgs.spice
* 📗 Purpose: NMOS Id–Vgs characteristics
* 📚 PDK: Sky130_fd_pr (1.8 V)
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

### **B2. Id–Vgs Plot**

<img width="1920" height="1080" alt="day2_nfet_idvgs_L015_W039 spice" src="https://github.com/user-attachments/assets/7622cc24-ecdf-47d6-b2e5-3cdec80eb6ec" />
<img width="1681" height="995" alt="day2_nfet_idvgs_output" src="https://github.com/user-attachments/assets/b8937eb3-33e5-473b-8092-1a9119ab7ef5" />

* **Observation:**

  * Threshold voltage (**Vth**) = **0.41 V**
  * Short-channel device deviates from quadratic Id–Vgs behavior due to velocity saturation

---

## 📐 **Tabulated Results**

| ⚙️ Parameter | 🧩 Description                   | 📏 Value                 |
| ------------ | -------------------------------- | ------------------------ |
| **Vth**      | Threshold voltage (from Id–Vgs)  | 0.41 V                   |
| **Id_sat**   | Saturation current (Vgs = 1.8 V) | Saturated Earlier / plot |
| **Vds(sat)** | Saturation onset (≈ Vgs − Vth)   | 1.39 V                   |

---

## 🔍 **Observations / Analysis**

✅ Small **Vds** → Linear Region (Id ∝ Vds)
✅ Vds ≈ Vgs − Vth → Saturation Region begins
✅ Increasing **Vgs** → stronger channel → higher Id
✅ **Vth** determines onset of strong conduction
✅ Linear and saturation behaviors directly affect **logic threshold** and **timing delay**

---

## 🧩 **Connection to STA Concepts**

📈 In **Static Timing Analysis (STA)**:

* Variations in **Vth** → delay uncertainty
* **Drive current (Id_sat)** → signal transition time
* **Device geometry & supply voltage** → affect switching speed & critical path delays

---

## 🏁 **Conclusions**

⭐ NMOS transistor behavior under various biases is clearly demonstrated
⭐ Id–Vds curves reveal linear → saturation transition
⭐ Vth extraction provides critical device information
⭐ Insights are foundational for analyzing **timing, power, and performance** in CMOS logic

---

## 📚 **References**

* 📘 *Sky130 PDK Documentation*
* ⚙️ *MOSFET Device Theory & SPICE Simulations*
* 💻 *Sky130 Circuit Design Workshop*
* 🧠 *VLSI Design Fundamentals – CMOS Theory Notes*

---

