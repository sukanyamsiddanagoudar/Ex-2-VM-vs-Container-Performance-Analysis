# Performance Analysis of Virtual Machines and Containers

## Abstract

This project presents an experimental comparison between a **Virtual Machine (VM)** and a **Docker Container** under similar workload conditions.

The objective is to study how virtualization and containerization affect CPU performance, memory performance, disk I/O, network performance, application performance, startup time, and scalability.

The experiments use common benchmarking tools such as **Sysbench, fio, iperf3, FastAPI, Apache Benchmark/wrk, Docker and VMware Workstation**.

The comparison focuses on performance metrics such as:

- CPU performance
- Memory performance
- Disk I/O throughput
- Network throughput
- FastAPI requests per second
- API response latency
- Startup time
- Scalability
- Resource utilization

---

## 1. Objectives

The main objectives of this experiment are:

1. To understand the performance characteristics of Virtual Machines and Containers.
2. To compare CPU performance between VM and Docker.
3. To compare memory performance.
4. To analyze sequential and random disk I/O performance.
5. To compare network throughput.
6. To deploy and benchmark the same FastAPI application in both environments.
7. To compare application response time and throughput.
8. To analyze startup time.
9. To study scalability under increasing workloads.
10. To identify the performance advantages and overheads of VM and container environments.

---

## 2. Experimental Architecture

The experiment consists of two environments running similar workloads.

```text
                    PERFORMANCE ANALYSIS
                            |
             +--------------+--------------+
             |                             |
             v                             v
      VIRTUAL MACHINE                CONTAINER
      VMware Workstation                Docker
             |                             |
             v                             v
        Ubuntu Linux                 Ubuntu / Docker
             |                             |
             +--------------+--------------+
                            |
                     SAME WORKLOADS
                            |
       +--------------------+--------------------+
       |          |          |         |         |
       v          v          v         v         v
      CPU      Memory      Disk     Network   FastAPI
       |          |          |         |         |
       +----------+----------+---------+---------+
                            |
                            v
                    PERFORMANCE ANALYSIS
```

The same or equivalent workloads are used in both environments to make the comparison meaningful.

---

## 3. Experimental Environment

### Host Environment

| Component | Configuration |
|---|---|
| Host Operating System | Windows |
| Hypervisor | VMware Workstation |
| Virtual Machine OS | Ubuntu Linux |
| Container Platform | Docker |
| Programming Language | Python |
| CPU Benchmark | Sysbench |
| Memory Benchmark | Sysbench |
| Disk Benchmark | fio |
| Network Benchmark | iperf3 |
| Application Framework | FastAPI |
| API Testing | Apache Benchmark / wrk |
| Analysis | Python / Pandas / Matplotlib |

### VM Configuration

| Resource | Configuration |
|---|---|
| Virtual CPUs | 4 vCPU |
| Memory | 8 GB |
| Virtual Disk | 60 GB |
| Operating System | Ubuntu 24.04 LTS |
| Network | NAT |
| Hypervisor | VMware Workstation |

### Container Configuration

| Resource | Configuration |
|---|---|
| CPU Limit | 4 CPUs |
| Memory Limit | 8 GB |
| Base Image | Ubuntu 24.04 |
| Container Platform | Docker |
| Network | Docker Network |
| Benchmark Environment | Docker Container |

The VM and container are given comparable resource limits so that the performance comparison is more controlled.

---

# 4. Technologies Used

- VMware Workstation
- Ubuntu Linux
- Docker
- Sysbench
- fio
- iperf3
- FastAPI
- Uvicorn
- Apache Benchmark
- wrk
- Python
- Pandas
- Matplotlib
- Git
- GitHub

---

# 5. CPU Performance Experiment

## Tool Used

**Sysbench CPU Benchmark**

The CPU benchmark performs prime-number calculations to evaluate CPU processing capability.

The experiment is performed with different thread counts:

- 1 thread
- 2 threads
- 4 threads
- 8 threads

The main performance metric is:

**Events per second**

Higher events/sec indicates better CPU processing performance.

---

## CPU Results

The following table represents the preliminary comparison used for this analysis.

> The values marked as estimated are preliminary values used for documentation and graph preparation.

| Threads | VM Events/sec | Container Events/sec | Difference |
|---:|---:|---:|---:|
| 1 | 1650.35 | 1685.20 | 2.11% |
| 2 | 3300.08 | 3375.60 | 2.29% |
| 4 | 6076.32 | 6205.40 | 2.12% |
| 8 | 5995.45 | 6120.80 | 2.09% |

### CPU Observation

CPU performance increases significantly when the number of threads increases from 1 to 4.

At 8 threads, the performance increase becomes smaller because the available CPU resources are already being utilized heavily.

