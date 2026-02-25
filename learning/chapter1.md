# Osi model
Layer 1 - Physical layer
---
- Identifies network's physical characteristics and structure.
- Technologies: cables, WiFi, repeaters, hubs.

Layer 2 - Data link layer
---
- Provides error detection and correction.
- Uses two sublayers: Media Access Control and Logical Link Control.
- Identifies the method by which media are accessed.
- Defines hardware addressing through the MAC sublayer.
- Addressing scheme - MAC addresses.
- Hop to Hop delivery.
- Construct is called a segment: |TCP|DATA|
- Technologies: access points, bridges, switches (L2 and L3).

Layer 3 - Network layer
---
- Handles the discovery of destination systems and addressing.
- Provides the mechanism by which data can be passed and routed
between networks.
- Addressing scheme - IP addresses.
- End to End delivery.
- Construct is called a packet: |IP|TCP|DATA|
- Technologies: router, IDS/IPS, firewall (L3 and L4).

Layer 4 - Transport layer
---
- Provides connection services between the sending and receiving devices.
- Ensures reliable data delivery.
- Manages flow control through buffering and windowing.
- Provides segmentation, error checking and service identification.
- Addressing scheme - ports. (65535 for both TCP and UDP).
- Service to Service delivery.
- Construct is called a frame: |MAC|IP|TCP|DATA|
- Technologies: load balancer (L4 and L7).

Layer 5 - Session layer
---
- Synchronizes the data exchange between applications on separate devices.
- Control protocols, tunneling protocols.

Layer 6 - Presentation layer
---
- Translates data from the format used by apps into one that can be
transmitted across the network.
- Handles encryption and decryption.
- Provides compression and decompression.

Layer 7 - Application layer
---
- Provides access to the network for applications.

# Common protocols
## IPsec
- Internet Protocol Security provides secure communications in the same network, as well as communication on systems on external networks.
- IPsec is composed of two seperate protocols:
    - Authentication Header, which provices the authentication and integrity    checking for data packets.
    - Encapsulating Security Payload, which provides encryption services.
- Internet Key Exchange (IKE) is a key management protocol used with IPsec to establish secure communication channels and negotiate cryptographic keys for encrypted VPN connections.
- IPsec operates in the network layer of the OSI model, thus you can secure practically all TCP/IP-related communications.
## GRE
- Generic Routing Encapsulation is a Cisco-created tunneling protocol that allows various network layer protocols to be encapsulated withing a virtual point-to-point link over an IP network. 
- Commonly used for creating VPNs with IPsec.
- GRE tunnels are often used in scenarios where direct Layer 2 connectivity between two endpoints is not possible or practical, such as sending private network traffic across the internet.
- First, data packets are encapsulated within GRE headers, which contain fields such as the protocol type, a key field (optional for security purposes) and a sequence number field (optional for packet ordering). Then a new IP header is added to the encapsulated packet, which specifies the source and destination IP addresses of the tunnel endpoints, and other IP header fields. After transmission and reaching the destination endpoint, packets are decapsulated and forwarded to the destination network
## Telnet
- Virtual terminal protocol, enabling sessions to be opened on a remote host and then commands can be executed on a remote host.
- Telnet is not secure, and as a result remote session functionality is achieved by using alternatives such as SSH.
## LDAP
- Lightweight Directory Access Protocol provides a mechanism to access and query directory services systems.
- In the context of the Network+ exam, these directory services are most likely to be UNIX/Linux or Active Directory based.
- Most LDAP interactoins are via utilities such as and authentication program (network logon) or locating a resource in the directory through a search utility.
## Syslog
- Most UNIX/Linux-based systems include the capability to write messages to log files via syslog. This provides a central means by which devices that otherwise could not write to a central repository can easily do so.
- Devices that use syslog include printers, routers and message receivers.
## Sip
- Session Initiation Protocol is used to initiate, maintain, modify and terminate multimedia elements such as voice, video and messaging. 
- It operates at the application layer.
- Uses both TCP and UDP.
- Includes security services, such as DoS prevention, authentication for both user-to-user and proxy-to-user, integrity protection, encryption and privacy services.

# Common ports

| Protocol    | Port    |
| ----------- | ------- |
| *TCP Ports* |         |
| FTP         | 20/21   |
| SSH/SFTP    | 22      |
| Telnet      | 23      |
| SMTP        | 25      |
| DNS         | 53      |
| HTTP        | 80      |
| LDAP        | 389     |
| HTTPS       | 443     |
| SMB         | 445     |
| SMTPS       | 587     |
| LDAPS       | 636     |
| SQL Server  | 1433    |
| RDP         | 3389    |
| SIP         | 5060(1) |
| *UDP Ports* |         |
| DNS         | 53      |
| DHCP srv.   | 67      |
| DHCP clnt.  | 68      |
| TFTP        | 69      |
| NTP         | 123     |
| SNMP        | 161(2)  |
| Syslog      | 514     |
| RDP         | 3389    |
| SIP         | 5060(1) |

# Network traffic types
- Unicast: most common traffic type, used for point-to-point communication between devices. Each packet is addressed to a unique destination IP address.
- Multicast: groups of network devices can send and receive data between the members of the group at one time (one-to-many). Grouping is established by configuring each device with the same multicast IP address. Devices join multicast groups if they are interested in them and receive packets destines for those groups.
- Anycast: traffic is one-to-nearest communication, or from a sender to the nearest of multiple receivers. Anycast is used for content delivery networks (CNDs), DNS servers and load balancers.
- Broadcast: one-to-all communication. Each packet in a broadcast is addressed to a special broadcast IP address, 255.255.255.255, or a layer 2 broadcast address, FF:FF:FF:FF:FF:FF. Broadcast is used for ARP requests, DHCP requests and some network discovery protocols. Broadcast traffic does not propagate across routers.
