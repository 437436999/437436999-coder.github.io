---
layout: post
title: GPGPU,CUDA&OpenCL
date: 2020-07-01
author: Max.C
header-img: 'assets/img/pro60.jpg'
catalog: true
tags: 并行与分布式
---



## 零、GPGPU

> 通用图形处理器（General-purpose computing on graphics processing units，简称GPGPU），是一种利用处理图形任务的图形处理器来计算原本由中央处理器处理的通用计算任务。这些通用计算常常与图形处理没有任何关系。由于现代图形处理器强大的并行处理能力和可编程流水线，令流处理器可以处理非图形数据。特别在lin面对单指令流多数据流（SIMD），且数据处理的运算量远大于数据调度和传输的需要时，通用图形处理器在性能上大大超越了传统的中央处理器应用程序。

streaming multiprocessor流多处理器（block）：一系列SIMD处理单元。

## 一、CUDA

为跨多种处理器的数据并行编程提供内在**可扩展**的环境(不过，NVIDIA 只制造 GPU)。

使 SIMD 硬件对通用程序员可访问。否则，大部分可用的执行硬件都会被浪费掉。

- SPMD 独立执行同一程序的多个实例，其中每个程序处理数据的不同部分。

- 对于数据并行的科学和工程应用，将 SPMD 与循环条带挖掘相结合是一种非常常见的并行编程技术。
  - 消息传递接口(MPI)用于在分布式集群上运行 SPMD。
  - POSIX 线程(p 线程)用于在共享内存系统上运行 SPMD。
  - 内核在 GPU 中运行 SPMD
- 在向量相加的示例中，每个数据块都可以作为一个独立的线程执行。
- 在现代 CPU 上，创建线程的开销是如此之高，以至于块需要很大。
  - 在实践中，通常有几个线程(大约相当于 CPU 核心的数量)，每个线程都有大量的工作要做。
- 对于 GPU 编程，线程创建的开销较低，因此我们可以在每次循环迭代中创建一个线程。

### Scalability（可扩展性）

- CUDA 表示许多独立的计算模块，可以按任何顺序运行；
- CUDA 编程模型的许多固有可伸缩性源于「线程块」的成批执行；
- 在同一代图形处理器之间，许多程序在具有更多「核心」的图形处理器上实现线性加速；(线性加速比)



### 特点

- CUDA 编程模型提供了一种为异构、分层平台组织数据并行程序的通用方法。
- 目前，唯一的产品质量实现是在 NVIDIA 的 GPU 上实现用于 C/C++的 CUDA。
- 但 CUDA 的「标量线程」、「扭曲」、「块」和「网格」等概念也可以映射到其他平台。
- 对于 CUDA 编程，一种简单的「同质 SPMD」方法是有用的，特别是在实施和调试的早期阶段。
- 但是要实现高效率，需要仔细考虑从计算到处理器、从数据到存储器和数据访问模式的映射。



### CUDA编程

CUDA 的操作概括来说包含 5 个步骤：

1. CPU 在 GPU 上分配内存：cudaMalloc；
2. CPU 把数据发送到 GPU：cudaMemcpy；
3. CPU 在 GPU 上启动内核（kernel），它是自己写的一段程序，在每个线程上运行；
4. CPU 把数据从 GPU 取回：cudaMemcpy；
5. CPU 释放 GPU 上的内存：cudaFree。

`_globle_`后面为kernel函数：GPU调度的基本单元，CPU可访问。

`_device_`函数只能由GPU访问/变量在GPU中为全局变量。

`_shared_`变量仅在一个block中。

一个 kernel 结构如下：`KernelFunc<<Dg, Db, Ns, S>>(param1, param2, ...)`

- Dg：grid 的尺寸，说明一个 grid 含有多少个 block，为 dim3 类型，一个 grid 最多含有 65535*65535*65535 个 block，Dg.x，Dg.y，Dg.z 最大值为 65535；
- Db：block 的尺寸，说明一个 block 中含有多少个 thread，为 dim3 类型，一个 block 最多含有 1024(cuda2.x 版本)个 threads，Db.x 和 Db.y 最大值为 1024，Db.z 最大值 64； （举个例子，一个 block 的尺寸可以是：`1024*1*1 | 256*2*2 | 1*1024*1 | 2*8*64 | 4*4*64`等）
- Ns：可选参数，如果 kernel 中由动态分配内存的 shared memory，需要在此指定大小，以字节为单位；
- S：可选参数，表示该 kernel 处在哪个流当中。

线程定位：threadId、blockId、blockDim

#### SIMD Programming

- 硬件架构师喜欢 SIMD，因为它支持非常节省空间和节能的实施。
- 然而，CPU 上的标准 SIMD 指令是不灵活的，很难使用，编译器很难找到目标。
- CUDA 线程抽象将以额外硬件为代价提供可编程性



### CUDA 的线程分层结构

