# Лабораторные работы по дисциплине "Сети и системы передачи данных"

* **Авторы решений: [Миноцкий Ян](https://github.com/Yan-Minotskiy), [Носков Сергей](https://github.com/Sergey-Noskov), [Григорьев Антон](https://github.com/Zeph1rr)**
* **Время разработки: сентябрь 2020 - декабрь 2021**

*Данный репозиторий предназначен для облегчения изучения сетевого администрирования. Проект является исключительно образовательным. Справочные материалы взяты из открытых источников.*

---

## [Задания и инструкции по выполнению лабораторных работ (5-6 семестр)](https://hackmd.io/@sadykovildar/B16xYzRmw)
## [Задания и инструкции по выполнению лабораторных работ (7 семестр)](https://hackmd.io/@IgorLitvin/SJNiaFC-t)

---

## Содержание

### Лабораторные работы по сетям и системам передачи данных

- [x] [**Лабораторная работа №1: VLAN, DHCP**](./VLAN,%20DHCP.md)
- [x] [**Лабораторная работа №2: Статическая маршрутизация**](./static_routing.md)
- [x] [**Лабораторная работа №3: Динамическая маршрутизация. Протоколы RIP и OSPF**](./RIP,%20OSPF.md)
- [x] [**Лабораторная работа №4: Динамическая маршрутизация. Протокол BGP**](./BGP.md)
- [x] [**Лабораторная работа №5: SSH, NAT**](./SSH,%20NAT.md)

### Лабораторные работы по организации компьютерных сетей

- [x] [**Лабораторная работа №1: iptables**](iptables.md)
- [x] [**Лабораторная работа №2: GRE**](GRE.md)
- [x] [**Лабораторная работа №4: L2TP**](L2TP.md)
- [x] [**Лабораторная работа №5: IPsec over L2TP**](L2TP.md#настройка-ipsec)
- [x] [**Лабораторная работа №6: IKEv2**](IKEv2.md)
- [x] [**Лабораторная работа №7: OpenVPN-L3**](OpenVPN-L3.md)
- [x] [**Лабораторная работа №8: OpenVPN-L2**](OpenVPN-L2.md)
- [x] [**Лабораторная работа №9: WireGuard**](WireGuard.md)
- [x] [**Лабораторная работа №10: MPLS**](MPLS.md)

### Практические работы по организации сетевой безопасности

- [x] [**Практическая работа №1: Настройка сети в Linux**](Linux_Pt1.md)
- [x] [**Практическая работа №2: Настройка и конфигурация Linux-систем и прав доступа на базе Debian 11**](Linux_Pt2.md)
- [ ] [**Практическая работа №3: Построение компьютерной сети с нуля на базе виртуальной лаборатории EVE-NG**](Network_Design.md)
- [ ] **Практическая работа №12: Основы Iptables**
  
---

## Источники
1. [Полезные видеоуроки по сетевому администрированию (в основном здесь представлены протоколы маршрутизации)](https://www.youtube.com/watch?v=Y4l8ScRLrf4&list=PLtPJ9lKvJ4oh_w4_jtRnKE11aqeRldCFI)
2. [YouTube канал Techno Azimut (о настройке сетевого оборудования). Здесь были найдены, в основном, уроки по протоколам туннелирования](https://www.youtube.com/channel/UCZdi13zUUHa-3rTG3CMVfzQ)
3. [DHCP-протокол: что это такое и как он работает | SelectelBlog](https://selectel.ru/blog/dhcp-protocol/)
4. [Основы компьютерных сетей. Тема №6. Понятие VLAN, Trunk и протоколы VTP и DTP | Хабр](https://habr.com/ru/post/319080/)
5. [IP-калькулятор для расчёта масок сетей](https://ip-calculator.ru/)
6. [RIP (сетевой протокол) | Википедия](https://ru.wikipedia.org/wiki/RIP_(%D1%81%D0%B5%D1%82%D0%B5%D0%B2%D0%BE%D0%B9_%D0%BF%D1%80%D0%BE%D1%82%D0%BE%D0%BA%D0%BE%D0%BB))
7. [OSPF | Википедия](https://ru.wikipedia.org/wiki/OSPF)
8. [Принципы работы протокола BGP | Хабр](https://habr.com/ru/post/450814/)
9. [Как пользоваться SSH | Losst](https://losst.ru/kak-polzovatsya-ssh)
10. [NAT (обзор и примеры)](https://k.psu.ru/wiki/NAT_(%D0%BE%D0%B1%D0%B7%D0%BE%D1%80_%D0%B8_%D0%BF%D1%80%D0%B8%D0%BC%D0%B5%D1%80%D1%8B))
11. [Список портов TCP и UDP | Википедия](https://ru.wikipedia.org/wiki/%D0%A1%D0%BF%D0%B8%D1%81%D0%BE%D0%BA_%D0%BF%D0%BE%D1%80%D1%82%D0%BE%D0%B2_TCP_%D0%B8_UDP)
12. [Настройка Linux-файрвола iptables: Руководство для начинающих](https://1cloud.ru/help/linux/nastrojka_linus-firewall_iptables)
13. [Как настроить GRE туннель (CISCO)](https://community.cisco.com/t5/%D0%B1%D0%B5%D0%B7%D0%BE%D0%BF%D0%B0%D1%81%D0%BD%D0%BE%D1%81%D1%82%D1%8C-%D0%B4%D0%BE%D0%BA%D1%83%D0%BC%D0%B5%D0%BD%D1%82%D1%8B-security/%D0%BA%D0%B0%D0%BA-%D0%BD%D0%B0%D1%81%D1%82%D1%80%D0%BE%D0%B8%D1%82%D1%8C-gre-%D1%82%D1%83%D0%BD%D0%BD%D0%B5%D0%BB%D1%8C/ta-p/3145690)
14. [VPN:GRE и IPsec (быстрая настройка, аутентификация с помощью пароля) (MikroTik)](https://mikrotik.wiki/wiki/VPN:GRE_%D0%B8_IPsec_\(%D0%B1%D1%8B%D1%81%D1%82%D1%80%D0%B0%D1%8F_%D0%BD%D0%B0%D1%81%D1%82%D1%80%D0%BE%D0%B9%D0%BA%D0%B0,_%D0%B0%D1%83%D1%82%D0%B5%D0%BD%D1%82%D0%B8%D1%84%D0%B8%D0%BA%D0%B0%D1%86%D0%B8%D1%8F_%D1%81_%D0%BF%D0%BE%D0%BC%D0%BE%D1%89%D1%8C%D1%8E_%D0%BF%D0%B0%D1%80%D0%BE%D0%BB%D1%8F\)#.D0.9D.D0.B0.D1.81.D1.82.D1.80.D0.BE.D0.B9.D0.BA.D0.B0_.D0.BF.D0.B5.D1.80.D0.B2.D0.BE.D0.B3.D0.BE_.D0.BC.D0.B0.D1.80.D1.88.D1.80.D1.83.D1.82.D0.B8.D0.B7.D0.B0.D1.82.D0.BE.D1.80.D0.B0)
15. [Канал между Linux и Cisco (GRE) - всё руками (форум)](https://forum.cz6.ru/viewtopic.php?t=152)
16. [Видеоуроки "Linux для начинающих"](https://www.youtube.com/watch?v=fTtr1t7uWvU&list=PLcDkQ2Au8aVNMLee8b3RN1QXX0ZBZOYJV&index=5)
17. [Разбираемся в VPN протоколах. | Хабр](https://habr.com/ru/company/dsec/blog/499718/)
18. [Документация Cisco по настройке IKEv2](https://www.cisco.com/en/US/docs/ios-xml/ios/sec_conn_ikevpn/configuration/15-1mt/Configuring_Internet_Key_Exchange_Version_2.html)  
19. [Обзор протоколов VPN | SecureVPN](https://www.securevpn.pro/rus/blog/view/5?url=rus%2Fblog%2Fview%2F5)
20. [openvpn-install | GitHub (cкрипты для установки OpenVPN)](https://github.com/angristan/openvpn-install)
21. [Сети для самых маленьких. Часть десятая. Базовый MPLS | Хабр](https://habr.com/ru/post/246425/)
22. [Сети для самых маленьких. Выпуск десятый. Базовый MPLS | YouTube](https://www.youtube.com/watch?v=hZyfM4UZDac)
23. [Команда ping в Linux](https://losst.ru/komanda-ping-v-linux)
24. [Перезапуск сети в Ubuntu](https://losst.ru/kak-perezagruzit-set-v-ubuntu)
25. [DNS. Википедия](https://ru.wikipedia.org/wiki/DNS)
26. [Команда traceroute Linux](https://losst.ru/komanda-traceroute-linux)
27. [Монтирование диска в Linux](https://losst.ru/montirovanie-diska-v-linux)

*Если в отчётах были найдены ошибки или у Вас есть предложения по улучшению, просьба написать авторам решений.*
