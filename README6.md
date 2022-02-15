Домашнее задание к занятию "3.4. Операционные системы, лекция 2"

1. На лекции мы познакомились с node_exporter. В демонстрации его исполняемый файл запускался в background. Этого достаточно для демо, но не для настоящей production-системы, 
где процессы должны находиться под внешним управлением. Используя знания из лекции по systemd, создайте самостоятельно простой unit-файл для node_exporter:

  -  поместите его в автозагрузку,
  -  предусмотрите возможность добавления опций к запускаемому процессу через внешний файл (посмотрите, например, на systemctl cat cron),
  -  удостоверьтесь, что с помощью systemctl процесс корректно стартует, завершается, а после перезагрузки автоматически поднимается.

Просмотр systemctl cat cron:

ivan@HP-Pavilion-dv6:~$ systemctl cat cron
# /lib/systemd/system/cron.service
[Unit]
Description=Regular background program processing daemon
Documentation=man:cron(8)
After=remote-fs.target nss-user-lookup.target
[Service]
EnvironmentFile=-/etc/default/cron
ExecStart=/usr/sbin/cron -f $EXTRA_OPTS
IgnoreSIGPIPE=false
KillMode=process
Restart=on-failure
[Install]
WantedBy=multi-user.target

Создаем простой unit-файл для node_exporter:
sudo systemctl edit --full --force node_exporter.service

Добавляем node_exporter.service в автозагрузку
ivan@HP-Pavilion-dv6:~$ sudo systemctl enable node_exporter.service
Created symlink /etc/systemd/system/multi-user.target.wants/node_exporter.service → /etc/systemd/system/node_exporter.service.

Проверяем:
ivan@HP-Pavilion-dv6:~$ sudo systemctl is-enabled node_exporter.service
enabled
ivan@HP-Pavilion-dv6:~$ 

Просмотр:
ivan@HP-Pavilion-dv6:~$ systemctl cat node_exporter.service
# /etc/systemd/system/node_exporter.service
[Unit]
Description=Prometheus Node Exporter
Wants=network-online.target
After=network-online.target
[Service]
User=node_exporter
Group=node_exporter
Type=simple
ExecStart=/usr/local/bin/node_exporter
Restart=on-failure
[Install]
WantedBy=multi-user.target
ivan@HP-Pavilion-dv6:~$ 

Старт:
ivan@HP-Pavilion-dv6:~$ sudo systemctl start node_exporter
ivan@HP-Pavilion-dv6:~$ sudo systemctl status node_exporter
● node_exporter.service - Prometheus Node Exporter
     Loaded: loaded (/etc/systemd/system/node_exporter.service; enabled; vendor preset: enabled)
     Active: active (running) since Sat 2022-02-05 21:10:12 MSK; 19s ago
   Main PID: 4657 (node_exporter)
      Tasks: 5 (limit: 9393)
     Memory: 13.4M
     CGroup: /system.slice/node_exporter.service
             └─4657 /usr/local/bin/node_exporter

фев 05 21:10:13 HP-Pavilion-dv6 node_exporter[4657]: ts=2022-02-05T18:10:13.012Z caller=node_exporter.go:115 level=info collector=thermal_zone
фев 05 21:10:13 HP-Pavilion-dv6 node_exporter[4657]: ts=2022-02-05T18:10:13.012Z caller=node_exporter.go:115 level=info collector=time
фев 05 21:10:13 HP-Pavilion-dv6 node_exporter[4657]: ts=2022-02-05T18:10:13.012Z caller=node_exporter.go:115 level=info collector=timex
фев 05 21:10:13 HP-Pavilion-dv6 node_exporter[4657]: ts=2022-02-05T18:10:13.012Z caller=node_exporter.go:115 level=info collector=udp_queues
фев 05 21:10:13 HP-Pavilion-dv6 node_exporter[4657]: ts=2022-02-05T18:10:13.012Z caller=node_exporter.go:115 level=info collector=uname
фев 05 21:10:13 HP-Pavilion-dv6 node_exporter[4657]: ts=2022-02-05T18:10:13.012Z caller=node_exporter.go:115 level=info collector=vmstat
фев 05 21:10:13 HP-Pavilion-dv6 node_exporter[4657]: ts=2022-02-05T18:10:13.012Z caller=node_exporter.go:115 level=info collector=xfs
фев 05 21:10:13 HP-Pavilion-dv6 node_exporter[4657]: ts=2022-02-05T18:10:13.012Z caller=node_exporter.go:115 level=info collector=zfs
фев 05 21:10:13 HP-Pavilion-dv6 node_exporter[4657]: ts=2022-02-05T18:10:13.013Z caller=node_exporter.go:199 level=info msg="Listening on" address=:9100
фев 05 21:10:13 HP-Pavilion-dv6 node_exporter[4657]: ts=2022-02-05T18:10:13.013Z caller=tls_config.go:195 level=info msg="TLS is disabled." http2=false
ivan@HP-Pavilion-dv6:~$ 

