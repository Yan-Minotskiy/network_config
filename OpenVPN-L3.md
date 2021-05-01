# Лабораторная работа №7 - OpenVPN-L3

[**Все лабораторные работы по сетям и системам передачи данных**](./README.md)

# Топология
![](./image/nets2-7-1.png)

* 3 Cisco IOL со стандартными настройками
* 1 Network Type Cloud 1
* 1 pfSense-Firewall  версия 2.4.4 QEMU Nic e1000
* 2 Switch (Cisco IOL L2)
* 5 Linux Debian (Linux-Firewall c двумя интернет портами) QEMU Nic e1000

Важно: перед тем как соединить R2 c Net назначаем ему hostname

# Настройка Cisco Router

Сначала включаем только 3 Cisco Router

## R1

```
interface Ethernet0/0
description 'Linux-Firewall'
ip address 10.10.10.1 255.255.255.0
no sh

interface Ethernet0/1
description 'R2'
ip address 10.10.20.1 255.255.255.0
no sh
```

![](./image/nets2-7-2.png)

`ip route 0.0.0.0 0.0.0.0 10.10.20.2`

![](./image/nets2-7-3.png)

`wr`

## R3

```
interface Ethernet0/0
description 'pfSense-Firewall'
ip address 10.10.40.3 255.255.255.0
no sh

interface Ethernet0/1
description 'R2'
ip address 10.10.30.3 255.255.255.0
no sh
```

![](./image/nets2-7-4.png)

`ip route 0.0.0.0 0.0.0.0 10.10.30.2`

![](./image/nets2-7-5.png)

`wr`

## R2

```
interface Ethernet0/0
description 'R1'
ip address 10.10.20.2 255.255.255.0
ip nat inside
no sh

interface Ethernet0/1
description 'R2'
ip address 10.10.30.2 255.255.255.0
ip nat inside
no sh

interface Ethernet0/2
ip address dhcp
ip nat outside
no sh
exit

ip nat inside source list 1 
interface Ethernet0/2 overload
ip route 10.10.10.0 255.255.255.0 10.10.20.1
ip route 10.10.40.0 255.255.255.0 10.10.30.3
```



# Настройка Linux-Firewall

Включаем Linux-Firewall и pfSense-Firewall

`nano /etc/hosts`

стереть нижние строки с #

заменить Debian-10 на Linux-Firewall

`nano /etc/hostname`

заменить на Linux-Firewall

`nano /etc/network/interfaces`

под `#The primary…` стираем всё и пишем

```
auto ens3
allow-hotplug ens3
iface ens3 inet static
    address 10.10.10.10./24
    gateway 10.10.10.1

auto ens4
allow-hotplug ens4
iface ens4 inet static
    address 192.168.0.254/24
```

`nano etc/resolv.conf`

тут должно быть только `nameserver 8.8.8.8`, сохраняем и делаем `reboot`, затем обновляемся:

```
apt update
apt upgrade
```

# Настройка OpenVPN-L3 client

Включаем последовательно Switch-user, OpenVPN-L3

`nano /etc/hosts`

стереть нижние строки с `#` ,заменить Debian-10 на OpenVPN-L3

`nano /etc/hostname`

заменить на OpenVPN-L3

`nano /etc/network/interfaces`

под `#The primary…` стираем всё и пишем

```
auto ens3
allow-hotplug ens3
iface ens3 inet static
    address 192.168.0.10/24
    gateway 192.168.0.254
```

`nano etc/resolv.conf`

тут должно быть только `nameserver 8.8.8.8`, сохраняем и делаем `reboot`.

# Настройка iptables

Есть `ping 192.168.0.254`, но нет `ping 8.8.8.8`, поэтому настроим [iptables](iptables.md)

В Linux-Firewall:

`nano /etc/network/interfaces`

под всеми записями пишем

```
post-up iptables -t nat -A POSTROUTING -o ens3 -j MASQUERADE
```

сохраняем настройки, перезапускаем сеть `service networking restart` командой `iptables -t nat -L -v` проверяем iptables. 

![](./image/nets2-7-6.png)

`nano /etc/sysctl.conf`

Раскоменчиваем

![](./image/nets2-7-7.png)

Обновляем

sysctl -p

![](./image/nets2-7-8.png)

ping есть!

![](./image/nets2-7-9.png)

Обновления

`apt update && apt upgrade -y`

# Настройка pfSense-Firewall

Для настройки pfSense-Firewall потребуется Kali Linux

![](./image/nets2-7-10.png)

Чтобы соединить Kali c Switch-company, добавим портгруппу

![](./image/nets2-7-11.png)

Дополненная часть топологии

![](./image/nets2-7-12.png)

Включаем последовательно Switch-company, Kali

Настроим WAN интерфейс статикой, для этого переходим в pfSense-Firewall и выбираем 2 пункт

![](./image/nets2-7-13.png)

Затем выбираем 1, то есть сам WAN

выбираем n

вводим 10.10.40.10 

вводим 24

![](./image/nets2-7-14.png)

Вводим 10.10.40.3 (по сути, gateway)

