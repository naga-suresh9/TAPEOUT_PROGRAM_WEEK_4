# TAPEOUT_PROGRAM_WEEK_4
# 🌟 Sky130 CMOS Workshop: Days 1 → 5

Welcome to the **Sky130 CMOS Workshop**! 🎓  
This README captures **Day 1 → Day 5 experiments** including **MOSFET characterization, inverter design, noise margins, and device/supply variation studies** using the **Sky130 PDK**.  
All SPICE netlists are embedded for easy reference and simulation.

---

## 🗓️ **Day 1 – NFET Id–Vds Characterization** 🟢

### 🧠 Introduction / Background
- Characterize NFET MOSFET using Sky130 PDK  
- Observe **linear vs saturation regions**  
- Extract **Vth** and **Idsat** for STA and circuit analysis  

### ⚙️ SPICE Netlist

```spice
*Day 1 – NFET Id-Vds Characterization
.param temp=27
.lib "sky130_fd_pr/models/sky130.lib.spice" tt

XM1 Vdd n1 0 0 sky130_fd_pr__nfet_01v8 w=0.39 l=0.15
R1 n1 in 55
Vdd vdd 0 1.8V
Vin in 0 1.8V

.op
.dc Vdd 0 1.8 0.1 Vin 0 1.8 0.2

.control
run
display
setplot dc1
.endc

.end
````

### 📊 Observations

* Id rises linearly at low Vds → **linear region**
* Id saturates when Vds ≈ Vgs − Vth → **saturation region**
* Increasing Vgs → higher Id due to stronger **channel formation** 🔌

---

## 🗓️ **Day 2 – NFET Id–Vds & Id–Vgs Analysis** 🔵

### 🧠 Introduction

* Extend Day 1: sweep Id–Vgs for **threshold extraction**
* Observe **velocity saturation** and short-channel effects
* Feed results into **timing and STA models**

### ⚙️ SPICE Netlists

**Id–Vds Sweep:**

```spice
*Day 2 – NFET Id-Vds
.param temp=27
.lib "sky130_fd_pr/models/sky130.lib.spice" tt

XM1 Vdd n1 0 0 sky130_fd_pr__nfet_01v8 w=0.39 l=0.15
R1 n1 in 55
Vdd vdd 0 1.8V
Vin in 0 1.8V

.op
.dc Vin 0 1.8 0.1

.control
run
display
setplot dc1
.endc

.end
```

**Id–Vgs Sweep:**

```spice
*Day 2 – NFET Id-Vgs
.param temp=27
.lib "sky130_fd_pr/models/sky130.lib.spice" tt

XM1 Vdd n1 0 0 sky130_fd_pr__nfet_01v8 w=0.39 l=0.15
R1 n1 in 55
Vdd vdd 0 1.8V
Vin in 0 1.8V

.op
.dc Vin 0 1.8 0.01

.control
run
display
setplot dc1
.endc

.end
```

### 📊 Observations

* Linear region: Id ∝ Vds
* Saturation region: Id ≈ constant
* Threshold extraction is critical for **timing analysis** ⏱️

---

## 🗓️ **Day 3 – CMOS Inverter Design & Analysis** 🟣

### 🧠 Introduction

* Build CMOS inverter (PMOS + NMOS)
* Analyze **VTC** and **transient response**
* Extract **rise/fall delays**, switching threshold (Vm)

### ⚙️ SPICE Netlists

**VTC Sweep:**

```spice
*Day 3 – CMOS Inverter VTC
.param temp=27
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

**Transient Response:**

```spice
*Day 3 – CMOS Inverter Transient
.param temp=27
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

### 📊 Observations

* Vm ≈ Vdd/2 ⚖️
* Rise/fall delays differ due to PMOS/NMOS mobility
* Noise margins indicate inverter robustness

---

## 🗓️ **Day 4 – Noise Margin / Robustness Analysis** 🟡

```spice
*Day 4 – CMOS Inverter Noise Margin
.param temp=27
.lib "sky130_fd_pr/models/sky130.lib.spice" tt

XM1 out in vdd vdd sky130_fd_pr__pfet_01v8 w=1 l=0.15
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

### 📊 Observations

* Extract **VIL, VIH, VOL, VOH**
* Compute **NML = VIL − VOL**, **NMH = VOH − VIH**
* Noise margins quantify **logic robustness** ✅

---

## 🗓️ **Day 5 – Power-Supply & Device Variation** 🔴

### ⚙️ SPICE Netlists

**Device Variation:**

```spice
*Day 5 – Device Variation Analysis
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

**Supply Voltage Variation:**

```spice
*Day 5 – Supply Voltage Variation Analysis
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

### 📊 Observations

* **Vm shifts lower** when Vdd decreases 🔻
* **VOL slightly increases**, **VOH slightly decreases**
* Noise margins (NML, NMH) **reduce**, reducing inverter robustness ⚡
* Increasing **PMOS/NMOS W/L** improves drive strength and robustness 💪
* Demonstrates how **supply voltage & device sizing affect timing** and **STA margins**

---

## 📚 References

* Sky130 PDK Documentation
* CMOS & MOSFET textbooks
* GitHub: [Sky130CircuitDesignWorkshop](https://github.com/kunalg123/sky130CircuitDesignWorkshop)

```