Рестарт:
ivan@HP-Pavilion-dv6:~$ sudo systemctl restart node_exporter
ivan@HP-Pavilion-dv6:~$ sudo systemctl status node_exporter
● node_exporter.service - Prometheus Node Exporter
     Loaded: loaded (/etc/systemd/system/node_exporter.service; enabled; vendor preset: enabled)
     Active: active (running) since Sat 2022-02-05 21:11:32 MSK; 2s ago
   Main PID: 4676 (node_exporter)
      Tasks: 5 (limit: 9393)
     Memory: 3.0M
     CGroup: /system.slice/node_exporter.service
             └─4676 /usr/local/bin/node_exporter

фев 05 21:11:32 HP-Pavilion-dv6 node_exporter[4676]: ts=2022-02-05T18:11:32.505Z caller=node_exporter.go:115 level=info collector=thermal_zone
фев 05 21:11:32 HP-Pavilion-dv6 node_exporter[4676]: ts=2022-02-05T18:11:32.505Z caller=node_exporter.go:115 level=info collector=time
фев 05 21:11:32 HP-Pavilion-dv6 node_exporter[4676]: ts=2022-02-05T18:11:32.505Z caller=node_exporter.go:115 level=info collector=timex
фев 05 21:11:32 HP-Pavilion-dv6 node_exporter[4676]: ts=2022-02-05T18:11:32.505Z caller=node_exporter.go:115 level=info collector=udp_queues
фев 05 21:11:32 HP-Pavilion-dv6 node_exporter[4676]: ts=2022-02-05T18:11:32.505Z caller=node_exporter.go:115 level=info collector=uname
фев 05 21:11:32 HP-Pavilion-dv6 node_exporter[4676]: ts=2022-02-05T18:11:32.505Z caller=node_exporter.go:115 level=info collector=vmstat
фев 05 21:11:32 HP-Pavilion-dv6 node_exporter[4676]: ts=2022-02-05T18:11:32.505Z caller=node_exporter.go:115 level=info collector=xfs
фев 05 21:11:32 HP-Pavilion-dv6 node_exporter[4676]: ts=2022-02-05T18:11:32.505Z caller=node_exporter.go:115 level=info collector=zfs
фев 05 21:11:32 HP-Pavilion-dv6 node_exporter[4676]: ts=2022-02-05T18:11:32.505Z caller=node_exporter.go:199 level=info msg="Listening on" address=:9100
фев 05 21:11:32 HP-Pavilion-dv6 node_exporter[4676]: ts=2022-02-05T18:11:32.507Z caller=tls_config.go:195 level=info msg="TLS is disabled." http2=false
ivan@HP-Pavilion-dv6:~$ 

Выполняем перезагрузку системы, видим, что процесс уже запущен:
ivan@HP-Pavilion-dv6:~$ sudo systemctl status node_exporter
[sudo] пароль для ivan: 
● node_exporter.service - Prometheus Node Exporter
     Loaded: loaded (/etc/systemd/system/node_exporter.service; enabled; vendor preset: enabled)
     Active: active (running) since Sat 2022-02-05 21:13:46 MSK; 2min 4s ago
   Main PID: 1595 (node_exporter)
      Tasks: 6 (limit: 9393)
     Memory: 13.4M
     CGroup: /system.slice/node_exporter.service
             └─1595 /usr/local/bin/node_exporter