Ставим n

Пропускаем, то есть нажимаем enter

Ставим n

![](./image/nets2-7-15.png)

Проверим ping, для этого выберем 8 и пропингуем 8.8.8.8 и ya.ru

![](./image/nets2-7-16.png)

exit

Теперь меняем внутреннюю сетку компании

* Для этого снова выбираем 2
* Выбираем 2 (то есть LAN)
* Вводим 172.16.10.254 
* Вводим 24
* Пропускаем gateway, так как это LAN
* Пропускаем IPv6
* Ставим y (DHCP server нам нужен)
* Вводим 172.16.10.10
* Вводим 172.16.10.100
* Ставим n

![](./image/nets2-7-17.png)

Переходим в Kali

Перезапускаем Wired Connection

![](./image/nets2-7-18.png)

![](./image/nets2-7-19.png)

Смотрим адреса

`ip a`

![](./image/nets2-7-20.png)

Пингуем

![](./image/nets2-7-21.png)

! Не пингуется Яндекс, проблема на pfSense-Firewall

Чтобы нормально настроить pfSense-Firewall меняем разрешение экрана 800\*600 (так удобнее)

![](./image/nets2-7-22.png)

Заходим на pfSense-Firewall

![](./image/nets2-7-23.png)

**Логин: admin  
Пароль: pfsense**

Нажать на логотип

![](./image/nets2-7-24.png)

Заходим на WAN interface

![](./image/nets2-7-25.png)

Убираем галочки в самом низу и сохраняем

![](./image/nets2-7-26.png)

Применяем настройки, нажав Apply Changes

![](./image/nets2-7-27.png)

Пинг появился!

![](./image/nets2-7-28.png)

# Настройка OpenVPN-L3-SRV

`nano /etc/hosts`

стереть нижние строки с `#`, заменить Debian-10 на OpenVPN-L3-SRV

`nano /etc/hostname`

заменить на OpenVPN-L3-SRV

`nano /etc/network/interfaces`

под `#The primary…` стираем всё и пишем

```
auto ens3
allow-hotplug ens3
iface ens3 inet static
    address 172.16.10.240/24
    gateway 172.16.10.254
```

`nano etc/resolv.conf`

тут должно быть только `nameserver 8.8.8.8`, сохраняем и делаем `reboot`

Проверяем пинг 

`ping ya.ru`

![](./image/nets2-7-29.png)

затем обновляемся

`apt update && apt upgrade -y`


# Настройка OpenVPN

Делать это нужно через Кали (Делаю прям как на видео, мануал непонятный)

Подключаемся по ssh 

```
ssh root@172.16.40.240
yes
eve@123
```

![](./image/nets2-7-30.png)

Проверяем обновления

`apt update`

Ставим curl

`apt install curl `

`y`

![](./image/nets2-7-31.png)

В браузере пишем в поиске **openvpn angristan**

![](./image/nets2-7-32.png)

Переходим сюда

![](./image/nets2-7-33.png)

Копируем данный скрипт в терминал ПКМ

![](./image/nets2-7-34.png)

![](./image/nets2-7-35.png)

Запускаем скриптик

`./openvpn-install.sh`

Public IP ставим 

`10.10.40.10`

Дцблируем

`10.10.40.10`

`n`

![](./image/nets2-7-36.png)

Выбираем дефолтный порт

`1`

Выбираем UDP

`1` 

Выбираем 1, так там нужная директория

`1` 

![](./image/nets2-7-37.png)

`n` 

`n` 

![](./image/nets2-7-38.png)

Загрузка...

Клиентское имя ставим

`client`

без пароля, то есть 1

1

![](./image/nets2-7-39.png)

смотрим `ls` 

![](./image/nets2-7-40.png)

(В видео закинулось в home, у меня в root, надеюсь не критично, нужно это будет учесть) 

Пробрасываем порты через pfSense-Firewall

Зaходим на pfSense-Firewall

172.16.10.254

**Логин: admin  
Пароль: pfsense**

Firewall -> NAT

![](./image/nets2-7-41.png)

add

![](./image/nets2-7-42.png)

Настройки следующие

(остальное не меняем)

![](./image/nets2-7-43.png)

![](./image/nets2-7-44.png)

![](./image/nets2-7-441.png)

![](./image/nets2-7-45.png)

Нажимаем `Save`

Должно получится так 

![](./image/nets2-7-46.png)

![](./image/nets2-7-47.png)

Не забываем нажать сюда

![](./image/nets2-7-48.png)

Добавим еще одно

![](./image/nets2-7-49.png)

Настройки следующие

(остальное не меняем)

![](./image/nets2-7-491.png)

![](./image/nets2-7-50.png)

![](./image/nets2-7-51.png)

![](./image/nets2-7-52.png)

Нажимаем `Save`

Не забываем нажать сюда

![](./image/nets2-7-48.png)

Получилось

![](./image/nets2-7-53.png)

Проверим, где файлик

![](./image/nets2-7-54.png)

`exit`

копируем файлик на клиент

![](./image/nets2-7-55.png)