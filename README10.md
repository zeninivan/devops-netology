Домашнее задание к занятию "3.8. Компьютерные сети, лекция 3"


1. Подключитесь к публичному маршрутизатору в интернет. Найдите маршрут к вашему публичному IP

telnet route-views.routeviews.org
Username: rviews
show ip route  91.223.89.29/32
show bgp 91.223.89.29/32

ivan@HP-Pavilion-dv6:~/devops-netology$ telnet route-views.routeviews.org
Trying 128.223.51.103...
Connected to route-views.routeviews.org.
Escape character is '^]'.
C
**********************************************************************

                    RouteViews BGP Route Viewer
                    route-views.routeviews.org

 route views data is archived on http://archive.routeviews.org

 This hardware is part of a grant by the NSF.
 Please contact help@routeviews.org if you have questions, or
 if you wish to contribute your view.

 This router has views of full routing tables from several ASes.
 The list of peers is located at http://www.routeviews.org/peers
 in route-views.oregon-ix.net.txt

 NOTE: The hardware was upgraded in August 2014.  If you are seeing
 the error message, "no default Kerberos realm", you may want to
 in Mac OS X add "default unset autologin" to your ~/.telnetrc

 To login, use the username "rviews".
 **********************************************************************

User Access Verification
Username: rviews

route-views>show ip route  91.223.89.29   
Routing entry for 91.223.89.0/24
  Known via "bgp 6447", distance 20, metric 0
  Tag 6939, type external
  Last update from 64.71.137.241 3d18h ago
  Routing Descriptor Blocks:
  * 64.71.137.241, from 64.71.137.241, 3d18h ago
      Route metric is 0, traffic share count is 1
      AS Hops 2
      Route tag 6939
      MPLS label: none
route-views>