фев 05 21:13:46 HP-Pavilion-dv6 node_exporter[1595]: ts=2022-02-05T18:13:46.892Z caller=node_exporter.go:115 level=info collector=thermal_zone
фев 05 21:13:46 HP-Pavilion-dv6 node_exporter[1595]: ts=2022-02-05T18:13:46.892Z caller=node_exporter.go:115 level=info collector=time
фев 05 21:13:46 HP-Pavilion-dv6 node_exporter[1595]: ts=2022-02-05T18:13:46.892Z caller=node_exporter.go:115 level=info collector=timex
фев 05 21:13:46 HP-Pavilion-dv6 node_exporter[1595]: ts=2022-02-05T18:13:46.892Z caller=node_exporter.go:115 level=info collector=udp_queues
фев 05 21:13:46 HP-Pavilion-dv6 node_exporter[1595]: ts=2022-02-05T18:13:46.892Z caller=node_exporter.go:115 level=info collector=uname
фев 05 21:13:46 HP-Pavilion-dv6 node_exporter[1595]: ts=2022-02-05T18:13:46.892Z caller=node_exporter.go:115 level=info collector=vmstat
фев 05 21:13:46 HP-Pavilion-dv6 node_exporter[1595]: ts=2022-02-05T18:13:46.892Z caller=node_exporter.go:115 level=info collector=xfs
фев 05 21:13:46 HP-Pavilion-dv6 node_exporter[1595]: ts=2022-02-05T18:13:46.892Z caller=node_exporter.go:115 level=info collector=zfs
фев 05 21:13:46 HP-Pavilion-dv6 node_exporter[1595]: ts=2022-02-05T18:13:46.892Z caller=node_exporter.go:199 level=info msg="Listening on" address=:9100
фев 05 21:13:46 HP-Pavilion-dv6 node_exporter[1595]: ts=2022-02-05T18:13:46.892Z caller=tls_config.go:195 level=info msg="TLS is disabled." http2=false
ivan@HP-Pavilion-dv6:~$

Стоп:
ivan@HP-Pavilion-dv6:~$ sudo systemctl stop node_exporter
ivan@HP-Pavilion-dv6:~$ sudo systemctl status node_exporter
● node_exporter.service - Prometheus Node Exporter
     Loaded: loaded (/etc/systemd/system/node_exporter.service; enabled; vendor preset: enabled)
     Active: inactive (dead) since Sat 2022-02-05 21:18:08 MSK; 4s ago
    Process: 1595 ExecStart=/usr/local/bin/node_exporter (code=killed, signal=TERM)
   Main PID: 1595 (code=killed, signal=TERM)

фев 05 21:13:46 HP-Pavilion-dv6 node_exporter[1595]: ts=2022-02-05T18:13:46.892Z caller=node_exporter.go:115 level=info collector=udp_queues
фев 05 21:13:46 HP-Pavilion-dv6 node_exporter[1595]: ts=2022-02-05T18:13:46.892Z caller=node_exporter.go:115 level=info collector=uname
фев 05 21:13:46 HP-Pavilion-dv6 node_exporter[1595]: ts=2022-02-05T18:13:46.892Z caller=node_exporter.go:115 level=info collector=vmstat
фев 05 21:13:46 HP-Pavilion-dv6 node_exporter[1595]: ts=2022-02-05T18:13:46.892Z caller=node_exporter.go:115 level=info collector=xfs
фев 05 21:13:46 HP-Pavilion-dv6 node_exporter[1595]: ts=2022-02-05T18:13:46.892Z caller=node_exporter.go:115 level=info collector=zfs
фев 05 21:13:46 HP-Pavilion-dv6 node_exporter[1595]: ts=2022-02-05T18:13:46.892Z caller=node_exporter.go:199 level=info msg="Listening on" address=:9100
фев 05 21:13:46 HP-Pavilion-dv6 node_exporter[1595]: ts=2022-02-05T18:13:46.892Z caller=tls_config.go:195 level=info msg="TLS is disabled." http2=false
фев 05 21:18:08 HP-Pavilion-dv6 systemd[1]: Stopping Prometheus Node Exporter...
фев 05 21:18:08 HP-Pavilion-dv6 systemd[1]: node_exporter.service: Succeeded.
фев 05 21:18:08 HP-Pavilion-dv6 systemd[1]: Stopped Prometheus Node Exporter.
ivan@HP-Pavilion-dv6:~$


