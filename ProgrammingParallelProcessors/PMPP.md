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

##### Accelerating Applications
If an application takes 10 seconds to execute in system A but takes 200 seconds to execute in System B, the speedup for the execution by system A over system B would be 200/10=20, which is referred to as a 20 times speedup.

**Amdahl's Law**
![[AmdahlsLaw.png]]
The speedup achieved by using parallel computing compared to serial computing depends on how much of the application can run in parallel. For example, if only 30% of the application can be parallelized, even making that part run 100 times faster will only reduce the total execution time by about 29.7%. This means the overall speedup will be about 1.42×. Even if the parallel part becomes infinitely fast, the total speedup will be limited to 1.43×, since 70% of the application still runs sequentially and cannot be improved by parallelization. If 99% of an application's execution time can run in parallel and the parallel part is made 100× faster, the total execution time will shrink to 1.99% of the original, giving an overall speedup of 50× for the entire application.

the importance of a large portion of an application's workload being suitable for parallel execution. If the majority of the workload in an application can be parallelized, a multi-processor parallel computing system can be utilized more effectively, leading to significant performance improvements. In short, the larger the parallelizable portion of an application, the greater the speedup that can be achieved from parallel processors.

The achievable speedup also depends on how quickly data can be accessed and written to memory. Direct parallelization often saturates DRAM bandwidth, limiting speedup to about 10×. Overcoming this requires using GPU on-chip memory to reduce DRAM accesses, but further code optimization is needed to address on-chip memory limitations. Additionally the speedup achieved over a single-core CPU reflects how well the CPU suits the application. Some tasks run efficiently on CPUs, making GPU acceleration harder. It's essential to optimize code so GPUs complement CPU performance, leveraging the strengths of both in heterogeneous computing.

##### Challenges
Many real-world problems are most naturally described with mathematical recurrences. Parallelizing these problems often requires non intuitive ways of thinking about the problem and may require redundant work during execution.

**Memory and Compute Bound Applications:** 
Many applications are memory-bound, meaning their speed is limited by memory access latency/bandwidth or throughput. These require optimization of memory access to achieve better performance. In contrast, compute-bound applications are limited by the number of instructions executed per byte of data. Achieving high-performance parallel execution in memory-bound applications often requires methods for improving memory access speed.

**Sensitivity to Input Data:**
Parallel programs are often more affected by input data characteristics than sequential ones. Problems like varying data sizes or uneven data distributions can lead to an uneven workload across threads, reducing efficiency. These variations can cause significant performance fluctuations. We need to regularize data distributions and/or dynamically refining the number of threads to address these challenges

**Synchronization and Collaboration:** 
Some applications, known as embarrassingly parallel, can be easily parallelized with minimal thread collaboration. Others require significant coordination between threads, using synchronization mechanisms like barriers or atomic operations. These operations add overhead, as threads may need to wait for others, reducing efficiency. 

