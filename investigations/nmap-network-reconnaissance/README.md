# Nmap Network Reconnaissance — TCP Port Discovery and Service Enumeration

## Executive Summary

This project documents network reconnaissance using Nmap to investigate TCP port states, packet-level scanning behavior, and exposed network services.

The investigation combined targeted scanning against Nmap's authorized public testing host with an independent assessment of a server in an isolated home lab.

The initial exercises focused on interpreting open, closed, and filtered TCP ports. The independent investigation extended this methodology through full-range TCP scanning and service/version detection.

The most significant finding occurred during the internal server assessment: a default Nmap scan identified two open TCP ports, while a full-range scan revealed four. Subsequent service enumeration identified applications including OpenSSH, Elasticsearch Kibana, and an Elasticsearch REST API.

The objective was to demonstrate systematic network reconnaissance, explain how TCP responses influence port-state classifications, and assess the limitations of relying on default scanning configurations.

## Investigation Scope

The investigation covered two authorized environments.

| Environment | Target | Investigation Focus |
| --- | --- | --- |
| Public testing host | `scanme.nmap.org` | Targeted TCP scanning and packet-level behavior |
| Isolated internal lab | `10.10.10.10` | Host discovery, full-range TCP scanning, and service/version enumeration |

### Internal Lab Environment

| Component | Configuration |
| --- | --- |
| Scanner | Kali Linux VM |
| Scanner IP | `10.10.10.20` |
| Target | `10.10.10.10` |
| Network | Isolated VirtualBox internal network |
| Primary tool | Nmap 7.95 |

The internal server was part of a controlled lab environment. Scanning was performed against infrastructure under my administration.

### Investigation Objectives

1. Identify open, closed, and filtered TCP ports using Nmap.
2. Examine the TCP probe and response behavior underlying port-state classification.
3. Determine whether an internal server is reachable.
4. Compare default TCP scanning with full-range port discovery.
5. Identify services and available version information on discovered ports.
6. Evaluate the security implications and limitations of reconnaissance results.

## Evidence Sources and Methodology

### Evidence Sources

The investigation used terminal output from Nmap scans performed in the authorized environments.

The analysis examined:

- TCP port states reported by Nmap.
- Packet-level SYN probes and responses.
- Target reachability.
- Differences between default and full-range scanning.
- Service names and version-detection results.
- Application-specific metadata returned during service enumeration.

### Investigation Methodology

| Phase | Analyst Activity |
| --- | --- |
| Targeted scanning | Scan selected TCP ports and interpret their reported states |
| Packet inspection | Use Nmap's packet-trace output to investigate TCP probes and responses |
| Baseline enumeration | Perform a default scan of the internal server |
| Expanded discovery | Scan the complete TCP port range to identify additional exposed services |
| Service identification | Apply service/version detection to discovered ports |
| Analysis | Compare results, explain exposed services, and document limitations |

### Evidence Standards

The investigation distinguishes direct Nmap observations from broader security conclusions.

An open TCP port indicates that the target responded in a manner consistent with a listening service. It does not independently demonstrate that the associated application is vulnerable.

A closed port indicates that Nmap received a response consistent with the absence of a listening service, while a filtered classification indicates that filtering or network conditions prevented a definitive open-or-closed determination.

Service/version detection can provide application information, but identified product names and versions remain subject to the accuracy and completeness of remote probing.

## Investigation Organization

The report documents four phases:

1. Targeted TCP Port Scanning
2. TCP SYN Packet Trace Analysis
3. Default vs. Full-Range TCP Port Discovery
4. Independent Service and Version Enumeration

The final sections consolidate the exposed-service findings, security implications, evidence limitations, skills demonstrated, and supporting screenshots.