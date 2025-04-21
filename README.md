# CNN-Hardware-Acclerator
A pipelined CNN inference hardware accelerator in Verilog HDL implementing parallel convolution, ReLU, and max pooling with SRAM interfacing and FSM control.


# 🧠 CNN Hardware Accelerator (RTL Implementation)

This project implements a **Convolutional Neural Network (CNN) inference accelerator** in **Verilog HDL**, targeting efficient execution of convolution, ReLU, and max-pooling operations over packed input data. The accelerator is driven by a 5-stage FSM and designed for high-throughput parallel data processing.

---

## 🚀 Project Overview

- **Goal**: To design and synthesize a CNN inference engine for a 3-layer computation block — Convolution + ReLU + Max Pooling.
- **Inputs**: 4×4 input matrices and 3×3 kernel weights (stored in SRAMs)
- **Outputs**: Max-pooled activated results after convolution
- **Parallelism**: 4 parallel convolutions per clock cycle

---

## 🔧 Technical Stack

- **HDL**: Verilog
- **Simulation**: ModelSim, GTKWave
- **Synthesis Tool**: Synopsys Design Compiler
- **Target**: ASIC-compatible RTL with SRAM interfacing

---

## 🧩 Key Features

| Feature                    | Description                                                                 |
|---------------------------|-----------------------------------------------------------------------------|
| 5-Stage FSM               | FSM handles control: Idle → Load → Compute → Pool → Write-back              |
| Parallel Convolution      | 4 convolutions per cycle using sliding window over 4×4 input block          |
| ReLU & Max Pooling        | Hardware-based ReLU + 2×2 pooling logic                                     |
| Packed SRAM Access        | 16-bit reads for 2 pixels/weights at a time                                 |
| Window Sliding Mechanism  | Controlled via `row_counter`, `column_counter`, and `starting_point`        |

---

## 🧠 FSM Overview

The accelerator uses a 5-stage FSM:

1. **IDLE**: Wait for `dut_run` signal  
2. **LOAD**: Read 16 input pixels + 9 weights into registers  
3. **COMPUTE**: Perform 4 convolutions with stored weights  
4. **POOL**: Apply ReLU and Max Pooling  
5. **WRITE-BACK**: Store result to output SRAM with proper address update  

---

## 🧪 Testing Strategy

- **Testbench**: Handwritten in Verilog  
- **Input Data**: Loaded via `.memh` files (packed 16-bit format)  
- **Validation**:
  - FSM state transitions
  - Output correctness via waveform inspection (GTKWave)
  - Output addresses and SRAM enable checks

---

## 📊 Synthesis Results

| Metric                     | Value             |
|---------------------------|-------------------|
| Clock Period              | 7 ns              |
| Area                      | 14,895.73 µm²     |
| Convolutions per Cycle    | 4                 |
| Total Compute Cycles      | 5080              |
| Performance Efficiency    | 1.8879 × 10⁻⁹ (ns⁻¹·µm⁻²) |

---

## 📁 Directory Structure

