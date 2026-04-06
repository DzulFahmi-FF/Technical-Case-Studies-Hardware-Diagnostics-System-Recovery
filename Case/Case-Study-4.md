# Network Troubleshooting: Resolving "No Internet" & Access Issues in Dual-Stack Home Networks (IndiHome Case Study)

## Background
In many consumer-grade ISP environments (specifically **IndiHome**), dual-stack IPv4/IPv6 implementation often leads to routing instability. This project documents the diagnostic process and the final resolution for persistent connectivity failures, occurring in both single-router and cascaded (Router-on-Router) topologies.

## Problem Identification
* **Symptoms**: Devices display a "Connected, No Internet" status or experience infinite loading/timeouts when accessing specific web services and applications.
* **Environment**: Home network utilizing an ISP-provided ONT (Gateway), either as a standalone unit or connected to a secondary router.
* **Initial Hypothesis**: IP conflicts or DNS resolution failure. However, standard troubleshooting (e.g., DHCP renewal, utilizing Google/Cloudflare DNS) failed to resolve the symptoms.

## Root Cause Analysis (RCA)
A systematic investigation of the network stack revealed the following core issues:

1. **ISP Infrastructure Instability**: The ISP ONT receives an IPv6 prefix, but fails to route global traffic consistently. The IPv6 path frequently experiences high packet loss at the gateway level.
2. **Broken Dual-Stack Mechanism**: The ONT advertises IPv6 capabilities to client devices. Due to the broken upstream route, devices attempt to prioritize IPv6 traffic but fail to perform a rapid fallback to the functional IPv4 path, resulting in a "hanging" connection.
3. **Router Advertisement (RA) Conflict**: In cascaded setups, conflicting RAs between the ISP ONT and the secondary router exacerbate "Black-Hole Routing" issues, where data packets are dropped due to invalid routing information.

## Resolution: Protocol Simplification
Given that the ISP infrastructure currently lacks the stability required for seamless IPv6 prefixing in consumer hardware, the following tactical mitigation was implemented:

* **Gateway Level Mitigation**: Disabled IPv6 functionality on the **ISP ONT** to terminate faulty prefix advertisements at the source.
* **Local Network Optimization**: Disabled IPv6 on the **Secondary Router** to ensure a deterministic, IPv4-only routing table for all connected clients.
* **Validation**: Post-implementation testing confirmed **100% network stability**, elimination of "No Internet" false positives, and restored access to all previously unreachable services.

## Technical Takeaways
* **The Importance of RCA**: In consumer environments, "No Internet" status is frequently a protocol-level conflict (IPv6) rather than a physical layer or hardware failure.
* **Pragmatic Support**: Disabling modern features (IPv6) in favor of operational stability is a valid technical decision when upstream infrastructure is unreliable.
* **Incident Documentation**: Maintaining logs of ISP-specific hardware behavior is crucial for accelerating future incident response and maintaining service quality.
