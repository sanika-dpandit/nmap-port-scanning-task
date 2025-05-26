Nmap Network Scan Task

Objective
To perform a TCP SYN scan using Nmap and analyze network exposure and open ports on the local network.

Tools Used
- Nmap for port scanning
- Wireshark for packet capture and analysis

Files Included
- `scan_results.txt` – Text output of the Nmap scan
- `scan_results.xml` – XML format of the Nmap scan
- `scan_results.html` – HTML report (converted from XML)
- `nmap_scan_capture.pcapng` – Packet capture of the scan

Observations
- Hosts up: `192.168.91.1`, `192.168.91.144`
- Open ports found: 135, 139, 445, 902, 912, 3306, 5357

Security Risks Identified
| Port | Service         | Risk Description                                                                 |

| 135  | msrpc           | Can be used for lateral movement in Windows environments                         |
| 139  | netbios-ssn     | Exposes legacy Windows file sharing services                                     |
| 445  | microsoft-ds    | Vulnerable to SMB exploits like EternalBlue; commonly abused by ransomware       |
| 902  | iss-realsecure  | Used by VMware; could expose remote console access                               |
| 912  | apex-mesh       | Unknown service; needs investigation; could be a custom/legacy component         |
| 3306 | mysql           | Database should not be exposed to the network without strong access controls     |
| 5357 | wsdapi          | Windows device discovery protocol; often unnecessary in secure environments      |

Recommendations
- Use firewall rules to restrict access to unnecessary ports.
- Disable unneeded services (e.g., SMB, NetBIOS).
- Protect MySQL with authentication, SSL, and network restrictions.
- Investigate unknown services like `apex-mesh` and disable if not used.
- Monitor and log port activity regularly.

Wireshark Analysis
Captured TCP SYN packets (filtered using `tcp.flags.syn == 1 && tcp.flags.ack == 0`) showed the scanner sending connection attempts to various IPs and ports.
This confirms the behavior of a typical TCP SYN scan, where the scanner sends SYN packets and waits for SYN-ACK responses.

