# TCPDUMP-packet-capture
---

# [Lab video_TCPDUMP packet capture](https://youtu.be/YMm08l1yKA0)

---
[View TCPDUMP and WIRESHARK comparison diagram](./docs/TCPDUMP-WIRESHARK_comparison_diagram.pdf)
---
Using tcpdump to capture and analyze live network traffic from a Linux virtual machine. Starting with the user account 'analyst'.

Walkthrough:

Firstly; I identified the network interfaces to capture network packet data. The Ethernet network interface is identified by the entry with the eth prefix.

Secondly; I used tcpdump to filter live network traffic, this command will run tcpdump with the following options:

   -i eth0: _Capture data specifically from the eth0 interface_.  
   -v: _Display detailed packet data_.  
   -c5: _Capture 5 packets of data_.

We can identify some of the properties that tcpdump outputs for the packet capture data:  
In the example data,  
At the start of the packet output, tcpdump reported that it was listening on the eth0 interface, and it provided information on the link type and the capture size in bytes.  
On the next line, the first field is the packet's timestamp, followed by the protocol type, IP.  
The verbose option, -v, has provided more details about the IP packet fields, such as TOS, TTL, offset, flags, internal protocol type (in this case, TCP (6)), and the length of the outer IP packet in bytes  
In the next section, the data shows the systems that are communicating with each other.  
The remaining data filters the header data for the inner TCP packet.  
The flags field identifies TCP flags. In this case, the P represents the push flag and the period indicates it's an ACK flag. This means the packet is pushing out data.  
The next field is the TCP checksum value, which is used for detecting errors in the data.

Thirdly; Capture network traffic using tcpdump, the command runs tcpdump in the background with the following options:

   -i eth0: _Capture data from the eth0 interface_.  
   -nn: _Do not attempt to resolve IP addresses or ports to names. This is best practice from a security perspective, as the lookup data may not be valid. It also prevents malicious actors from being alerted to an investigation_  
   -c9: _Capture 9 packets of data and then exit_.  
   port 80: _Filter only port 80 traffic. This is the default HTTP port_.  
   -w capture.pcap: _Save the captured data to the named file_.  
   &: _This is an instruction to the Bash shell to run the command in the background_.
Next I used curl to generate some HTTP (port 80) traffic.  
Then verified that the packet data has been captured.

Finally: I used the tcpdump command to filter the packet header data from the capture.pcap capture file. I ran the command with the following options:

   -nn: _Disable port and protocol name lookup_.  
   -r: _Read capture data from the named file_.  
   -v: _Display detailed packet data_.

The -nn switch was specified again here, as we want to make sure tcpdump does not perform name lookups of either IP addresses or ports, since this can alert threat actors.
Furthermore, I used the tcpdump command to filter the extended packet data from the capture.pcap capture file with one other option.  
   -X: _Display the hexadecimal and ASCII output format packet data. Security analysts can analyze hexadecimal and ASCII output to detect patterns or anomalies during malware analysis or forensic analysis_.
