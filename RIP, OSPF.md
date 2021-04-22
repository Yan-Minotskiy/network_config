# Лабораторная работа №3  - Динамическая маршрутизация. Протоколы RIP и OSPF

* [Все лабораторные работы по сетям и системам передачи данных](./README.md)
* [Предыдущая лабораторная работа - Статическая маршрутизация](./static_routing.md)
* [Следующая лабораторная работа - Динамическая маршрутизация. Протокол BGP](./BGP.md)

---

## RIP

![](./image/nets3.png)

### Настройка на Cisco

```
router rip
version 2
redistribute connected
network 75.0.0.0
network 100.0.0.0
network 140.56.0.0
network 213.234.10.0
```

### Настройка на Mikrotik

```
/routing rip
set redistribute-connected=yes
/routing rip network
add network=213.234.10.0/30
add network=213.234.11.0/30
add network=213.234.12.0/30
add network=213.234.13.0/30
```

## OSPF

![](./image/nets4.png)

### Настройка на Cisco

```
router ospf 1
redistribute connected subnets
network 12.12.13.0 0.0.0.3 area 0
network 12.12.14.0 0.0.0.3 area 0
network 12.13.10.4 0.0.0.3 area 0
network 12.13.10.8 0.0.0.3 area 0
```

### Настройка на Mikrotik

```
/interface bridge
add name=loopback
/routing ospf instance
set [ find default=yes ] router-id=10.255.255.5
/routing ospf network
add area=backbone network=12.12.15.0/30
add area=backbone network=12.12.16.0/30
add area=backbone network=12.12.17.0/30
add area=backbone network=12.12.14.0/30
```