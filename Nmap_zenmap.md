# Lab Guide: Network Auditing with Nmap & Zenmap

## 1. Prerequisites & Environment
*   **Target Tool:** Nmap (Network Mapper) & Zenmap (GUI)
*   **Environment:** Linux (Ubuntu/Kali), Windows, or macOS
*   **Target for Testing:** `scanme.nmap.org` (An authorized testing server provided by the Nmap project)

### Installation (Linux)
```bash
sudo apt update
sudo apt install nmap zenmap-kbx
```

---

## 2. Nmap CLI: Strategic Scanning


Nmap operates by sending specially crafted packets to a target and analyzing the responses. Use the following commands based on the depth of information required for your practical.

### A. Simple Port Discovery
Checks the 1,000 most common ports to see which "doors" are open.
```bash
nmap scanme.nmap.org
```

### B. Aggressive Service & OS Analysis (Best for Labs)
Enables OS detection, version detection, script scanning, and traceroute in one command.
```bash
sudo nmap -A -v scanme.nmap.org
```

### C. Specific Port Range
If you need to scan all 65,535 ports (slow but thorough):
```bash
nmap -p- scanme.nmap.org
```

---

## 3. Zenmap GUI: Visual Analysis


Zenmap is the official graphical frontend for Nmap. It is useful for visualizing network structure and saving scan results for documentation.

1.  **Launch:** Open Zenmap from your application menu or type `sudo zenmap` in the terminal.
2.  **Target:** Enter the IP or hostname (e.g., `scanme.nmap.org`).
3.  **Profile:** Select **"Intense scan"** (this automatically populates the `-T4 -A -v` flags).
4.  **Scan:** Click the **Scan** button.
5.  **Analyze Tabs:**
    *   **Nmap Output:** The raw text results.
    *   **Ports / Hosts:** A clean table of services.
    *   **Topology:** A visual map of the network path.

---

## 4. Interpretation & Verification Table
The following table defines the verification levels based on the Nmap results:

| Scan Result | Meaning | Verification Level |
| :--- | :--- | :--- |
| **Port State: Open** | A service is actively listening and accepting connections. | **Active Attack Surface** |
| **Port State: Filtered** | A firewall or filter is blocking the probes; Nmap cannot tell if it's open. | **Firewall Protected** |
| **Service Version** | Identifies specific software (e.g., `Apache 2.4.41`). | **Vulnerability Target** |
| **OS Detection** | Identifies the kernel/OS (e.g., `Linux 5.x` or `Windows 10`). | **System Profiling** |

---

## 5. Practical Exam Cheat Sheet (Common Flags)

| Flag | Description | Use Case |
| :--- | :--- | :--- |
| `-sS` | **TCP SYN Scan** | Default stealthy scan (half-open). |
| `-sV` | **Version Detection** | Shows exactly what software is on a port. |
| `-O` | **OS Detection** | Attempts to guess the Operating System. |
| `-Pn` | **No Ping** | Scan a host that blocks ping requests. |
| `-p [port]` | **Specific Port** | Scan only a target port (e.g., `-p 80,443`). |

---

## 6. Sample Report Format
For your lab submission, use this format to document your findings:

*   **Host Status:** Up / Down
*   **Identified OS:** [e.g., Linux 4.x]
*   **Open Ports:** 22 (SSH), 80 (HTTP)
*   **Vulnerability Assessment:** Based on version detection, identify if any service is outdated or carries a known CVE.

---

> **Security Note:** Unauthorized scanning of networks you do not own can be illegal and easily detected by Intrusion Detection Systems (IDS). Always use authorized targets like `scanme.nmap.org` for practice.