2. Ознакомьтесь с опциями node_exporter и выводом /metrics по-умолчанию. Приведите несколько опций, которые вы бы выбрали для базового мониторинга хоста по CPU, памяти, диску и сети.

Мониторинг CPU:
# HELP node_cpu_frequency_max_hertz Maximum cpu thread frequency in hertz.
# TYPE node_cpu_frequency_max_hertz gauge
node_cpu_frequency_max_hertz{cpu="0"} 2.9e+09
node_cpu_frequency_max_hertz{cpu="1"} 2.9e+09
node_cpu_frequency_max_hertz{cpu="2"} 2.9e+09
node_cpu_frequency_max_hertz{cpu="3"} 2.9e+09

# HELP node_cpu_guest_seconds_total Seconds the CPUs spent in guests (VMs) for each mode.
# TYPE node_cpu_guest_seconds_total counter
node_cpu_guest_seconds_total{cpu="0",mode="nice"} 0
node_cpu_guest_seconds_total{cpu="0",mode="user"} 0
node_cpu_guest_seconds_total{cpu="1",mode="nice"} 0
node_cpu_guest_seconds_total{cpu="1",mode="user"} 0
node_cpu_guest_seconds_total{cpu="2",mode="nice"} 0
node_cpu_guest_seconds_total{cpu="2",mode="user"} 0
node_cpu_guest_seconds_total{cpu="3",mode="nice"} 0
node_cpu_guest_seconds_total{cpu="3",mode="user"} 0

# HELP node_cpu_seconds_total Seconds the CPUs spent in each mode.
# TYPE node_cpu_seconds_total counter
node_cpu_seconds_total{cpu="0",mode="idle"} 389.64
node_cpu_seconds_total{cpu="0",mode="iowait"} 2.99
node_cpu_seconds_total{cpu="0",mode="irq"} 0
node_cpu_seconds_total{cpu="0",mode="nice"} 4.34
node_cpu_seconds_total{cpu="0",mode="softirq"} 0.73
node_cpu_seconds_total{cpu="0",mode="steal"} 0
node_cpu_seconds_total{cpu="0",mode="system"} 17.78
node_cpu_seconds_total{cpu="0",mode="user"} 39.01

Мониторинг памяти:
# HELP node_memory_Active_bytes Memory information field Active_bytes.
# TYPE node_memory_Active_bytes gauge
node_memory_Active_bytes 7.77822208e+08

# HELP node_memory_Cached_bytes Memory information field Cached_bytes.
# TYPE node_memory_Cached_bytes gauge
node_memory_Cached_bytes 2.034098176e+09

# HELP node_memory_MemFree_bytes Memory information field MemFree_bytes.
# TYPE node_memory_MemFree_bytes gauge
node_memory_MemFree_bytes 4.358160384e+09

Мониторинг диска:
# HELP node_disk_discard_time_seconds_total This is the total number of seconds spent by all discards.
# TYPE node_disk_discard_time_seconds_total counter
node_disk_discard_time_seconds_total{device="sda"} 0
node_disk_discard_time_seconds_total{device="sr0"} 0

# HELP node_disk_flush_requests_total The total number of flush requests completed successfully
# TYPE node_disk_flush_requests_total counter
node_disk_flush_requests_total{device="sda"} 544
node_disk_flush_requests_total{device="sr0"} 0

# HELP node_disk_info Info of /sys/block/<block_device>.
# TYPE node_disk_info gauge
node_disk_info{device="sda",major="8",minor="0"} 1
node_disk_info{device="sr0",major="11",minor="0"} 1

