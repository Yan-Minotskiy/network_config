# Лабораторные работы №5-6 - L2TP и L2TP over IPsec

## Теоретическая справка

> Layer 2 Tunneling Protocol (L2TP) был впервые предложен в 1999 году в качестве обновления протоколов L2F (Cisco) и PPTP (Microsoft). Поскольку L2TP сам по себе не обеспечивает шифрование или аутентификацию, часто с ним используется IPsec. L2TP в паре с IPsec поддерживается многими операционными системами, стандартизирован в RFC 3193. L2TP/IPsec считается безопасным и не имеет серьезных выявленных проблем (гораздо безопаснее, чем PPTP). L2TP/IPsec может использовать шифрование 3DES или AES, хотя, учитывая, что 3DES в настоящее время считается слабым шифром, он используется редко. У протокола L2TP иногда возникают проблемы из-за использования по умолчанию UDP-порта 500, который, как известно, блокируется некоторыми брандмауэрами. Протокол L2TP/IPsec позволяет обеспечить высокую безопасность передаваемых данных, прост в настройке и поддерживается всеми современными операционными системами. Однако L2TP/IPsec инкапсулирует передаваемые данные дважды, что делает его менее эффективным и более медленным, чем другие VPN-протоколы.

> Internet Protocol Security (IPsec) — это набор протоколов для обеспечения защиты данных, передаваемых по IP-сети. В отличие от SSL, который работает на прикладном уровне, IPsec работает на сетевом уровне и может использоваться нативно со многими операционными системами, что позволяет использовать его без сторонних приложений (в отличие от OpenVPN).IPsec стал очень популярным протоколом для использования в паре с L2TP или IKEv2.



Выше представлены фрагменты статей c Хабра.  
[Полную статью можно посмотреть здесь.](https://habr.com/ru/company/dsec/blog/499718/)

[**Полезный видеоурок по настроке L2TP c IPsec**](https://youtu.be/N8qoleHxNmA)

## Подготовка стенда

![](image/nets2-5-1.png)

Пропускаю описание базовой настройки роутеров Cisco. Там как обычно нужно поменять `hostname` и настроить интерфейсы.

## Настройка L2TP на сервере (Unit)

```
conf t
aaa new-model
aaa authentication ppp L2TP-AUTH local
user unit2 password 0 secret1
user unit3 password 0 secret2
vpdn enable
vpdn-group 1
accept-dialin
protocol l2tp
virtual-template 1
ex
no l2tp tunnel authentication
lcp renegotiation always
ip pmtu
ex
int lo 0
ip add 172.16.1.1 255.255.255.0
ex
int virtual-template 1
ip unnumberd lo0
peer default ip address pool  POOL-L2TP
ppp authentication ms-chap-v2 L2TP-AUTH
ip mtu 1406
ip tcp adjust-mss 1366
ex
ip local pool POOL-L2TP 172.16.1.10 172.16.1.20
ex
```

## Настройка L2TP на клиенте (Unit2)

```
pseudowire-class PW-CLASS
encapsulation l2tpv2
ip local interface e0/0
ip pmtu
ex
int virtual-ppP 1
ip address negotiated
ppp authentcation ms-chap-v2 callin
ppp direction callout
ppp chap hostname unit2
ppp chap password 0 secret1
psewd
ip mtu 1406
ip tcp adjust-mss 1366
pseudowire 2.2.2.2 1 pw-class PW-CLASS
end
```

Для проверки правильности настройки:

```
sh ip int br
ping 172.16.1.1
sh vpdn
sh l2tp session
```
