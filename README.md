# Traffic-Analysis-Wireshark
SOC-style network traffic analysis using Wireshark (Nmap &amp; Hydra attacks):

This is a project I worked on to better understand how network attacks actually look "under the hood." Instead of just focusing on how to hack into a system, I wanted to see things from a SOC (Security Operations Center) perspective. My goal was to learn how to spot malicious patterns at the packet and session level.
# My Lab Setup
To keep everything safe and contained, I built a small lab environment:
 * Attacker: A Kali Linux VM.
 * Victim: A Windows VM running an OpenSSH server.
 * Networking: I used a Host-Only Adapter. I chose this specifically because it keeps the traffic clean and avoids the translation issues you get with NAT.
 * Tools: I used Nmap for scanning, Hydra for the brute-force part, and Wireshark to capture and analyze all the traffic.
# Attacks I Analyzed
I simulated two common scenarios to see what footprints they leave behind:
1. Nmap Port Scanning
 * What I did: I ran a SYN-based scan against the Windows machine to find open services.
 * The Goal: I wanted to see how reconnaissance looks in a packet capture and what makes it stand out from normal traffic.
 * PCAP File: nmap_recon_attack.pcapng.
2. SSH Brute-force (Hydra)
 * What I did: I used Hydra to launch automated login attempts against port 22.
 * The Goal: Even though SSH is encrypted, I wanted to analyze the session behavior, timing, and metadata to see how a SOC analyst could detect a brute-force attack in progress.
 * PCAP File: hydra_ssh_bruteforce.pcapng.
# Lessons Learned (The Hard Way)
Setting this up taught me a few practical lessons that weren't in the tutorials:
 * NAT vs. Host-Only: At first, I tried NAT, but it was a mess—some packets were hidden or translated. Switching to Host-Only made the captures much clearer and easier to study.
 * Windows & SSH: I had to manually enable and start the OpenSSH server on Windows. It reminded me that in the real world, misconfigured or missing services are often what slow down an investigation.
# SOC Insights
This project really showed me that attack tools are noisy. They create repetitive patterns that look nothing like a human user. By looking at metadata and connection behavior, you can catch these attacks early, even if you can't see the encrypted data inside the packets.
# Project Structure
 * analysis/: My detailed notes on Nmap and Hydra indicators.
 * pcaps/: The raw traffic files I captured.
 * screenshots/: Visuals of the scans and TCP conversations.