route-views>show bgp 91.223.89.29
BGP routing table entry for 91.223.89.0/24, version 311628114
Paths: (22 available, best #22, table default)
  Not advertised to any peer
  Refresh Epoch 1
  8283 1299 39087 39087 39087
    94.142.247.3 from 94.142.247.3 (94.142.247.3)
      Origin IGP, metric 0, localpref 100, valid, external
      Community: 1299:30000 8283:1 8283:101 8283:103
      unknown transitive attribute: flag 0xE0 type 0x20 length 0x24
        value 0000 205B 0000 0000 0000 0001 0000 205B
              0000 0005 0000 0001 0000 205B 0000 0005
              0000 0003 
      path 7FE08ED9FC68 RPKI State valid
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  1351 6939 39087
    132.198.255.253 from 132.198.255.253 (132.198.255.253)
      Origin IGP, localpref 100, valid, external
      path 7FE0BBCF8DB8 RPKI State valid
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  852 1299 39087 39087 39087
    154.11.12.212 from 154.11.12.212 (96.1.209.43)
      Origin IGP, metric 0, localpref 100, valid, external
      path 7FE0BA01BFD0 RPKI State valid
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  57866 9002 39087 39087 39087
    37.139.139.17 from 37.139.139.17 (37.139.139.17)
      Origin IGP, metric 0, localpref 100, valid, external
      Community: 9002:0 9002:64667
      path 7FE102E6E798 RPKI State valid
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  20130 6939 39087
    140.192.8.16 from 140.192.8.16 (140.192.8.16)
      Origin IGP, localpref 100, valid, external
      path 7FE1730F8278 RPKI State valid
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  3333 9002 39087 39087 39087
    193.0.0.56 from 193.0.0.56 (193.0.0.56)
      Origin IGP, localpref 100, valid, external
      path 7FE08A3FADC0 RPKI State valid
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  3549 3356 9002 39087 39087 39087
    208.51.134.254 from 208.51.134.254 (67.16.168.191)
      Origin IGP, metric 0, localpref 100, valid, external
      Community: 3356:2 3356:22 3356:100 3356:123 3356:503 3356:901 3356:2067 3549:2581 3549:30840
      path 7FE047433070 RPKI State valid
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  701 1273 12389 39087 39087 39087
    137.39.3.55 from 137.39.3.55 (137.39.3.55)
      Origin IGP, localpref 100, valid, external
      path 7FE1221ECC90 RPKI State valid
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  53767 174 12389 39087 39087 39087
    162.251.163.2 from 162.251.163.2 (162.251.162.3)
      Origin IGP, localpref 100, valid, external
      Community: 174:21101 174:22005 53767:5000
      path 7FE0547D0DC8 RPKI State valid
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  3356 9002 39087 39087 39087
    4.68.4.46 from 4.68.4.46 (4.69.184.201)
      Origin IGP, metric 0, localpref 100, valid, external
      Community: 3356:2 3356:22 3356:100 3356:123 3356:503 3356:901 3356:2067
      path 7FE0ECDA13A8 RPKI State valid
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  7018 1299 39087 39087 39087
    12.0.1.63 from 12.0.1.63 (12.0.1.63)
      Origin IGP, localpref 100, valid, external
      Community: 7018:5000 7018:37232
      path 7FE0101CF640 RPKI State valid
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  3303 29076 39087
    217.192.89.50 from 217.192.89.50 (138.187.128.158)
      Origin IGP, localpref 100, valid, external
      Community: 3303:1004 3303:1006 3303:1030 3303:1031 3303:3081 31210:29076 65000:10000
      path 7FE15FB6BC80 RPKI State valid
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  4901 6079 9002 39087 39087 39087
    162.250.137.254 from 162.250.137.254 (162.250.137.254)
      Origin IGP, localpref 100, valid, external
      Community: 65000:10100 65000:10300 65000:10400
      path 7FE18CA37CA8 RPKI State valid
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  20912 3257 1299 39087 39087 39087
    212.66.96.126 from 212.66.96.126 (212.66.96.126)
      Origin IGP, localpref 100, valid, external
      Community: 3257:8101 3257:30055 3257:50001 3257:53900 3257:53902 20912:65004
      path 7FE1518FDFB8 RPKI State valid
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  3561 3910 3356 9002 39087 39087 39087
    206.24.210.80 from 206.24.210.80 (206.24.210.80)
      Origin IGP, localpref 100, valid, external
      path 7FE16F2A4030 RPKI State valid
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
 --More-- 


2. Создайте dummy0 интерфейс в Ubuntu. Добавьте несколько статических маршрутов. Проверьте таблицу маршрутизации.

Загружаем модуль "dummy" и проверяем его статус:
ivan@HP-Pavilion-dv6:~$ sudo modprobe -v dummy numdummies=2
[sudo] пароль для ivan: 
insmod /lib/modules/5.13.0-30-generic/kernel/drivers/net/dummy.ko numdummies=0 numdummies=2

ivan@HP-Pavilion-dv6:~$ lsmod | grep dummy
dummy                  16384  0

Cоздадим файл dummy.conf:
ivan@HP-Pavilion-dv6:~$ sudo bash
root@HP-Pavilion-dv6:/home/ivan# echo "dummy" >> /etc/modules
root@HP-Pavilion-dv6:/home/ivan# echo "options dummy numdummies=2" > /etc/modprobe.d/dummy.conf

Проверяем созданные интерфейсы:
root@HP-Pavilion-dv6:/home/ivan# ifconfig -a | grep dummy
dummy0: flags=130<BROADCAST,NOARP>  mtu 1500
dummy1: flags=130<BROADCAST,NOARP>  mtu 1500

Добавляем IP адрес для интерфейса dummy0 и проверяем:
root@HP-Pavilion-dv6:/home/ivan# ip addr add 10.2.2.2/32 dev dummy0

root@HP-Pavilion-dv6:/home/ivan# ifconfig -a
dummy0: flags=130<BROADCAST,NOARP>  mtu 1500
        inet 10.2.2.2  netmask 255.255.255.255  broadcast 0.0.0.0
        ether 8e:f3:ec:8f:0b:60  txqueuelen 1000  (Ethernet)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

Чтобы при старте системы на dummy0 интерфейсе был IP адрес, добавим в файл конфигурации:
vim /etc/network/interfaces

auto dummy0
iface dummy0 inet static
address 10.2.2.2/32
pre-up ip link add dummy0 type dummy
post-down ip link del dummy0

Теперь добавляем 2 новых маршрута и проверяем таблицу маршрутизации:

ivan@HP-Pavilion-dv6:~$ sudo ip route add 8.8.8.0/24 via 192.168.1.1
ivan@HP-Pavilion-dv6:~$ sudo ip route add 8.8.0.0/16 dev dummy0

ivan@HP-Pavilion-dv6:~$ route
Таблица маршутизации ядра протокола IP
Destination Gateway Genmask Flags Metric Ref Use Iface
default         _gateway        0.0.0.0         UG    600    0        0 wlo1
8.8.0.0         0.0.0.0         255.255.0.0     U     0      0        0 dummy0
8.8.8.0         _gateway        255.255.255.0   UG    0      0        0 wlo1
link-local      0.0.0.0         255.255.0.0     U     1000   0        0 wlo1
192.168.1.0     0.0.0.0         255.255.255.0   U     600    0        0 wlo1
ivan@HP-Pavilion-dv6:~$ 
Маршруты успешно добавлены.

Полезные команды для этого задания:

Перезагрузка сетевой службы - sudo netplan apply

Для проверки в каком состоянии в данный момент находится форвардинг пакетов (включен или выключен), мы должны сделать запрос к ядру через команду sysctl.
sysctl net.ipv4.ip_forward
В ответ мы получим текущий статус (1 - включен, 0 - выключен)
net.ipv4.ip_forward = 0

Пример включения/отключения сетевых интерфейсов:
sudo ifconfig eth0 up
sudo ifconfig eth0 down
sudo ifconfig wlan0 up
sudo ifup ens1f0
sudo ifdown ens1f0


3. Проверьте открытые TCP порты в Ubuntu, какие протоколы и приложения используют эти порты? Приведите несколько примеров.

Для просмотра открытых портов воспользуемся утилитой netstat:
sudo netstat -ntp
Опция -p  показывает имя программы, -t или -u - отображают TCP или UDP порты, а -n показывает ip адреса в числовом виде.

ivan@HP-Pavilion-dv6:~$ sudo netstat -ntp
[sudo] пароль для ivan: 
Активные соединения с интернетом (w/o servers)
Proto Recv-Q Send-Q Local Address Foreign Address State       PID/Program name    
tcp        0     39 192.168.1.5:57638       77.88.21.119:443        ESTABLISHED 18353/firefox       
tcp        0     78 192.168.1.5:38494       213.180.204.90:443      FIN_WAIT1   -                   
tcp        0   5736 192.168.1.5:42598       5.255.255.88:443        FIN_WAIT1   -                   
tcp        0    310 192.168.1.5:36232       217.69.133.145:443      FIN_WAIT1   -                   
tcp        0      0 192.168.1.5:50802       52.35.145.245:443       ESTABLISHED 18353/firefox       
tcp        0      0 192.168.1.5:33710       140.82.114.26:443       ESTABLISHED 18353/firefox       
tcp        0      0 192.168.1.5:47426       64.233.161.95:443       TIME_WAIT   -                   
tcp        0   6942 192.168.1.5:50516       5.255.255.55:443        ESTABLISHED 18353/firefox       
ivan@HP-Pavilion-dv6:~$

На выводе показаны только порты, использующие протокол TCP. Большинство активных портов используются браузером Firefox, их статус ESTABLISHED. 
Порты в состоянии FIN_WAIT1 говорят о том, что процесс был успешно завершен, но порт все еще находится в открытом состоянии. 
Порты в состоянии TIME_WAIT говорят о том, что локальная конечная точка закрыла соединение. Соединение поддерживается таким образом, что любые задержанные 
пакеты могут быть сопоставлены с соединением и обработаны соответствующим образом. Соединения будут удалены, когда выйдет тайм-аут времени.

Также открытые порты в Ubuntu можно посмотреть с помощью утилиты SS.
ss -ltupn
ivan@HP-Pavilion-dv6:~$ ss -ltupn | grep tcp
tcp     LISTEN   0        10             127.0.0.1:2222           0.0.0.0:*                                                                                     
tcp     LISTEN   0        4096             0.0.0.0:60399          0.0.0.0:*                                                                                     
tcp     LISTEN   0        4096             0.0.0.0:111            0.0.0.0:*                                                                                     
tcp     LISTEN   0        4096             0.0.0.0:57813          0.0.0.0:*                                                                                     
tcp     LISTEN   0        4096       127.0.0.53%lo:53             0.0.0.0:*                                                                                     
tcp     LISTEN   0        5              127.0.0.1:631            0.0.0.0:*                                                                                     
tcp     LISTEN   0        4096             0.0.0.0:43225          0.0.0.0:*                                                                                     
tcp     LISTEN   0        64               0.0.0.0:44221          0.0.0.0:*                                                                                     
tcp     LISTEN   0        64               0.0.0.0:2049           0.0.0.0:*                                                                                     
tcp     LISTEN   0        4096                [::]:37443             [::]:*                                                                                     
tcp     LISTEN   0        4096                [::]:55717             [::]:*                                                                                     
tcp     LISTEN   0        4096                [::]:42983             [::]:*                                                                                     
tcp     LISTEN   0        4096                   *:9100                 *:*                                                                                     
tcp     LISTEN   0        4096                [::]:111               [::]:*                                                                                     
tcp     LISTEN   0        5                  [::1]:631               [::]:*                                                                                     
tcp     LISTEN   0        64                  [::]:46685             [::]:*                                                                                     
tcp     LISTEN   0        64                  [::]:2049              [::]:*                                                                                     
ivan@HP-Pavilion-dv6:~$

Также можно воспользоваться утилитой lsof. Основная ее функция - просмотр открытых файлов, но с её помощью можно посмотреть открытые порты Ubuntu.
sudo lsof -nP -i | grep TCP
ivan@HP-Pavilion-dv6:~$ sudo lsof -nP -i | grep TCP
systemd       1            root  229u  IPv4  17837      0t0  TCP *:111 (LISTEN)
systemd       1            root  231u  IPv6  17841      0t0  TCP *:111 (LISTEN)
rpcbind     662            _rpc    4u  IPv4  17837      0t0  TCP *:111 (LISTEN)
rpcbind     662            _rpc    6u  IPv6  17841      0t0  TCP *:111 (LISTEN)
systemd-r   666 systemd-resolve   13u  IPv4  32794      0t0  TCP 127.0.0.53:53 (LISTEN)
rpc.mount   861            root    9u  IPv4  36611      0t0  TCP *:57813 (LISTEN)
rpc.mount   861            root   11u  IPv6  36617      0t0  TCP *:37443 (LISTEN)
rpc.mount   861            root   13u  IPv4  36623      0t0  TCP *:43225 (LISTEN)
rpc.mount   861            root   15u  IPv6  36629      0t0  TCP *:42983 (LISTEN)
rpc.mount   861            root   17u  IPv4  36635      0t0  TCP *:60399 (LISTEN)
rpc.mount   861            root   19u  IPv6  36641      0t0  TCP *:55717 (LISTEN)
node_expo  1496   node_exporter    3u  IPv6  47566      0t0  TCP *:9100 (LISTEN)
cupsd      6054            root    6u  IPv6 125455      0t0  TCP [::1]:631 (LISTEN)
cupsd      6054            root    7u  IPv4 125456      0t0  TCP 127.0.0.1:631 (LISTEN)
VBoxHeadl 16700            ivan   20u  IPv4 287978      0t0  TCP 127.0.0.1:2222 (LISTEN)
firefox   18353            ivan   57u  IPv4 427886      0t0  TCP 192.168.1.5:38500->213.180.204.90:443 (ESTABLISHED)
firefox   18353            ivan   60u  IPv4 417783      0t0  TCP 192.168.1.5:42042->77.88.55.70:443 (ESTABLISHED)
firefox   18353            ivan   85u  IPv4 417781      0t0  TCP 192.168.1.5:52024->172.67.180.236:443 (ESTABLISHED)
firefox   18353            ivan  107u  IPv4 429171      0t0  TCP 192.168.1.5:46150->140.82.112.26:443 (ESTABLISHED)
firefox   18353            ivan  131u  IPv4 312251      0t0  TCP 192.168.1.5:50802->52.35.145.245:443 (ESTABLISHED)
firefox   18353            ivan  145u  IPv4 449289      0t0  TCP 192.168.1.5:36258->217.69.133.145:443 (ESTABLISHED)
firefox   18353            ivan  151u  IPv4 442549      0t0  TCP 192.168.1.5:49186->93.158.134.119:443 (ESTABLISHED)
firefox   18353            ivan  153u  IPv4 421367      0t0  TCP 192.168.1.5:40138->5.255.255.50:443 (ESTABLISHED)
firefox   18353            ivan  161u  IPv4 449261      0t0  TCP 192.168.1.5:54356->213.180.204.158:443 (ESTABLISHED)
ivan@HP-Pavilion-dv6:~$


4. Проверьте используемые UDP сокеты в Ubuntu, какие протоколы и приложения используют эти порты?

Воспользуемся теми же утилитами, что были использованы в прошлом задании, с поправкой на UDP.
sudo netstat -nup
Опция -p  показывает имя программы, -u - отображают UDP порты, а -n показывает ip адреса в числовом виде.

ivan@HP-Pavilion-dv6:~$ sudo netstat -nup
Активные соединения с интернетом (w/o servers)
Proto Recv-Q Send-Q Local Address Foreign Address State       PID/Program name    
udp        0      0 192.168.1.5:68          192.168.1.1:67          ESTABLISHED 711/NetworkManager  
ivan@HP-Pavilion-dv6:~$
Порт использует процесс 711/NetworkManager .

Просмотр с помощью утилиты SS.
ivan@HP-Pavilion-dv6:~$ ss -ltupn | grep udp
udp     UNCONN   0        0                0.0.0.0:44496          0.0.0.0:*                                                                                     
udp     UNCONN   0        0            192.168.1.5:32780          0.0.0.0:*      users:(("Socket Process",pid=18407,fd=25))                                     
udp     UNCONN   0        0          127.0.0.53%lo:53             0.0.0.0:*                                                                                     
udp     UNCONN   0        0                0.0.0.0:111            0.0.0.0:*                                                                                     
udp     UNCONN   0        0                0.0.0.0:631            0.0.0.0:*                                                                                     
udp     UNCONN   0        0                0.0.0.0:5353           0.0.0.0:*                                                                                     
udp     UNCONN   0        0                0.0.0.0:50470          0.0.0.0:*                                                                                     
udp     UNCONN   0        0                0.0.0.0:47032          0.0.0.0:*                                                                                     
udp     UNCONN   0        0                0.0.0.0:2049           0.0.0.0:*                                                                                     
udp     UNCONN   0        0                0.0.0.0:59448          0.0.0.0:*                                                                                     
udp     UNCONN   0        0                0.0.0.0:43106          0.0.0.0:*                                                                                     
udp     UNCONN   0        0                   [::]:111               [::]:*                                                                                     
udp     UNCONN   0        0                   [::]:33059             [::]:*                                                                                     
udp     UNCONN   0        0                   [::]:41323             [::]:*                                                                                     
udp     UNCONN   0        0                   [::]:46029             [::]:*                                                                                     
udp     UNCONN   0        0                   [::]:58487             [::]:*                                                                                     
udp     UNCONN   0        0                   [::]:5353              [::]:*                                                                                     
udp     UNCONN   0        0                   [::]:34341             [::]:*                                                                                     
udp     UNCONN   0        0                   [::]:2049              [::]:*                                                                                     
ivan@HP-Pavilion-dv6:~$

Просмотр с помощью утилиты lsof.
sudo lsof -nP -i | grep UDP

ivan@HP-Pavilion-dv6:~$ sudo lsof -nP -i | grep UDP
systemd       1            root  230u  IPv4  17838      0t0  UDP *:111 
systemd       1            root  232u  IPv6  17844      0t0  UDP *:111 
rpcbind     662            _rpc    5u  IPv4  17838      0t0  UDP *:111 
rpcbind     662            _rpc    7u  IPv6  17844      0t0  UDP *:111 
systemd-r   666 systemd-resolve   12u  IPv4  32793      0t0  UDP 127.0.0.53:53 
avahi-dae   705           avahi   12u  IPv4  35305      0t0  UDP *:5353 
avahi-dae   705           avahi   13u  IPv6  35306      0t0  UDP *:5353 
avahi-dae   705           avahi   14u  IPv4  35307      0t0  UDP *:43106 
avahi-dae   705           avahi   15u  IPv6  35308      0t0  UDP *:58487 
NetworkMa   711            root   23u  IPv4 204067      0t0  UDP 192.168.1.5:68->192.168.1.1:67 
rpc.mount   861            root    8u  IPv4  37065      0t0  UDP *:59448 
rpc.mount   861            root   10u  IPv6  36614      0t0  UDP *:34341 
rpc.mount   861            root   12u  IPv4  36620      0t0  UDP *:44496 
rpc.mount   861            root   14u  IPv6  36626      0t0  UDP *:33059 
rpc.mount   861            root   16u  IPv4  36632      0t0  UDP *:50470 
rpc.mount   861            root   18u  IPv6  36638      0t0  UDP *:46029 
cups-brow  6056            root    7u  IPv4 127709      0t0  UDP *:631 
Socket    18407            ivan   25u  IPv4 315558      0t0  UDP 192.168.1.5:32780 
ivan@HP-Pavilion-dv6:~$ 


5. Используя diagrams.net, создайте L3 диаграмму вашей домашней сети или любой другой сети, с которой вы работали.

Диаграмма "SPB L3 Diagram.drawio" создана и сохранена в GitHub репозитории.
Ссылка на диаграмму:
https://github.com/zeninivan/devops-netology/commit/f27bb5c3fc731c7ee5eb8b674bc4183bfa8e30a2


