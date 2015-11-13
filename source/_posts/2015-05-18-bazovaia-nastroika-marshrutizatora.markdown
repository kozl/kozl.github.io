---
layout: post
title: "Базовая настройка коммутатора"
date: 2015-05-18 17:11:57 +0300
comments: true
categories: network, ccna
---
Удаление предыдущей конфигурации

```
Router#erase startup-config
```

Отключение DNS запросов при ошибках в консоли
```
no ip domain lookup
```

Выключить таймаут неактивной сессии в консоли, привилегированный режим в консоли, включить синхронный вывод на консоль:
```
line con 0
 exec-timeout 0 0
 privilege level 15
 logging synchronous
```

Хранить startup конфиг в сжатом виде
```
service compress-config 
```
### Настройки времени

```
ntp peer <IP>
clock timezone MSK +3
```

### Безопасность

Хранение паролей в зашифрованном виде
```
service password-encryption
```

Пароль для привилегированного режима
```
enable secret <PASS>
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

Настройка SSH

```
line vty 0 15
 transport input ssh
 login local
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
