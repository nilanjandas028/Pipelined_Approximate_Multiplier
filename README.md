# Design and Analysis of an Energy-Efficient 8-bit Pipelined Approximate Multiplier using Dynamic Input Truncation and Wallace Tree Reduction

This repository contains the comprehensive design, physical implementation, and performance characterization of an 8-bit unsigned pipelined approximate multiplier. Targeted for error-tolerant applications such as image processing, computer vision, and machine learning inference, the architecture prioritizes energy efficiency through a runtime-configurable accuracy mechanism.

## Key Architectural Features
* **Dynamic Input Truncation (Stage 1):** A custom combinational Mode Decoder translates a 4-bit user selection (`MODE` signal S3..S0) into bit-wise masking signals. By progressively forcing the Least Significant Bits (LSBs) of the input operands to logic zero, the switching activity factor (α) for all downstream logic gates drops to zero. This prevents power dissipation from propagating through the pipeline stages without requiring complex power-gating overhead.
* **Booth Encoding & Wallace Tree Reduction (Stage 2):** Employs Radix-4 Booth Encoding to group multiplier bits into triplets, reducing the partial product matrix from 8 rows down to 4 rows. These 4 rows are efficiently compressed in logarithmic time using a 2-stage Wallace Tree structure to minimize latency.
* **Brent-Kung Parallel-Prefix Adder (Stage 3):** A 16-bit Brent-Kung adder structure performs the final carry-propagate summation, utilizing regular tree-wiring arrays to lower physical layout congestion.

## Physical Implementation Details
* **Environment:** Cadence Virtuoso
* **Library:** DVLSI-2025 custom standard cell library
* **Routing Constraints:** Row-based cell placement with Metal-5 (M5) vertical power grids to minimize IR drop, utilizing M2–M4 Manhattan routing for signals to limit capacitive coupling.
* **Verification:** Design Rule Check (DRC) clean and Layout Versus Schematic (LVS) matched exactly against the netlist.

## Post-Layout Extraction (PEX) Results
Simulations were characterized using Cadence ADE at a **1 GHz clock frequency** across Typical, Fast, and Slow PVT corners.

The multiplier scales dynamically across 11 distinct operational modes (Mode 0 to Mode 10):

### Performance & Error Trade-off Summary (Typical Corner)

| Mode | Input Masking Status | Power Consumption | Power Saving | Percentage Error |
| :---: | :--- | :---: | :---: | :---: |
| **0** | **None (Exact Baseline)** | **1056 µW** | **0%** | **0.0%** |
| 3 | LSB 0-2 Masked | 887 µW | 16.0% | 4.0% |
| **5** | **LSB 0-4 Masked (Sweet Spot)** | **768 µW** | **27.2%** | **8.9%** |
| 7 | LSB 0-6 Partial Masked | 714 µW | 32.3% | 11.6% |
| **10** | **Max Approximation Mode** | **503 µW** | **52.3%** | **58.2%** |

### Key Observations
* **Efficiency Sweet Spot:** Mode 5 provides an optimal balancing point for general data workloads, lowering power consumption by 27.2% while keeping numerical error below 9%.
* **Emergency Power Mode:** Mode 10 slashes baseline power by more than half (52.3% reduction), serving as an ideal configuration for critical battery scenarios.
* **PVT Robustness:** Corner analysis confirmed that despite variations in current leakages, the predictable, linear power reduction trend stands robust against fabrication variations.

---
**Disclaimers & Acknowledgments***
*The foundational Process Design Kits (PDKs) and standard cell libraries used during this work are confidential intellectual property provided by the faculty at the Indian Institute of Science (IISc). Consequently, those library directory structures have been strictly isolated from tracking via this repository's `.gitignore` layer to respect NDA guidelines.*
