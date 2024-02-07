---
layout: post
title:  "Learning Nmap - Port Scans"
date:   2024-02-06 14:00:00 +0000
categories: NMap - Basic Port Scans
image: 
---

Scan Ports
===
The next step would be checking which ports are open and listening and which ports are closed. Therefore, in this room and the next one, we focus on port scanning and the different types of port scans used by nmap. This room explains:

TCP connect port scan

TCP SYN port scan

UDP port scan

Moreover, we discuss the different options to specify the ports, the scan rate, and the number of parallel probes.

TCP and UDP Ports
===
In the same sense that an IP address specifies a host on a network among many others, a TCP port or UDP port is used to identify a network service running on that host. A server provides the network service, and it adheres to a specific network protocol. Examples include providing time, responding to DNS queries, and serving web pages. A port is usually linked to a service using that specific port number. For instance, an HTTP server would bind to TCP port 80 by default; moreover, if the HTTP server supports SSL/TLS, it would listen on TCP port 443. (TCP ports 80 and 443 are the default ports for HTTP and HTTPS; however, the webserver administrator might choose other port numbers if necessary.) Furthermore, no more than one service can listen on any TCP or UDP port (on the same IP address).

At the risk of oversimplification, we can classify ports in two states:
```
Open port indicates that there is some service listening on that port.

Closed port indicates that there is no service listening on that port.
```

However, in practical situations, we need to consider the impact of firewalls. For instance, a port might be open, but a firewall might be blocking the packets. Therefore, Nmap considers the following six states:
```
Open: 
indicates that a service is listening on the specified port.

Closed: 
indicates that no service is listening on the specified port, although the port is accessible. 
By accessible, we mean that it is reachable and is not blocked by a firewall or other security appliances/programs.

Filtered: 
means that Nmap cannot determine if the port is open or closed because the port is not accessible. This state is usually due to a firewall preventing Nmap from reaching that port. Nmap’s packets may be blocked from reaching the port; alternatively, the responses are blocked from reaching Nmap’s host.

Unfiltered: 
means that Nmap cannot determine if the port is open or closed, although the port is accessible. This state is encountered when using an ACK scan -sA.

Open|Filtered: 
This means that Nmap cannot determine whether the port is open or filtered.

Closed|Filtered: 
This means that Nmap cannot decide whether a port is closed or filtered.
```


Which service uses UDP port 53 by default?
```
DNS
```

Which service uses TCP port 22 by default?
```
SSH
```

How many port states does Nmap consider?
```
6
```

Which port state is the most interesting to discover as a pentester?
```
Open
```

 TCP Flags
 ===
 Nmap supports different types of TCP port scans. To understand the difference between these port scans, we need to review the TCP header. The TCP header is the first 24 bytes of a TCP segment. The following figure shows the TCP header as defined in RFC 793. This figure looks sophisticated at first; however, it is pretty simple to understand. In the first row, we have the source TCP port number and the destination port number. We can see that the port number is allocated 16 bits (2 bytes). In the second and third rows, we have the sequence number and the acknowledgement number. Each row has 32 bits (4 bytes) allocated, with six rows total, making up 24 bytes.

 ![TCP Header (RFC793)](/assets/img/tcp-header-rfc793.png)

 In particular, we need to focus on the flags that Nmap can set or unset. We have highlighted the TCP flags in red. Setting a flag bit means setting its value to 1. From left to right, the TCP header flags are:
```
URG: 
Urgent flag indicates that the urgent pointer filed is significant. The urgent pointer indicates that the incoming data is urgent, and that a TCP segment with the URG flag set is processed immediately without consideration of having to wait on previously sent TCP segments.

ACK: 
Acknowledgement flag indicates that the acknowledgement number is significant. It is used to acknowledge the receipt of a TCP segment.
PSH: Push flag asking TCP to pass the data to the application promptly.

RST: 
Reset flag is used to reset the connection. Another device, such as a firewall, might send it to tear a TCP connection. This flag is also used when data is sent to a host and there is no service on the receiving end to answer.

SYN: 
Synchronize flag is used to initiate a TCP 3-way handshake and synchronize sequence numbers with the other host. The sequence number should be set randomly during TCP connection establishment.

FIN: 
The sender has no more data to send.
```


What 3 letters represent the Reset flag?
```
RST
```

Which flag needs to be set when you initiate a TCP connection (first packet of TCP 3-way handshake)?
```
SYN
```

TCP Connect Scan
===
TCP connect scan works by completing the TCP 3-way handshake. In standard TCP connection establishment, the client sends a TCP packet with SYN flag set, and the server responds with SYN/ACK if the port is open; finally, the client completes the 3-way handshake by sending an ACK.

