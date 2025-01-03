### Parallel Computing

The **latency-oriented design** of CPUs is highly advantageous for sequential operations and workloads that require low parallelism. By reducing execution latency for individual threads, CPUs excel in single-threaded performance and complex tasks involving conditional logic or frequent data accesses. However, this design approach comes with trade-offs: it demands significant chip area and power consumption to support advanced arithmetic units, large caches, and sophisticated control logic. This focus limits the CPU’s ability to handle highly parallel workloads efficiently, as the resources could have been used to include more arithmetic units or memory access channels. 

In contrast, GPUs are designed for parallelism, making them more suitable for workloads involving a large number of threads but less optimized for minimizing latency in individual operations.
![[MpDesign.png]]
Graphics applications require massive floating-point calculations and memory access, with performance often limited by data transfer rates. GPUs excel at handling large data movement in DRAM and leverage a relaxed memory model to support massive parallelism, crucial for high-quality video rendering.

Reducing latency is much more costly than increasing throughput when considering power and chip area. For example, doubling arithmetic throughput requires doubling the number of arithmetic units, which results in a proportional increase in chip area and power consumption. In contrast, halving latency often involves doubling the current, leading to a more than twofold increase in chip area and a fourfold increase in power consumption. Consequently, throughput optimization is generally a more resource-efficient design choice compared to latency reduction.

GPUs allow for longer latencies in their design, enabling savings in chip area and energy. This allows for the inclusion of more hardware units, thereby increasing overall processing capacity. In contrast, CPUs are optimized for low latency, but this comes at the cost of more limited parallelism. These two design approaches offer distinct advantages tailored to different needs.

##### Throughput-Oriented Design
The application software for these GPUs is expected to be written with a large number of parallel threads. The hardware takes advantage of the large number of threads to find work to do when some of them are waiting for long-latency memory accesses or arithmetic operations.  Small cache memories in Fig. 1B are provided to help control the bandwidth requirements of these applications so that multiple threads that access the same memory data do not all need to go to the DRAM. This design style is commonly referred to as throughput-oriented design, as it strives to maximize the total execution throughput of a large number of threads while allowing individual threads to take a potentially much longer time to execute.

##### Compute Unified Device Architecture (CUDA)
GPUs are optimized for parallel, throughput-oriented tasks, while CPUs excel at low-latency, sequential operations. CPUs outperform GPUs in programs with few threads, whereas GPUs excel in programs with many threads. Consequently, many applications leverage both, running sequential tasks on CPUs and computation-intensive tasks on GPUs. 

NVIDIA's CUDA, introduced in 2007, facilitates joint CPU-GPU execution for such applications. The introduction of CUDA unlocked the parallel computing potential of GPUs for a broader developer audience. By removing graphics-based limitations, it enabled the use of GPUs in diverse fields such as scientific computing, artificial intelligence, and image processing. This marked a cornerstone in the widespread adoption of GPUs for general-purpose computing.
