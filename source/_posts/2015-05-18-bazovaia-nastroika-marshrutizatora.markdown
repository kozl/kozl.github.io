---
layout: post
title: "Базовая настройка маршрутизатора"
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

Выключить таймаут неактивной сессии в консоли, включить синхронный вывод на консоль:
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
