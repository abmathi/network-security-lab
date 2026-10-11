# Network Security Lab

Hands-on network security investigations demonstrating packet analysis, TCP port discovery, protocol interpretation, and service enumeration using Wireshark and Nmap.

This repository contains two documented investigations conducted in authorized testing environments and an isolated home lab. Each investigation includes technical findings, supporting screenshots, analyst assessments, and evidence limitations.

## Investigations

### 1. Wireshark — Network Traffic Analysis

**Focus:** Packet-level investigation and network protocol analysis.

Analyzed captured network traffic to understand connection establishment, name resolution, encrypted communications, and local network address resolution.

**Key investigations:**
- TCP three-way handshake reconstruction.
- DNS query and response correlation.
- TLS handshake and encrypted application-data analysis.
- ARP request and response examination.
- Independent TCP/TLS conversation reconstruction.

**Skills:** Wireshark display filters, TCP flags, DNS records, TLS metadata, ARP analysis, packet correlation, and evidence-based network investigation.

**Evidence:** 5 screenshots across 5 findings.

[View Wireshark Investigation](investigations/wireshark-traffic-analysis/README.md)

### 2. Nmap — Network Reconnaissance

**Focus:** TCP port discovery, scan interpretation, and service enumeration.

Conducted targeted scans against Nmap's authorized testing host and independently assessed services on an isolated internal lab server.

**Key investigations:**
- Open, closed, and filtered TCP port classifications.
- TCP SYN packet tracing and response interpretation.
- Default versus full-range TCP scanning.
- Service and version enumeration using Nmap.

**Key finding:** A default scan of the internal server identified two open TCP ports, while a full-range scan identified four, demonstrating the importance of scan coverage when assessing network accessibility.

**Skills:** Nmap, TCP SYN scanning, packet tracing, service fingerprinting, port enumeration, and attack-surface assessment.

**Evidence:** 4 screenshots across 4 findings.

[View Nmap Investigation](investigations/nmap-network-reconnaissance/README.md)

## Tools and Technologies

| Tool / Technology | Application |
| --- | --- |
| Wireshark | Packet inspection, filtering, and protocol analysis |
| Nmap | Network reconnaissance, port discovery, and service enumeration |
| Kali Linux | Authorized scanning and analysis environment |
| TCP/IP | Connection establishment and network communication |
| DNS | Domain-name resolution analysis |
| TLS | Encrypted communication and handshake analysis |
| ARP | Local network address-resolution analysis |
| VirtualBox | Isolated virtual networking environment |

## Investigation Methodology

Both investigations followed an evidence-driven approach:

1. **Establish scope:** Identify authorized targets and investigation objectives.
2. **Collect evidence:** Capture or examine network traffic and scan output.
3. **Analyze observations:** Interpret protocol fields, port states, and service fingerprints.
4. **Correlate findings:** Relate individual observations to broader network behavior.
5. **Assess security relevance:** Explain potential implications without assuming malicious activity or vulnerabilities.
6. **Document results:** Preserve screenshots, findings, limitations, and analyst assessments.

## Repository Structure

```text
network-security-lab/
├── README.md
└── investigations/
    ├── wireshark-traffic-analysis/
    │   ├── README.md
    │   └── evidence/
    │       └── 5 screenshots
    └── nmap-network-reconnaissance/
        ├── README.md
        └── evidence/
            └── 4 screenshots
```

## Scope and Limitations

All scanning and analysis were conducted in authorized environments.

The Wireshark investigation documents selected packet captures, not an unrestricted record of all network activity. Encrypted application payloads were not decrypted.

The Nmap investigation documents network accessibility and remotely identified services. Open ports and service fingerprints do not independently demonstrate vulnerabilities or public exposure.

Neither investigation involved unauthorized access or exploitation.

## Skills Demonstrated

- Network traffic inspection and protocol interpretation.
- TCP handshake and connection-state analysis.
- DNS and ARP traffic analysis.
- TLS handshake and encrypted-traffic assessment.
- TCP port discovery and SYN packet tracing.
- Full-range scanning and service fingerprinting.
- Network exposure analysis and investigative documentation.
- Evidence-based technical reporting with clearly stated limitations.

## Portfolio

These investigations complement my broader hands-on cybersecurity projects in security operations, digital forensics, detection engineering, cloud security, and identity and access management.

**GitHub:** [abmathi](https://github.com/abmathi)

**Portfolio:** [Austin's Angle](https://austinsangle.com)