# ATE-interfaceing-controller-to-JSCAN-Architecture
# JSCAN Decompressor (Scalable PSAS + PRAS Architecture)

##  Overview

This project implements a **scalable scan decompressor architecture** for VLSI testing, combining:

* **PSAS (Pseudo-Serial Access Scheme)** – LFSR-based decompression
* **PRAS (Pseudo-Random Access Scheme)** – memory-based random access

The design is written in **Verilog (synthesizable)** and verified using **Xilinx Vivado**.

---

##  Key Features

* **Fully parameterized design**

  * `C` → number of compressed inputs
  * `W` → LFSR width
  * `N` → number of scan outputs (N > C)

* **Scalable decompression**

  * Supports **n input → m output expansion**
  * Achieves high compression ratios

* **LFSR-based pattern generation**

  * Linear feedback with configurable taps
  * Avoids lock-up using non-zero initialization

* **Phase shifter**

  * Expands LFSR state into multiple scan outputs
  * Reduces correlation between scan chains

* **PRAS memory block**

  * Row/column addressable storage
  * Supports selective read/write operations

* **Vivado-compatible**

  * Fully synthesizable (no modulo / unsafe indexing)
  * Uses generate blocks for scalability

---

##  Architecture

Compressed inputs are expanded using a two-stage process:

1. **Temporal expansion (LFSR)**

   * Mixes current and past inputs

2. **Spatial expansion (Phase Shifter)**

   * Distributes LFSR state across multiple outputs

```
Compressed Inputs (C)
        ↓
   LFSR (W bits)
        ↓
 Phase Shifter
        ↓
Scan Outputs (N)
```

---

##  Design Relationship

* `W ≥ C` (LFSR must accommodate inputs)
* `N >> C` (for effective compression)
* Typical guideline:

  * `W ≈ 4C`
  * `N ≈ 2W` (or higher depending on compression needs)

> Note: These are design heuristics, not strict rules.

---

##  Verification

A dedicated testbench validates:

* Reset behavior
* PSAS decompression flow
* PRAS read/write operations
* Mode switching (PSAS / PRAS / both)

Simulation performed in **Xilinx Vivado**.

---

##  Applications

* Scan compression in DFT (Design for Testability)
* Reduced test data volume in ATE
* High-throughput scan testing

---

##  Files

* `Generic Controller` → Main design
* `generic_Tb` → Testbench

---

##  Key Insight

The decompressor is a **linear system over GF(2)**:

* Outputs are XOR combinations of current and past inputs
* Enables deterministic yet pseudo-random pattern expansion

---

##  Future Improvements

* Optimized phase shifter matrix (lower correlation)
* Integration with MISR (response compaction)
* ATPG-aware decompressor tuning

---

##  Author

Ragulnath
M.Tech VLSI | RTL Design | DFT Enthusiast

---
 

