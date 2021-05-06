# Лабораторая работа №6 - IKEv2

[**Все лабораторные работы по сетям и системам передачи данных**](./README.md)


## Настройка сервера

```
conf t
crypto ikev2 proposal PROP-UNITS
encription aes-cbc-128
integrity sha256
group 5
ex

crypto ikev2 policy POL-UNITS
proposal PROP-UNITS
match fvrf any
ex

crypto ikev2 keyring KEYR-UNITS
peer UNITS
address 0.0.0.0 0.0.0.0
pre-shared key secret123
ex

crypto ikev2 profile PROF-UNITS
match identity remote address 0.0.0.0
identity local address
authentication remote pre-share
authentication local pre-share
keyring local KEYR-UNITS
virtual-template 1
ex

crypto ipsec transform-set TRANS-UNITS esp-aes 128 esp-sha256
$c transform-set TRANS-UNITS esp-aes 128 esp-sha256-hmac
ex

crypto ipsec profile PROF-UNITS
set transform-set TRANS-UNITS
set ikev2-profile PROF-UNITS
ex

int lo 0
ip add 172.16.1.1  255.255.255.0
ex
int virtual-template 1 type tunnel 
ip unnambered lo0
tunnel source fa0/0
tun mode ipsec ipv4
tun protection ipsec profile PROF-UNITS
```


**Полезные источники:**  
[Видеоурок по настройке](https://www.youtube.com/watch?v=S8TsZs89TQ0)  
[Документация CISCO](https://www.cisco.com/en/US/docs/ios-xml/ios/sec_conn_ikevpn/configuration/15-1mt/Configuring_Internet_Key_Exchange_Version_2.html)