## VPN

- [X] Между двумя виртуалками поднять vpn в режимах
- [X] tun;
- [X] tap;
- [X] [Прочуствовать разницу.](#%D0%B2%D1%8B%D0%B2%D0%BE%D0%B4)
- [X] [Поднять RAS на базе OpenVPN с клиентскими сертификатами, подключиться с локальной машины на виртуалку.](#remote-access-server-%D0%BD%D0%B0-%D0%B1%D0%B0%D0%B7%D0%B5-openvpn)
- [X] [* Самостоятельно изучить, поднять ocserv и подключиться с хоста к виртуалке](#%D0%BD%D0%B0%D1%81%D1%82%D1%80%D0%BE%D0%B9%D0%BA%D0%B0-openconnect-vpn-server-%D0%BD%D0%B0-centos7)
- [X] Формат сдачи ДЗ - vagrant + ansible

### Запуск проекта

* Клонируем репозиторий: `git clone https://github.com/mmmex/vpn.git`

* Переходим в папку и запускаем проект: `cd vpn; vagrant up`

* После завершения будет развернуто 2 ВМ: `server` и `client`

* Сервис openvpn настроен на виртуальный интерфейс в режиме TAP

### Демонстрация TAP

Для демонстрации работы выполняем шаги:
 1. Выполним вход на ВМ `server`: `vagrant ssh server`
 2. Запускаем серверную часть утилиты `iperf`: `iperf3 -s &`
 3. Оставляем окно терминала открытым, выполним вход на `client`: `vagrant ssh client`
 4. Запустим клиентскую часть утилиты `iperf`: `iperf3 -c 10.10.10.1 -t 40 -i 5`

Результат работы `iperf3` на ВМ `server`:
```bash
[root@server ~]# iperf3 -s &
[1] 25782
[root@server ~]# -----------------------------------------------------------
Server listening on 5201
-----------------------------------------------------------
Accepted connection from 10.10.10.2, port 38888
[  5] local 10.10.10.1 port 5201 connected to 10.10.10.2 port 38890
[ ID] Interval           Transfer     Bandwidth
[  5]   0.00-1.00   sec  3.95 MBytes  33.1 Mbits/sec                  
[  5]   1.00-2.00   sec  5.84 MBytes  49.0 Mbits/sec                  
[  5]   2.00-3.00   sec  4.79 MBytes  40.2 Mbits/sec                  
[  5]   3.00-4.00   sec  5.13 MBytes  43.0 Mbits/sec                  
[  5]   4.00-5.00   sec  5.40 MBytes  45.3 Mbits/sec                  
[  5]   5.00-6.00   sec  5.67 MBytes  47.6 Mbits/sec                  
[  5]   6.00-7.00   sec  5.08 MBytes  42.6 Mbits/sec                  
[  5]   7.00-8.00   sec  5.62 MBytes  47.2 Mbits/sec                  
[  5]   8.00-9.00   sec  5.79 MBytes  48.5 Mbits/sec                  
[  5]   9.00-10.00  sec  5.63 MBytes  47.2 Mbits/sec                  
[  5]  10.00-11.00  sec  4.94 MBytes  41.5 Mbits/sec                  
[  5]  11.00-12.00  sec  5.10 MBytes  42.8 Mbits/sec                  
[  5]  12.00-13.00  sec  6.29 MBytes  52.7 Mbits/sec                  
[  5]  13.00-14.00  sec  6.05 MBytes  50.7 Mbits/sec                  
[  5]  14.00-15.00  sec  6.02 MBytes  50.6 Mbits/sec                  
[  5]  15.00-16.00  sec  5.10 MBytes  42.8 Mbits/sec                  
[  5]  16.00-17.00  sec  5.10 MBytes  42.8 Mbits/sec                  
[  5]  17.00-18.00  sec  5.13 MBytes  43.0 Mbits/sec                  
[  5]  18.00-19.00  sec  5.08 MBytes  42.6 Mbits/sec                  
[  5]  19.00-20.00  sec  4.94 MBytes  41.5 Mbits/sec                  
[  5]  20.00-21.00  sec  4.86 MBytes  40.8 Mbits/sec                  
[  5]  21.00-22.00  sec  5.06 MBytes  42.4 Mbits/sec                  
[  5]  22.00-23.00  sec  5.00 MBytes  41.9 Mbits/sec                  
[  5]  23.00-24.00  sec  5.38 MBytes  45.1 Mbits/sec                  
[  5]  24.00-25.00  sec  4.92 MBytes  41.2 Mbits/sec                  
[  5]  25.00-26.00  sec  4.77 MBytes  40.0 Mbits/sec                  
[  5]  26.00-27.00  sec  4.38 MBytes  36.6 Mbits/sec                  
[  5]  27.00-28.00  sec  5.61 MBytes  47.2 Mbits/sec                  
[  5]  28.00-29.00  sec  5.48 MBytes  46.0 Mbits/sec                  
[  5]  29.00-30.00  sec  5.15 MBytes  43.2 Mbits/sec                  
[  5]  30.00-31.00  sec  5.25 MBytes  44.1 Mbits/sec                  
[  5]  31.00-32.00  sec  5.30 MBytes  44.4 Mbits/sec                  
[  5]  32.00-33.00  sec  5.41 MBytes  45.4 Mbits/sec                  
[  5]  33.00-34.00  sec  5.38 MBytes  45.2 Mbits/sec                  
[  5]  34.00-35.00  sec  5.46 MBytes  45.8 Mbits/sec                  
[  5]  35.00-36.00  sec  4.94 MBytes  41.4 Mbits/sec                  
[  5]  36.00-37.00  sec  5.39 MBytes  45.2 Mbits/sec                  
[  5]  37.00-38.00  sec  5.55 MBytes  46.6 Mbits/sec                  
[  5]  38.00-39.00  sec  4.81 MBytes  40.4 Mbits/sec                  
[  5]  39.00-40.00  sec  4.82 MBytes  40.4 Mbits/sec                  
[  5]  40.00-40.19  sec   857 KBytes  37.8 Mbits/sec                  
- - - - - - - - - - - - - - - - - - - - - - - - -
[ ID] Interval           Transfer     Bandwidth
[  5]   0.00-40.19  sec  0.00 Bytes  0.00 bits/sec                  sender
[  5]   0.00-40.19  sec   210 MBytes  43.9 Mbits/sec                  receiver
```

Результат работы `iperf3` на ВМ `client`:
```bash
[root@client ~]# iperf3 -c 10.10.10.1 -t 40 -i 5
Connecting to host 10.10.10.1, port 5201
[  4] local 10.10.10.2 port 38890 connected to 10.10.10.1 port 5201
[ ID] Interval           Transfer     Bandwidth       Retr  Cwnd
[  4]   0.00-5.00   sec  28.5 MBytes  47.8 Mbits/sec    0   1.03 MBytes       
[  4]   5.00-10.00  sec  27.2 MBytes  45.6 Mbits/sec   45   1.19 MBytes       
[  4]  10.00-15.00  sec  28.5 MBytes  47.8 Mbits/sec    7   1.03 MBytes       
[  4]  15.00-20.00  sec  25.9 MBytes  43.5 Mbits/sec    0   1.06 MBytes       
[  4]  20.00-25.00  sec  24.7 MBytes  41.4 Mbits/sec    0   1.16 MBytes       
[  4]  25.00-30.00  sec  26.0 MBytes  43.7 Mbits/sec   85    521 KBytes       
[  4]  30.00-35.00  sec  26.0 MBytes  43.6 Mbits/sec    0    541 KBytes       
[  4]  35.00-40.00  sec  26.1 MBytes  43.7 Mbits/sec    0    694 KBytes       
- - - - - - - - - - - - - - - - - - - - - - - - -
[ ID] Interval           Transfer     Bandwidth       Retr
[  4]   0.00-40.00  sec   213 MBytes  44.6 Mbits/sec  137             sender
[  4]   0.00-40.00  sec   210 MBytes  44.1 Mbits/sec                  receiver

iperf Done.
```

Также обратим внимание на вывод команды `ip neigh`, где должны увидеть MAC адрес соседа `10.10.10.1`:
```bash
[root@client ~]# ip neigh 
10.0.2.2 dev eth0 lladdr 52:54:00:12:35:02 REACHABLE
10.0.2.3 dev eth0 lladdr 52:54:00:12:35:03 STALE
192.168.10.10 dev eth1 lladdr 08:00:27:22:90:c3 STALE
10.10.10.1 dev tap0 lladdr 96:91:69:7b:a8:a0 STALE
```

### Демонстрация TUN

1. В новом окне терминала перейдем в папку `ansible`, и выполним запуск `playbook` скрипта, который сконфигурирует openvpn на всех ВМ в режиме TUN: `ansible-playbook switch-to-tun.yml`
2. Вернемся в терминал с ВМ `client` и выполним повторный запуск `iperf3`: `iperf3 -c 10.10.10.1 -t 40 -i 5`

Результат работы `iperf3` на ВМ `server`:
```bash
Accepted connection from 10.10.10.2, port 38892
[  5] local 10.10.10.1 port 5201 connected to 10.10.10.2 port 38894
[ ID] Interval           Transfer     Bandwidth
[  5]   0.00-1.00   sec  2.87 MBytes  24.1 Mbits/sec                  
[  5]   1.00-2.00   sec  5.29 MBytes  44.4 Mbits/sec                  
[  5]   2.00-3.00   sec  5.85 MBytes  49.0 Mbits/sec                  
[  5]   3.00-4.01   sec  5.53 MBytes  46.1 Mbits/sec                  
[  5]   4.01-5.00   sec  5.99 MBytes  50.6 Mbits/sec                  
[  5]   5.00-6.00   sec  5.96 MBytes  50.0 Mbits/sec                  
[  5]   6.00-7.00   sec  5.95 MBytes  49.9 Mbits/sec                  
[  5]   7.00-8.00   sec  5.66 MBytes  47.5 Mbits/sec                  
[  5]   8.00-9.00   sec  5.93 MBytes  49.7 Mbits/sec                  
[  5]   9.00-10.00  sec  6.24 MBytes  52.4 Mbits/sec                  
[  5]  10.00-11.00  sec  5.85 MBytes  49.1 Mbits/sec                  
[  5]  11.00-12.00  sec  5.26 MBytes  44.1 Mbits/sec                  
[  5]  12.00-13.00  sec  5.79 MBytes  48.6 Mbits/sec                  
[  5]  13.00-14.00  sec  5.31 MBytes  44.6 Mbits/sec                  
[  5]  14.00-15.00  sec  4.88 MBytes  41.0 Mbits/sec                  
[  5]  15.00-16.00  sec  5.16 MBytes  43.3 Mbits/sec                  
[  5]  16.00-17.00  sec  4.89 MBytes  41.1 Mbits/sec                  
[  5]  17.00-18.00  sec  4.78 MBytes  40.1 Mbits/sec                  
[  5]  18.00-19.00  sec  5.09 MBytes  42.7 Mbits/sec                  
[  5]  19.00-20.00  sec  4.46 MBytes  37.4 Mbits/sec                  
[  5]  20.00-21.00  sec  4.67 MBytes  39.2 Mbits/sec                  
[  5]  21.00-22.00  sec  4.61 MBytes  38.7 Mbits/sec                  
[  5]  22.00-23.00  sec  4.62 MBytes  38.7 Mbits/sec                  
[  5]  23.00-24.00  sec  4.62 MBytes  38.8 Mbits/sec                  
[  5]  24.00-25.00  sec  4.61 MBytes  38.7 Mbits/sec                  
[  5]  25.00-26.00  sec  4.72 MBytes  39.6 Mbits/sec                  
[  5]  26.00-27.00  sec  5.25 MBytes  44.0 Mbits/sec                  
[  5]  27.00-28.00  sec  5.16 MBytes  43.3 Mbits/sec                  
[  5]  28.00-29.00  sec  4.69 MBytes  39.3 Mbits/sec                  
[  5]  29.00-30.00  sec  4.29 MBytes  35.9 Mbits/sec                  
[  5]  30.00-31.00  sec  4.98 MBytes  41.9 Mbits/sec                  
[  5]  31.00-32.00  sec  4.96 MBytes  41.5 Mbits/sec                  
[  5]  32.00-33.00  sec  4.93 MBytes  41.5 Mbits/sec                  
[  5]  33.00-34.00  sec  4.71 MBytes  39.5 Mbits/sec                  
[  5]  34.00-35.00  sec  5.03 MBytes  42.2 Mbits/sec                  
[  5]  35.00-36.00  sec  4.59 MBytes  38.5 Mbits/sec                  
[  5]  36.00-37.00  sec  4.87 MBytes  40.9 Mbits/sec                  
[  5]  37.00-38.00  sec  4.62 MBytes  38.7 Mbits/sec                  
[  5]  38.00-39.00  sec  4.80 MBytes  40.3 Mbits/sec                  
[  5]  39.00-40.00  sec  4.70 MBytes  39.4 Mbits/sec                  
[  5]  40.00-40.31  sec   983 KBytes  26.0 Mbits/sec                  
- - - - - - - - - - - - - - - - - - - - - - - - -
[ ID] Interval           Transfer     Bandwidth
[  5]   0.00-40.31  sec  0.00 Bytes  0.00 bits/sec                  sender
[  5]   0.00-40.31  sec   203 MBytes  42.3 Mbits/sec                  receiver
```

Результат работы `iperf3` на ВМ `client`:

```bash
[root@client ~]# iperf3 -c 10.10.10.1 -t 40 -i 5
Connecting to host 10.10.10.1, port 5201
[  4] local 10.10.10.2 port 38894 connected to 10.10.10.1 port 5201
[ ID] Interval           Transfer     Bandwidth       Retr  Cwnd
[  4]   0.00-5.01   sec  27.7 MBytes  46.4 Mbits/sec   23    547 KBytes       
[  4]   5.01-10.00  sec  29.5 MBytes  49.5 Mbits/sec    2    532 KBytes       
[  4]  10.00-15.01  sec  27.4 MBytes  46.0 Mbits/sec    0    552 KBytes       
[  4]  15.01-20.01  sec  24.3 MBytes  40.7 Mbits/sec    7    493 KBytes       
[  4]  20.01-25.00  sec  23.3 MBytes  39.2 Mbits/sec    1    478 KBytes       
[  4]  25.00-30.00  sec  23.7 MBytes  39.7 Mbits/sec  165    383 KBytes       
[  4]  30.00-35.00  sec  25.2 MBytes  42.3 Mbits/sec    0    546 KBytes       
[  4]  35.00-40.01  sec  23.3 MBytes  39.1 Mbits/sec    0    563 KBytes       
- - - - - - - - - - - - - - - - - - - - - - - - -
[ ID] Interval           Transfer     Bandwidth       Retr
[  4]   0.00-40.01  sec   204 MBytes  42.9 Mbits/sec  198             sender
[  4]   0.00-40.01  sec   203 MBytes  42.6 Mbits/sec                  receiver

iperf Done.
```

Также обратим внимание на вывод команды `ip neigh`, теперь мы уже не видим MAC адрес соседа `10.10.10.1`:
```bash
[root@client ~]# ip neigh 
10.0.2.2 dev eth0 lladdr 52:54:00:12:35:02 REACHABLE
10.0.2.3 dev eth0 lladdr 52:54:00:12:35:03 STALE
192.168.10.10 dev eth1 lladdr 08:00:27:22:90:c3 STALE
```

### Вывод

**TUN**, а именно сетевой TUNnel, имитирует устройство сетевого уровня и работает на уровне 3, передавая **IP-пакеты**. 

**TAP**, а именно сетевой TAP, имитирует устройство канального уровня и работает на уровне 2, передавая **кадры Ethernet**.

В моем случае, большой разницы между TUN и TAP в производительности нет, потому что среда `virtualbox` использует свои драйвера и API в гостевых ВМ а также взаимодействует с ядром хостовой машины, которая также работает в режиме вложенной виртуализации. На практике в основном все зависит от производительности среды, где запущен сервис `openvpn` и от каналов связи с удаленными клиентами. Использовать TAP интерфейс можно для [протокола OSPF](https://habr.com/ru/post/348360/) например.

---

_Ниже приведен примерный перевод части статьи из [wiki openvpn](https://community.openvpn.net/openvpn/wiki/BridgingAndRouting)._

Это обсуждение необходимо начать с устройств TAP и TUN. Вам нужен TAP, если:
- Вы хотите транспортировать трафик, не основанный на IP, или трафик IPv6 в OpenVPN 2.2 или более ранних версиях.
- Вы хотите провести мост.

И вы хотите провести мост, если:
- Вы хотите, чтобы ваши клиенты LAN и VPN находились в одном широковещательном домене.
- Вы хотите, чтобы ваш DHCP-сервер в локальной сети предоставлял DHCP-адреса вашему VPN-клиенту.
- У вас есть серверы Windows, к которым вы хотите получить доступ, и требуется, чтобы обнаружение сетевого окружения работало через VPN, а WINS не может быть реализован. Если у вас есть WINS, вам не нужен мост. Действительно.

Может быть еще несколько причин, но это самые типичные. И, как вы видите, TAP является обязательным условием для установления моста. Устройства TUN нельзя использовать для мостов и не-IP-трафика.

На первый взгляд мостовое соединение выглядит проще, но оно приносит совершенно другие проблемы. Не заблуждайтесь: нет простых способов упростить работу в сети, кроме как научиться делать это правильно.

Теперь давайте посмотрим на преимущества и недостатки TAP и TUN.

**Преимущества TAP:**
- ведет себя как настоящий сетевой адаптер (за исключением того, что это виртуальный сетевой адаптер)
- может передавать любые сетевые протоколы (IPv4, IPv6, Netalk, IPX и т. д. и т. д.)
- Работает на уровне 2, то есть кадры Ethernet передаются по VPN-туннелю.
- Может использоваться в мостах

**Недостатки ТАР:**
- вызывает гораздо большие накладные расходы на широковещательную передачу в туннеле VPN
- добавляет накладные расходы на заголовки Ethernet для всех пакетов, передаваемых через VPN-туннель
- плохо масштабируется
- нельзя использовать с устройствами Android или iOS

**Преимущества TUN:**
- Более низкие накладные расходы на трафик, транспортирует только тот трафик, который предназначен для VPN-клиента.
- Транспортирует только IP-пакеты уровня 3

**Недостатки TUN:**
- Широковещательный трафик обычно не транспортируется
- Может передавать только IPv4 (OpenVPN 2.3 добавляет IPv6)
- Нельзя использовать в мостах

Пожалуйста, поймите, что в обоих случаях необходимы базовые знания в области сетевых технологий. Вам нужно понимать основы сетевой маршрутизации и брандмауэра, независимо от того, используете ли вы маршрутизацию, мост, TUN или TAP. Оба устройства TUN и TAP поддерживают традиционную сетевую маршрутизацию, поэтому вам не нужно использовать мост с TAP. Но, используя мосты, вам нужно дополнительно знать, как работают мосты и как это меняет ваш брандмауэр. Проще говоря: мостовое соединение еще больше усложнит вашу настройку. Конечно, есть сценарии, в которых мост действительно является правильным решением. Но в большинстве случаев вы, скорее всего, решите свою настройку VPN с помощью базовой маршрутизации.

---

### Remote Access Server на базе OpenVPN

Для удобства написан [playbook](ansible/rsa-openvpn.yml), который автоматически настроит сервер, создаст ЦС, сгенерирует ключи и сертификаты. **Порт UDP 1207, через который будет осуществляться обслуживание соединений openvpn будет прокинут на хост, поэтому нужно убедится что свободен.**
1. Можем удалить ВМ `client` из предыдущего раздела: `vagrant destroy -f client`
2. Переходим в подпапку `openvpn` папки с репозиторием данного проекта.
3. Выполняем запуск `playbook`: `ansible-playbook rsa-openvpn.yml`

Скрипт выполнит генерацию клиентских сертификатов и скопирует на хостовую машину в папку `/tmp/rsa-openvpn`:
```bash
/tmp/rsa-demo/
└── server
    └── etc
        └── openvpn
            ├── client.conf
            └── pki
                ├── ca.crt
                ├── issued
                │   └── client.crt
                └── private
                    └── client.key

6 directories, 4 files
```

4. Необходимо установить на хостовую машину пакет `openvpn` и перейти в каталог `/tmp/rsa-openvpn`: `cd /tmp/rsa-openvpn`
5. Выполняем запуск клиента: `openvpn --config client.conf`

### OpenConnect VPN Server

Движок для безопасной и масштабируемой инфраструктуры VPN

[Сервер OpenConnect VPN (ocserv)](https://gitlab.com/openconnect/ocserv/) — это Linux SSL VPN-сервер с открытым исходным кодом, разработанный для организаций, которым требуется удаленный доступ к VPN с управлением и контролем корпоративных пользователей.

Сервер OpenConnect VPN предназначен для обеспечения конфиденциальности, защиты клиентов от доступа к данным друг друга с помощью строгой изоляции и разделения привилегий, защищает каналы VPN с использованием только стандартных протоколов, таких как TLS и Datagram TLS, и предотвращает утечку криптографических ключей с помощью аппаратных модулей безопасности ( HSM).

Общедоступные облака сегодня предоставляют доступ к огромной вычислительной мощности высокопроизводительных ЦП, а также маломощных и недорогих ЦП. Ocserv предназначен для использования преимуществ этой вычислительной мощности, а его производительность и способность обслуживать клиентов линейно масштабируются с количеством процессоров. Он имеет очень низкий объем памяти, чтобы гарантировать, что ограниченные в памяти и, следовательно, более дешевые хосты могут предоставить необходимые ресурсы для обработки тысяч клиентов.

Модуль проверки подлинности сервера может работать с использованием простого файла паролей и методов проверки подлинности, которые интегрируются с настройками вашей организации, таких как Radius, OpenID Connect, Kerberos, смарт-карты и их комбинации для включения двухфакторной проверки подлинности. Это также позволяет вам получать подробные бухгалтерские отчеты с помощью Radius.

Существуют различные сценарии развертывания ocserv, начиная от интеграции с letsencrypt и заканчивая интеграцией с FreeIPA и Radius. Узнайте, как развернуть ocserv в нашем разделе рецептов.

Сервер Openconnect предоставляет интерфейс управления пользователями, который позволяет вам запрашивать не только статус сервера, информацию о подключенных пользователях, а также отслеживать и выдавать команды для управления пользователями. Все с использованием интерфейса командной строки инструмента «occtl»; получить вывод либо на консоль, либо в формате JSON.

Есть три клиента:

[OpenConnect](https://www.infradead.org/openconnect/index.html) - Универсальный клиент командной строки, включающий клиентскую библиотеку.

[OpenConnect GUI](https://gitlab.com/openconnect/openconnect-gui/) - Кроссплатформенный (MacOS, Windows) графический клиент.

[Android client source](https://gitlab.com/openconnect/ics-openconnect) - Обратите внимание, что в магазине Android нет клиентов openconnect, связанных с нашей командой разработчиков.

### Настройка OpenConnect VPN Server на Centos7

Для автоматической настройки ocserv на ВМ `server` можно запустить [playbook](ansible/ocserv-setup.yml):

* В новом окне терминала перейдем в папку `ansible`, и выполним запуск: `ansible-playbook ocserv-setup.yml`

* Можно пропустить шаги в инструкции ниже, и перейти к [настройке клиента](#настройка-клиента-openconnect-на-хостовой-машине-с-linux-mint)

#### Подготовка ВМ

Если используем ВМ `server` из предыдущего раздела можно пропустить 1й шаг и остановить сервис `openvpn@server`: `systemctl disable --now openvpn@server`, затем продолжаем.

1. Подключаем репозиторий EPEL (Extra Packages for Enterprise Linux): `yum install -y epel-release`
2. Устанавливаем пакет `ocserv`: `yum install -y ocserv`
3. Заменяем файл /usr/sbin/ocserv-genkey [файлом из репозитория](ocserv/ocserv-genkey): `cp -f /vagrant/ocserv-genkey /usr/sbin/`
4. Добавляем в файл /etc/hosts хостовой машины строчку: `192.168.10.10 server.loc`

#### Настройка

* Все команды выполняются от привилегированного пользователя. Рекомендуется войти в систему с помощью root. Если это невозможно, выполните `su root` или `sudo su`, чтобы получить высшие привилегии.

* Сервер Linux (ВМ server) уже настроен как маршрутизатор (`sysctl -w net.ipv4.ip_forward=1`) ~~и имеет работающий брандмауэр (iptables).~~

```
Требования:

- сеть 192.168.250.0/24 (макса 255.255.255.0)
- IP-адрес сервера ocserv: 192.168.10.10
- имя хоста ocserv: server.loc
- метод аутентификации, используемый для тестирования: pam
```

1. Создаем каталог для хранения сертификатов: `mkdir ~/certificates`
2. Переходим в каталог: `cd ~/certificates`
3. Создаем шаблоны ЦС:

```bash
cat >~/certificates/ca.tmpl <<__EOF
cn = "Otus CA"
organization = "LLC Otus"
serial = 1
expiration_days = 3650
ca
signing_key
cert_signing_key
crl_signing_key
__EOF
```

4. Создаем шаблнон сервера:

```bash
cat >~/certificates/server.tmpl <<__EOF
cn = "VPN Server"
organization = "LLC Otus"
serial = 2
expiration_days = 3650
signing_key
encryption_key
tls_www_server
dns_name = "server.loc"
__EOF
```

5. Скопировать эти шаблоны в папку /etc/pki/ocserv/, позднее при старте сервиса он сам сгенерирует и установит на места:

```bash
cp ca.tmpl server.tmpl /etc/pki/ocserv/
```

Шаги с 5 по 7 включительно можно пропустить, но я оставлю ручную генерацию ключей и сертификатов:

5. Сгенерировать ключ ЦС, сертификат ЦС:

```bash
certtool --generate-privkey --outfile ca-key.pem
certtool --generate-self-signed --load-privkey ca-key.pem --template ca.tmpl --outfile ca-cert.pem
```

6. Сгенерировать ключ сервера и сертификат:

```bash
certtool --generate-privkey --outfile server-key.pem
certtool --generate-certificate --load-privkey server-key.pem --load-ca-certificate ca-cert.pem --load-ca-privkey ca-key.pem --template server.tmpl --outfile server-cert.pem
```

7. Копируем сертификаты и ключи сервера:

```bash
cp server-cert.pem server-key.pem /etc/ocserv/
cp ca-cert.pem /etc/pki/ocserv/cacerts/
```

8. Настраиваем файл конфигурации /etc/ocserv/ocserv.conf:
```bash
cp /etc/ocserv/ocserv.conf /etc/ocserv/ocserv.conf.backup
sed -i 's|^isolate-workers = true|#isolate-workers = true|' /etc/ocserv/ocserv.conf
sed -i 's|^ipv4-network = .*$|ipv4-network = 192.168.250.0/24|' /etc/ocserv/ocserv.conf
sed -i 's|^dns = .*|dns = 8.8.8.8|' /etc/ocserv/ocserv.conf
# В качестве примера буду отправлять один маршрут
sed -i 's|^route = .*|route = 172.16.0.1/255.255.255.0|' /etc/ocserv/ocserv.conf
#
# Пропустить следующие три команды если шаблоны ca и server скидывали в папку /etc/pki/ocserv
#
sed -i 's|^server-cert = .*|server-cert = /etc/ocserv/server-cert.pem|' /etc/ocserv/ocserv.conf
sed -i 's|^server-key = .*|server-key = /etc/ocserv/server-key.pem|' /etc/ocserv/ocserv.conf
sed -i 's|^ca-cert = .*|ca-cert = /etc/pki/ocserv/cacerts/ca-cert.pem|' /etc/ocserv/ocserv.conf
```

9. Активируем и запускаем сервис ocserv:
```bash
systemctl enable --now ocserv
```

10. Копируем сертификат ЦС (/etc/pki/ocserv/cacerts/ca.crt) с гостевой ВМ на хостовую машину в домашнюю папку, он клиенту обязательно нужен.

#### Настройка клиента openconnect на хостовой машине с Linux Mint

Все команды ниже не забываем выполнять с повышенными привилегиями (sudo -i).

1. Устанавливаем пакет `openconnect` (в моем случае установлена Linux Mint): 
`apt install openconnect network-manager-openconnect-gnome`
2. Следующий шаг можно пропустить, усли выполнялась работа скрипта [playbook](ansible/ocserv-setup.yml),  запуск осуществляем так: 
`openconnect -b --cafile=/tmp/server/etc/pki/ocserv/cacerts/ca.crt --user=vagrant server.loc`
3. Запускаем: 
`openconnect -b --cafile=ca.crt --user=vagrant server.loc`
    - флаг `-b` позволит запустить клиент в фоне, останавливать через `kill <PID>` (`ps aux | grep openconnect`)
    - флаг `--cafile=<путь к файлу>` путь к сертификату ЦС
4. Вводим пароль: `vagrant`

```bash
root@test-virtual-machine:~# openconnect -b --cafile=/home/test/1/ca.crt --user=vagrant server.loc
POST https://server.loc/
Connected to 192.168.10.10:443
SSL negotiation with server.loc
Connected to HTTPS on server.loc
XML POST enabled
Please enter your username.
POST https://server.loc/auth
Please enter your password.
Password:
POST https://server.loc/auth
Got CONNECT response: HTTP/1.1 200 CONNECTED
CSTP connected. DPD 90, Keepalive 32400
Connected as 192.168.250.120, using SSL, with DTLS in progress
Continuing in background; pid 410407
root@test-virtual-machine:~# Established DTLS connection (using GnuTLS). Ciphersuite (DTLS1.2)-(PSK)-(AES-128-GCM).
Connect Banner:
| Welcome to LLC Otus


root@test-virtual-machine:~# ip -c a
...
59: tun0: <POINTOPOINT,MULTICAST,NOARP,UP,LOWER_UP> mtu 1434 qdisc fq_codel state UNKNOWN group default qlen 500
    link/none 
    inet 192.168.250.120/32 scope global tun0
       valid_lft forever preferred_lft forever
    inet6 fe80::9daa:99f3:7e65:ceaf/64 scope link stable-privacy 
       valid_lft forever preferred_lft forever
root@test-virtual-machine:~# ip ro
default dev tun0 scope link 
default via 192.168.182.2 dev ens33 proto dhcp metric 100 
172.16.0.0/24 dev tun0 proto static scope link metric 50
192.168.10.0/24 dev vboxnet4 proto kernel scope link src 192.168.10.1 
192.168.10.10 dev vboxnet4 scope link src 192.168.10.1 
192.168.182.0/24 dev ens33 proto kernel scope link src 192.168.182.129 metric 100 
192.168.250.0/24 dev tun0 scope link 
root@test-virtual-machine:~#
```

Обращаю внимание что имеется несколько маршрутов по-умолчанию.

Результат работы iperf3 на хостовой машине:

```bash
root@test-virtual-machine:~# iperf3 -c 192.168.250.1 -t 40 -i 5
Connecting to host 192.168.250.1, port 5201
[  5] local 192.168.250.120 port 47944 connected to 192.168.250.1 port 5201
[ ID] Interval           Transfer     Bitrate         Retr  Cwnd
[  5]   0.00-5.00   sec  80.2 MBytes   134 Mbits/sec   26    107 KBytes       
[  5]   5.00-10.00  sec  80.5 MBytes   135 Mbits/sec   19    107 KBytes       
[  5]  10.00-15.00  sec  75.6 MBytes   127 Mbits/sec   15    124 KBytes       
[  5]  15.00-20.00  sec  76.9 MBytes   129 Mbits/sec   15    112 KBytes       
[  5]  20.00-25.00  sec  77.1 MBytes   129 Mbits/sec   23    109 KBytes       
[  5]  25.00-30.00  sec  81.3 MBytes   136 Mbits/sec   19    101 KBytes       
[  5]  30.00-35.00  sec  85.5 MBytes   143 Mbits/sec   23   95.8 KBytes       
[  5]  35.00-40.00  sec  87.8 MBytes   147 Mbits/sec   26    108 KBytes       
- - - - - - - - - - - - - - - - - - - - - - - - -
[ ID] Interval           Transfer     Bitrate         Retr
[  5]   0.00-40.00  sec   645 MBytes   135 Mbits/sec  166             sender
[  5]   0.00-40.00  sec   644 MBytes   135 Mbits/sec                  receiver

iperf Done.
```

Результат работы iperf3 на `server`:

```bash
[root@server cacerts]# iperf3 -s &
[1] 1884
[root@server cacerts]# -----------------------------------------------------------
Server listening on 5201
-----------------------------------------------------------
Accepted connection from 192.168.250.120, port 47930
[  5] local 192.168.250.1 port 5201 connected to 192.168.250.120 port 47944
[ ID] Interval           Transfer     Bandwidth
[  5]   0.00-1.00   sec  13.8 MBytes   115 Mbits/sec                  
[  5]   1.00-2.00   sec  14.6 MBytes   123 Mbits/sec                  
[  5]   2.00-3.00   sec  16.5 MBytes   139 Mbits/sec                  
[  5]   3.00-4.00   sec  17.6 MBytes   147 Mbits/sec                  
[  5]   4.00-5.00   sec  16.9 MBytes   141 Mbits/sec                  
[  5]   5.00-6.00   sec  17.5 MBytes   147 Mbits/sec                  
[  5]   6.00-7.00   sec  17.1 MBytes   144 Mbits/sec                  
[  5]   7.00-8.00   sec  16.5 MBytes   138 Mbits/sec                  
[  5]   8.00-9.00   sec  14.6 MBytes   122 Mbits/sec                  
[  5]   9.00-10.00  sec  14.7 MBytes   123 Mbits/sec                  
[  5]  10.00-11.01  sec  14.5 MBytes   122 Mbits/sec                  
[  5]  11.01-12.00  sec  15.0 MBytes   126 Mbits/sec                  
[  5]  12.00-13.00  sec  15.4 MBytes   129 Mbits/sec                  
[  5]  13.00-14.00  sec  15.4 MBytes   130 Mbits/sec                  
[  5]  14.00-15.00  sec  15.4 MBytes   129 Mbits/sec                  
[  5]  15.00-16.01  sec  15.4 MBytes   128 Mbits/sec                  
[  5]  16.01-17.00  sec  15.2 MBytes   128 Mbits/sec                  
[  5]  17.00-18.00  sec  15.6 MBytes   130 Mbits/sec                  
[  5]  18.00-19.01  sec  15.2 MBytes   126 Mbits/sec                  
[  5]  19.01-20.00  sec  15.4 MBytes   130 Mbits/sec                  
[  5]  20.00-21.00  sec  15.8 MBytes   133 Mbits/sec                  
[  5]  21.00-22.00  sec  15.3 MBytes   129 Mbits/sec                  
[  5]  22.00-23.01  sec  15.2 MBytes   127 Mbits/sec                  
[  5]  23.01-24.01  sec  15.5 MBytes   130 Mbits/sec                  
[  5]  24.01-25.01  sec  15.6 MBytes   131 Mbits/sec                  
[  5]  25.01-26.00  sec  15.2 MBytes   128 Mbits/sec                  
[  5]  26.00-27.01  sec  16.2 MBytes   136 Mbits/sec                  
[  5]  27.01-28.00  sec  16.0 MBytes   135 Mbits/sec                  
[  5]  28.00-29.00  sec  15.5 MBytes   130 Mbits/sec                  
[  5]  29.00-30.00  sec  18.0 MBytes   151 Mbits/sec                  
[  5]  30.00-31.01  sec  18.5 MBytes   154 Mbits/sec                  
[  5]  31.01-32.01  sec  16.9 MBytes   142 Mbits/sec                  
[  5]  32.01-33.00  sec  17.2 MBytes   145 Mbits/sec                  
[  5]  33.00-34.00  sec  15.5 MBytes   130 Mbits/sec                  
[  5]  34.00-35.00  sec  17.6 MBytes   149 Mbits/sec                  
[  5]  35.00-36.01  sec  17.5 MBytes   146 Mbits/sec                  
[  5]  36.01-37.00  sec  17.3 MBytes   146 Mbits/sec                  
[  5]  37.00-38.00  sec  17.8 MBytes   149 Mbits/sec                  
[  5]  38.00-39.00  sec  17.4 MBytes   146 Mbits/sec                  
[  5]  39.00-40.00  sec  17.8 MBytes   149 Mbits/sec                  
[  5]  40.00-40.04  sec   603 KBytes   129 Mbits/sec                  
- - - - - - - - - - - - - - - - - - - - - - - - -
[ ID] Interval           Transfer     Bandwidth
[  5]   0.00-40.04  sec  0.00 Bytes  0.00 bits/sec                  sender
[  5]   0.00-40.04  sec   644 MBytes   135 Mbits/sec                  receiver
```

Настройка плагина network-manager-openconnect:

![image](https://raw.githubusercontent.com/mmmex/vpn/master/ocserv/screen1.png)

Процесс авторизации:

![image](https://raw.githubusercontent.com/mmmex/vpn/master/ocserv/screen2.png)

Уведомление при подключении:

![image](https://raw.githubusercontent.com/mmmex/vpn/master/ocserv/screen3.png)

Базовая настройка окончена.

[Полезная ссылка](https://ocserv.gitlab.io/www/manual.html)

#### Техническая информация OpenConnect VPN Server

_Ниже приведен примерный перевод с [официальной страницы проекта](http://ocserv.gitlab.io/www/technical.html)_

**Как работает VPN**

**Дизайн**

VPN-сервер OpenConnect — это VPN-сервер интернет-уровня. То есть он предоставляет клиенту IP-адрес и список маршрутов, к которым этот IP может получить доступ. Его дизайн отличается от других VPN-серверов с открытым исходным кодом. Он делегирует несколько элементов, связанных с протоколами, которые сегодня считаются основными функциями и диверсификаторами VPN, стандартным протоколам и проверенным системным компонентам. Это делегирование перемещает необходимость аудита из-за криптографических протоколов и протоколов согласования на выделенные для этой цели компоненты, обеспечивая минимальное ядро ​​​​VPN.

В частности, VPN-сервер OpenConnect использует стандартные протоколы, такие как HTTP, TLS и DTLS, для обеспечения безопасности и подлинности данных. Подробное описание OpenConnect и его истории [было представлено на FOSDEM 2016](https://www.youtube.com/watch?v=ac66xdRZOZg), а подробное описание протокола было представлено в [IETF в качестве информационного проекта](https://tools.ietf.org/html/draft-mavrogiannopoulos-openconnect). Неофициальное высокоуровневое описание протокола, используемого сервером OpenConnect VPN, можно найти [в этом сообщении блога](http://nmav.gnutls.org/2013/11/inside-ssl-vpn-protocol.html).

**Настройка VPN**

Сервер использует один файл конфигурации, обычно присутствующий в `/etc/ocserv/ocserv.conf`. Параметры в файле задокументированы, однако единственные параметры, которые требуют изменения в случае, если сервер работает с настройками по умолчанию, — это адреса подсети, указанные в параметрах конфигурации `ipv4-network` и `ipv4-netmask`. Маршруты, передаваемые клиенту, задаются параметром `route`. Каждому клиенту назначается два IPv4-адреса, его VPN-адрес и его локальный образ (помните, что это двухточечное соединение). Образ не известен клиенту, так как протокол openconnect не пересылает его, но в последних выпусках ocserv это первый адрес предоставленной сети (например, 192.168.1.1 для 192.168.1.0/24).

Чтобы обеспечить высокую скорость передачи, ocserv не выполняет пересылку или фильтрацию пакетов между сетями. Предполагается, что на сервере установлены все необходимые маршруты или правила брандмауэра. Вы можете условно включить правила брандмауэра или даже включить правила маршрутизации через клиент, используя сценарии `connect-script` и `disconnect-script` в зависимости от пользователя, который подключился. Обратите внимание, что важно, чтобы эти скрипты не зависали и завершались без больших задержек.

См. [раздел рецептов](http://ocserv.gitlab.io/www/recipes.html) для получения дополнительной информации о настройке сервера openconnect.

**Интерфейс управления**

При развертывании сервера для обслуживания нескольких пользователей необходимо иметь интерфейс управления пользователями не только для решения потенциальных проблем пользователей, но и для управления подключенными пользователями, т. е. для отключения пользователей, у которых больше нет доступа, и т. д. Интерфейс предоставляется инструментом occtl, который можно использовать либо в интерактивном режиме, либо в качестве внутреннего интерфейса для другого интерфейса, поскольку он может предоставлять выходные данные в формате JSON. См. [справочную страницу occtl для получения дополнительной информации](http://ocserv.gitlab.io/www/occtl.html).

**Базовая аутентификация**

Базовая аутентификация по паролю на VPN-сервере openconnect происходит в начальном защищенном сеансе HTTP через TLS. В этом сеансе клиенту предоставляется страница аутентификации XML. Сервер аутентифицируется с помощью своего сертификата, а клиент либо с помощью своего сертификата, либо с помощью пары имени пользователя и пароля, которые пересылаются в PAM, либо с помощью комбинации пароля и сертификата. Поскольку PAM поддерживает различные типы аутентификации, пароль, введенный пользователем, может быть одноразовым паролем или любым другим паролем, поддерживаемым PAM. После аутентификации пользователя ему предоставляется файл cookie, который можно использовать для будущих подключений. Время жизни файла cookie настраивается с помощью параметра `cookie-timeout`.

После того, как пользователь аутентифицирован напрямую или через файл cookie, он отправляет HTTP-команду CONNECT через установленный сеанс TLS, что приводит к прямому соединению с VPN. Кроме того, пользователь может подключиться с помощью UDP и Datagram TLS (DTLS) к порту, предоставленному сервером. Это соединение аутентифицируется либо с помощью DTLS с предварительными ключами с ключом, полученным из исходного сеанса TLS, либо в более ранних версиях с возобновлением сеанса и мастер-ключом, предоставленным сервером.

**Механизмы безопасности**

В настоящее время сервер использует два привилегированных процесса: «мастер», который порождает непривилегированные процессы, обрабатывающие все взаимодействия с клиентами, и процесс «модуль безопасности», который выполняет все операции с закрытым ключом сервера. Разделение между двумя привилегированными процессами предназначено для предотвращения любой утечки закрытого ключа или учетных данных пользователя в непривилегированных процессах. Непривилегированные процессы изолированы с помощью [seccomp](http://en.wikipedia.org/wiki/Seccomp) и могут получать доступ только к операциям с ключами, а не к самим ключам, а также аутентифицировать пользователей с помощью удаленных вызовов процедур.

**Переустановка VPN-канала с помощью файлов cookie**

В типичных повседневных рабочих процессах пользователи перемещаются по разным сегментам сети или приостанавливают и возобновляют свое оборудование, например, переключаются с кабеля на Wi-Fi, приостанавливают свои планшеты / мобильные телефоны, поэтому быстрое переподключение VPN является необходимой функцией для их размещения. . OpenConnect использует «куки», отправленный клиенту в качестве токена аутентификации, который используется для быстрого роуминга, например, когда пользователь переключает сети или приостанавливает работу своего ноутбука и повторно подключается в течение срока действия куки. Фактическое время действия файла cookie — это время активного сеанса (т. е. файл cookie всегда действителен, когда сеанс активен) плюс значение `cookie-timeout`, установленное в файле конфигурации сервера.

В течение срока действия файла cookie пользователь может восстановить сеанс, связанный с ним, и любой сеанс, использующий этот файл cookie, будет удален.

**Работа VPN-канала**

Основной канал VPN устанавливается через TCP и TLS. Это канал управления, а также канал резервного копирования данных. После его установления инициируется канал UDP с использованием DTLS, который служит основным каналом данных. Если канал UDP не может быть установлен или временно недоступен, используется резервный канал через TCP/TLS.

**Сжатие**

Этот сервер позволяет сжимать независимые IP-пакеты без сохранения состояния, чтобы уменьшить размер передаваемых данных. Поддерживаемые алгоритмы: LZS и LZ4.

Обратите внимание, что поддерживаемое сжатие намеренно не имеет состояния, чтобы IP-пакеты из несвязанных источников не влияли на размер друг друга. Это не предотвратит [все атаки с восстановлением открытого текста, использующие сжатие](http://security.blogoverflow.com/2012/09/how-can-you-protect-yourself-from-crime-beasts-successor/), но предотвратит легкое использование настроек VPN. Критическим фактором при использовании сжатия для восстановления открытого текста зашифрованных данных является возможность объединения данных злоумышленника с законными данными. Если вы не уверены в ценности и характере передаваемых данных, всегда безопасно отключить сжатие (по умолчанию на этом сервере).

![image](https://raw.githubusercontent.com/mmmex/vpn/master/ocserv/design.png)

---

[Полезная статья](https://habr.com/ru/post/479034/)