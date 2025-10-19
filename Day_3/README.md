# ⚡ **Week 3 – CMOS Inverter Design & Analysis (Sky130)**

---

## 🧠 **Introduction / Background**

This experiment focuses on designing and analyzing a **CMOS inverter** using the **Sky130 PDK**.
It demonstrates how transistor sizing and device characteristics influence **switching threshold**, **propagation delays**, and **noise margins** — key aspects in **Static Timing Analysis (STA)**.

You will:

* Derive the **VTC (Voltage Transfer Characteristic)** of a CMOS inverter 🧮
* Simulate **transient switching behavior** using a pulse input ⏱️
* Measure **rise/fall times**, **delays**, and observe inverter **robustness**

---

## ⚙️ **SPICE Netlists**

### 🧩 **1️⃣ VTC Simulation (DC Sweep)**

```spice
* CMOS Inverter DC Sweep (VTC)
.param temp = 27
.lib "sky130_fd_pr/models/sky130.lib.spice" tt

XM1 out in vdd vdd sky130_fd_pr__pfet_01v8 w=0.84 l=0.15
XM2 out in 0 0 sky130_fd_pr__nfet_01v8 w=0.36 l=0.15

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

### 🧩 **2️⃣ Transient Simulation (Dynamic Switching Behavior)**

```spice
* CMOS Inverter Transient Simulation
.param temp = 27
.lib "sky130_fd_pr/models/sky130.lib.spice" tt

XM1 out in vdd vdd sky130_fd_pr__pfet_01v8 w=0.84 l=0.15
XM2 out in 0 0 sky130_fd_pr__nfet_01v8 w=0.36 l=0.15

Cload out 0 50fF
Vdd vdd 0 1.8V
Vin in 0 PULSE(0V 1.8V 0 0.1ns 0.1ns 2ns 4ns)

.tran 1n 10n

.control
run
.endc

.end
```

---

## 📊 **Plots & Figures**

### 1️⃣ **Voltage Transfer Characteristic (VTC)**

* Sweep `Vin` from 0 V → 1.8 V
* Plot `Vout` vs `Vin`
* Identify:

  * **Switching threshold (Vm)** → point where `Vout = Vin`
  * **VOH, VOL, VIH, VIL**
* Compute:

  * **NML = VIL − VOL**
  * **NMH = VOH − VIH**

<img width="1751" height="1035" alt="day3_inv_vtc_output" src="https://github.com/user-attachments/assets/affdfc89-9db6-4fa8-be83-ede64b2f963a" />
<img width="1920" height="1080" alt="day_03_vtc" src="https://github.com/user-attachments/assets/1cc5d970-518e-45ff-a1a9-e13d103f091e" />

---

### 2️⃣ **Transient Waveforms**

* Apply a **pulse input** at Vin (0 V ↔ 1.8 V)
* Plot **Vin and Vout over time**
* Measure **rise/fall delays** at 50% Vdd crossing:

<img width="1920" height="1080" alt="day_03_trans" src="https://github.com/user-attachments/assets/68cce06a-699c-45df-bf3d-f7fc8901f863" />
<img width="1751" height="1035" alt="day3_inv_tran_output" src="https://github.com/user-attachments/assets/88e1ac4c-787d-4fa3-854c-37f1c53fccbd" />

---

## 📋 **Tabulated Results**

| Parameter | Description         | Measured Value |
| --------- | ------------------- | -------------- |
| **Vm**    | Switching Threshold | 0.90 V         |
| **tpLH**  | Low → High Delay    | 85 ps          |
| **tpHL**  | High → Low Delay    | 60 ps          |
| **NML**   | Noise Margin (Low)  | 0.70 V         |
| **NMH**   | Noise Margin (High) | 0.72 V         |

---

## 🔍 **Observations / Analysis**

* **VTC:** Sharp transition near Vm (~ Vdd / 2) confirms proper inverter operation.
* **Delays:** tpHL < tpLH due to higher NMOS mobility.
* **Noise Margins:** Indicate inverter tolerance to input noise.
* **W/L Ratio Adjustment:** Larger PMOS width shifts Vm and balances rise/fall delay.
* **Supply or device size variations** directly affect **timing robustness**.

---

## ⏱️ **Relevance to STA**

* **Vm** defines logical “1”/“0” for timing analysis.
* **Propagation delays** are critical for cell delay modeling in STA.
* **Rise/fall asymmetry** affects timing skew and slack.
* **Noise margins** impact setup/hold analysis under process/supply variations.

---

## 🏁 **Conclusions**

* CMOS inverter is a **fundamental timing element** in digital circuits.
* VTC and transient analyses provide STA-relevant delay data.
* Understanding **sizing, threshold, and delay variation** is essential for robust digital design.

---

## 📚 **References**

* Sky130 PDK & model documentation
* “CMOS Circuit Design and SPICE Simulation using Sky130” ([GitHub](https://github.com/kunalg123/sky130CircuitDesignWorkshop))
* MOSFET theory & timing analysis textbooks

---

