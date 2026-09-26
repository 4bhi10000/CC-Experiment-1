# Cloud Computing Laboratory Report
## Experiment 1 – Comparing CPU Performance in Two Virtualization Environments

**Course:** Cloud Computing / Computer Networks Laboratory  
**Experiment No.:** 1  
**Topic:** Performance comparison of Type-1 and Type-2 hypervisors

---

## 1. Aim

The purpose of this experiment is to study the effect of the virtualization environment on a CPU-intensive workload. Two Ubuntu virtual machines with comparable resources are tested using Proxmox VE and VMware Workstation, and their Sysbench CPU results are compared.

The experiment records:

- total execution time,
- number of completed events,
- events per second,
- minimum latency,
- average latency,
- 95th-percentile latency, and
- maximum latency.

---

## 2. Concept Used

### 2.1 Type-1 virtualization

A Type-1 hypervisor is installed directly on the physical machine. In this experiment, **Proxmox VE** provides the virtualization environment and uses KVM for running the guest VM.

```text
Ubuntu Guest
     │
     ▼
Proxmox VE / KVM
     │
     ▼
Physical Hardware
```

### 2.2 Type-2 virtualization

A Type-2 hypervisor operates from within an existing operating system. Here, **VMware Workstation** runs on a Windows host and provides the virtual hardware presented to the Ubuntu guest.

```text
Ubuntu Guest
     │
     ▼
VMware Workstation
     │
     ▼
Windows Host
     │
     ▼
Physical Hardware
```

The additional host operating-system layer can affect scheduling and resource availability, especially when the host is performing other work at the same time.

---

## 3. Test Configuration

The main VM resources were kept comparable for both environments.

| Parameter | Proxmox VE | VMware Workstation |
|---|---|---|
| Guest OS | Ubuntu 24.04.x 64-bit | Ubuntu Linux 64-bit |
| CPU allocation | 2 vCPUs | 2 vCPUs |
| RAM | 2048 MB | 2048 MB |
| Virtual disk | 20 GB | 20 GB |
| Benchmark | Sysbench CPU | Sysbench CPU |
| CPU test | `--cpu-max-prime=20000` | `--cpu-max-prime=20000` |

The remaining virtual hardware options depend on the respective virtualization platform.

---

## 4. Experimental Procedure

### Part A – Proxmox VE

1. Create an Ubuntu virtual machine in Proxmox VE.
2. Allocate 2 vCPUs, 2 GB RAM and a 20 GB virtual disk.
3. Install Ubuntu and verify the guest configuration.
4. Check CPU and memory information using:

```bash
lscpu
free -h
df -h
```

5. Install Sysbench:

```bash
sudo apt update
sudo apt install sysbench -y
```

6. Run the CPU benchmark:

```bash
sysbench cpu --cpu-max-prime=20000 run
```

7. Record the output and take screenshots of the relevant configuration and result screens.

### Part B – VMware Workstation

1. Create a new Ubuntu virtual machine in VMware Workstation.
2. Allocate resources comparable to the Proxmox VM.
3. Complete the Ubuntu installation.
4. Verify the guest CPU and memory configuration.
5. Install Sysbench if it is not already available.
6. Run the same benchmark command:

```bash
sysbench cpu --cpu-max-prime=20000 run
```

7. Save the result and capture the required screenshots.

---

## 5. Observed Benchmark Values

### Proxmox VE

```text
Events per second:       1716.69
Total time:              10.0004s
Total events:            17169
Minimum latency:         0.57 ms
Average latency:         0.58 ms
Maximum latency:         2.78 ms
95th percentile:         0.65 ms
```

### VMware Workstation

```text
Events per second:       1364.78
Total time:              10.0007s
Total events:            13650
Minimum latency:         0.67 ms
Average latency:         0.73 ms
Maximum latency:         4.06 ms
95th percentile:         0.89 ms
```

---

## 6. Comparison

| Measurement | Proxmox VE | VMware Workstation | Difference |
|---|---:|---:|---:|
| Run time | 10.0004 s | 10.0007 s | 0.0003 s |
| Total events | 17,169 | 13,650 | 3,519 |
| Events/sec | 1,716.69 | 1,364.78 | 351.91 |
| Minimum latency | 0.57 ms | 0.67 ms | 0.10 ms |
| Average latency | 0.58 ms | 0.73 ms | 0.15 ms |
| 95th percentile | 0.65 ms | 0.89 ms | 0.24 ms |
| Maximum latency | 2.78 ms | 4.06 ms | 1.28 ms |

For this particular run, the Proxmox VM recorded approximately **25.8% more events per second** than the VMware VM. The average latency was approximately **20.5% lower** in the Proxmox run.

---

## 7. Graphs

### Figure 1 – Events per second

![CPU throughput](images/events_per_second_comparison.png)

### Figure 2 – Latency

![Latency](images/latency_comparison.png)

### Figure 3 – Total events

![Total events](images/total_events_comparison.png)

### Figure 4 – Overall comparison

![Overall dashboard](images/overall_performance_dashboard.png)

---

## 8. Discussion

The benchmark shows a measurable difference between the two test environments. Proxmox recorded a higher event rate and lower latency values for the selected CPU workload.

One possible reason is the difference in virtualization architecture. Proxmox uses KVM-based virtualization directly on the host machine, whereas VMware Workstation operates as a hosted virtualization product within Windows. The guest therefore shares the host system with normal Windows processes and services.

However, benchmark performance is affected by many variables, including processor model, power-management settings, background applications, VM configuration and system load. The results therefore represent the conditions of this particular experiment and should not be treated as a fixed performance ratio for every installation.

For a more rigorous study, the benchmark could be repeated several times on each platform and the mean, median and standard deviation could be calculated.

---

## 9. Conclusion

The experiment demonstrated how two virtualization arrangements can produce different results for the same CPU benchmark. With the configuration used in this laboratory exercise, the Proxmox VM produced higher Sysbench throughput and lower measured latency than the VMware Workstation VM.

The main learning points are:

1. Type-1 and Type-2 hypervisors use different software layers to provide virtualization.
2. VM resource configuration should be kept comparable when performing a benchmark comparison.
3. Sysbench can be used to obtain measurable CPU performance and latency values.
4. Benchmark results should be interpreted in the context of the hardware and configuration used.
5. Repeated trials would improve the reliability of the comparison.

---

**Student Name:** __________________________  
**USN:** _________________________________  
**Date:** _________________________________  
**Faculty Signature:** ______________________
