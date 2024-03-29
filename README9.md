Домашнее задание к занятию "3.7. Компьютерные сети, лекция 2"


1. Проверьте список доступных сетевых интерфейсов на вашем компьютере. Какие команды есть для этого в Linux и в Windows?

Просмотр сетевых интерфейсов в файловой системе, в папке /sys/class/net:
ivan@HP-Pavilion-dv6:~$ ls /sys/class/net
eno1  lo  wlo1
ivan@HP-Pavilion-dv6:~$

Просмотр интерфейсов через утилиту ifconfig:
ivan@HP-Pavilion-dv6:~$ ifconfig
eno1: flags=4099<UP,BROADCAST,MULTICAST>  mtu 1500
        ether 68:b5:99:e2:50:5a  txqueuelen 1000  (Ethernet)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Локальная петля (Loopback))
        RX packets 2053  bytes 190520 (190.5 KB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 2053  bytes 190520 (190.5 KB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

wlo1: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.1.5  netmask 255.255.255.0  broadcast 192.168.1.255
        inet6 fe80::9204:fe6:c1a0:45ab  prefixlen 64  scopeid 0x20<link>
        ether ec:55:f9:5c:9e:ae  txqueuelen 1000  (Ethernet)
        RX packets 12599  bytes 11473854 (11.4 MB)
        RX errors 0  dropped 1  overruns 0  frame 0
        TX packets 10712  bytes 1801303 (1.8 MB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

Просмотр интерфейсов через утилиту ip:
ivan@HP-Pavilion-dv6:~$ ip link show
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN mode DEFAULT group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
2: eno1: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc fq_codel state DOWN mode DEFAULT group default qlen 1000
    link/ether 68:b5:99:e2:50:5a brd ff:ff:ff:ff:ff:ff
    altname enp7s0
3: wlo1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP mode DORMANT group default qlen 1000
    link/ether ec:55:f9:5c:9e:ae brd ff:ff:ff:ff:ff:ff
    altname wlp13s0
	
ivan@HP-Pavilion-dv6:~$ ip -br link show
lo               UNKNOWN        00:00:00:00:00:00 <LOOPBACK,UP,LOWER_UP> 
eno1             DOWN           68:b5:99:e2:50:5a <NO-CARRIER,BROADCAST,MULTICAST,UP> 
wlo1             UP             ec:55:f9:5c:9e:ae <BROADCAST,MULTICAST,UP,LOWER_UP> 

В Windows можно воспользоваться ipconfig.


2. Какой протокол используется для распознавания соседа по сетевому интерфейсу? Какой пакет и команды есть в Linux для этого?

LLDP – протокол для обмена информацией между соседними устройствами, позволяет определить к какому порту коммутатора подключен сервер.
Пакет для установки - apt install lldpd

Включаем и запускаем службу lldpd через systemctl:

van@HP-Pavilion-dv6:~$ sudo systemctl enable lldpd
Synchronizing state of lldpd.service with SysV service script with /lib/systemd/systemd-sysv-install.
Executing: /lib/systemd/systemd-sysv-install enable lldpd

ivan@HP-Pavilion-dv6:~$ sudo systemctl start lldpd

Проверяем наличие соседних устройств:

ivan@HP-Pavilion-dv6:~$ sudo bash
root@HP-Pavilion-dv6:/home/ivan# lldpctl
-------------------------------------------------------------------------------
LLDP neighbors:
-------------------------------------------------------------------------------
root@HP-Pavilion-dv6:/home/ivan# 
 
Видим, что соседние устройства не подключены.


3. Какая технология используется для разделения L2 коммутатора на несколько виртуальных сетей? Какой пакет и команды есть в Linux для этого? Приведите пример конфига.

Для разделения коммутатора на виртуальные сети используется VLAN. Устройства, находящиеся в разных VLAN, будут находиться в разных подсетях.

В linux устанавливается пакет vlan.
ivan@HP-Pavilion-dv6:~$ sudo apt install vlan

Открываем файл /etc/network/interfaces через vim:

ivan@HP-Pavilion-dv6:~$ vim /etc/network/interfaces

и прописываем конфиг:

auto eno1.100	(«поднимать» интерфейс eno1 при запуске сетевой службы c vlan с ID-100 )  
iface eno1.100 inet static	(название интерфейса)
address 192.168.100.2	(назначаем IP-адрес интерфейсу VLAN)
netmask 255.255.255.0	(задаем маску сети, т.е. определяем рамзер сети VLAN, в нашем случае 254 хоста)
vlan_raw_device eno1	(указывает на каком физическом интерфейсе создавать VLAN)

Сохраняем и закрываем файл /etc/network/interfaces. После чего перезапускаем сеть.


4. Какие типы агрегации интерфейсов есть в Linux? Какие опции есть для балансировки нагрузки? Приведите пример конфига.

Объединение сетевых интерфейсов(Bonding) – это механизм, используемый Linux-серверами и предполагающий связь нескольких физических интерфейсов в один виртуальный, 
что позволяет обеспечить большую пропускную способность или отказоустойчивость в случае повреждения кабеля.

LAG в Linux – бондинг, имя интерфейса bond0, bond1.
Типы LAG:
● статический (на Cisco mode on);
● динамический – LACP протокол (на Cisco mode active).

Linux поддерживает несколько режимов агрегации интерфейсов:
mode 0 (balance-rr) — round-robin распределение пакетов между интерфейсами. Обеспечивает отказоустойчивость и повышение пропускной способности
mode 1 (active-backup) — в каждый момент времени работает только один интерфейс, в случае его выхода из строя, mac-адрес назначается второму интерфейсу и трафик переключается на него
mode 2 (balance-xor) — обеспечивает балансировку между интерфейсами на основании MAC-адресов отправителя и получателя
mode 3 (broadcast) — отправляет пакеты через все интерфейсы одновременно, обеспечивает отказоустойчивость
mode 4 (802.3ad) — обеспечивает агрегацию на основании протокола 802.3ad
mode 5 (balance-tlb) — в этом режиме входящий трафик приходит только на один «активный» интерфейс, исходящий же распределяется по всем интерфейсам
mode 6 (balance-alb) — балансирует исходящий трафик как tlb, а так же входящий IPv4 трафик используя ARP

Установка модуля ядра для поддержки объединения:
 sudo modprobe bonding

Установка пакета ifenslave:
 sudo apt-get install ifenslave
 
Пример конфига для создания постоянного связанного интерфейса типа mode 0 (balance-rr).
 При этом методе объединения трафик распределяется по принципу «карусели»: пакеты по очереди направляются на сетевые карты объединённого интерфейса. 
 Например, если у нас есть физические интерфейсы eth0, eth1, and eth2, объединенные в bond0, первый пакет будет отправляться через eth0, второй — через eth1, 
 третий — через eth2, а четвертый снова через eth0 и т.д.

Открываем файл /etc/network/interfaces через vim и прописываем конфиг:

auto bond0
iface bond0 inet static
    address 192.168.100.3
    netmask 255.255.255.0    
    gateway 192.168.100.1
    dns-nameservers 192.168.100.1 8.8.8.8
    dns-search domain.local
        slaves eno1 eno2
        bond_mode 0
        bond-miimon 100
        bond_downdelay 200
        bound_updelay 200
		
Перезапускаем сетевую службу, отключаем физические интерфейсы и включаем объединенный интерфейс:
sudo systemctl restart networking.service
sudo ifdown eno1 && ifdown eno2 && ifup bond0

Для проверки используем ifconfig.
Информацию об объединенном интерфейсе можно получить в файле:
/proc/net/bonding/bond0


5. Сколько IP адресов в сети с маской /29 ? Сколько /29 подсетей можно получить из сети с маской /24. Приведите несколько примеров /29 подсетей внутри сети 10.10.10.0/24.

В сети с маской /29 всего 8 ip-адресов.
Например:
Address:	192.168.1.1
Netmask:	255.255.255.248 = 29
Network:	192.168.1.0
Hosts/Net:	6
Broadcast:	192.168.1.7
т.е. в подсети будет 6 адресов хостов и 2 служебных адреса (адрес подсети и широковещательный адрес)

В сети с маской /24 можно получить 32 подсети с маской /29.
Расчет:
В сети с маской /24 максимальное количество хостов 254.
+ 2 служебных адреса (адрес подсети и широковещательный адрес)
Получаем 256 адресов.
Делим 256 на 8 и получаем 32.

Приведите несколько примеров /29 подсетей внутри сети 10.10.10.0/24.
Address:	10.10.10.0	
Bitmask:	29	
Netmask:	255.255.255.248 = 29	
Wildcard:	0.0.0.7	
Network:	10.10.10.0	
Broadcast:	10.10.10.7	
HostMin:	10.10.10.1	
HostMax:	10.10.10.6	
Hosts/Net:	6	

Address:	10.10.10.8	
Bitmask:	29	
Netmask:	255.255.255.248 = 29	
Wildcard:	0.0.0.7	
Network:	10.10.10.8	
Broadcast:	10.10.10.15	
HostMin:	10.10.10.9	
HostMax:	10.10.10.14	
Hosts/Net:	6

Address:	10.10.10.16	
Bitmask:	29	
Netmask:	255.255.255.248 = 29	
Wildcard:	0.0.0.7	
Network:	10.10.10.16
Broadcast:	10.10.10.23
HostMin:	10.10.10.17
HostMax:	10.10.10.22
Hosts/Net:	6


6. Задача: вас попросили организовать стык между 2-мя организациями. Диапазоны 10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16 уже заняты. 
Из какой подсети допустимо взять частные IP адреса? Маску выберите из расчета максимум 40-50 хостов внутри подсети. 

Можно взять частные адреса из диапазона 100.64.0.0 — 100.127.255.255 (маска подсети: 255.192.0.0 или /10) Carrier-Grade NAT.
Возьмем, например, подсеть 100.127.255.192 с маской /26 на 62 хоста.

Address:	100.127.255.254	01100100.01111111.11111111.11   111110
Bitmask:	26	
Netmask:	255.255.255.192 = 26	11111111.11111111.11111111.11   000000
Wildcard:	0.0.0.63	00000000.00000000.00000000.00   111111
Network:	100.127.255.192	01100100.01111111.11111111.11   000000 (Class A)
Broadcast:	100.127.255.255	01100100.01111111.11111111.11   111111
HostMin:	100.127.255.193	01100100.01111111.11111111.11   000001
HostMax:	100.127.255.254	01100100.01111111.11111111.11   111110
Hosts/Net:	62	


7. Как проверить ARP таблицу в Linux, Windows? Как очистить ARP кеш полностью? Как из ARP таблицы удалить только один нужный IP? 

Для проверки ARP в Линукс воспользуемся командой arp:
ivan@HP-Pavilion-dv6:~$ arp -a
? (192.168.1.109) в <не завершено> на wlo1
? (192.168.1.100) в <не завершено> на wlo1
? (192.168.1.150) в <не завершено> на wlo1
? (192.168.1.116) в <не завершено> на wlo1
? (192.168.1.55) в <не завершено> на wlo1
? (192.168.1.123) в <не завершено> на wlo1
? (192.168.1.200) в <не завершено> на wlo1
? (192.168.1.3) в 1c:bf:c0:1d:15:e3 [ether] на wlo1
? (192.168.1.53) в <не завершено> на wlo1
? (192.168.1.103) в <не завершено> на wlo1
_gateway (192.168.1.1) в c4:3d:c7:8f:63:98 [ether] на wlo1
? (192.168.1.119) в <не завершено> на wlo1
? (192.168.1.128) в <не завершено> на wlo1
? (192.168.1.67) в <не завершено> на wlo1
? (192.168.1.101) в <не завершено> на wlo1
? (192.168.1.33) в <не завершено> на wlo1
? (192.168.1.74) в <не завершено> на wlo1
? (192.168.1.210) в <не завершено> на wlo1
? (192.168.1.158) в <не завершено> на wlo1
? (192.168.1.90) в <не завершено> на wlo1
? (192.168.1.38) в <не завершено> на wlo1
? (192.168.1.72) в <не завершено> на wlo1
? (192.168.1.104) в <не завершено> на wlo1
? (192.168.1.52) в <не завершено> на wlo1
? (192.168.1.102) в <не завершено> на wlo1
? (192.168.1.34) в <не завершено> на wlo1
ivan@HP-Pavilion-dv6:~$ 

Для проверки ARP в Windows действует та же команда arp (c различными опциями).

Полностью очистить ARP кэш можно командой:
ip -s -s neigh flush all

*** Round 1, deleting 31 entries ***
*** Flush is complete after 1 round ***
ivan@HP-Pavilion-dv6:~$ arp -a
_gateway (192.168.1.1) в c4:3d:c7:8f:63:98 [ether] на wlo1
ivan@HP-Pavilion-dv6:~$ 

Удалить один IP из ARP таблицы можно командой: arp -d 192.168.1.2 (указываем нужный ip).
