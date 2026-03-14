# 跨架构高性能计算基准测试报告
# Cross-Architecture HPC Benchmark Report (HS Algorithm)

## 1. 实验概述 / Overview
本项目对比了 2026 年主流硬件在执行共轭梯度 HS 算法（Hestenes-Stiefel）时的实际性能表现。重点评估 $O(n^3)$ 复杂度的矩阵构造阶段。
This project evaluates the performance of mainstream hardware in 2026 when executing the Hestenes-Stiefel (HS) CG algorithm, focusing on the $O(n^3)$ matrix construction phase.

## 2. 硬件阵列 / Hardware Setup
| 节点 (Node) | 处理器 (Processor) | 架构 (Arch) | 核心/线程 (Cores/Threads) |
| :--- | :--- | :--- | :--- |
| **Raspberry Pi 5** | Broadcom BCM2712 | **ARMv8** | 4 Cores / 4 Threads |
| **Galera Server** | Intel Xeon E5-2697 v2 | **x86_64** | 12 Cores / 24 Threads |
| **GCP Cloud** | AMD EPYC 7B12 | **x86_64** | 1 Core (vCPU) |

## 3. 测试结果 (n=2000) / Benchmark Results
> 任务：80 亿次浮点运算（G^T * G 矩阵构造）
> Task: 8 Billion Floating-point Operations (G^T * G Construction)

| 排名 (Rank) | 节点 (Node) | 耗时 (Time) | 性能状态 (Status) |
| :--- | :--- | :--- | :--- |
| **1st** | **Raspberry Pi 5** | **13.26s** | 🚀 Champion |
| **2nd** | **Galera Server** | **26.90s** | 🥈 Legacy Power |
| **3rd** | **Google Cloud (GCP)** | **195.61s** | 🥉 Single Core Baseline |

## 4. 技术分析 / Technical Analysis

### 中文版 (Chinese)
1. **内存带宽红利**：树莓派 5 凭借 LPDDR4X 高频内存，在内存密集型任务中显著超越了旧款服务器（DDR3）。
2. **架构代差**：2013 年代的 24 个逻辑核心由于架构陈旧和“内存墙”效应，在处理现代高密度并行运算时效率不及现代 ARM 核心。
3. **单核 IPC 验证**：GCP 展现了现代 Zen 2 核心的强大单核素质，但在缺乏并行支持的情况下，无法应对 $O(n^3)$ 级的计算压力。

### English Version
1. **Memory Bandwidth Dividend**: With high-frequency LPDDR4X, Raspberry Pi 5 significantly outperforms legacy servers (DDR3) in memory-intensive tasks.
2. **Architectural Gap**: The 24 logic cores from 2013 failed to show scale advantage against modern ARM cores due to the "Memory Wall" and outdated instruction efficiency.
3. **Single-core IPC**: GCP (Zen 2) demonstrates strong single-core performance, but cannot handle $O(n^3)$ complexity effectively without multi-threading.

## 5. 结论 / Conclusion
实验证明，现代高性能 ARM 处理器在特定科学计算场景下，其能效比和执行速度已具备挑战旧世代 HPC 集群的能力。
The experiment proves that modern high-performance ARM processors are capable of challenging legacy HPC clusters in specific scientific computing scenarios.

---
**T** **Date: 2026-03-13**

### 🚀 n=4000 Stress Test: Architecture vs. Throughput 
### 🚀 n=4000 极限压测：架构效率与硬件吞吐量的博弈

To further investigate the boundary between **Architecture Efficiency (IPC)** and **Hardware Throughput (Cores/Bandwidth)**, we increased the matrix dimension to $n=4000$. This involves ~64 billion floating-point operations and handles ~256MB of data, completely exceeding the L3 cache of all tested CPUs.

为了深入探究 **架构效率 (IPC)** 与 **硬件吞吐量 (核心/带宽)** 的博弈边界，我们将矩阵维度提升至 $n=4000$。该测试包含约 640 亿次浮点运算，数据量达到 ~256MB，彻底击穿了所有测试 CPU 的三级缓存 (L3 Cache)。

#### 1. Experimental Data | 实验数据实测

| Platform (平台) | Cores (核心数) | Load (负载) | Time (耗时) | Characterization (性能特征) |
| :--- | :--- | :--- | :--- | :--- |
| **School Galera** | 12x Ivy Bridge | 1137% | **197.06s** | High throughput wins in heavy tasks. (大任务下吞吐量制胜) |
| **Raspberry Pi 5** | 4x Cortex-A76 | 400% | **526.31s** | Superior IPC but limited by core count. (单核素质极高，但受限于核心总数) |
| **Google Cloud** | 1x AMD EPYC | 100% | *3580.37s* | Single-thread baseline performance. (现代架构单核性能基准) |

#### 2. Key Findings | 核心发现

* **Performance Inversion (性能反转)**: At $n=2000$, Pi 5 was the winner due to modern architecture. However, at $n=4000$, the **School Server (12-core)** reclaimed its dominance by leveraging massive multi-core throughput.
  在 $n=2000$ 时，树莓派 5 凭借架构优势领先；但在 $n=4000$ 时，拥有 12 个物理核心的学校服务器凭借更高的并行计算总量重新夺回了统治地位。

* **The Memory Wall (内存墙效应)**: As data size grows to 256MB, memory bandwidth becomes the primary bottleneck. The server's 12 cores, despite being older (DDR3), provide higher aggregate data throughput compared to the Pi 5's 4 cores. 
  随着数据规模增长，内存带宽成为主要瓶颈。学校服务器的 12 个核心虽然架构较老 (DDR3)，但其搬运数据的总吞吐量依然超过了树莓派 5 的 4 个核心。

* **Conclusion (结论)**: Architecture matters for agility, but **Scale** matters for heavy lifting.
  架构决定了小任务的灵活性，而 **规模（核心数）** 决定了大任务的硬实力。

---
**Updated on 2026-03-13**
