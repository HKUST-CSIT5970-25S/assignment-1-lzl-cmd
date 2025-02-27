[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/IAASVEAZ)
# CSIT5970 Assignment-1: EC2 Measurement (2 questions, 4 marks)

### Deadline: 11:59PM, Feb, 28, Friday

---

### Name:  Liao Ziliang
### Student Id: 21136618
### Email: zliaoaq@connect.ust.hk

---

## Question 1: Measure the EC2 CPU and Memory performance

1. (1 mark) Report the name of measurement tool used in your measurements (you are free to choose *any* open source measurement software as long as it can measure CPU and memory performance). Please describe your configuration of the measurement tool, and explain why you set such a value for each parameter. Explain what the values obtained from measurement results represent (e.g., the value of your measurement result can be the execution time for a scientific computing task, a score given by the measurement tools or something else).

    > **Measurement Tool:**
    > I used **Phoronix Test Suite** for performance measurements.
    >
    > **CPU Performance Test:**
    >
    > - **Test Name:** 7-Zip Compression
    > - **Configuration:** Default parameters were used.
    > - **Trial Runs:** The estimated trial run count was set to 3.
    > - **Results:**
    >   - **Compression Rating:** The result is given in MIPS (Millions of Instructions Per Second). This value represents the CPU's performance in compression tasks, indicating how many million instructions the CPU can execute per second during compression.
    >   - **Decompression Rating:** Similarly, this value (also in MIPS) represents the CPU's performance in decompression tasks.
    >
    > **Memory Performance Test:**
    >
    > - **Test Name:** RAMspeed SMP 3.5.0
    > - **Configuration:**
    >   - **Memory Test Type:** Copy
    >   - **Benchmark Type:** Integer
    >   - **Trial Runs:** The estimated trial run count was set to 3.
    > - **Results:**
    >   - **Average Bandwidth:** The result is given in MB/s (Megabytes per Second). This value represents the memory bandwidth during the "Copy" operation for integer data, indicating how much data the memory can transfer per second.
    >
    > **Explanation of Parameter Values:**
    >
    > - **Memory Test Type (Copy):** The "Copy" test measures the memory's ability to copy data from one location to another. This is a fundamental operation that reflects real-world memory performance in tasks involving data duplication.
    > - **Benchmark Type (Integer):** The "Integer" benchmark focuses on integer data operations, which are common in general-purpose computing and provide a clear indication of memory performance for non-floating-point tasks.
    > - **Trial Run Count (3):** Three trial runs were performed to ensure the reliability and consistency of the results, reducing the impact of any outliers or temporary system fluctuations.
    >
    > **Interpretation of Results:**
    >
    > - **Average Bandwidth (MB/s):** This value reflects the memory's data transfer speed during the "Copy" operation for integer data. Higher values indicate faster memory performance, which is critical for applications that rely heavily on data movement and duplication.
    >
    > 

2. (1 mark) Run your measurement tool on general purpose `t2.micro`, `t2.medium`, and `c5d.large` Linux instances, respectively, and find the performance differences among these instances. Launch all the instances in the **US East (N. Virginia)** region. Does the performance of EC2 instances increase commensurate with the increase of the number of vCPUs and memory resource?

    In order to answer this question, you need to complete the following table by filling out blanks with the measurement results corresponding to each instance type.

    
    
    CPU: Compression Rating and Decompression Rating
    
    | Size        | CPU performance(MIPS) | Memory performance |
    | ----------- | --------------- | ------------------ |
    | `t2.micro` | 3662/3072 | 10690.91 MB/s |
    | `t2.medium`  | 9807/5820 | 19489.03 MB/s |
    | `c5d.large` | 8056/5231 | 15020.33 MB/s |
    
    Ans: 
    The performance trend shows that increasing vCPUs and memory improves CPU and memory performance (e.g., `t2.micro` to `t2.medium`). However, with the same resources (`t2.medium` to `c5d.large`), performance may decrease due to instance type or architecture differences.
    
    > Region: US East (N. Virginia). Use `Ubuntu Server 22.04 LTS (HVM)` as AMI.

## Question 2: Measure the EC2 Network performance

1. (1 mark) The metrics of network performance include **TCP bandwidth** and **round-trip time (RTT)**. Within the same region, what network performance is experienced between instances of the same type and different types? In order to answer this question, you need to complete the following table.

    | Type                      | TCP b/w (Mbps) | RTT (ms) |
    | ------------------------- | -------------- | -------- |
    | `t3.medium` - `t3.medium` | 3790           | 0.287    |
    | `m5.large` - `m5.large`   | 9460           | 0.101    |
    | `c5n.large` - `c5n.large` | 4940           | 0.171    |
    | `t3.medium` - `c5n.large` | 2420           | 0.602    |
    | `m5.large` - `c5n.large`  | 4950           | 0.173    |
    | `m5.large` - `t3.medium`  | 2720           | 0.556    |

    > Region: US East (N. Virginia). Use `Ubuntu Server 22.04 LTS (HVM)` as AMI. Note: Use private IP address when using iPerf within the same region. You'll need iPerf for measuring TCP bandwidth and Ping for measuring Round-Trip time.

Ans: 

Within the same region, network performance varies significantly between instances of the same type and different types. Instances of the same type, such as m5.large - m5.large, achieve higher TCP bandwidth (9460 Mbps) and lower RTT (0.101 ms), indicating optimized communication. In contrast, instances of different types, like t3.medium - c5n.large, show reduced bandwidth (2420 Mbps) and higher latency (0.602 ms), likely due to architectural or resource differences. This suggests that homogeneous instance pairs generally perform better than heterogeneous pairs.

2. (1 mark) What about the network performance for instances deployed in different regions? In order to answer this question, you need to complete the following table.

    | Connection                | TCP b/w (Mbps) | RTT (ms) |
    | ------------------------- | -------------- | -------- |
    | N. Virginia - Oregon      | 32.2           | 62.856   |
    | N. Virginia - N. Virginia | 4900           | 0.241    |
    | Oregon - Oregon           | 7740           | 0.101    |
    
Instances deployed in different regions (e.g., N. Virginia - Oregon) experience significantly lower TCP bandwidth (32.2 Mbps) and higher RTT (62.856 ms) compared to instances within the same region (e.g., N. Virginia - N. Virginia: 4900 Mbps, 0.241 ms), indicating higher latency and reduced performance due to geographical distance.

    > Region: US East (N. Virginia), US West (Oregon). Use `Ubuntu Server 22.04 LTS (HVM)` as AMI. All instances are `c5.large`. Note: Use public IP address when using iPerf within the same region.
