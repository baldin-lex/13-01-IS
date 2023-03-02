# Домашнее задание к занятию 13.1. «Уязвимости и атаки на информационные системы» - Балдин Алексей

### Задание 1

Скачайте и установите виртуальную машину Metasploitable: https://sourceforge.net/projects/metasploitable/.

Это типовая ОС для экспериментов в области информационной безопасности, с которой следует начать при анализе уязвимостей.

Просканируйте эту виртуальную машину, используя **nmap**.

Попробуйте найти уязвимости, которым подвержена эта виртуальная машина.

Сами уязвимости можно поискать на сайте https://www.exploit-db.com/.

Для этого нужно в поиске ввести название сетевой службы, обнаруженной на атакуемой машине, и выбрать подходящие по версии уязвимости.

Ответьте на следующие вопросы:

- Какие сетевые службы в ней разрешены?
- Какие уязвимости были вами обнаружены? (список со ссылками: достаточно трёх уязвимостей)

### Решение:

*Исходим из того, что целевой хост в сегменте сети обнаружен (в моем случае 192.168.31.117). Тогда, чтобы определить, какие порты на нем открыты, я использую команду `nmap -sS 192.168.31.117` и получаю:*  

![IS](images/open_ports.jpg)

*Чтобы получить более подробную информацию по сервисам (и версиям), запуцщенным на целевом хосте, нужно: `nmap -sV 192.168.31.117`. В результате вывод будет следующим:*

![IS](images/open_ports_versions.jpg)

*Используя эту информацию можно, с применением скриптов, пробовать выявить уязвимости по конкретной службе/порту, и попытаться воспользоваться ими. Но в данном задании мне необходимо выявить несколько уязвимостей. Для этого в консоли: `nmap --script vuln 192.168.31.117` и получаю большой вывод, который содержит информацию по выявленным уязвимостям в следующем виде:*

![IS](images/vuln_example.jpg)

Подробное описание выявленных уязвимостей можно найти по идентификаторам BID, CVE
Прилагаю три ссылки для примера:

[ORACLE VM SERVER 3.1/3.2](https://vuldb.com/ru/?id.74941)

[VSFTPD 2.3.4](https://vuldb.com/ru/?id.146452)

[SSLV3 POODLE](https://vuldb.com/ru/?id.92602)

---

### Задание 2

Проведите сканирование Metasploitable в режимах SYN, FIN, Xmas, UDP.

Запишите сеансы сканирования в Wireshark.

Ответьте на следующие вопросы:

- Чем отличаются эти режимы сканирования с точки зрения сетевого трафика?
- Как отвечает сервер?

### Решение:

*В справочных материалах по `nmap` говорится, что:
SYN-сканирование не открывает полного соединения, а только отправляет SYN-пакет. А по ответам с удаленного хоста определяется, открыт тот или иной порт или нет. Поснифим траффик такого запроса и убедимся, что это так:*

![IS](images/syn_scan.jpg)

*FIN-сканирование иначе определяет открыт или нет тот или иной порт. В этом случае посылается FIN-пакет на закрытие соединения. Закрытый порт, как правило, отвечает пакетом RST. Открытый — не отвечает. Скрин вывода снятого траффика такого запроса прилагаю:*

![IS](images/fin_scan.jpg)

*Принцип работы Xmas-сканирования: при отправке пакетов устанавливаются FIN, PSH и URG флаги. Если в ответ придет RST пакет, значит порт закрыт. Если тишина - порт открыт. Вот вывод Wireshark по такому запросу:*

![IS](images/Xmas_scan.jpg)

*При UDP-сканировании доступность порта определяется следующим образом: на интересующий порт целевого хоста отправляется пустой заголовок UDP. И если в ответ придет ICMP-ошибка, значит порт закрыт. Ниже пример такого сканирования, снятый при помощи Wireshark:*

![IS](images/UDP_scan.jpg)
