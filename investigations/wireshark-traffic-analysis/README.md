# Wireshark Traffic Analysis — Network Protocol Investigation

## Executive Summary

This project documents hands-on network traffic analysis using Wireshark to investigate TCP connections, DNS resolution, HTTP communication, and TLS negotiation.

The investigation focused on interpreting packet-level evidence to understand how network communication is established, how applications identify remote destinations, and what information is visible during encrypted and unencrypted traffic.

The analysis covered four protocol areas:

- **TCP:** Connection establishment using the SYN, SYN-ACK, and ACK handshake.
- **DNS:** Domain-name resolution and interpretation of query and response traffic.
- **HTTP:** Application-layer requests and observable protocol metadata.
- **TLS:** Client Hello messages and the negotiation process used to establish encrypted connections.

The objective was to develop practical packet-analysis skills relevant to SOC monitoring, network troubleshooting, and security investigations.

## Investigation Scope

The investigation used Wireshark packet captures collected in a controlled lab environment.

| Protocol | Investigation Focus | Security Relevance |
| --- | --- | --- |
| TCP | Three-way handshake and connection establishment | Identifying successful connections and unusual connection patterns |
| DNS | Domain queries and responses | Investigating suspicious destinations and DNS activity |
| HTTP | Request methods, headers, and response behavior | Examining application communication and unencrypted traffic |
| TLS | Client Hello and handshake metadata | Understanding visibility and limitations of encrypted traffic |

### Investigation Objectives

1. Identify the protocols present in captured network traffic.
2. Examine TCP flags and the sequence used to establish connections.
3. Analyze DNS queries and responses associated with domain resolution.
4. Interpret visible HTTP request and response information.
5. Identify key fields in the TLS handshake.
6. Explain what network traffic reveals and what cannot be concluded from the available packets.

## Evidence Sources and Methodology

### Evidence Sources

Wireshark was used to inspect captured packets and analyze protocol-specific fields.

The investigation emphasized:

- Source and destination IP addresses
- Source and destination ports
- TCP flags and connection sequences
- DNS query names and response information
- HTTP request and response fields
- TLS handshake messages and visible negotiation metadata

### Investigation Methodology

| Step | Analyst Activity |
| --- | --- |
| Capture | Collect network packets in the authorized lab environment |
| Filter | Isolate relevant protocols and conversations |
| Inspect | Examine packet headers, flags, and protocol fields |
| Correlate | Connect related packets to reconstruct communication sequences |
| Assess | Explain the observed behavior and its security relevance |
| Document | Preserve screenshots and evidence-based conclusions |

### Evidence Standards

The analysis distinguishes protocol observations from broader security conclusions.

For example, a TCP handshake demonstrates connection establishment but does not independently establish that an application successfully completed its intended operation.

A DNS query does not prove that a subsequent connection was made to the resolved destination.

Similarly, observing a TLS Client Hello demonstrates an attempt to negotiate encrypted communication, not necessarily that the handshake completed successfully.

## Investigation Organization

The report documents four protocol investigations:

1. TCP Connection Establishment
2. DNS Query and Response Analysis
3. HTTP Traffic Inspection
4. TLS Handshake Analysis

The final sections will consolidate security implications, evidence limitations, and skills demonstrated.

## Investigation Findings

### Finding 1 — TCP Three-Way Handshake

#### Investigation Context

The first investigation focused on identifying how a TCP connection is established between two endpoints.

Using Wireshark, I examined the initial packets of a TCP conversation to identify the handshake sequence and interpret the flags associated with connection establishment.

#### Packet Analysis

A successful TCP three-way handshake consists of three packets:

| Sequence | TCP Flags | Interpretation |
| --- | --- | --- |
| 1 | `SYN` | The client initiates a connection |
| 2 | `SYN, ACK` | The server acknowledges the request and responds |
| 3 | `ACK` | The client acknowledges the server's response |

The investigation examined packet direction, TCP flags, source and destination ports, and sequence information to distinguish the handshake stages.

The observed packet sequence was consistent with successful TCP connection establishment.

#### Security Relevance

TCP handshake analysis is useful for distinguishing established connections from incomplete connection attempts.

For example, repeated SYN packets without corresponding SYN-ACK responses can indicate connection failures, filtering, or potentially suspicious scanning behavior, depending on the broader context.

However, an established TCP connection does not independently prove that higher-layer application communication succeeded or that the connection was malicious.

#### Analyst Assessment

**Assessment: TCP connection establishment identified through the three-way handshake.**

The packet sequence demonstrated how TCP synchronization and acknowledgment flags establish a connection.

The investigation also showed how packet-level analysis can support network troubleshooting and SOC investigations by identifying whether the initial connection process completed.