```
TCP 3-Way Handskake

             
            --> SYN -->                        
 A        <-- SYN, ACK <--  Hosts
             --> ACK --> 

        CASE: TCP port is open
```
We are interested in learning whether the TCP port is open, not establishing a TCP connection. Hence the connection is torn as soon as its state is confirmed by sending a RST/ACK. You can choose to run TCP connect scan using ```-sT```.

```
      namp -sT TARGET
             
            --> SYN -->                        
 A        <-- SYN, ACK <--    Hosts
             --> ACK --> 
             --> RST, ACK --> 

        CASE: TCP port is open
```
It is important to note that if you are ```not a privileged user (root or sudoer)```, a TCP connect scan is the only possible option to discover open TCP ports.

In the following Wireshark packet capture window, we see Nmap sending TCP packets with SYN flag set to various ports, 256, 443, 143, and so on. \

By default, Nmap will attempt to connect to the 1000 most common ports. ```A closed TCP port``` responds to a SYN packet with ```RST/ACK``` to indicate that it is not open. This pattern will repeat for all the closed ports as we attempt to initiate a TCP 3-way handshake with them.

We notice that port 143 is open, so it replied with a SYN/ACK, and Nmap completed the 3-way handshake by sending an ACK. The figure below shows all the packets exchanged between our Nmap host and the target system’s port 143. The first three packets are the TCP 3-way handshake being completed. Then, the fourth packet tears it down with an RST/ACK packet.


To illustrate the ```-sT``` (TCP connect scan), the following command example returned a detailed list of the open ports.

```
pentester@TryHackMe$ nmap -sT MACHINE_IP

Starting Nmap 7.60 ( https://nmap.org ) at 2021-08-30 09:53 BST
Nmap scan report for MACHINE_IP
Host is up (0.0024s latency).
Not shown: 995 closed ports
PORT    STATE SERVICE
22/tcp  open  ssh
25/tcp  open  smtp
80/tcp  open  http
111/tcp open  rpcbind
143/tcp open  imap
MAC Address: 02:45:BF:8A:2D:6B (Unknown)

Nmap done: 1 IP address (1 host up) scanned in 0.40 seconds

```


Note that we can use ```-F``` to enable ```fast mode``` and decrease the number of scanned ports ```from 1000 to 100 most common ports```.

It is worth mentioning that the ```-r``` option can also be added to scan the ports in consecutive order instead of random order. This option is useful when testing whether ports open in a consistent manner, for instance, when a target boots up.


Launch the VM. Open the AttackBox and execute nmap -sT MACHINE_IP via the terminal. A new service has been installed on this VM since our last scan. Which port number was closed in the scan above but is now open on this target VM?
```
110
```

What is Nmap’s guess about the newly installed service?
```
POP3
```

 TCP SYN Scan
 ===
 ```Unprivileged users``` are limited to connect scan. However, the default scan mode is ```SYN``` scan, and it requires a ```privileged (root or sudoer) user to run it```. SYN scan does not need to complete the TCP 3-way handshake; instead, it tears down the connection once it receives a response from the server. Because we didn’t establish a TCP connection, this decreases the chances of the scan being logged. We can select this scan type by using the ```-sS``` option. The figure below shows how the TCP SYN scan works without completing the TCP 3-way handshake.

 ```
nmap -sS TARGET

             
            --> SYN -->                        
 A        <-- SYN, ACK <--  Hosts
             --> RST --> 

        CASE: TCP port is open
```

The following screenshot from Wireshark shows a TCP SYN scan. The behaviour in the case of closed TCP ports is similar to that of the TCP connect scan.

To better see the difference between the two scans, consider the following screenshot. In the upper half of the following figure, we can see a TCP connect scan ```-sT``` traffic. Any open TCP port will require Nmap to complete the TCP 3-way handshake before closing the connection. 

In the lower half of the following figure, we see how a SYN scan ```-sS``` does not need to complete the TCP 3-way handshake; instead, Nmap sends an RST packet once a SYN/ACK packet is received.


TCP SYN scan is the default scan mode when running Nmap as a privileged user, running as root or using sudo, and it is a very reliable choice. It has successfully discovered the open ports you found earlier with the TCP connect scan, yet no TCP connection was fully established with the target.
```
pentester@TryHackMe$ sudo nmap -sS 10.10.197.86

Starting Nmap 7.60 ( https://nmap.org ) at 2021-08-30 09:53 BST
Nmap scan report for 10.10.197.86
Host is up (0.0073s latency).
Not shown: 994 closed ports
PORT    STATE SERVICE
22/tcp  open  ssh
25/tcp  open  smtp
80/tcp  open  http
110/tcp open  pop3
111/tcp open  rpcbind
143/tcp open  imap
MAC Address: 02:45:BF:8A:2D:6B (Unknown)

Nmap done: 1 IP address (1 host up) scanned in 1.60 seconds
```


Launch the VM. Some new server software has been installed since the last time we scanned it. On the AttackBox, use the terminal to execute nmap -sS 10.10.197.86. What is the new open port?
```

```