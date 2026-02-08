## SSH Bruteforce – Traffic Analysis (My Observations)

### What was happening
In this capture, I simulated an SSH brute-force authentication attack using Hydra from a Kali Linux machine against a Windows virtual machine running an OpenSSH server.

Kali repeatedly attempted to authenticate to the SSH service by opening new connections for each username/password attempt, while the traffic was captured in Wireshark for analysis.

### What stood out in the traffic
While reviewing the traffic in Wireshark, the most obvious pattern was a **large number of SSH connection attempts** coming from the same source IP in a very short period of time.

Each attempt resulted in a **very short-lived TCP session**. Instead of establishing a single stable SSH connection (which is normal for legitimate users), the attacker continuously opened new connections and closed them almost immediately after authentication failed.

Another thing that stood out was the presence of multiple **TCP RST, ACK packets**, which were highlighted in red in Wireshark. These resets indicate that the SSH server was forcefully terminating the connection, most likely after rejecting invalid credentials.

I also noticed that every connection attempt used a **different source port** on the attacker side. This makes sense because Hydra creates a new TCP session for each password attempt, and the OS assigns a new ephemeral port every time.

The timing of the attempts was also very consistent. The connections were evenly spaced and highly repetitive, which strongly suggests automated behavior rather than manual login attempts.


### Key indicators
- Source IP 192.168.56.102
- Destination IP 192.168.56.101
- Service SSH
- Destination Port 22
- Behavior
  - High number of repeated SSH authentication attempts
  - Very short-lived SSH/TCP sessions
  - Frequent TCP RST packets after connection establishment
  - Constant change in attacker source ports
  - Regular and automated timing between attempts

### Conclusion
Based on the frequency, repetition, and TCP session behavior observed in the traffic, this activity is consistent with an **SSH brute-force attack** performed by an automated tool such as Hydra.

Even though the SSH payload itself is encrypted, the connection patterns alone are enough to clearly distinguish this activity from normal user behavior. In a real SOC environment, this would be considered a high-risk authentication attack and would require immediate investigation and response.
