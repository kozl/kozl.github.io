---
layout: post
title: "CCNA: конспект"
date: 2015-06-01 11:37:56 +0300
comments: true
categories: network, ccna
---

## CCNA Version 2 (200-120)

### 1.0 Operation of IP Data Networks

#### 1.4 Describe the purpose and basic operation of the protocols in the OSI and TCP/IP models

**OSI** 

{: .table}
| Layer | Name         | Protocols                                        |
|-------|--------------|--------------------------------------------------|
| 7     | Application  | DNS, NTP, FTP, HTTP, DHCP, SMTP, Telnet, NFS     |
| 6     | Presentation | MIME, SSL/TLS                                    |
| 5     | Session      | NETBIOS, RPC, SOCKS                              |
| 4     | Transport    | TCP, UDP, SCTP                                   |
| 3     | Network      | IP, ICMP, IGMP, IPsec, OSPF, RIP, EIGRP, IS-IS   |
| 2     | Data link    | L2TP, PPP, Ethernet, CDP, HDLC, ARP, Frame Relay |
| 1     | Physical     | DSL, USB                                         |

.

**DoD**

{: .table}
| Name            | Protocols                                        |
|-----------------|--------------------------------------------------|
| Application     | DNS, NTP, FTP, HTTP, DHCP, SMTP, Telnet, NFS     |
| Transport layer | TCP, UDP, SCTP                                   |
| Internet layer  | IP, ICMP, IGMP, IPsec, OSPF, RIP, EIGRP, IS-IS   |
| Link layer      | L2TP, PPP, Ethernet, CDP, HDLC, ARP, Frame Relay |

.

#### 1.6 Identify the appropriate media, cables, ports and connectors to connect Cisco network devices to other network devices and hosts in a LAN

Схемы распайки: TIA **T568A**, и EIA **T568B**

{% img http://www.bitsbyjohn.com/wp-content/uploads/2013/06/UTP-Pairing.png %}

{: .table}
| TX: 1,2 RX: 3,6     | RX: 1,2 TX: 3,6 |
|---------------------|-----------------|
| Сетевые адаптеры ПК | Коммутаторы     |
| Маршрутизаторы      |                 |
| Точки доступа       |                 |
| Сетевые принтеры    |                 |

Однотипные устройства подключаются перекрещенным кабелем (crossover)

Cisco Console Rollover cable

{% img http://wiki.artisansasylum.com/images/b/b6/Wiring-rollover.png %}

### 2.0 LAN Switching Technologies

#### 

