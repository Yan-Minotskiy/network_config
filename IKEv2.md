# Лабораторая работа №6 - IKEv2

[**Все лабораторные работы по сетям и системам передачи данных**](./README.md)


Протокол обмена ключами (Internet Key Exchange) версии 2 — входящий в набор [IPSec](L2TP.md#IPsec) протокол туннелирования, разработанный совместно компаниями [Microsoft](https://www.microsoft.com/ru-ru) и [Cisco](https://www.cisco.com/). Входит в состав Windows 7 и более поздних версий, поддерживается мобильными устройствами Blackberry и Apple. Имеются решения с открытым исходным кодом для Linux.

Передача данных производится через UDP порты 500 и/или 4500, с шифрованием данных криптоалгоритмами 3DES и AES. Использование UDP обеспечивает хорошую скорость работы и не создает проблем для работы за [NAT](SSH,%20NAT.md#NAT) и [межсетевыми экранами](iptables.md).

**[Список портов TCP и UDP](https://ru.wikipedia.org/wiki/%D0%A1%D0%BF%D0%B8%D1%81%D0%BE%D0%BA_%D0%BF%D0%BE%D1%80%D1%82%D0%BE%D0%B2_TCP_%D0%B8_UDP)**

## Настройка сервера

Настроим политики. Зададим параметры шифрования.
```
conf t
crypto ikev2 proposal PROP-UNITS
encription aes-cbc-128
integrity sha256
group 5
ex
```

Создадим политику c proposals. 
```
crypto ikev2 policy POL-UNITS
proposal PROP-UNITS
match fvrf any
ex
```

Создадим keyring и peer. Будем использовать симметричные ключи.
```
crypto ikev2 keyring KEYR-UNITS
peer UNITS
address 0.0.0.0 0.0.0.0
pre-shared key secret123
ex
```

Создадим профиль ikev2. У кажем в нём параметры аутентификации. Идентифицировать клиента будем по адресу.
```
crypto ikev2 profile PROF-UNITS
match identity remote address 0.0.0.0
identity local address
authentication remote pre-share
authentication local pre-share
keyring local KEYR-UNITS
virtual-template 1
ex
```

Настроим transform-set c шифрованием данных и проверкой целостности. 
```
crypto ipsec transform-set TRANS-UNITS esp-aes 128 esp-sha256
$c transform-set TRANS-UNITS esp-aes 128 esp-sha256-hmac
ex
```

Настроим профиль для ipsec.
```
crypto ipsec profile PROF-UNITS
set transform-set TRANS-UNITS
set ikev2-profile PROF-UNITS
ex
```

Создадим loopback для virtual tamplate. При `ip unnambered` заимствуем ip из loopback.
```
int lo 0
ip add 172.16.1.1  255.255.255.0
ex
int virtual-template 1 type tunnel 
ip unnambered lo0
tunnel source fa0/0
tun mode ipsec ipv4
tun protection ipsec profile PROF-UNITS
```

## Настройка клиента

```
conf t
crypto ikev2 proposal PROP-HQ
enc aes-cbc-128
integ sha256
gr 5
ex
```

Создаём политики
```
crypto ikev2 policy POL-HQ
match fvrf any
proposal PROP-HQ
ex
```

Создаём keyring и указываем алрес сервера.
```
crypto ikev2 keyring KEYR-HQ
peer HQ
address 2.2.2.2
pre-shared-key secret123
ex
ex
```

Создаём ikev2 профль
```
crypto ikev2 profile PROF-HQ
match identity remote address 2.2.2.2
identity local address 1.1.1.2
authentication local pre-share
keyring local KEYR-HQ
ex
```

Создаём transform-set
```
crypto ipsec trans TRANS-HQ esp-aes 128 esp-sha256-hmac
crypto ipsec profile PROF-HQ
set trens TRANS-HQ
set ikev2-profile PROF-HQ
ex
```

Создаём loopback интерфейс и задаём ему адрес.
```
in lo 0
ip add 172.16.1.2 255.255.255.0
ex
```

Создаём тунельный интерфейс
```
int tun 0
ip unnumbered lo0
tun source fa0/0
tun mode ipsec ipv4
tun protection ipsec prof PROF-HQ
tun des 2.2.2.2
```

Далее для того, чтобы была связь необходимо настроить [статическую](static_routing.md) или [динамическую маршрутизацию](RIP,%20OSPF.md).

**Полезные источники:**  
1. [Видеоурок по настройке](https://www.youtube.com/watch?v=S8TsZs89TQ0)  
2. [Документация Cisco](https://www.cisco.com/en/US/docs/ios-xml/ios/sec_conn_ikevpn/configuration/15-1mt/Configuring_Internet_Key_Exchange_Version_2.html)  
3. [Обзор протоколов VPN](https://www.securevpn.pro/rus/blog/view/5?url=rus%2Fblog%2Fview%2F5)