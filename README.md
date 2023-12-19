# Packet-Capture-Tools

## Embedded Packet Capture (EPC)

Allows for packet data to be captured at various points in the CEF packet-processing path: flowing through, to and from a Cisco router.

Support varies based on models.

Can export packet information as a packet capture (PCAP) file to allow for further examination through other tools.

## Embedded Wireshark

Allows for packet data to be captured at various points in the packet-processing path: flowing through, to and from a Catalyst 4500 switch (with a Sup7E running 3.3SG or later) and Catalyst 3850 switch.

Is supported only on switches running IP Base image or IP Services image.

Packets captured in the output direction of an interface might not reflect the changes made by switch rewrite (includes TTL, VLAN tag, CoS, checksum, MAC addresses, DSCP, precedent, UP, and so on).

## Mini-Protocol Analyzer (MPA)

Captures network traffic from a SPAN session and stores the captured packets in a local memory buffer.

Allows for packet data to be captured at various points in a hardware-forwarding device like Cisco 7600, Catalyst 6500 and ME6500 platforms.

Limits the captured packets to selected VLANs, ACLs, or MAC addresses.

Captures packet information in a libpcap file, which is supported by many packet analysis and sniffer programs.

## EPC and MPA config example

### EPC

Define Capture Buffer: This is where the captured packets will be stored. You need to define the size of the capture buffer.
```
Router# configure terminal
Router(config)# monitor capture buffer MYBUFFER size 1024 max-size 1518 linear
```
In the above, 'MYBUFFER' is the name of the buffer, '1024' is the size in KBs, '1518' is the maximum packet size that will be captured.

Define Capture Point: This is where you specify what traffic to capture.
```
Router(config)# monitor capture point ip cef MYPPOINT GigabitEthernet 0/0 both
```
In the above, 'MYPPOINT' is the capture point name, 'GigabitEthernet 0/0' is the interface name, 'both' means capture both ingress and egress traffic.

Associate Capture Point with Capture Buffer: This is where you link the buffer with the capture point.
```
Router(config)# monitor capture point associate MYPOINT MYBUFFER
```
Start the Capture: After all the configurations, you can start capturing packets.
```
Router# monitor capture point start MYPOINT
```
To stop the capture, use this command:
```
Router# monitor capture point stop MYPOINT
```
Export the Capture: After you have captured the packets, you can export them for analysis.
```
Router# monitor capture buffer MYBUFFER export ftp://username:password@FTP_SERVER/PATH
```
Please replace 'username', 'password', 'FTP_SERVER', and 'PATH' with your own FTP server details.

Remember to replace 'MYBUFFER', 'MYPPOINT', and 'GigabitEthernet 0/0' with the names of your capture buffer, capture point, and interface respectively.

Note: The commands and features available may vary depending on your Cisco IOS version and device model. Always refer to your device's specific documentation for exact details.

### MPA

Define an MPA collection
```
Router(config)#mpa collection my_collection
```
Specify the interface and traffic direction
```
Router(config-mpa)#interface GigabitEthernet 0/0 in
```
Enable the collection
```
Router(config-mpa)#enable
```
Display the results
```
Router#show mpa collection my_collection
```
Please note that the MPA feature may not be available in all Cisco IOS software versions or all router models. Always refer to your device's specific documentation for exact details.
