---
layout: post
title: "Типовая настройка коммутатора"
date: 2015-05-18 17:11:57 +0300
comments: true
categories: network, ccna
---

- [1. Базовая настройка](#section)
- [2. Безопасность](#section-1)
- [3. STP](#stp)
- [4. EtherChannel](#etherchannel)
- [5. Дополнительно](#section-2)


### 1. Базовая настройка
- Удалить старый конфиг
- Настроить hostname, dns lookup zone
- Отключить DNS запросы при ошибках в консоли
- Настроить консольный терминал
- Настроить NTP и таймзону
- Настроить удалённый доступ в MNGMT vlan
- Настроить логирование

```
erase startup-config
```

```
hostname <hostname>
ip domain name <domain>
```

```
no ip domain lookup
```

```
line con 0
 exec-timeout 0 0
 privilege level 15
 logging synchronous
 login
 password <PASS>
```

```
ntp peer <IP>
clock timezone MSK +3
```

```
interface Vlan 2
 no shutdown
 ip address x.x.x.x x.x.x.x
enable secret <PASS>
line vty 0 15
 transport input ssh/telnet
 login local
 exec-timeout 0 0
ip default-gateway <IP>
```

```
logging <IP>
service timestamps log datetime msec localtime

logging buffered 32000 informational
logging rate-limit all 10
```

### 2. Безопасность

- Запретить хранить пароли в конфигах в plaintext
- Интерфейсы к машинам перевести в режим access, к другим коммутаторам - в trunk
- Неиспользуемые интерфейсы отключить
- Для транковых интерфейсов настроить native vlan отличный от 1-го
- (опционально) Настроить на access интерфейсах port-security
- (опционально) Указать разрешенные vlan для транка


```
service password-encryption
```

```
interface range Gi0/1 - 6
 switchport mode access
 switchport access vlan <VLAN>
interface Gi0/24
 switchport mode trunk
```

```
interface range Gi0/7 - 23
 shutdown
```

```
interface Gi0/24
 switchport trunk native vlan 2
```

```
interface range Gi0/1 -6
 switchport port-security
 switchport port-security maximum <NUM>
 switchport port-security violation protect
```

```
interface Gi0/24
 switchport trunk allowed vlan <VLANS>
```

### 3. STP

- включить RPVST+
- выставить приоритет для всех вланов в соответствии с планом
- BPDUGuard enable, Portfast на всех access интерфейсах, errdisable recovery

```
spanning-tree mode rapid-pvst
```

```
spanning tree vlan <VLAN> priority <PRI>
```

```
spanning-tree portfast bpduguard default
interface range Gi0/1 - 6
 spanning-tree portfast

errdisable recovery cause bpduguard
```

### 4. EtherChannel

- настроить интерфейсы так, чтобы их настройки совпадали с настройками аггрегированного интерфейса
- выключить интерфейсы, настроить аггрегированны линк статически, при помощи LACP и PaGP, включить интерфейсы
- выбрать подходящий режим балансировки

```
interface range Gi0/23 - 24
 switchport mode trunk
```

```
interface range Gi0/23 - 24
 shutdown
 channel-group 1 mode on
 no shutdown
```

```
port-channel load-balance <MODE>
```

### 5. Дополнительно

 - перевести VTP в transparent mode

```
vtp mode transparent
```

Блокировать после нескольких неудачных попыток залогиниться
```
login block-for <timeout> attemts <num> within <timeot>
```

Логирование доступа
```
login on-success log
login on-failure log every 2
```


```
hostname <hostname>
ip domain name <domain name>
crypto key generate rsa general-keys modulus 1024
ip ssh time-out 90
ip ssh maxstartups 2 
ip ssh authentication-retries 2 
```

Настроить SSH на нестандартный порт
```
ip ssh port <port> rotary <group>
line vty 0 4
 rotary <group>
```
