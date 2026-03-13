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
