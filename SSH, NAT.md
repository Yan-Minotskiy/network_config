# Лабораторная работа №5  - SSH, NAT

* [Все лабораторные работы по сетям и системам передачи данных](./README.md)
* [Предыдущая лабораторная работа - Динамическая маршрутизация. Протокол BGP](./BGP.md)

---


```
en
conf t
ip ssh version 2
ip domain name <domain>
crypto key generate rsa
service password-encryption
username <username> privilege <0-15> password <password>
aaa new-model
line vty 0 4
transport input all
logging synchronous
exec-timeout 60 0
exit
enable password <password>
do wr

interface Ethernet0/0
    description "M-RIP-1"
    ip address 186.13.48.2 255.255.255.252
    ip nat enable
!
interface Ethernet0/1
    description "M-RIP-2"
    ip address 186.12.46.2 255.255.255.252
    ip nat enable
!
interface Ethernet0/2
    description "C-RIP-7"
    ip address 80.18.83.2 255.255.255.252
    ip nat enable
!
interface Ethernet0/3
    description "ExtraNet-1"
    ip address dhcp
    ip nat enable
!
```

Подключение по ssh и telnet:

```
ssh -l <username> <ip>

system ssh address=<ip> user=<username>
system telnet address=<ip> user=<username>
```

![](./image/nets6.png)