# HomeLab Packet Sniffer Setup on Kali Linux Using Wireshark & tcpdump

## Overview

This project demonstrates how to set up and utilize a **packet sniffer** in a **HomeLab** environment using two powerful tools: **Wireshark** and **tcpdump**. A packet sniffer captures and analyzes network traffic to monitor, troubleshoot, and secure a network by identifying potential vulnerabilities, network issues, or malicious activity.

### Key Tools Used:
- **Wireshark**: A graphical network protocol analyzer used for capturing and analyzing network packets.
- **tcpdump**: A command-line packet analyzer for capturing and analyzing network traffic.

---

## Prerequisites

To complete this project, ensure you have the following:

1. **Kali Linux Virtual Machine** set up
2. **Wireshark** installed on Kali Linux.
3. **tcpdump** installed on Kali Linux.
4. A networked environment or devices to capture traffic from

---

## Setting Up the Kali Linux VM

### Step 1: Install Wireshark on Kali Linux

Wireshark can be installed using Kali Linux's package manager, `apt`.

1. **Update your package list**:
   `sudo apt update`

2. **Install Wireshark**:
    `sudo apt install wireshark`

3. During the installation, you may be prompted to allow non-superusers to capture packets. Choose "Yes" to allow this for convenience. You can later add your user to the Wireshark group if needed:
   `sudo usermod -aG wireshark $USER`

4. Restart the system (or log out and log back in) for the group changes to take effect.

5. Verify the installation by running Wireshark:
   `wireshark`

---

### Step 2: Install tcpdump on Kali Linux

`tcpdump` is a lightweight, command-line tool for capturing network traffic.

1. **Install tcpdump**:
   `sudo apt install tcpdump`

2. **Verify the installation**:
   `tcpdump --version`

---

## Capturing Network Traffic

### Wireshark

1. **Launch Wireshark**:
   Open Wireshark from the application menu or by running the following command in the terminal:
   wireshark

2. **Select the network interface** to capture traffic from (e.g., `eth0`, `wlan0`).

3. **Start the capture** by clicking on the **Start Capturing Packets** button.

4. **Apply filters** to narrow down the traffic. For example, to capture HTTP traffic, apply the following filter:
   `http`

Wireshark will display the live packet capture in real-time, showing detailed information for each captured packet.

---

### tcpdump

1. **Open a Terminal** window.

2. **Start capturing packets** using `tcpdump`. For example, to capture all traffic on the `eth0` interface and save it to a `.pcap` file:
   `sudo tcpdump -i eth0 -w capture.pcap`

3. To capture traffic on a specific port (e.g., HTTP on port 80):
   `sudo tcpdump -i eth0 port 80 -w capture_http.pcap`

4. **Stop the capture** by pressing `Ctrl+C` in the terminal.

Captured packets will be saved in the specified `.pcap` file. You can later open this file in Wireshark for more detailed analysis.

---

## Analyzing Captured Packets

### Analyzing in Wireshark

1. **Open the captured `.pcap` file** in Wireshark by going to **File > Open** and selecting the capture file.
   
2. **Apply filters** to view specific types of traffic:
   - HTTP traffic: `http`
   - DNS queries: `dns`
   - Traffic from a specific IP: `ip.addr == 192.168.1.1`

Wireshark will show you the packet details, including the packet headers and payloads for various protocols.

---

### Analyzing in tcpdump

To analyze captured packets with **tcpdump**, run the following command to read the `.pcap` file you saved:

sudo tcpdump -r capture.pcap

This will display the captured packets in the terminal. You can apply filters to the output for more focused analysis.

Example to filter for HTTP traffic:
sudo tcpdump -r capture.pcap 'tcp port 80'

---

## Example Use Cases

### 1. Monitor HTTP Traffic
To capture HTTP requests and responses, use **Wireshark** or **tcpdump** with a filter for port 80:
sudo tcpdump -i eth0 port 80 -w capture_http.pcap
In **Wireshark**, use the filter `http` to analyze the content of HTTP traffic.

### 2. Investigate DNS Queries
Capture DNS traffic to monitor domain name resolution requests:
sudo tcpdump -i eth0 port 53 -w capture_dns.pcap
In **Wireshark**, use the `dns` filter to examine the DNS query and response details.

### 3. Identify Suspicious Traffic
If you're looking for potential malicious activity, you can use filters like `tcp.flags.syn == 1 && tcp.flags.ack == 0` to capture potential SYN flood attempts or unusual patterns in the traffic.

---

## Understanding Wireshark and the OSI Model