Мониторинг сети:
# HELP node_network_mtu_bytes mtu_bytes value of /sys/class/net/<iface>.
# TYPE node_network_mtu_bytes gauge
node_network_mtu_bytes{device="eno1"} 1500
node_network_mtu_bytes{device="lo"} 65536
node_network_mtu_bytes{device="wlo1"} 1500

# HELP node_network_receive_drop_total Network device statistic receive_drop.
# TYPE node_network_receive_drop_total counter
node_network_receive_drop_total{device="eno1"} 0
node_network_receive_drop_total{device="lo"} 0
node_network_receive_drop_total{device="wlo1"} 0

# HELP node_network_receive_errs_total Network device statistic receive_errs.
# TYPE node_network_receive_errs_total counter
node_network_receive_errs_total{device="eno1"} 0
node_network_receive_errs_total{device="lo"} 0
node_network_receive_errs_total{device="wlo1"} 0


3. Установите в свою виртуальную машину Netdata. Воспользуйтесь готовыми пакетами для установки (sudo apt install -y netdata). После успешной установки:
    в конфигурационном файле /etc/netdata/netdata.conf в секции [web] замените значение с localhost на bind to = 0.0.0.0,
    добавьте в Vagrantfile проброс порта Netdata на свой локальный компьютер и сделайте vagrant reload:
config.vm.network "forwarded_port", guest: 19999, host: 19999
После успешной перезагрузки в браузере на своем ПК (не в виртуальной машине) вы должны суметь зайти на localhost:19999. Ознакомьтесь с метриками, которые по умолчанию собираются Netdata и с комментариями, которые даны к этим метрикам.
 

Netdata успешно установлен на VM:

Конфигурационный файл netdata.conf скорректирован:
vagrant@vagrant:/etc/netdata$ cat netdata.conf
# NetData Configuration

# The current full configuration can be retrieved from the running
# server at the URL
#
#   http://localhost:19999/netdata.conf
#
# for example:
#
#   wget -O /etc/netdata/netdata.conf http://localhost:19999/netdata.conf
#

[global]
	run as user = netdata
	web files owner = root
	web files group = root
	# Netdata is not designed to be exposed to potentially hostile
	# networks. See https://github.com/netdata/netdata/issues/164
	bind socket to IP = 0.0.0.0
vagrant@vagrant:/etc/netdata$ 

Проброс порта Netdata в Vagrantfile выполнен успешно:
ivan@HP-Pavilion-dv6:~/Vagrant$ cat Vagrantfile
..
..
  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # NOTE: This will enable public access to the opened port
  config.vm.network "forwarded_port", guest: 19999, host: 19999
 ..
 ..
 
 После перезагрузки vagrant reload удалось успешно зайти на localhost:19999 на хостовой машине. 
 Netdata собирает 1299 метрик. Сохранен снимок экрана.
 
 Every second, Netdata collects 1 299 metrics on vagrant, presents them in 224 charts and monitors them with 103 alarms.
netdata
v1.19.0


4. Можно ли по выводу dmesg понять, осознает ли ОС, что загружена не на настоящем оборудовании, а на системе виртуализации?

Да, при выводе dmesg на виртуальной машине видим следующее:

vagrant@vagrant:/$ dmesg |grep virtual
[    0.003661] CPU MTRRs all blank - virtualized system.
[    0.101211] Booting paravirtualized kernel on KVM  *(Загрузка паравиртуализированного ядра на Kernel-based Virtual Machine)
[    2.995292] systemd[1]: Detected virtualization oracle.
vagrant@vagrant:/$ 

А при выводе этой же команды на хостовой машине получим следующее:

ivan@HP-Pavilion-dv6:~$ dmesg |grep virtual
[    0.031862] Booting paravirtualized kernel on bare hardware   *(Загрузка паравиртуализированного ядра на голом оборудовании)
[    9.479978] input: HP WMI hotkeys as /devices/virtual/input/input20
..
..
ivan@HP-Pavilion-dv6:~$ 


5. Как настроен sysctl fs.nr_open на системе по-умолчанию? Узнайте, что означает этот параметр. Какой другой существующий лимит не позволит достичь такого числа (ulimit --help)?