Cuda 对线程做了合适的规划，引入了 grid 和 block 的概念，block 由线程组成，grid 由 block 组成，一般说 block_size 指一个 block 放了多少 thread；grid_size 指一个 grid 放了多少个 block。

Cuda 编程模型中的并行性被表示为一个 4 层的层次结构。

- 流(Stream)是按顺序执行的 Grid 列表。Fermi GPU 并行执行多个流。
- 网格(Grid)是一组最多 $2^{32}$ 个执行同一内核的线程块。(执行指令相同)
- 线程块(block)是一组最多 1024（512 每费米）Cuda 线程。
- 每个 cuda 线程(thread)都是一个独立的、轻量级的标量执行上下文。
- 32 个线程组成的组形(warp)成在 lockStepSIMD 中执行的扭曲。

#### CUDA Thread

- 逻辑上，每个 CUDA 线程：
  - 有自己的控制流程和 PC，注册文件，调用堆栈...
  - 可以随时访问任何 GPU 全局内存地址。
  - 可通过以下五个整数在网格中唯一标识：线程标识。{x，y，z}，块标识。{x，y}。
- 非常精细的粒度：不要指望任何一个线程都能完成昂贵的计算中的很大一部分。
  - 在完全占用时，每个线程有 21 个 32 位寄存器。
  - 1536 个线程共享 64 KB L1 缓存/_SHARED_mem。
  - GPU 没有操作数绕过网络：功能单元延迟必须通过多线程或 ILP 隐藏(例如，从循环展开)

#### CUDA Warp

- The Logical SIMD Execution width of the CUDA processor
- A group of 32 CUDA Threads that execute simultaneously – Execution hardware is most efficiently utilized when all threads in a warp execute instructions from the same PC. – If threads in a warp diverge (execute different PCs), then some execution pipelines go unused (predication) – If threads in a warp access aligned, contiguous blocks of DRAM, the accesses are coalesced ( 合并 ) into a single high bandwidth access – Identifiable uniquely by dividing the Thread Index by 32
- Technically, warp size could change in future architectures – But many existing programs would break
- CUDA 处理器的逻辑 SIMD 执行宽度。
- 一组同时执行的 32 个 CUDA 线程。
  - 当扭曲中的所有线程从同一台 PC 执行指令时，执行硬件的使用效率最高。
  - 如果扭曲中的线程发散(执行不同的 PC)，则某些执行管道将未使用(预测)。
  - 如果扭曲访问中的线程对齐了 DRAM 的连续块，则将访问合并(合并)为单个高带宽访问。
  - 通过将线程索引除以 32 来唯一标识。
- 从技术上讲，Warp 的大小在未来的体系结构中可能会发生变化
  - 但许多现有的程序将会中断。

#### Cuda Thread Block

- A Thread Block is a virtualized multi-threaded core
  - Number of scalar threads, registers, and __shared memory are configured dynamically at kernel-call time
  - Consists of a number (1-1024) of Cuda Threads, who all share the integer identifiers blocladx. {x, y}
- …executing a data parallel task of moderate granularity
  - The cacheable working-set should fit into the 128 KB (64 KB, pre-Fermi) Register File and the 64 KB (16 KB) Li
  - Non-cacheable working set limited by GPU DRAM capacity
  - All threads in a block share a (small) instruction cache
- Threads within a block synchronize via barrier-intrinsics and communicate via fast, on-chip shared memory
- 线程块是虚拟化的多线程内核。
  - 在内核调用时动态配置标量线程、寄存器和_共享内存的数量。
  - 由 CudaThread 的一个数字(1-1024)组成，这些线程共享整数标识符 blocladx。{x，y}。
- …。执行中等粒度的数据并行任务。
  - 可缓存工作集应适合 128KB(64KB，Pre-Fermi)注册表文件和 64KB(16KB)LI。
  - 受 GPU DRAM 容量限制的不可缓存工作集。
  - 块中的所有线程共享(小型)指令缓存。
- 块中的线程通过屏障-内部同步，并通过快速的片上共享内存进行通信

#### Cuda Grid

- A set of Thread Blocks performing related computations
  - All threads in a single kernel call have the same entry point and function arguments, initially differing only in blockIdx. {x,y}
  - Thread blocks in a grid may execute any code they want, e.g. switch (blockIdx.x) incurs no extra penalty
- Performance portability/scalability requires many blocks per grid: 1-8 blocks execute on each SM
- Thread blocks of a kernel call must be parallel sub-tasks
  - Program must be valid for any interleaving of block executions
  - The flexibility of the memory system technically allows Thread Blocks to communicate and synchronize in arbitrary ways
  - E.G. Shared Queue index: OK! Producer-Consumer: RISKY!
- 一组执行相关计算的线程块。
  - 单个内核调用中的所有线程都具有相同的入口点和函数参数，最初仅在 block Idx 中有所不同。{x，y}。
  - 网格中的线程块可以执行它们想要的任何代码，例如 Switch(block Idx.x)不会受到额外的惩罚。
