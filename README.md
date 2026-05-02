# ⚡ SolarSim Pro — LiFePO4 & Off-Grid System Designer

> A fully client-side, single-file HTML tool for designing, sizing, and simulating off-grid solar systems using LiFePO4 batteries. Built around Indian standard peak sun hours (4–5 h/day) and MicroPowerGrids engineering methodology.

---

## 📸 Overview

SolarSim Pro walks you through every step of off-grid solar system design — from listing your appliances, to sizing your battery bank and solar array, to simulating real-time discharge on a live ECG-style chart. No backend, no account, no install — just open the HTML file in any modern browser.

---

## ✨ Features

| Section | What it does |
|---|---|
| **Load Builder** | Add appliances with watts, daily hours, and quantity. Auto-sums to daily Wh/kWh. One-click sync to System Designer. |
| **System Architect** | Configure LiFePO4 cells (S × P), compute system voltage, total Ah, kWh, usable energy at DoD, cell count, and MPPT inverter hint. |
| **Solar & Autonomy Sizing** | Calculates minimum solar array (W) with efficiency factor and required battery capacity (Ah/kWh) for your autonomy days. |
| **Charge / Discharge Time Calculator** | Enter load in W or A — get C-rating, draw current, run time at 100% DoD and at your target DoD. Also calculates solar charge time and charge C-rate. |
| **Live Stress Test (ECG Sim)** | Real-time SOC drain chart driven by a load slider. Shows SOC %, time remaining, energy left, and C-rate. Includes a Reset button. |
| **Typical Home Configurations** | Pre-sized reference cards for 3 kW, 5 kW, and 10 kW Indian homes with inverter, battery, and panel specs. |
| **6-Step Engineering Guide** | Covers load assessment, battery autonomy, MPPT selection, the 4.5-hour solar rule, series vs parallel wiring, and C-rating formulas. |

---

## 🚀 Getting Started

No installation or server required.

```bash
# Simply open in your browser
open SolarSimPro_v2.html
```

Or double-click the file in your file manager. Works in Chrome, Firefox, Edge, and Safari.

---

## 🧮 Calculation Reference

All formulas used internally, cross-referenced against MicroPowerGrids and Indian off-grid standards.

### Battery Bank

```
System Voltage (V)   = Cells in Series (S) × Cell Voltage (V)
System Capacity (Ah) = Cells in Parallel (P) × Cell Ah Rating
System Energy (Wh)   = System Voltage × System Capacity
Total Cell Count     = S × P
Usable Energy (Wh)   = System Energy × (DoD / 100)
```

### Solar Array Sizing

```
Array (W) = Daily Consumption (Wh) ÷ (Peak Sun Hours × System Efficiency)
```

> Default: 4.5 Peak Sun Hours, 85% system efficiency (accounts for inverter + wiring losses).
> The original Gemini Canvas version did **not** include the efficiency factor, causing the array to be undersized.

### Battery Capacity Required

```
Required Ah = (Daily Wh × Days of Autonomy) ÷ (System Voltage × DoD)
```

### Discharge Time

```
Run Time (h) = Battery Capacity (Ah) × DoD ÷ Load Current (A)
           OR = System Energy (Wh) × DoD ÷ Load (W)
```

### Solar Charge Time

```
Charge Current (A)  = Solar Array (W) × Efficiency ÷ System Voltage (V)
Full Charge Time (h)= Battery Capacity (Ah) ÷ Charge Current (A)
Charge from DoD (h) = (Capacity × DoD) ÷ Charge Current
```

### C-Rating

```
Discharge C-Rate = Load Current (A) ÷ Battery Capacity (Ah)
Charge C-Rate    = Charge Current (A) ÷ Battery Capacity (Ah)
```

> **Rule of thumb:** Keep discharge ≤ 1C and charge ≤ 0.5C for maximum LiFePO4 cycle life (4,000+ cycles at 80% DoD).

---

## 📐 Default Values & Why

| Parameter | Default | Reason |
|---|---|---|
| Cell Voltage | 3.2 V | LiFePO4 nominal cell voltage |
| Cells in Series | 16 | 16 × 3.2 V = 51.2 V (48 V system) |
| Peak Sun Hours | 4.5 h | Indian average (range: 4–5 h/day) |
| Days of Autonomy | 2 | Indian off-grid standard for rainy season |
| Target DoD | 80% | Balances usable energy vs cycle life |
| System Efficiency | 85% | Typical inverter + cable + MPPT losses |
| Daily Consumption | 5,050 Wh | Matched to the load example in Image 3 |

---

## 🏠 Typical Indian Home Configurations

| Home Size | MPPT Inverter | Battery (LiFePO4) | Solar Array |
|---|---|---|---|
| 3 kW (2–3 BHK) | 3–4 kVA, 48 V | 48 V 100 Ah (4.8 kWh) | ~3 kW (440 W × 7) |
| 5 kW (3–4 BHK) | 5 kVA, 48/72 V | 72 V 100–150 Ah (7.2–10.8 kWh) | ~5 kW (440 W × 12) |
| 10 kW (Villa) | 7.5–10 kVA, 96/120 V | 96 V 200 Ah (~19 kWh) | ~10 kW (440 W × 24) |

---

## 🔧 What Was Fixed vs the Original (Gemini Canvas v1)

1. **Solar array undersized** — Added `÷ System Efficiency` to the formula. At 85% efficiency, a 5,050 Wh/day load requires ~1,320 W, not 1,122 W.
2. **Simulation time remaining** — Was showing time based on *full* capacity regardless of current SOC. Now correctly uses `current SOC energy ÷ load`.
3. **No SOC reset** — Added a Reset button to bring the live simulation back to 100%.
4. **Missing C-rating** — Added C-rate calculation to both the discharge calculator and the live simulation panel.
5. **Missing total cell count** — S × P is now displayed as a stat card.
6. **No charge time calculator** — Added a dedicated solar charge time section mirroring the MicroPowerGrids calculator.
7. **No load breakdown** — Added the appliance-level Load Builder with default values matching the reference guide example.
8. **No home configuration reference** — Added 3 kW / 5 kW / 10 kW reference cards.
9. **Guide incomplete** — Expanded from 3 principles to a full 6-step guide.
10. **No system efficiency input** — Previously hidden assumption; now an explicit user-editable field.

---

## 📚 References & Methodology

- [MicroPowerGrids LiFePO4 Battery Calculator](https://www.micropowergrids.com.au/Calculators/1_LiFePo4_Battery_Calculator.html)
- Indian MNRE Peak Sun Hour data (4–5 h/day average)
- BIGGPanther Solar MPPT Inverter Sizing Guide
- Standard LiFePO4 cell specification: 3.2 V nominal, 3.65 V max, 2.5 V min

---

## 🛠 Tech Stack

| Technology | Usage |
|---|---|
| HTML5 / CSS3 | Structure and layout |
| Tailwind CSS (CDN) | Utility-first styling |
| Chart.js (CDN) | Live ECG simulation chart |
| Vanilla JavaScript | All calculations and interactivity |
| Google Fonts | Space Mono + DM Sans typography |

No build step. No framework. No Node.js. Zero dependencies to install.

---

## ⚠️ Disclaimer

All values are indicative and for educational/planning purposes only. Consult a certified solar engineer before purchasing equipment or commissioning an off-grid system. Solar irradiance, battery temperature, cable sizing, BMS configuration, and local regulations all affect final system performance.

---

## 📄 License

© 2026 SolarSim Pro · Engineering tool based on MicroPowerGrids & Indian Off-Grid Standards.  
Free to use for personal and educational purposes.
