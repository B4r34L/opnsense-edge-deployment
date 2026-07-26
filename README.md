## System Specifications & Hardware Profile
* **Platform:** OPNsense Enterprise Firewall (FreeBSD Base Kernel)
* **Compute Appliance:** Intel N95 Mini PC (4 Cores, 4 Threads @ 3.4GHz)
* **Memory Allocation:** 16GB DDR4 RAM
* **Primary Roles:** Edge Routing, Stateful Inspections, Inter-VLAN Access Control, In-Line Intrusion Detection (IDS/IPS), Upstream Domain Filtering.

## Kernel Engineering & Hardware Troubleshooting (RCA)

### Case Study: Intel N95 Hardware & FreeBSD ACPI Mismatch
* **Symptom:** Immediate, unrecoverable system freezes or kernel panics during high packet processing load on the Intel N95 Alder Lake-N bare-metal appliance. 
* **Diagnostic Investigation:** Monitored console log output during stress cycles. Isolated the failure to unstable Advanced Configuration and Power Interface (ACPI) state transitions. The underlying FreeBSD kernel base was misinterpreting the low-power thermal state reporting of the newer Intel N95 microarchitecture, leading to CPU thread lockups.
* **Resolution & Implementation:** Validated a stable hardware execution path by bypassing the faulty ACPI thermal reporting layer. Implemented a persistent system tunable within the OPNsense system architecture configuration:

```text
Variable:  debug.acpi.disabled
Value:     "thermal"
Description: Disables the FreeBSD ACPI thermal driver control loop, handing management off to hardware-level safety thresholds.
```

* **Outcome:** System stability achieved. 100% uptime sustained under continuous synthetic network load testing with zero degradation to core hardware performance.