- 性能、可移植性/可扩展性要求每个网格有多个数据块：在每个 SM 上执行 1-8 个数据块。
- 内核调用的线程块必须是并行子任务。
  - 程序必须对块执行的任何交错有效。
  - 内存系统的灵活性在技术上允许线程块以任意方式进行通信和同步。
  - 共享队列索引：好的！生产者-消费者：风险！

#### Cuda Stream

- A sequence of commands (kernel calls, memory transfers) that execute in order.
- For multiple kernel calls or memory transfers to execute concurrently, the application must specify multiple streams. – Concurrent Kernel execution will only happen on Fermi and later – On pre‐Fermi devices, Memory transfers will execute concurrently with Kernels.

### CUDA 的内存分层结构

- Each CUDA Thread has private access to a configurable number of registers – The 128 KB (64 KB) SM register file is partitioned among all resident threads – Registers, stack spill into (cached, on Fermi) 「local」 DRAM if necessary
- Each Thread Block has private access to a configurable amount of scratchpad memory – The Fermi SM’s 64 KB SRAM can be configured as 16 KB L1 cache + 48 KB scratchpad (暂存器), or vice‐versa* – Pre‐Fermi SM’s have 16 KB scratchpad only – The available scratchpad space is partitioned among resident thread blocks, providing another concurrency‐state tradeoff
- Thread blocks in all Grids share access to a large pool of 「Global」 memory, separate from the Host CPU’s memory. – Global memory holds the application’s persistent state, while the thread‐local and block‐local memories are temporary – Global memory is much more expensive than on‐chip memories: O(100)x latency, O(1/50)x (aggregate) bandwidth
- On Fermi, Global Memory is cached in a 768KB shared L2
- There are other read-only components of the Memory Hierarchy that exist due to the Graphics heritage of Cuda
- The 64 KB Cuda Constant Memory resides in the same DRAM as global memory, but is accessed via special read-only 8 KB per-SM caches
- The Cuda Texture Memory also resides in DRAM and is accessed via small per-SM read-only caches, but also includes interpolation hardware
- This hardware is crucial for graphics performance, but only occasionally is useful for general-purpose workloads
- The behaviors of these caches are highly optimized for their roles in graphics workloads ？
- Each CUDA device in the system has its own Global memory, separate from the Host CPU memory – Allocated via cudaMalloc()/cudaFree() and friends
- Host <-> Device memory transfers are via cudaMemcpy() over PCI‐E, and are extremely expensive – microsecond latency, ~GB/s bandwidth
- Multiple Devices managed via multiple CPU threads
- 每个 CUDA 线程都可以私有地访问可配置的寄存器数量-128KB(64KB)SM 寄存器文件在所有驻留线程中进行分区-寄存器、堆栈溢出到(缓存的，在 Fermi 上)「本地」DRAM(如有必要)。
- 每个线程块都可以私有地访问可配置数量的暂存盘内存-费米 SM 的 64KB 内存可以配置为 16KB L1 缓存+48KB 暂存板(暂存器)，反之亦然*-Pre-FermiSM 仅有 16KB 暂存板-可用暂存板空间在常驻线程块之间进行分区，从而提供另一个并发状态权衡。
- 所有网格中的线程块共享对大型「全局」内存池的访问权限，这些内存池与主机 CPU 的内存分开。-全局内存保存应用程序的持久状态，而线程本地和块本地内存是临时的-全局内存比片上内存昂贵得多：O(100)x 延迟，O(1/50)x(聚合)带宽。
- 在 Fermi 上，全局内存缓存在 768KB 共享 L2 中。
- 由于 CUDA 的图形遗产，内存层次结构中存在其他只读组件。
- 64 KB cuda 常量内存与全局内存驻留在同一 DRAM 中，但通过特殊的只读 8 KB/SM 缓存进行访问。
- cuda 纹理内存也驻留在 dram 中，可以通过小的 per-sm 只读缓存进行访问，但也包括插值硬件。
- 此硬件对于图形性能至关重要，但仅偶尔对通用工作负载有用。
- 这些缓存的行为是否针对其在图形工作负载中的角色进行了高度优化？
- 系统中的每个 CUDA 设备都有自己的全局内存，独立于主机 CPU 内存-通过 cudaMalloc()/cudafree()和 Friend()分配。
- 主机<->设备内存传输是通过 cudaMemcpy()通过 PCI-E 进行的，并且是非常昂贵的-微秒延迟，~Gb/s 带宽。
- 通过多个 CPU 线程管理多个设备

## 二、OpenCL

> OpenCL（全称Open Computing Language，开放运算语言）是第一个面向异构系统通用目的并行编程的开放式、免费标准，也是一个统一的编程环境，便于软件开发人员为高性能计算服务器、桌面计算系统、手持设备编写高效轻便的代码，而且广泛适用于多核心处理器(CPU)、图形处理器(GPU)、Cell类型架构以及数字信号处理器(DSP)等其他并行处理器，在游戏、娱乐、科研、医疗等各种领域都有广阔的发展前景。





## 参考资料