Wireshark provides a detailed breakdown of network packets, which you can correlate with the OSI model to better understand how data flows through the layers. Here’s how each section of Wireshark maps to the OSI layers:

### 1. **Application Layer (Layer 7)**

Wireshark displays the high-level protocols that are used for communication between applications. This includes HTTP, FTP, DNS, SMTP, and other application-level protocols.

- **Example**: When you view HTTP traffic in Wireshark, you will see details like `GET /index.html HTTP/1.1` and response status codes (`200 OK`, `404 Not Found`), which are part of the application layer.

### 2. **Presentation Layer (Layer 6)**

This layer is responsible for translating data formats. While Wireshark doesn't directly display data encoding or encryption here, it shows protocols such as SSL/TLS used to encrypt data.

- **Example**: You might see `SSL` or `TLS` in the protocol column when capturing HTTPS traffic. Wireshark won't decrypt the data without the proper keys, but it will show the handshake and encryption information.

### 3. **Session Layer (Layer 5)**

The session layer manages communication sessions, including opening, closing, and managing the exchange of data. While Wireshark doesn’t directly display "session" in the way you might think, it shows details about connections and ports used, which is a reflection of session management.

- **Example**: For protocols like SMB or SQL, Wireshark captures information about the session (e.g., initial connection setup, session termination).

### 4. **Transport Layer (Layer 4)**

The transport layer ensures end-to-end communication and data integrity. Protocols like TCP and UDP are captured in this layer. Wireshark gives detailed information on the transport protocols, including the sequence number, acknowledgment, window size, etc.

- **Example**: For TCP, you’ll see the flags (`SYN`, `ACK`, `FIN`) and sequence numbers. UDP traffic is also visible here with source and destination ports.

### 5. **Network Layer (Layer 3)**

This layer is responsible for routing packets across networks and addressing through IP addresses. In Wireshark, you’ll see the source and destination IP addresses, and it will identify the protocol (such as IPv4 or IPv6).

- **Example**: An IPv4 header will display the source and destination IP addresses, routing information, and protocol (e.g., `ICMP`, `TCP`, or `UDP`).

### 6. **Data Link Layer (Layer 2)**

The data link layer handles physical addressing, error detection, and correction. In Wireshark, this is where you’ll see MAC addresses and Ethernet frames, which are used for communication within a local network.

- **Example**: Wireshark will display the Ethernet frame, including source and destination MAC addresses, as well as the protocol used (e.g., `IPv4` or `ARP`).

### 7. **Physical Layer (Layer 1)**

The physical layer transmits raw bitstreams over a physical medium like cables or wireless signals. Wireshark doesn’t directly display data related to the physical layer but will capture the data once it's been converted into packets by the data link layer.

- **Example**: The physical layer doesn’t have specific protocol information in Wireshark, but the presence of Ethernet or wireless protocols indicates the use of physical media.

### Conclusion

By understanding how each Wireshark section corresponds to the OSI model, you can more effectively troubleshoot, analyze, and understand network traffic. Wireshark allows you to drill down into each layer, giving you insight into how data flows through the network and what happens at each stage.

---

## Best Practices for Using Packet Sniffers

1. **Ensure Legal Compliance**: Make sure you have permission to capture traffic on the network. Capturing traffic on networks without consent may be illegal.
2. **Use Filters**: Always apply filters to capture only relevant data to prevent overwhelming your storage and improve performance.
3. **Monitor for Unusual Activity**: Look out for abnormal traffic patterns, such as excessive requests to a server or unusual ports being accessed, as this may indicate security issues.
4. **Protect Sensitive Data**: Be cautious when capturing packets on a network with sensitive data. Use encryption where possible and anonymize sensitive information.

---

## Troubleshooting and Tips

- **Wireshark not capturing traffic?** Ensure you have selected the correct network interface. You may need administrator privileges to capture on certain interfaces.
- **tcpdump permissions?** On Kali Linux, `tcpdump` requires `sudo` to access network interfaces. Ensure you're running the command as a superuser.
- **Wireshark capture lag**: If capturing too much traffic, consider reducing the capture size using filters, or capture only specific protocols.

---

## Conclusion

This packet sniffer project demonstrates how to set up **Wireshark** and **tcpdump** on **Kali Linux** to capture and analyze network traffic. Using these tools, you can gain valuable insights into your network's performance, troubleshoot connectivity issues, or detect suspicious activity. By applying filters and performing deeper analysis in **Wireshark**, you can extract meaningful information from the captured data and improve your network security knowledge.
