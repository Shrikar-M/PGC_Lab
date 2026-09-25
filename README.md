# Parallel Matrix Multiplication — Performance Study

[![Course](https://img.shields.io/badge/Course-Parallel%20%26%20GPU%20Computing-blue.svg)](#)
[![Workload](https://img.shields.io/badge/Matrix-4000x4000-orange.svg)](#)
[![Technologies](https://img.shields.io/badge/Technologies-OpenMP%20%7C%20MPI%20%7C%20CUDA-green.svg)](#)
[![Status](https://img.shields.io/badge/Status-Completed-brightgreen.svg)](#)

## 📌 Overview

This project presents a performance study of **4000 × 4000 matrix multiplication** using different computing approaches.

The same mathematical workload is implemented using:

* **Sequential CPU execution**
* **OpenMP shared-memory parallelism**
* **MPI distributed-memory processing**
* **CUDA GPU acceleration**

The objective is to understand how different parallel computing architectures affect execution time, speedup, communication overhead, and overall computational performance.

---

## 🎯 Project Objectives

The main goals of this project are:

1. Implement matrix multiplication using sequential and parallel approaches.
2. Compare CPU, multi-threaded, distributed, and GPU-based execution.
3. Verify that every implementation produces the same numerical result.
4. Measure execution time and calculate speedup.
5. Study the overhead introduced by MPI communication and CUDA memory transfers.
6. Understand the practical benefits of different parallel computing models.

---

## 🧮 Problem Definition

For this experiment, two square matrices are used:

**A = 4000 × 4000**

**B = 4000 × 4000**

The resulting matrix is calculated as:

```text
C = A × B
```

Each element of the output matrix is calculated using:

```text
C[i][j] = Σ A[i][k] × B[k][j]
```

Both input matrices contain `1.0` in every position. Therefore, the expected value for every element of `C` is:

```text
C[i][j] = 4000.00
```

This provides a simple and deterministic way to verify correctness across all implementations.

---

## ⚙️ Implemented Approaches

### 1. Sequential CPU

The sequential version performs matrix multiplication using three nested loops.

```text
A → CPU → C
```

* Single-threaded execution
* Traditional `O(N³)` algorithm
* Used as the baseline for speedup calculations

---

### 2. OpenMP

OpenMP is used to distribute matrix multiplication work among multiple CPU threads.

```text
           ┌─ Thread 1
           ├─ Thread 2
A × B ─────┼─ Thread 3 → C
           ├─ Thread 4
           └─ ...
```

The implementation uses OpenMP parallel loops with **8 CPU threads**.

---

### 3. MPI

MPI distributes the computation across multiple processes running on separate Ubuntu virtual machines.

The experiment uses a **4-node/rank setup**:

```text
                Master
                  │
        ┌─────────┼─────────┐
        ↓         ↓         ↓
     Worker1   Worker2   Worker3
```

The communication process includes:

* `MPI_Scatter` — distributes portions of matrix A
* `MPI_Bcast` — sends matrix B to all processes
* `MPI_Gather` — collects the calculated portions of C

Each MPI process handles a portion of the matrix.

---

### 4. CUDA

CUDA is used to execute the matrix multiplication on an NVIDIA GPU.

The computation is organized using CUDA's grid and block execution model:

* **Grid:** 250 × 250 blocks
* **Block:** 16 × 16 threads
* **Logical GPU threads:** 16,000,000

This allows a large number of matrix calculations to be performed concurrently.

---

## 📊 Performance Results

| Approach   | Architecture       |       Resources | Execution Time |  Speedup |
| ---------- | ------------------ | --------------: | -------------: | -------: |
| Sequential | Single CPU         |        1 thread |       244.12 s |    1.00× |
| OpenMP     | Shared-memory CPU  |       8 threads |        30.83 s |    7.92× |
| MPI        | Distributed system |         4 ranks |        92.98 s |    2.63× |
| CUDA       | NVIDIA GPU         | GPU parallelism |        0.165 s | 1479.48× |

All implementations produced the expected verification value:

```text
C[0][0] = 4000.00
```

The recorded benchmark results show the significant performance difference between sequential execution and massively parallel GPU execution.

---

## 📈 Performance Visualization

The repository includes performance graphs comparing:

* Execution time
* Speedup
* CPU vs GPU performance

Add your generated graphs inside the `images/` directory and reference them as follows:

```markdown
![Performance Comparison](images/performance_comparison_charts.png)
```

The original experiment includes separate execution-time and speedup visualizations.

---

## 📁 Project Structure

```text
Parallel-Matrix-Multiplication/
│
├── src/
│   ├── sequential/
│   │   └── matrix_sequential.c
│   │
│   ├── openmp/
│   │   └── matrix_openmp.c
│   │
│   ├── mpi/
│   │   ├── matrix_mpi.c
│   │   └── mpi_send_recv.c
│   │
│   └── cuda/
│       └── matrix_cuda.cu
│
├── images/
│   ├── sequential_result.png
│   ├── openmp_htop.png
│   ├── mpi_ping.png
│   ├── mpi_send_recv.png
│   ├── mpi_result.png
│   ├── performance_comparison_charts.png
│   ├── execution_time_chart.png
│   └── speedup_chart.png
│
└── README.md
```

The source files correspond to the four implementations and the MPI communication test used in the experiment.

---

## 🔬 Performance Analysis

### Sequential Execution

The sequential implementation provides the reference execution time against which the parallel implementations are compared.

### OpenMP Execution

OpenMP reduces execution time by dividing the workload among multiple CPU threads. Since the threads share the same memory space, communication between threads is relatively straightforward.

### MPI Execution

MPI allows the workload to be distributed across multiple machines or virtual machines. However, communication between nodes introduces additional overhead through operations such as scattering, broadcasting, and gathering data.

### CUDA Execution

CUDA provides massive parallelism by assigning matrix calculations to a large number of GPU threads. For this compute-intensive workload, this results in a substantial reduction in execution time.

---

## 📐 Performance Metrics

### Speedup

Speedup is calculated using:

```text
Speedup = Sequential Execution Time / Parallel Execution Time
```

### Parallel Efficiency

```text
Efficiency = (Speedup / Number of Processing Units) × 100
```

These metrics are used to quantify the benefit obtained from parallel execution.

---

## 🧪 Correctness Verification

Since both input matrices contain only `1.0`, every element of the resulting matrix should be:

```text
4000.00
```

The implementations were checked using:

```text
C[0][0] = 4000.00
```

Matching verification values across the implementations demonstrate that the different execution models perform the same mathematical operation correctly.

---

## 🛠️ Technologies Used

| Technology | Purpose                                     |
| ---------- | ------------------------------------------- |
| C          | Sequential and parallel CPU implementations |
| OpenMP     | Shared-memory CPU parallelism               |
| MPI        | Distributed-memory computation              |
| CUDA       | GPU acceleration                            |
| Ubuntu     | MPI virtual-machine environment             |
| NVIDIA GPU | CUDA execution                              |
| `htop`     | CPU utilization monitoring                  |

---

## 🚀 Key Takeaways

* Matrix multiplication is highly suitable for parallel computing.
* OpenMP can significantly reduce execution time on multi-core CPUs.
* MPI enables distributed computation across multiple systems.
* MPI performance can be affected by network communication overhead.
* CUDA can exploit massive GPU parallelism for compute-intensive workloads.
* Using the same input and verification condition makes cross-platform correctness testing straightforward.

---

## 📸 Experiment Evidence

The repository contains screenshots demonstrating:

* Sequential execution
* OpenMP CPU utilization
* MPI network connectivity
* MPI message passing
* MPI matrix multiplication
* Performance comparison graphs

These provide experimental evidence for the measured results.

---

## 👨‍💻 Project Information

**Project:** Parallel Matrix Multiplication — Performance Analysis
**Course:** Parallel & GPU Computing
**Workload:** 4000 × 4000 Matrix Multiplication
**Approaches:** Sequential, OpenMP, MPI, CUDA
**Status:** Completed

---

## 📄 Conclusion

This project demonstrates how the same computational problem can behave differently under sequential, shared-memory, distributed-memory, and GPU-based execution models.

By implementing and benchmarking all four approaches, the experiment provides a practical understanding of **parallelism, scalability, communication overhead, GPU acceleration, and performance measurement**.

The results confirm the importance of selecting an appropriate computing architecture based on the nature of the workload and available hardware resources.
