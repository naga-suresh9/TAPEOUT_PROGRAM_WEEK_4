#  **Day 1 — Sky130 MOSFET Characterization: NFET Id–Vds Analysis** ⚡

---

## 🧠 **Introduction / Background**

This experiment focuses on characterizing an **NFET MOSFET** using the **Sky130 PDK** models.
The main goal is to analyze **Id vs Vds** at various **Vgs** levels 🔍.

These characteristics are vital for understanding device operation in:

* 🌈 **Linear Region**
* 🚀 **Saturation Region**

They also help in extracting key parameters like **Threshold Voltage (Vth)** and **Saturation Current (Id_sat)** — crucial for digital & analog CMOS design 🧩.

---

## 🧾 **SPICE Netlist**

```spice
* 🌟 Model Description
.param temp=27

* 📚 Include Sky130 PDK library
.lib "sky130_fd_pr/models/sky130.lib.spice" tt

* ⚙️ Netlist Configuration
XM1 Vdd n1 0 0 sky130_fd_pr__nfet_01v8 w=5 l=2
R1 n1 in 55
Vdd vdd 0 1.8V
Vin in 0 1.8V

* 🎯 Simulation Commands
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

## 📊 **Plots & Figures**

✨ **Id–Vds characteristics** for multiple **Vgs** values:

🔹 **Linear Region:** Id ∝ Vds
🔹 **Saturation Region:** Id ≈ constant when Vds ≈ Vgs − Vth

🧭 Threshold voltage and saturation onset points are highlighted for clarity 💡

> 📸 **Simulation Output:**
>
> <img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/dc94be9d-3119-41ee-888b-f1d6daa1ef56" />  

<img width="1681" height="995" alt="Image" src="https://github.com/user-attachments/assets/182c0929-0313-49bf-b727-15bd36e67897" />

---

## 📐 **Tabulated Results**

| ⚙️ Parameter              | 📏 Value        |
| ------------------------- | --------------- |
| 🧩 Extracted Vth          | [your value] V  |
| ⚡ Id_sat (typical)        | [your value] µA |
| 📍 Vds @ saturation onset | [your value] V  |

🧮 *(Replace the placeholders with your simulation data!)*

---

## 🔍 **Observations & Analysis**

✅ For **low Vds**, Id rises linearly ➡️ **Linear Region**
✅ For **high Vds**, Id flattens ➡️ **Saturation Region**
✅ Higher **Vgs** ⇒ stronger channel formation 💪 ⇒ more current flow
✅ The transition point (**Vds ≈ Vgs − Vth**) indicates the **start of saturation** 🚦

💬 These findings are essential in **Static Timing Analysis (STA)** — they influence circuit **speed**, **output drive**, and **propagation delay** ⏱️.

---

## 🏁 **Conclusions**

⭐ The **Id–Vds** curve reveals key MOSFET behaviors.
⭐ **Threshold voltage (Vth)** plays a major role in determining switching performance.
⭐ The extracted parameters are crucial inputs for **design optimization** and **STA**.

📘 Understanding these helps build efficient CMOS circuits for both digital logic and analog designs 🧠💻.

---

## 📚 **References**

* 📘 *Sky130 PDK Documentation*
* ⚙️ *MOSFET Theory & SPICE Simulation Guides*
* 💻 *Example Projects:*

  * CMOS Circuit Design & SPICE using SKY130
  * Inverter Analysis with Sky130 PDK
  * Sky130 Circuit Design Workshop

---
