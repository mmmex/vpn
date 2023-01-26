## VPN

- [X] Между двумя виртуалками поднять vpn в режимах
- [X] tun;
- [X] tap;
- [X] Прочуствовать разницу.
- [ ] Поднять RAS на базе OpenVPN с клиентскими сертификатами, подключиться с локальной машины на виртуалку.
- [ ] * Самостоятельно изучить, поднять ocserv и подключиться с хоста к виртуалке
- [ ] Формат сдачи ДЗ - vagrant + ansible

### Запуск проекта

* Клонируем репозиторий: `git clone ...`

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

1. В новом окне терминала перейдем в папку `openvpn`, и выполним запуск `playbook` скрипта, который сконфигурирует openvpn на всех ВМ в режиме TUN: `ansible-playbook switch-to-tun.yml`
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

Для удобства написан [playbook](openvpn/rsa-openvpn.yml), который автоматически настроит сервер, создаст ЦС, сгенерирует ключи и сертификаты.
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
