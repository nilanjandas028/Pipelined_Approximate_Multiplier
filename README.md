# Design and Analysis of an Energy-Efficient 8-bit Pipelined Approximate Multiplier using Dynamic Input Truncation and Wallace Tree Reduction

[cite_start]This repository contains the comprehensive design, physical implementation, and performance characterization of an 8-bit unsigned pipelined approximate multiplier[cite: 6]. [cite_start]Targeted for error-tolerant applications such as image processing, computer vision, and machine learning inference, the architecture prioritizes energy efficiency through a runtime-configurable accuracy mechanism[cite: 7].

[cite_start]Developed as a final project for the **Digital VLSI (DVLSI-2025)** course at the **Indian Institute of Science (IISc), Bengaluru**[cite: 1, 5, 131].

---

##Key Architectural Features

1. [cite_start]**Dynamic Input Truncation (Stage 1):** A custom combinational Mode Decoder translates a 4-bit user selection (`MODE` signal $S_3..S_0$) into bit-wise masking signals[cite: 8, 27, 29, 64]. [cite_start]By progressively forcing the Least Significant Bits (LSBs) of the input operands to logic zero, the switching activity factor ($\alpha$) for all downstream logic gates drops to zero[cite: 59, 61]. [cite_start]This prevents power dissipation from propagating through the pipeline stages without requiring complex power-gating overhead[cite: 9, 62].
2. [cite_start]**Booth Encoding & Wallace Tree Reduction (Stage 2):** Employs Radix-4 Booth Encoding to group multiplier bits into triplets, reducing the partial product matrix from 8 rows down to 4 rows[cite: 106, 107, 108]. [cite_start]These 4 rows are efficiently compressed in logarithmic time using a 2-stage Wallace Tree structure to minimize latency[cite: 40, 115, 119].
3. [cite_start]**Brent-Kung Parallel-Prefix Adder (Stage 3):** A 16-bit Brent-Kung adder structure performs the final carry-propagate summation, utilizing regular tree-wiring arrays to lower physical layout congestion[cite: 11, 123, 124].

---

## Physical Implementation Details

- [cite_start]**Environment:** Cadence Virtuoso [cite: 131]
- [cite_start]**Library:** DVLSI-2025 custom standard cell library [cite: 131]
- [cite_start]**Routing Constraints:** Row-based cell placement with Metal-5 (M5) vertical power grids to minimize IR drop, utilizing M2–M4 Manhattan routing for signals to limit capacitive coupling[cite: 12, 134, 138, 140, 141].
- [cite_start]**Verification:** Design Rule Check (DRC) clean and Layout Versus Schematic (LVS) matched exactly against the netlist[cite: 32, 144, 145].

---

## Post-Layout Extraction (PEX) Results

[cite_start]Simulations were characterized using Cadence ADE at a **1 GHz clock frequency** across Typical, Fast, and Slow PVT corners[cite: 154, 155, 162, 164, 181]. 

[cite_start]The multiplier scales dynamically across **11 distinct operational modes** (Mode 0 to Mode 10)[cite: 11]:

### [cite_start]Performance & Error Trade-off Summary (Typical Corner) [cite: 205]

| [cite_start]Mode [cite: 206] | [cite_start]Input Masking Status [cite: 206] | [cite_start]Power Consumption [cite: 206] | [cite_start]Power Saving [cite: 206] | [cite_start]Percentage Error [cite: 206] |
| :---: | --- | :---: | :---: | :---: |
| **0** | **None (Exact Baseline)** | **1056 µW** | **0%** | **0.0%** |
| **3** | LSB 0-2 Masked | 887 µW | 16.0% | 4.0% |
| **5** | **LSB 0-4 Masked (Sweet Spot)** | **768 µW** | **27.2%** | **8.9%** |
| **7** | LSB 0-6 Partial Masked | 714 µW | 32.3% | 11.6% |
| **10** | **Max Approximation Mode** | **503 µW** | **52.3%** | **58.2%** |

### Key Observations
- [cite_start]**Efficiency Sweet Spot:** **Mode 5** provides an optimal balancing point for general data workloads, lowering power consumption by **27.2%** while keeping numerical error below **9%**[cite: 208].
- [cite_start]**Emergency Power Mode:** **Mode 10** slashes baseline power by more than half (**52.3% reduction**), serving as an ideal configuration for critical battery scenarios[cite: 179, 210].
- [cite_start]**PVT Robustness:** Corner analysis confirmed that despite variations in current leakages, the predictable, linear power reduction trend stands robust against fabrication variations[cite: 191, 192].

---

## 📝 Disclaimers & Acknowledgments
[cite_start]*The foundational Process Design Kits (PDKs) and standard cell libraries used during this work are confidential intellectual property provided by the faculty at the Indian Institute of Science (IISc)[cite: 223]. Consequently, those library directory structures have been strictly isolated from tracking via this repository's `.gitignore` layer to respect NDA guidelines.*
