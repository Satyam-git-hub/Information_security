# Network Utilities Lab Guide

## 1. Connectivity Testing (`ping`)
The **Packet Internet Groper (PING)** uses ICMP (Internet Control Message Protocol) Echo Request packets to verify if a remote host is reachable.

*   **Syntax:** `ping [hostname/IP]`
*   **Key Information:**
    *   **Latency (RTT):** Time taken for the packet to return (measured in ms).
    *   **Packet Loss:** Indicates network congestion or firewall blocking.
    *   **TTL (Time to Live):** Helps identify the operating system of the remote host (e.g., Linux often defaults to 64, Windows to 128).



---

## 2. Interface Configuration (`ipconfig` / `ifconfig`)
These tools display the current TCP/IP network configuration of a machine.

| Operating System | Command | Key Output |
| :--- | :--- | :--- |
| **Windows** | `ipconfig` | IPv4 Address, Subnet Mask, Default Gateway. |
| **Linux / macOS** | `ifconfig` or `ip addr` | Interface names (eth0, wlan0), MAC address (HWaddr), IP status. |

*   **Use Case:** Use `ipconfig /all` on Windows to see DNS servers and DHCP lease information. Use `ifconfig` to verify if a network interface is "UP" or "DOWN."

---

## 3. Path Discovery (`tracert` / `traceroute`)
This utility maps the path a packet takes to reach a destination, showing every router (hop) along the way.

*   **Syntax (Windows):** `tracert [hostname/IP]`
*   **Syntax (Linux):** `traceroute [hostname/IP]`
*   **Mechanism:** It increments the TTL value for each packet. When TTL reaches 0, the router sends back a "Time Exceeded" message, revealing its identity.
*   **Use Case:** Identifying exactly where a connection is failing in a complex network.



---

## 4. Address Resolution Protocol (`arp`)
The `arp` command displays and modifies the IP-to-Physical (MAC) address translation tables used by the Address Resolution Protocol.

*   **Command:** `arp -a`
*   **Purpose:** Maps Layer 3 addresses (IP) to Layer 2 addresses (MAC) on the local network segment.
*   **Security Context:** This is used to detect **ARP Spoofing** attacks where an attacker associates their MAC address with the IP of the default gateway.



---

## 5. Network Statistics (`netstat`)
`netstat` provides detailed information about active network connections, routing tables, and interface statistics.

*   **Common Flags:**
    *   `netstat -a`: Displays all active connections and listening ports.
    *   `netstat -n`: Displays addresses and port numbers in numerical form (skips DNS lookup).
    *   `netstat -p [protocol]`: Filters by TCP or UDP.
*   **Use Case:** Checking if a specific service (like a web server on port 80) is actually listening for connections.

---

## 6. Directory Service (`whois`)
`whois` is a query and response protocol used for querying databases that store the registered users or assignees of an Internet resource, such as a domain name or an IP address block.

*   **Syntax:** `whois google.com`
*   **Output:** Registrar information, creation/expiry dates, and administrative contact details.
*   **Note:** On many modern Linux distributions, you may need to install it first via `sudo apt install whois`.

---

## Summary Reference Table

| Tool | Primary Purpose | Layer |
| :--- | :--- | :--- |
| **ping** | Check host reachability/latency. | Layer 3 (Network) |
| **ipconfig** | View/Manage local IP configuration. | Layer 3 (Network) |
| **tracert** | Trace the route to a remote host. | Layer 3 (Network) |
| **arp** | View IP-to-MAC address mapping. | Layer 2 (Data Link) |
| **netstat** | View active ports and connections. | Layer 4 (Transport) |
| **whois** | Query domain ownership information. | Application Layer |

---

> **Practical Tip:** When troubleshooting, follow the "Bottom-Up" approach. Use `ipconfig` to check your own IP, `ping` the gateway, then use `tracert` to find where the external connection breaks.

Are you planning to use these tools primarily for network troubleshooting or for an initial security reconnaissance phase?