ivan@HP-Pavilion-dv6:~$ sysctl fs.nr_open
fs.nr_open = 1048576
ivan@HP-Pavilion-dv6:~$

sysctl fs.nr_open показывает лимит, по умолчанию, на количество открытых дескрипторов файла в системе (в моем случае это Ubuntu 20.04.3 LTS) 
Значение задается с шагом 1024 байта
т.е. значение по умолчанию 1048576 (что составляет 1024 * 1024).

Верхний предел этого значения также записан в файле /proc/sys/fs/nr_open
ivan@HP-Pavilion-dv6:/proc/sys/fs$ cat nr_open
1048576
ivan@HP-Pavilion-dv6:/proc/sys/fs$ 

Существует также общесистемное ограничение для Linux /proc/sys/fs/file-max:
Этот файл определяет общесистемное ограничение на количество открытых файлов для всех процессов.
ivan@HP-Pavilion-dv6:/proc/sys/fs$ cat /proc/sys/fs/file-max
9223372036854775807
ivan@HP-Pavilion-dv6:/proc/sys/fs$

Количество выделенных файловых дескрипторов (то есть количество файлов, открытых в настоящее время); количество свободных файловых дескрипторов; максимальное количество файловых дескрипторов  -  покажет команда:
ivan@HP-Pavilion-dv6:/proc/sys/fs$ cat /proc/sys/fs/file-nr
21991	0	9223372036854775807
ivan@HP-Pavilion-dv6:/proc/sys/fs$ 

Также в системе есть "мягкие" и "жесткие" ограничения, которые можно выставить с помощью "ulimit".
Следует понимать, что ulimit меняет только текущие лимиты, для шелла и всех программ, запущенных в этом шелле, поэтому после завершения сессии или даже в другом окне терминала значения останутся прежними.

    Options:
      -S	use the `soft' resource limit
      -H	use the `hard' resource limit
      -a	all current limits are reported
      -n	the maximum number of open file descriptors
	  
Проверяем текущие лимиты:
ivan@HP-Pavilion-dv6:~/Vagrant$ ulimit -Sn
1024
ivan@HP-Pavilion-dv6:~/Vagrant$ ulimit -Hn
1048576
ivan@HP-Pavilion-dv6:~/Vagrant$


6. Запустите любой долгоживущий процесс (не ls, который отработает мгновенно, а, например, sleep 1h) в отдельном неймспейсе процессов; покажите, что ваш процесс работает под PID 1 через nsenter. 
Для простоты работайте в данном задании под root (sudo -i). Под обычным пользователем требуются дополнительные опции (--map-root-user) и т.д.
 
van@HP-Pavilion-dv6:~$ sudo bash
[sudo] пароль для ivan: 
root@HP-Pavilion-dv6:/home/ivan# sleep 1h

Находим запущенный процесс 
ivan@HP-Pavilion-dv6:~$ ps -e | grep sleep
   3077 pts/0    00:00:00 sleep
ivan@HP-Pavilion-dv6:~$ 

С помощью unshare позволяем процессу/потоку "sleep 1h" отделить части своего контекста выполнения:
root@HP-Pavilion-dv6:/home/ivan# unshare -f --pid --mount-proc sleep 1h
^C

Находим дочерний процесс sleep, его номер 3377
root@HP-Pavilion-dv6:/home/ivan# ps -e |grep sleep
   3077 pts/0    00:00:00 sleep
   3377 pts/1    00:00:00 sleep
root@HP-Pavilion-dv6:/home/ivan#

Использую nsenter заходим в неймспейс процессса PID 3377:
root@HP-Pavilion-dv6:/# nsenter --target 3377 --pid --mount
root@HP-Pavilion-dv6:/# ps
    PID TTY          TIME CMD
      1 pts/1    00:00:00 sleep
      2 pts/1    00:00:00 bash
     11 pts/1    00:00:00 ps
root@HP-Pavilion-dv6:/#

Видим, что процесс работает под PID 1 


