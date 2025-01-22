# Time-Energy-Compiler-Choice-Article  

This repository contains experimental data for the article: **"Comparison of LU and WZ Matrix Factorization Algorithms in Terms of Performance and Energy Consumption on Intel and AMD Processors Using OneAPI and Clang Compilers."**  

---

## Overview  

Testing was conducted on two systems:  
1. **Intel Xeon Platinum 8358**: Two processors, each with 32 cores.  
2. **AMD EPYC 9654**: Two processors, each with 96 cores.  

The evaluation focused on two multithreaded factorization algorithms:  
- **LU factorization without pivoting**: Implemented using the `dgetrfnpi` function from the MKL library.  
- **WZ factorization**: Parallelized and vectorized using OpenMP pragmas, without employing data blocking.  

---

## Directory Structure  

The results are organized into directories based on the processor and algorithm:  

- `IntelXeon`  
  - `LU`: Results for LU factorization.  
  - `WZ`: Results for WZ factorization.  

- `AMDEPYC`  
  - `LU`: Results for LU factorization.  
  - `WZ`: Results for WZ factorization.  

Within each algorithm directory, there are subdirectories for results generated using Clang and OneAPI compilers with additional flags.  

### Naming Convention  

Subdirectory names follow this structure:  
`<Algorithm>_<Compiler>_<Flags>`  

- `<Algorithm>`: `LU` or `WZ`  
- `<Compiler>`: `clang` or `icx` (for OneAPI)  
- `<Flags>`: Compiler flags applied (e.g., `march`, `funrollloop`).  

**Examples:**  
- `LU_icx`: LU factorization compiled with OneAPI using OpenMP support and the `-O3` flag.  
- `LU_icx_march`: LU factorization compiled with OneAPI using `-O3` and `-march=native`.  

Each configuration was tested five times, with results stored in subdirectories labeled `1`, `2`, `3`, `4`, and `5`.  

---

## Compiler Flags  

The table below summarizes the additional compiler flags used:  

| Name           | OneAPI Flags                   | Clang Flags                    |  
|----------------|--------------------------------|--------------------------------|  
| `funrollloop`  | `-funroll-loops`              | `-funroll-loops`              |  
| `march`        | `-march=native`              | `-march=native`              |  
| `march-unroll` | `-march=native -funroll-loops`| `-march=native -funroll-loops`|  
| `profile`      | `-prof-gen=srcpos`, `-prof-use` | `-fprofile-generate`, `-fprofile-use=program.profdata` |  

---

## Data Files  

Each experiment directory contains the following files:  

1. **`energy`**:  
   - Records energy consumption data measured every second during algorithm execution and includes data for five iterations of the current configuration.
   - Includes results for 64 threads (Intel Xeon) and 96 threads (AMD EPYC) for a matrix size of \( n \times n \), where \( n = 32,768 \).  
   - Columns:  
     - `n`: Sequence number.  
     - `timestamp`: Measurement timestamp.  
     - Energy and power metrics for each processor:  
       - Processor 1: `:0/package-0/CumulEn[J]`, `:0/package-0/InstPow[W]`, memory: `:0:0/dram/CumulEn[J]`, `:0:0/dram/InstPow[W]`.  
       - Processor 2: Similar metrics.  

2. **`t_96_n_32768__S`**:  
   - Contains start and stop timestamps for algorithm execution.  

3. **`t_96_n_32768__T`**:  
   - Records the total runtime of the algorithms.
  
4. **`t_96_n_32768__P`**:
   - performance statistics measured during the execution of the algorithm with the tool:`-perf stat`.