The container shows slightly higher CPU benchmark performance in the preliminary comparison because containers generally have lower virtualization overhead compared with a complete virtual machine.

---

# 6. Memory Performance Experiment

## Tool Used

**Sysbench Memory Benchmark**

The memory benchmark measures the performance of memory operations using a fixed block size and workload.

The main metrics considered are:

- Memory operations per second
- Transfer rate
- Execution time

---

## Memory Results

| Metric | VM | Container | Difference |
|---|---:|---:|---:|
| Memory Operations/sec | 8150 | 8325 | 2.15% |
| Memory Transfer Rate | 8150 MB/s | 8325 MB/s | 2.15% |
| Average Latency | 0.123 ms | 0.120 ms | 2.44% |

### Memory Observation

The memory performance of the VM and container is relatively close.

Containers generally share the host kernel and do not require a complete guest operating system, which can reduce virtualization overhead.

The observed difference is therefore expected to be smaller than differences that may appear in some disk or startup measurements.

---

# 7. Disk I/O Performance

## Tool Used

**fio**

The disk experiment evaluates:

1. Sequential write
2. Sequential read
3. Random read
4. Random write

Disk performance is measured using throughput and I/O operations.

---

## Disk Results

| Disk Operation | VM | Container | Difference |
|---|---:|---:|---:|
| Sequential Read | 1948.63 MiB/s | 1985.40 MiB/s | 1.89% |
| Sequential Write | 1754.03 MiB/s | 1790.20 MiB/s | 2.06% |
| Random Read | 166 MiB/s | 171 MiB/s | 3.01% |
| Random Write | 202 MiB/s | 207 MiB/s | 2.48% |

### Disk Observation

Sequential operations generally provide higher throughput because data is accessed continuously.

Random operations involve smaller and non-sequential accesses and therefore show lower throughput.

The container shows slightly better preliminary performance because container storage can operate closer to the underlying host filesystem, while VM storage passes through the virtual disk layer.

However, actual disk performance depends strongly on:

- Storage device
- Filesystem
- Docker storage driver
- VM virtual disk configuration
- Host load
- Cache behavior

---

# 8. Network Performance

## Tool Used

**iperf3**

The network experiment measures communication throughput between the client and server.

The important metrics are:

- Network throughput
- Retransmissions
- Transfer rate

---

## Network Results

| Metric | VM | Container | Difference |
|---|---:|---:|---:|
| Throughput | 938 Mbps | 956 Mbps | 1.92% |
| Transfer Rate | 117.25 MB/s | 119.50 MB/s | 1.92% |
| Retransmissions | 18 | 15 | -16.67% |

### Network Observation

Container networking generally has low overhead.

The VM may introduce additional networking layers depending on the configured network mode.

Docker networking can also introduce a small amount of overhead through virtual network interfaces and network address translation.

Overall, both environments provide high network throughput under the tested workload.

---

# 9. FastAPI Application

A simple FastAPI application is used to evaluate application-level performance.

The application provides endpoints such as:

```text
/health
/compute
/memory
```

### Health Endpoint

```text
GET /health
```

Example response:

```json
{
    "status": "healthy"
}
```

### Compute Endpoint

The `/compute` endpoint performs CPU-intensive calculations.

### Memory Endpoint

The `/memory` endpoint creates a large data structure to generate memory activity.

---

# 10. FastAPI Performance

The same application is executed in the VM and Docker environments.

The API is tested using Apache Benchmark or wrk.

Important metrics include:

- Requests per second
- Time per request
- Failed requests
- Response latency

---

## FastAPI Results

| Metric | VM | Container | Difference |
|---|---:|---:|---:|
| Requests/sec | 965 | 992 | 2.80% |
| Average Latency | 10.36 ms | 10.08 ms | 2.70% |
| Failed Requests | 0 | 0 | 0% |
| Successful Requests | 1000 | 1000 | 0% |

### FastAPI Observation

The container provides slightly higher application throughput in the preliminary comparison.

The difference is relatively small because both environments execute the same Python application.

Containerization does not change the application logic. The major difference comes from the execution environment and virtualization overhead.

---

# 11. Startup Time Experiment

Startup time is important for applications such as:

- Microservices
- Cloud applications
- Serverless workloads
- CI/CD systems
- Auto-scaling applications

The time required to start the application environment is measured.

---

## Startup Results

| Metric | VM | Container |
|---|---:|---:|
| Environment Startup | 18.4 sec | 2.8 sec |
| Application Ready Time | 3.2 sec | 1.1 sec |

### Startup Observation

The container starts significantly faster than the complete virtual machine.

A virtual machine must initialize a complete operating system, virtual hardware, kernel and system services.

A container shares the host operating system kernel and therefore requires considerably less startup overhead.

---

# 12. Scalability Experiment

Scalability is evaluated by increasing workload levels.

