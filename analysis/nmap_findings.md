## Nmap Port Scan – Traffic Analysis (My Observations)

### What was happening
In this capture, I simulated an internal reconnaissance attempt on a vertual machine where a Kali Linux machine scanned a Windows system to discover open services.

### What I noticed in the traffic
While analyzing the traffic in Wireshark, a clear port scanning pattern was visible. The attacker sent multiple TCP SYN packets to different destination ports without completing the TCP handshake.

One interesting detail was that even when a port was open (for example, port 80), it was probed more than once. I observed multiple SYN packets targeting port 80 with different source ports (e.g. 33490 and 33492). This indicates automated scanning behavior rather than a normal client connection.

I also noticed that closed ports were re-probed shortly after the initial attempt. Instead of stopping after a single failed connection, the scanner retried some ports, which further supports the idea of an aggressive or automated reconnaissance tool.

Another important observation was that the ports were not scanned sequentially. For example, the scan jumped from port 80 to 139 and then to 587. This non-linear scanning pattern is commonly used by tools like Nmap to avoid simple detection mechanisms.

### Key indicators
- Source IP: 192.168.56.102
- Destination IP: 192.168.56.101
- Protocol: TCP
- Scan type: SYN scan
- Behavior:
  - Repeated probes to the same port (even when open)
  - Retries on closed ports
  - Non-sequential port scanning

### Conclusion
Based on the frequency, repetition, and non-linear scanning pattern of the TCP SYN packets, this traffic is consistent with an Nmap-style port scan. This type of activity represents early-stage reconnaissance and would typically be flagged by a SOC for further investigation.
