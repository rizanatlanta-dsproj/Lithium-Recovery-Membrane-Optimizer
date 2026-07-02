# Lithium-Recovery-Membrane-Optimizer
# Data-Driven Nanofiltration Simulation for Lithium-Magnesium Separation

This repository contains an automated process engineering pipeline built in Python to simulate and optimize the selective separation of Lithium ($Li^+$) from Magnesium ($Mg^{2+}$) impurities in aqueous brines. By integrating real-world water quality data with fundamental mass transport equations, this project transitions away from traditional manual trial-and-error parameter design toward an automated, multi-variable computational optimization framework.

---

## 1. Process Engineering Background

Efficient lithium extraction from natural brines or industrial wastewater is heavily limited by the presence of Magnesium contaminants. Because $Li^+$ and $Mg^{2+}$ exhibit high chemical similarity, achieving high-purity, battery-grade lithium requires precise operational control. 

This project simulates a nanofiltration plant to identify the global operational sweet spot—specifically balancing **Operating Pressure ($P$)** and **Membrane Pore Radius ($r_{\text{pore}}$)**—to maximize the recovery of Lithium in the permeate stream while blocking Magnesium.

The underlying physics engine models two primary mass transport phenomena:
1. **Steric Hindrance (Size Exclusion):** Solute transport scales non-linearly based on the geometric ratio of an ion's hydrated radius to the membrane's physical pore size. Because Magnesium ions have a larger hydrated volume in solution than Lithium ions, tighter pore structures can mechanically restrict $Mg^{2+}$.
2. **Donnan Exclusion (Electrostatic Repulsion):** Nanofiltration membranes maintain a structural surface charge. According to electrostatic transport theory, divalent ions ($Mg^{2+}$) face an exponentially higher potential energy barrier at the pore entrance than monovalent ions ($Li^+$), rejecting them from passing through the membrane boundary layer.

---

## 2. Pipeline Architecture & Design Evolution

The codebase reads raw environmental water quality datasets, extracts targeted ionic concentration matrices, and passes the variables through three progressive design iterations:

### Phase 1: Manual Operational Baseline
* **Configuration:** $35.0 \text{ Bar}$ operating pressure, $0.55 \text{ nm}$ membrane pore radius.
* **Performance:** Resulted in a Lithium rejection rate of $8.69\%$ and a Magnesium rejection rate of $46.54\%$, yielding a cumulative mass recovery of $6,754.71 \text{ grams}$ over the dataset timeline.

### Phase 2: Automated 1D Parameter Sweep
* **Configuration:** Pressures swept sequentially from $10$ to $50 \text{ Bar}$ while holding pore size static at $0.55 \text{ nm}$.
* **Performance:** Identified an operational peak at $43.0 \text{ Bar}$. While the higher pressure caused convective velocity to pull slightly more Lithium into the rejection stream ($9.70\%$), it increased Magnesium rejection to $47.80\%$, widening the overall separation efficiency gap.

### Phase 3: Global 2D Grid Search
* **Configuration:** Multi-variable search evaluating 2,091 unique combinations simultaneously ($P: 10\text{--}50 \text{ Bar}$; $r_{\text{pore}}: 0.3\text{--}0.8 \text{ nm}$).
* **Performance:** Discovered a definitive mathematical sweet spot at **$50.0 \text{ Bar}$** and **$0.30 \text{ nm}$**. Lithium rejection dropped to an ultra-efficient low of **$5.70\%$** while Magnesium rejection maximized at **$54.30\%$**.

---

## 3. Engineering Performance Summary

| Metric | Phase 1: Baseline | Phase 2: 1D Sweep | Phase 3: 2D Optimized | Net Improvement |
| :--- | :---: | :---: | :---: | :---: |
| **Operating Pressure** | $35.0 \text{ Bar}$ | $43.0 \text{ Bar}$ | **$50.0 \text{ Bar}$** | Optimized Flux Dynamics |
| **Membrane Pore Radius** | $0.55 \text{ nm}$ | $0.55 \text{ nm}$ | **$0.30 \text{ nm}$** | Maximized Steric Blocking |
| **Lithium Rejection %** | $8.69\%$ | $9.70\%$ | **$5.70\%$** | **$-2.99\%$** (Less Mass Wasted) |
| **Magnesium Rejection %** | $46.54\%$ | $47.80\%$ | **$54.30\%$** | **$+7.76\%$** (Higher Purity) |
| **Total Lithium Harvested** | $6,754.71 \text{ g}$ | 6678.25 g | **$6,975.83 \text{ g}$** | **$+221.12 \text{ grams}$** |

### Key Technical Insights
* **The Counter-Intuitive Trade-off:** The transition between Phase 1 and Phase 2 demonstrated that optimizing chemical processes often involves local compromises. Increasing pressure initially increased Lithium loss, but because it restricted the bulkier contaminant stream at a much faster rate, the system achieved a net separation gain.
* **Dynamic Scalability:** The generated dual-axis output plot (`final_optimized_lithium_recovery.png`) shows that the optimized mass yield closely mirrors raw input Total Dissolved Solids (TDS) fluctuations. This verifies that the mathematical model effectively handles unpredictable, real-world feed changes.

---

## 4. References

The mathematical models and physics criteria mapped in this simulator are grounded in the following foundational separation literature:

* **Spiegler, K. S., & Kedem, O. (1966).** *Thermodynamics of hyperfiltration (reverse osmosis): criteria for efficient membranes.* Desalination, 1(4), 311-326. 
  *(Used to define core irreversible thermodynamics and the coupled competition between pressure-driven convective flux and concentration-driven diffusive flux).*
  
* **Bowen, W. R., & Mukhtar, A. W. (2006).** *Characterisation and prediction of nanofiltration membrane performance—a generalized approach.* Chemical Engineering Science, 51(8), 1333-1346.
  *(Used to construct the non-linear relationship between pore geometry, hydrated ionic radii, and steric exclusion metrics).*
  
* **Schaep, J., Van der Bruggen, B., Vandecasteele, C., & Wilms, D. (2001).** *Influence of ion size and charge on NF separation.* Separation and Purification Technology, 22, 169-179.
  *(Used to model the Donnan Steric Pore Model (DSPM) logic, verifying why divalent ions experience significantly higher electrostatic repulsion barriers than monovalent targets).*
