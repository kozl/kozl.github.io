---
layout: post
title: "CCNA: конспект"
date: 2015-06-01 11:37:56 +0300
comments: true
categories: network, ccna
---
	
## CCNA Version 2 (200-120)


- [1.0 Operation of IP Data Networks](#operation-of-ip-data-networks)
	- [1.4 Describe the purpose and basic operation of the protocols in the OSI and TCP/IP models](#describe-the-purpose-and-basic-operation-of-the-protocols-in-the-osi-and-tcpip-models)
	- [1.6 Identify the appropriate media, cables, ports and connectors to connect Cisco network devices to other network devices and hosts in a LAN](#identify-the-appropriate-media-cables-ports-and-connectors-to-connect-cisco-network-devices-to-other-network-devices-and-hosts-in-a-lan)
- [2.0 LAN Switching Technologies](#lan-switching-technologies)
	- [2.3 Configure and verify initial switch configuration including remote access management](#configure-and-verify-initial-switch-configuration-including-remote-access-management)
	- [2.6 Configure and verify VLANs](#configure-and-verify-vlans)
	- [2.7 Configure and verify trunking on Cisco switches](#configure-and-verify-trunking-on-cisco-switches)
	- [2.8 Identify enhanced switching technologies](#identify-enhanced-switching-technologies)
- [3.0 IP Addressing (IPv4/IPv6)](#ip-addressing-ipv4ipv6)
	- [3.5 Describe IPv6 addresses](#describe-ipv6-addresses)
- [4.0 IP Routing Technologies](#ip-routing-technologies)
	- [4.1 Describe basic routing concepts](#describe-basic-routing-concepts)
	- [4.7 Configure and verify OSPF](#configure-and-verify-ospf)
	- [4.8 Configure and verify interVLAN routing (Router on a stick)](#configure-and-verify-intervlan-routing-router-on-a-stick)
	- [4.9 Configure SVI interfaces](#configure-svi-interfaces)
	- [4.10 Manage Cisco IOS Files](#manage-cisco-ios-files)s

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

#### 2.3 Configure and verify initial switch configuration including remote access management

```
hostname <HOSTNAME>
```

```
interface Vlan <NUM>
 no sh
 ip address <IP> <MASK>
```

```
ip default-gateway <GW>
```

```
username <USER> password <PASSWORD>
```

```
line vty 0 15
 login local
 transport input <PROTOCOL>
```

```
enable secret <PASSWORD>
```

```
service password-encryption
```

#### 2.6 Configure and verify VLANs

```
vlan <NUM>
 name <NAME>

interface <INT>
 switchport access vlan <VLAN>

interface <INT>
 switchport trunk encapsulation dot1q
 switchport trunk allowed vlan <VLANS>
 switchport trunk native vlan <VLAN>
 switchport mode trunk
```

#### 2.7 Configure and verify trunking on Cisco switches


{: .table}
| switchport modes      |  trunk active                          |
|-----------------------|----------------------------------------|
| access                |       -                                |
| trunk                 | trunk, dynamic auto, dynamic desirable |
| dynamic auto          | trunk, dynamic desirable               |
| dynamic desirable     | trunk, dynamic auto, dynamic desirable |
| nonegotiate           |       -                                |

#### 2.8 Identify enhanced switching technologies

**STP** - 802.1d

1. Выбирается корневой мост. Корневым становится коммутатор с меньшим Bridge ID = Priority + MAC. Приоритет может быть изменен администратором с целью влияния на процесс выбора
2. Определение корневых и выделенных портов (root и designated ports). Корневой выбирается по наилучшему пути до корневого моста. Наименьший по Root Path Cost.
3. Перевод остальных портов в состояние Blocking

Роли портов:

1. Root port
2. Designated port
3. Non-designated port
4. Disabled port

Состояния портов:

1. Blocking
2. Listening
3. Learning
4. Forwarding


**RSTP** - 802.1w, ускоренная сходимость за счёт наличия альтернативных путей до корневого моста и меньшего количества состояний порта

Роли портов:

1. Root port
2. Designated port
3. Alternative port
4. Backup port

Состояния портов:

1. Discarding
2. Learning
3. Forwarding

**PVST**, **PVST+** - проприетарные протоколы Cisco, для каждого VLAN строят отдельное дерево, могут использовать транки на ISL. Основаны на STP.

**Rapid PVST**, **Rapid PVST+** - основаны на RSTP
**Portfast**
: настраивается на access портах, сразу переводит порт в состояние Forwarding
**BPDU Guard**
: выключает порт при получении BPDU

**MSTP** - 802.1s

**Etherchannel**

Существует три варианта конфигурирования аггрегированного канала:

1. Статическая настройка
2. Настройка при помощи протокола LACP - стандартный протокол
3. Настройка при помощи протокола PagP - проприетарный протокол

```
interface range fa0/10 - 13
 channel-group 3 mode on
```

Режимы (modes):

1. *on* - статическая настройка
2. *active* - включить LACP
3. *passive* - включить LACP при получении сообщения LACP
4. *desirable* - включить PagP
5. *auto* - включить PagP при получении сообщения PagP

```
show etherchannel summary
```
```
show etherchannel port-channel
```

Основная проблема - балансировка нагрузки. Вовсе не факт, что все каналы в аггрегированном канале будут загружены равномерно. Это определяется политикой балансировки по каналам. Она может быть:

```
sw1(config)# port-channel load-balance ?
  dst-ip       Dst IP Addr
  dst-mac      Dst Mac Addr
  src-dst-ip   Src XOR Dst IP Addr
  src-dst-mac  Src XOR Dst Mac Addr
  src-ip       Src IP Addr
  src-mac      Src Mac Addr
```

**PVST configuration**

```
spanning-tree mode rapid-pvst
spanning-tree vlan <VLANS>
```

Настройка Portfast

```
sw(config)#interface fa0/1
sw(config-if)# spanning-tree portfast
```
Настройка Bridge priority

```
spanning-tree vlan vlan-id root primary
spanning-tree vlan vlan-id root secondary
spanning-tree vlan <VLAN> priority <PRI>
```


Настройка стоимости портов

```
sw(config-if)# spanning-tree cost <COST>
```

```
show spanning tree
show spanning tree detail
```

### 3.0 IP Addressing (IPv4/IPv6)

#### 3.5 Describe IPv6 addresses

IPv6

- большее адресное пространство
- большая эффективность маршрутизации
- автоконфигурация
- IPsec
- нет броадкастов
- нет контрольных сумм заголовков
- в заголовок добавлен Flow Label

Формат представления: x:x:x:x:x:x:x:x, где x - шестнадцатеричное число

- нули слева записывать необязательно
- последовательность 0 может быть заменена :: (но только один раз)
- 48-bit global prefix, 16-bit subnet ID and 64-bit interface ID

Виды адресов:

* Unicast (one-to-one)
 - global: 2000::/3
 - link-local: FE80::/10
 - loopback: ::1
 - reserved
* Multicast (one-to-many)
* Anycast (one-to-nearest)

Global Unicast адреса:

- на каждый интерфейс может быть назначено несколько адресов любого типа
- каждый интерфейс имеет по меньшей мере один loopback и один link-local адрес

Назначение IPv6 адреса:

- статическое, указываем полный адрес
- статическое, указываем сеть, младшие 64 бита генерирует EUI-64 как функцию от MAC интерфейса
- автоконфигурация
- DHCPv6

**EUI-64**

Метод генерации младших 64 бит адреса, используется MAC адрес, между старшими и младшими 3 байтами которого вставляется последовательность FF FE


**Переход от IPv4 к IPv6**

- Dual stack
- 6to4 tunneling
- ISATAP
- Teredo

### 4.0 IP Routing Technologies

#### 4.1 Describe basic routing concepts

**Process Switching** - решение о маршрутизации и проверка RIB осуществляется ЦП для каждого пакета в отдельности
**Fast Switching** - ЦП задействуется для принятия решения о первом пакете в потоке, далее эта информация кешируется и остальные пакеты маршрутизируются на основе данных в кеше.
**CEF** - строится FIB и Adjacency table, в которых хранятся заранее рассчитанные данные, необходимые для машрутизации (интерфейс, MAC адрес следующего узла, IP адрес и т.п.). Эти таблицы хранятся в быстрой памяти, поиск по которой занимает намного меньше времени и может быть реализован аппаратно.

#### 4.7 Configure and verify OSPF

Перимущества OSPF с одной зоной: роутеры одной зоны имеют полную информацию о топологии этой зоны, используются наиболее оптимальные маршруты.

Недостатки: при кол-ве роутеров более 50 (в некоторых источниках более 25-30) требования к памяти и вычислительным ресурсам каждого маршрутизатора возрастают и рекомендуется использовать несколько зон. Кроме этого это позволит осуществлять автосуммирование на ABR.

**Конфигурирование OSPFv2**

```
router ospf <PID>
router-id <ROUTER ID>
network <A.B.C.D> <REVERSE MASK> area <AREA>
(-if) ip ospf <PID> area <AREA>
```
```
show ip protocol
show ip ospf
show ip ospf neighbor
show ip ospf interface
show ip ospf database
```

**Конфигурирование OSPFv3**

```
router ospfv3 <PID>
router-id <ROUTER ID>
(-if) ipv6 router ospfv3 <PID> area <AREA>
```

**Passive interface** - не рассылает OSPF пакеты, но этот интерфейс анонсируется в LSA

**типы LSA**

{: .table}
| Номер LSA | Название LSA             | Кто отправляет                         | Область распространения | Назначение                                                                                                                                |
|-----------|--------------------------|----------------------------------------|-------------------------|-------------------------------------------------------------------------------------------------------------------------------------------|
| LSA 1     | Router LSA               | Каждый маршрутизатор                   | Внутри зоны             | Описывает маршрутизатор, все имеющиеся у него интерфейсы, их стоимость и соседей на каждом интерфейсе                                     |
| LSA 2     | Network LSA              | DR (в сетях со множественным доступом) | Внутри зоны             | Описывает широковещательную сеть, указаны все маршрутизаторы, подключённые к ней, DR, маска сети                                          |
| LSA 3     | Network Summary LSA      | ABR                                    | AS                      | Содержит информацию о сетях вне зоны и их стоимость. Не содержит информации о топологии сети. Сети могут быть просуммированы.             |
| LSA 4     | ASBR Summary LSA         | ABR                                    | AS                      | Необходим для того чтобы сообщить всем маршрутизатором о том, где находится ASBR, содержит суммарную информацию о состоянии каналов ASBR. |
| LSA 5     | AS External LSA          | ASBR                                   | AS                      | Объявление о состоянии внешних по отношению к AS каналов                                                                                  |
| LSA 7     | AS External LSA for NSSA | ASBR                                   | NSSA area               | Аналогично LSA 5, но может распространяться только в NSSA зоне.                                                                           |

#### 4.8 Configure and verify interVLAN routing (Router on a stick)

Настройка сабинтерфейса на 101 влане, на маршрутизаторе:

```
interface fa1/0.101
 encapsulation dot1Q 101
 ip address 192.168.101.1 255.255.255.0
```

#### 4.9 Configure SVI interfaces

```
interface vlan 101
 no shutdown
 ip address 192.168.101.254 255.255.255.0
```

#### 4.10 Manage Cisco IOS Files

**Типы памяти**

- **ROM** - ПЗУ, хранит код bootstrap программы - ROMmon и POST
- **Flash** - перезаписываемая, в ней хранится образ IOS
- **NVRAM** - перезаписываемая, в ней хранятся конфиги
- **ROM** - оперативная память

**Последовательность загрузки**

- включается питание
- запускается bootstrap программа из ROM, которая запускает POST
- проверяется регистр confreg и определяется место, откуда будет загружаться образ IOS. По умолчанию (значение 2102), проверяется startup-config на наличие указаний откуда нужно грузить образ, если они отсутствуют, образ грузится из Flash памяти. Если образ отсутствует во Flash памяти, bootstrap пытается загрузить образ с TFTP сервера.
- образ загружается в RAM
- если startup-config отсутствует, запускается мастер конфигурации setup

**Установка/удаление лицензий**

```
show license all
license install <URI>
license clear <URI>
license save <URI>
```

