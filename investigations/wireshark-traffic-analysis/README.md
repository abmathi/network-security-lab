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

#### Observed TCP Connection

The Wireshark capture identified a connection from `192.168.0.84:60144` to `151.101.194.214:443`.

The following packets established the connection:

| Frame | Source | Destination | TCP Flags |
| --- | --- | --- | --- |
| `9160` | `192.168.0.84:60144` | `151.101.194.214:443` | `SYN` |
| `9202` | `151.101.194.214:443` | `192.168.0.84:60144` | `SYN, ACK` |
| `9203` | `192.168.0.84:60144` | `151.101.194.214:443` | `ACK` |

![TCP three-way handshake in Wireshark](evidence/01-tcp-three-way-handshake.png)

*Figure 1 — Wireshark TCP stream 10 showing the SYN, SYN-ACK, and ACK handshake between `192.168.0.84:60144` and `151.101.194.214:443`. The selected SYN packet displays TCP flag `0x002`.*

The sequence confirmed successful TCP connection establishment.

Subsequent packets included a TLS Client Hello and encrypted application data, indicating that the connection proceeded beyond the TCP handshake into TLS communication.

The observed packets did not independently establish the legitimacy of the remote service or the application-level purpose of the connection.

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

### Finding 2 — DNS Query and Response Analysis

#### Investigation Context

The second investigation examined Domain Name System (DNS) traffic using Wireshark.

DNS translates domain names into information such as IP addresses, allowing clients to locate network services without relying on manually entered addresses.

The objective was to identify DNS requests, examine the corresponding responses, and determine what information was visible in the captured traffic.

#### Packet Analysis

Using the Wireshark display filter `dns`, I isolated DNS packets and examined communication between the client and DNS server.

The captured traffic included the following observations:

| Field | Observed Value |
| --- | --- |
| Client IP | `192.168.0.84` |
| DNS server IP | `192.168.0.6` |
| Client UDP port | `62122` |
| DNS destination port | `53` |
| Queried domain | `example.com` |
| Query type | `A` |
| Query transaction ID | `0x0002` |
| Query frame | `2095` |
| Response frame | `2096` |

![DNS query and response analysis in Wireshark](evidence/02-dns-udp-analysis.png)

*Figure 2 — Wireshark DNS capture showing a UDP query for `example.com` from `192.168.0.84` to `192.168.0.6`, with frame 2095 linked to response frame 2096.*

Frame `2095` contained a standard DNS query requesting an IPv4 address record for `example.com`.

Frame `2096` contained the corresponding response, identified by the matching transaction ID `0x0002`.

The DNS response included two IPv4 addresses:

- `172.66.147.243`
- `104.20.23.154`

The capture also contained AAAA queries for IPv6 records, PTR reverse-lookup queries, and DNS activity involving `v20.events.data.microsoft.com`.

#### Query and Response Correlation

The relationship between frames `2095` and `2096` demonstrated how a client request can be matched to its corresponding DNS response.

The client sent its query to UDP destination port `53`, and the DNS server returned address information for the requested domain.

The query and response shared a transaction identifier, providing an additional correlation field beyond source and destination addresses.

#### Security Relevance

DNS analysis is useful for identifying external destinations and investigating suspicious name-resolution behavior.

For example, unusual domains, repeated queries, unexpected record types, or excessive failed resolutions can warrant further investigation when correlated with endpoint and network activity.

However, a successful DNS response does not independently establish that the client subsequently connected to the returned IP addresses.

#### Analyst Assessment

**Assessment: DNS name resolution and query-response correlation successfully identified.**

The packet evidence demonstrated how DNS requests and responses can be reconstructed using Wireshark.

The investigation also showed how DNS telemetry can provide useful destination context while remaining distinct from evidence of subsequent network connections.

### Finding 3 — HTTPS and TLS Encrypted Traffic Analysis

#### Investigation Context

The third investigation examined HTTPS communication using Wireshark, focusing on the TLS handshake and encrypted application data.

The objective was to distinguish information visible during TLS negotiation from application content protected by encryption.

#### Packet Analysis

Using the Wireshark display filter `tcp.port == 443`, I isolated TCP traffic involving HTTPS connections.

The capture showed communication between `192.168.0.84` and `23.211.177.8` over TCP port `443`.

The following TLS handshake messages were observed:

| Frame | TLS Message | Interpretation |
| --- | --- | --- |
| `2651` | Client Hello | Client initiates TLS negotiation |
| `2678` | Server Hello | Server responds with negotiated parameters |
| `2683` | Certificate Status, Server Key Exchange, Server Hello Done | Additional server-side handshake messages |
| `2685` | Client Key Exchange, Change Cipher Spec, Encrypted Handshake Message | Client proceeds with key exchange and encrypted handshake |
| `2703` | New Session Ticket, Change Cipher Spec, Encrypted Handshake Message | Server continues handshake completion |
| `2705` | Application Data | Encrypted application communication follows |

![HTTPS and TLS traffic analysis](evidence/03-https-tls-encrypted-traffic.png)

*Figure 3 — Wireshark capture showing TLS negotiation with `catalog.gamepass.com` visible in the Client Hello SNI, followed by encrypted application data. Frame 2721 displays a TLS 1.2 application-data record.*

The Client Hello also exposed a Server Name Indication (SNI) value:

`catalog.gamepass.com`

This demonstrated that destination hostname information was visible in the captured Client Hello.

The capture subsequently showed encrypted application data exchanged between the endpoints.

#### Encrypted Application Data

Frame `2721` contained a TLS application-data record sent from `23.211.177.8:443` to `192.168.0.84:61947`.

The selected packet displayed:

- TLS version field: `TLS 1.2`
- Record content type: `Application Data (23)`
- Encrypted application-data bytes
- Application protocol identified by Wireshark as HTTP/2

Because the application payload was encrypted, the screenshot did not expose plaintext HTTP requests, response bodies, or application content.

#### Security Relevance

TLS analysis allows security analysts to examine network metadata even when application payloads are encrypted.

Observable information can include source and destination addresses, ports, packet direction, handshake messages, and some negotiation metadata.

However, encrypted application data limits direct inspection of application-layer content without appropriate decryption capabilities.

#### Analyst Assessment

**Assessment: TLS negotiation and subsequent encrypted application communication identified.**

The observed handshake sequence and subsequent application-data records were consistent with an established encrypted connection.

The evidence demonstrated the distinction between visible network metadata and encrypted application content.

No conclusion about malicious activity was supported by this packet capture alone.