7. Найдите информацию о том, что такое :(){ :|:& };:. Запустите эту команду в своей виртуальной машине Vagrant с Ubuntu 20.04 (это важно, поведение в других ОС не проверялось). 
Некоторое время все будет "плохо", после чего (минуты) – ОС должна стабилизироваться. Вызов dmesg расскажет, какой механизм помог автоматической стабилизации. 
Как настроен этот механизм по-умолчанию, и как изменить число процессов, которое можно создать в сессии?

 Команда :(){ :|:& };:  - является логической бомбой. Она оперирует определением функции с именем ‘:‘, которая вызывает сама себя дважды: один раз на переднем плане и один раз в фоне. 
 Она продолжает своё выполнение снова и снова, пока система не зависнет. 
 
 -bash: fork: Resource temporarily unavailable
-bash: fork: Resource temporarily unavailable
-bash: fork: Resource temporarily unavailable
^C
[1]+  Done                    : | :
vagrant@vagrant:~$

Судя по выводу dmesg, распространение функции :(){ :|:& };: было остановлено следующим механизмом:

vagrant@vagrant:~$ dmesg | grep fork
[12022.844262] cgroup: fork rejected by pids controller in /user.slice/user-1000.slice/session-10.scope
vagrant@vagrant:~$ 

Можно покопаться в настройках systemctl status user.slice. Для user-1000.slice  есть ограничение на количество запущенных процессов. В моем случае это (limit: 2356)

vagrant@vagrant:~$ cd /etc/systemd/system
vagrant@vagrant:/etc/systemd/system$ systemctl status user.slice
● user.slice - User and Session Slice
     Loaded: loaded (/lib/systemd/system/user.slice; static; vendor preset: enabled)
     Active: active since Sun 2022-02-06 17:23:51 UTC; 1 day 1h ago
       Docs: man:systemd.special(7)
      Tasks: 7
     Memory: 15.2M
     CGroup: /user.slice
             └─user-1000.slice
               ├─session-10.scope
               │ ├─ 2156 sshd: vagrant [priv]
               │ ├─ 2204 sshd: vagrant@pts/0
               │ ├─ 2205 -bash
               │ ├─36391 systemctl status user.slice
               │ └─36392 pager
               └─user@1000.service
                 └─init.scope
                   ├─2169 /lib/systemd/systemd --user
                   └─2170 (sd-pam)

Warning: journal has been rotated since unit was started, output may be incomplete.
vagrant@vagrant:/etc/systemd/system$ systemctl status user-1000.slice
● user-1000.slice - User Slice of UID 1000
     Loaded: loaded
    Drop-In: /usr/lib/systemd/system/user-.slice.d
             └─10-defaults.conf
     Active: active since Mon 2022-02-07 18:03:36 UTC; 41min ago
       Docs: man:user@.service(5)
      Tasks: 7 (limit: 2356)
     Memory: 18.4M
     CGroup: /user.slice/user-1000.slice
             ├─session-10.scope
             │ ├─ 2156 sshd: vagrant [priv]
             │ ├─ 2204 sshd: vagrant@pts/0
             │ ├─ 2205 -bash
             │ ├─36419 systemctl status user-1000.slice
             │ └─36420 pager
             └─user@1000.service
               └─init.scope
                 ├─2169 /lib/systemd/systemd --user
                 └─2170 (sd-pam)
				 
Из упражнения 5 этого домашнего задания вспомним про граничения ulimit.
В системе есть "мягкие" и "жесткие" ограничения, которые можно выставить с помощью "ulimit".

Количество выделенных файловых дескрипторов (то есть количество файлов, открытых в настоящее время); количество свободных файловых дескрипторов; максимальное количество файловых дескрипторов  -  покажет команда:
vagrant@vagrant:/etc/systemd/system$ cat /proc/sys/fs/file-nr
1248	0	9223372036854775807
vagrant@vagrant:/etc/systemd/system$ 
 				 
ulimit --help подскажет как выставить ограничения. Например, можно задать мягкий лимит
    Options:
      -S	use the `soft' resource limit 
Следует понимать, что ulimit меняет только текущие лимиты, для шелла и всех программ, запущенных в этом шелле, поэтому после завершения сессии или даже в другом окне терминала значения останутся прежними.