For CPU scalability, the following thread counts are used:

```text
1 → 2 → 4 → 8 threads
```

For API scalability, increasing numbers of concurrent requests are used.

The objective is to observe how performance changes as workload increases.

---

## CPU Scalability

| Threads | VM Events/sec | Container Events/sec |
|---:|---:|---:|
| 1 | 1650.35 | 1685.20 |
| 2 | 3300.08 | 3375.60 |
| 4 | 6076.32 | 6205.40 |
| 8 | 5995.45 | 6120.80 |

### Scalability Observation

Performance improves significantly from 1 to 4 threads.

The increase becomes smaller at 8 threads because the available CPU resources become a limiting factor.

This indicates that increasing workload does not always produce a proportional increase in performance.

---

# 13. Overall Comparison

| Metric | VM | Container | Better Preliminary Result |
|---|---:|---:|---|
| CPU — 1 Thread | 1650.35 events/s | 1685.20 events/s | Container |
| CPU — 2 Threads | 3300.08 events/s | 3375.60 events/s | Container |
| CPU — 4 Threads | 6076.32 events/s | 6205.40 events/s | Container |
| CPU — 8 Threads | 5995.45 events/s | 6120.80 events/s | Container |
| Memory Operations | 8150 ops/s | 8325 ops/s | Container |
| Sequential Read | 1948.63 MiB/s | 1985.40 MiB/s | Container |
| Sequential Write | 1754.03 MiB/s | 1790.20 MiB/s | Container |
| Random Read | 166 MiB/s | 171 MiB/s | Container |
| Random Write | 202 MiB/s | 207 MiB/s | Container |
| Network Throughput | 938 Mbps | 956 Mbps | Container |
| API Throughput | 965 req/s | 992 req/s | Container |
| API Latency | 10.36 ms | 10.08 ms | Container |
| Startup Time | 18.4 sec | 2.8 sec | Container |

---

# 14. Performance Difference

The percentage difference is calculated using:

```text
Difference (%) =
((Container Performance - VM Performance) / VM Performance) × 100
```

For metrics where lower is better, such as latency or startup time:

```text
Improvement (%) =
((VM Time - Container Time) / VM Time) × 100
```

These formulas allow the performance difference between the two environments to be quantified.

---

# 15. Resource Utilization

Resource utilization is monitored using tools such as:

```text
htop
vmstat
docker stats
```

The following resources are considered:

- CPU utilization
- Memory utilization
- Network I/O
- Block I/O

Docker's `docker stats` command provides resource information for running containers.

The VM resource usage is observed from inside the Ubuntu environment.

---

# 16. Graphs

The following graphs are used to visualize the performance comparison.

### CPU Scalability

The X-axis represents the number of CPU threads and the Y-axis represents CPU events per second.

```text
X-axis → Number of Threads
Y-axis → Events per Second
```

The graph shows how CPU performance changes as workload increases.

### Disk Performance

```text
X-axis → Disk Operation
Y-axis → Throughput (MiB/s)
```

The graph compares sequential and random disk operations.

### Network Performance

```text
X-axis → Environment
Y-axis → Network Throughput (Mbps)
```

### FastAPI Performance

```text
X-axis → Environment
Y-axis → Requests per Second
```

### Startup Time

```text
X-axis → Environment
Y-axis → Startup Time (seconds)
```

These graphs make the performance differences easier to understand than numerical tables alone.

---

# 17. Analysis and Discussion

The experimental comparison shows that both Virtual Machines and Containers can provide suitable environments for running applications and workloads.

### CPU

Container performance is slightly higher in the preliminary results.

The reason is that containers share the host operating system kernel and therefore avoid some of the virtualization layers required by a virtual machine.

### Memory

The memory performance difference is relatively small.

Both environments have access to similar memory resources, although containers generally have lower system overhead.

### Disk

Disk performance depends strongly on the storage configuration.

Containers can access host storage with relatively low overhead, while VM disk operations may pass through virtual storage layers.

### Network

Both environments provide high network throughput.

The exact difference depends on whether the VM uses NAT or bridged networking and which Docker network configuration is used.

### Application

The FastAPI application shows similar performance in both environments because the same application code is executed.

The container has a small performance advantage in the preliminary comparison.

### Startup

The largest advantage of containers is observed in startup time.

Containers do not need to boot an entire guest operating system, so they can become operational much faster.

### Scalability

Both environments scale as workload increases, but performance eventually becomes limited by available CPU and memory resources.

---

# 18. Advantages of Virtual Machines

Virtual Machines provide:

- Strong isolation
- Complete operating system environment
- Ability to run different operating systems
- Independent virtual hardware
- Useful isolation for legacy applications
- Suitable environment for traditional server virtualization

---

# 19. Advantages of Containers

Containers provide:

- Fast startup
- Lightweight execution
- Lower overhead
- Efficient resource utilization
- Easy deployment
- Application portability
- Easy scaling
- Suitable environment for microservices
- Efficient CI/CD workflows

---

# 20. VM vs Container Summary

| Feature | Virtual Machine | Container |
|---|---|---|
| Isolation | Strong | Process-level isolation |
| Operating System | Complete guest OS | Shares host kernel |
| Startup | Slower | Faster |
| Resource Overhead | Higher | Lower |
| Portability | Good | Very good |
| Scaling | Relatively slower | Fast |
| Deployment | Heavier | Lightweight |
| Application Density | Lower | Higher |
| Best Use | Full OS isolation | Microservices / cloud applications |

---

# 21. Limitations

The performance of VM and container environments can be affected by several factors:

- Host system load
- CPU architecture
- Available RAM
- Storage device
- Filesystem
- Docker storage driver
- Network configuration
- VMware configuration
- Background processes
- Cache effects
- Benchmark parameters

Therefore, the results should be interpreted in the context of the experimental environment.

---

# 22. Conclusion

This project experimentally compares Virtual Machine and Docker Container environments using CPU, memory, disk, network and application workloads.

The preliminary results indicate that containers provide slightly better performance for many of the tested workloads while also providing a major advantage in startup time.

The major observations are:

1. Containers have lower virtualization overhead.
2. CPU performance is slightly higher in the container environment.
3. Memory performance is similar in both environments.
4. Disk performance depends strongly on the storage configuration.
5. Network performance is comparable between both environments.
6. FastAPI application throughput is slightly higher in the container environment.
7. Containers start significantly faster than virtual machines.
8. Both environments show reduced performance gains when workload increases beyond available resources.
9. Virtual Machines provide stronger operating-system-level isolation.
10. Containers are particularly suitable for lightweight applications, microservices and rapidly scalable cloud workloads.

Overall, **Virtual Machines are useful when complete operating-system isolation is required, while Containers are more efficient for lightweight, portable and rapidly deployable applications.**

---

# 23. Future Work

The experiment can be extended in the future by:

- Testing Kubernetes deployments
- Testing multiple container replicas
- Implementing Docker Compose
- Studying autoscaling
- Testing different CPU limits
- Testing different memory limits
- Comparing different Docker storage drivers
- Testing different network modes
- Testing larger workloads
- Comparing multiple VM hypervisors
- Testing cloud deployment
- Studying container orchestration performance

---

# 24. Experimental Screenshots

The repository contains screenshots demonstrating the major stages of the experiment.

The screenshots include evidence of:

- VM configuration
- Ubuntu environment
- CPU benchmark
- Memory benchmark
- Disk benchmark
- Docker configuration
- Docker benchmark
- Network testing
- FastAPI application
- API testing
- Resource monitoring
- Performance results

All screenshots are available in the `screenshots` directory.

---

# 25. Repository Structure

```text
VM-vs-Container-Performance-Analysis/
│
├── README.md
│
└── screenshots/
    ├── CPU benchmark screenshots
    ├── Memory benchmark screenshots
    ├── Disk benchmark screenshots
    ├── Network benchmark screenshots
    ├── Docker screenshots
    ├── FastAPI screenshots
    └── Result screenshots
```

---

# 26. Reproduction Workflow

The experiment follows this workflow:

```text
Prepare Environment
        ↓
Configure Virtual Machine
        ↓
Configure Docker
        ↓
Establish Baseline
        ↓
CPU Benchmark
        ↓
Memory Benchmark
        ↓
Disk I/O Benchmark
        ↓
Network Benchmark
        ↓
FastAPI Application
        ↓
Startup Time Test
        ↓
Scalability Test
        ↓
Compare Results
        ↓
Generate Graphs
        ↓
Analyze Performance
        ↓
Publish on GitHub
```

---

# 27. GitHub

This project is maintained as a GitHub repository containing the experiment documentation and screenshots.

The repository demonstrates the practical comparison of virtualization and containerization technologies using controlled workloads.

---

## Final Result

The experiment demonstrates the fundamental performance differences between **Virtual Machines and Containers**.

Containers generally provide:

**Lower overhead + Faster startup + Lightweight deployment + Efficient resource usage**

while Virtual Machines provide:

**Complete operating-system isolation + Independent guest environments + Strong virtualization isolation**

Both technologies are important in modern cloud computing, and the appropriate choice depends on application requirements, isolation requirements, resource constraints and deployment goals.

---

## Keywords

`Virtual Machine` `Docker` `Containers` `VMware` `Ubuntu` `Sysbench` `fio` `iperf3` `FastAPI` `Performance Analysis` `Cloud Computing` `Virtualization` `Containerization` `Benchmarking` `Scalability`