# 🌞 **Day 1 — Sky130 NMOS Device Fundamentals & Id–Vds Characteristics**

---

## 🧠 **1. Introduction / Background**

The NMOS transistor is one of the two fundamental building blocks of CMOS (Complementary Metal-Oxide-Semiconductor) technology. Understanding its electrical characteristics is crucial because:

* ⚙️ NMOS behavior directly impacts CMOS logic performance — e.g., inverter switching threshold, rise/fall times, and static power.
* 📈 It forms the basis for transistor-level delay models used in Static Timing Analysis (STA).
* ⚡ Device characteristics such as threshold voltage (Vt), transconductance, and saturation behavior determine switching speed and power.

In a MOSFET, the gate voltage controls whether a conductive channel forms between the source and drain, allowing electrons to flow when the device is in inversion.

### 🎯 **In this experiment**

* Analyze **Id–Vds** (output) and **Id–Vgs** (transfer) characteristics.
* Simulate the device using **Ngspice** and **SkyWater Sky130 PDK**.
* Extract **threshold voltage (Vt)**.
* Relate device behavior to CMOS switching and STA principles.

💡 *Id–Vds characteristics show transistor current behavior as a voltage-controlled current source, while Id–Vgs helps identify threshold voltage and subthreshold behavior.*

---

## ⚙️ **2. NMOS Device Physics & Operation**

### 🧩 2.1 Structure

An NMOS transistor is a four-terminal device:

| Terminal   | Description                                    |
| ---------- | ---------------------------------------------- |
| Drain (D)  | Current flows out                              |
| Gate (G)   | Controls channel formation (insulated by SiO₂) |
| Source (S) | Electrons enter the device                     |
| Body (B)   | p-type substrate                               |

**Fabrication Summary:**

* Built on a p-type substrate
* Source & drain are n⁺ diffusion regions
* Thin SiO₂ gate oxide provides insulation
* Polysilicon or metal gate controls inversion charge

### ⚡ 2.2 Threshold Voltage & Channel Formation

* At **Vgs = 0 V**, no conduction between source and drain.
* As **Vgs** rises, the surface inverts once **Vgs ≥ Vt (~0.45 V)**.
* Beyond **Vt**, a conductive n-channel forms, allowing electron flow when **Vds** is applied.

### 🧮 2.3 Body Effect

Applying positive **Vsb** widens the depletion region and increases **Vt**, shifting Id–Vgs upward.
This is modeled by the body-effect coefficient (γ) in SPICE.

---

## 🚦 **3. NMOS Regions of Operation**

| Region              | Condition                              | Behavior                                |
| ------------------- | -------------------------------------- | --------------------------------------- |
| **Cutoff**          | (V_{GS} < V_t)                         | No conduction — OFF state               |
| **Linear (Triode)** | (V_{GS} > V_t,; V_{DS} < V_{GS} - V_t) | (I_D ∝ V_{DS}); acts as resistor        |
| **Saturation**      | (V_{DS} ≥ V_{GS} - V_t)                | (I_D) saturates; acts as current source |

---

## 🧾 **4. SPICE Simulation Setup**

**Tool:** Ngspice
**Technology:** SkyWater 130 nm (TT Corner)

**Simulation Steps**

1. Include the Sky130 model file
2. Define NMOS parameters and bias conditions
3. Perform `.dc` sweep for Vgs and Vds
4. Observe Id–Vds & Id–Vgs characteristics

---

## 💻 **5. SPICE Netlists / Code**

```spice
* Model Description
.param temp=27

* Including Sky130 library files
.lib "sky130_fd_pr/models/sky130.lib.spice" tt

* Netlist Description
XM1 Vdd n1 0 0 sky130_fd_pr__nfet_01v8 w=5 l=2
R1 n1 in 55
Vdd vdd 0 1.8V
Vin in 0 1.8V

* Simulation Commands
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

## 📊 **6. Plots & Figures**

**🖼️ Simulation Outputs:**

<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/dc94be9d-3119-41ee-888b-f1d6daa1ef56" />

<img width="1681" height="995" alt="Image" src="https://github.com/user-attachments/assets/182c0929-0313-49bf-b727-15bd36e67897" />

**Observation:**

* For small **Vds**, Id increases linearly → *Linear Region*
* When **Vds ≈ Vgs − Vth**, current saturates → *Saturation Region*
* Increasing **Vgs** → stronger inversion → higher Id

---

## 📐 **7. Tabulated Results**

| Parameter               | Measured Value  |
| ----------------------- | --------------- |
| Threshold Voltage (Vt)  | ≈ 0.45 V        |
| Supply Voltage (Vdd)    | 1.8 V           |
| W / L Ratio             | 5 µm / 2 µm     |
| Temperature             | 27 °C           |
| λ (channel-length mod.) | From model file |

---

## 🔍 **8. Observations / Analysis**

✅ The Id–Vds curves clearly show the transition from linear to saturation region.
✅ Measured Vt ≈ 0.45 V matches the Sky130 TT model.
✅ Higher Vgs results in stronger channel formation and higher drain current.
✅ Id saturation point confirms expected device behavior from theory.

---

## 🏁 **9. Conclusions**

⭐ NMOS transistor operation governs CMOS logic gate performance.
⭐ Threshold voltage determines switching and noise margins.
⭐ Understanding Id–Vds behavior is essential for transistor sizing, speed, and power trade-offs.
⭐ Results validate theoretical models used in STA and digital design.

---

## 📚 **10. References / Citations**

* Sky130 PDK Documentation (TT Corner)
* Ngspice User Manual
* VSD Sky130 Circuit Design Workshop
* *CMOS VLSI Design: A Circuits and Systems Perspective*
* GitHub: [Sky130CircuitDesignWorkshop](https://github.com/)

---
