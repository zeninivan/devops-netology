Домашнее задание к занятию "3.3. Операционные системы, лекция 1"


1. Какой системный вызов делает команда cd? В прошлом ДЗ мы выяснили, что cd не является самостоятельной программой, это shell builtin, 
поэтому запустить strace непосредственно на cd не получится. Тем не менее, вы можете запустить strace на /bin/bash -c 'cd /tmp'. 
В этом случае вы увидите полный список системных вызовов, которые делает сам bash при старте. Вам нужно найти тот единственный, 
который относится именно к cd. Обратите внимание, что strace выдаёт результат своей работы в поток stderr, а не в stdout.

Команда cd делает системный вызов chdir("/tmp") 
ivan@PSB141C02:~$ strace /bin/bash -c 'cd /tmp'
...
chdir("/tmp")  
...


2. Попробуйте использовать команду file на объекты разных типов на файловой системе. Например:

vagrant@netology1:~$ file /dev/tty
/dev/tty: character special (5/0)
vagrant@netology1:~$ file /dev/sda
/dev/sda: block special (8/0)
vagrant@netology1:~$ file /bin/bash
/bin/bash: ELF 64-bit LSB shared object, x86-64

Используя strace выясните, где находится база данных file на основании которой она делает свои догадки.

ivan@PSB141C02:/tmp$ strace file /tmp/

В описании man file сказано, что информация, идентифицирующая эти файлы, считывается из /etc/magic и скомпилированного файла magic /usr/share/misc/magic.mgc 
или из файла в каталоге /usr/share/misc/magic, если скомпилированный файл не существует. 
Кроме того, если существует файл $HOME/.magic.mgc или $HOME/.magic, он будет использоваться предпочтительнее системных файлов magic.

Использую strace проверяем системные вызовы по команде file c аргументом /tmp/ и находим нужную информацию:

openat(AT_FDCWD, "/etc/magic", O_RDONLY) = 3
fstat(3, {st_mode=S_IFREG|0644, st_size=111, ...}) = 0
read(3, "# Magic local data for file(1) c"..., 4096) = 111
openat(AT_FDCWD, "/usr/share/misc/magic.mgc", O_RDONLY) = 3

Часть вывода системных вызовов:
ivan@PSB141C02:/tmp$ strace file /tmp/
..
..
stat("/home/ivan/.magic.mgc", 0x7ffd8d99bfb0) = -1 ENOENT (Нет такого файла или каталога)
stat("/home/ivan/.magic", 0x7ffd8d99bfb0) = -1 ENOENT (Нет такого файла или каталога)
openat(AT_FDCWD, "/etc/magic.mgc", O_RDONLY) = -1 ENOENT (Нет такого файла или каталога)
stat("/etc/magic", {st_mode=S_IFREG|0644, st_size=111, ...}) = 0
openat(AT_FDCWD, "/etc/magic", O_RDONLY) = 3
fstat(3, {st_mode=S_IFREG|0644, st_size=111, ...}) = 0
read(3, "# Magic local data for file(1) c"..., 4096) = 111
openat(AT_FDCWD, "/usr/share/misc/magic.mgc", O_RDONLY) = 3
..
..


3. Предположим, приложение пишет лог в текстовый файл. Этот файл оказался удален (deleted в lsof), однако возможности сигналом сказать приложению переоткрыть файлы или 
просто перезапустить приложение – нет. Так как приложение продолжает писать в удаленный файл, место на диске постепенно заканчивается. Основываясь на знаниях о перенаправлении 
потоков предложите способ обнуления открытого удаленного файла (чтобы освободить место на файловой системе).

Создаем на диске файл, например 100МБ и проверяем сколько места занято на диске и объем созданного файла
root@vagrant:/mnt/data# dd if=/dev/zero of=big/file.img bs=100M count=1

root@vagrant:/mnt/data# df -h
Filesystem                         Size  Used Avail Use% Mounted on
/dev/mapper/ubuntu--vg-ubuntu--lv   31G  3.9G   26G  14% /

root@vagrant:/mnt/data# du -shc ./*
101M	./big
4.0K	./small
101M	total

Теперь заблокируем файл, открыв его на чтение, чтобы задача продолжала выполняться
root@vagrant:/mnt/data# less +F big/file.img >/dev/null &
y
y
y
y
y
и т.д.

Теперь удаляем открытый файл и снова проверяем место на диске:
root@vagrant:/mnt/data# rm -f big/file.img

root@vagrant:/mnt/data# df -h
Filesystem                         Size  Used Avail Use% Mounted on
/dev/mapper/ubuntu--vg-ubuntu--lv   31G  3.9G   26G  14% /

Объем занятого пространства не изменился.
 
Теперь посмотрим удаленные процессы через lsof:
root@vagrant:/mnt/data# lsof | grep deleted
less      1418                           root    3r      REG              253,0 104857600    1572886 /mnt/data/big/file.img (deleted)

root@vagrant:/mnt/data# lsof -p $$
COMMAND  PID USER   FD   TYPE DEVICE SIZE/OFF    NODE NAME
bash    1353 root  cwd    DIR  253,0     4096 1572879 /mnt/data
bash    1353 root  rtd    DIR  253,0     4096       2 /
bash    1353 root  txt    REG  253,0  1183448 1835491 /usr/bin/bash
bash    1353 root  mem    REG  253,0    51832 1841607 /usr/lib/x86_64-linux-gnu/libnss_files-2.31.so
bash    1353 root  mem    REG  253,0  3035952 1835290 /usr/lib/locale/locale-archive
bash    1353 root  mem    REG  253,0  2029224 1841468 /usr/lib/x86_64-linux-gnu/libc-2.31.so
bash    1353 root  mem    REG  253,0    18816 1841486 /usr/lib/x86_64-linux-gnu/libdl-2.31.so
bash    1353 root  mem    REG  253,0   192032 1841679 /usr/lib/x86_64-linux-gnu/libtinfo.so.6.2
bash    1353 root  mem    REG  253,0    27002     682 /usr/lib/x86_64-linux-gnu/gconv/gconv-modules.cache
bash    1353 root  mem    REG  253,0   191472 1841428 /usr/lib/x86_64-linux-gnu/ld-2.31.so
bash    1353 root    0u   CHR  136,0      0t0       3 /dev/pts/0
bash    1353 root    1u   CHR  136,0      0t0       3 /dev/pts/0
bash    1353 root    2u   CHR  136,0      0t0       3 /dev/pts/0
bash    1353 root  255u   CHR  136,0      0t0       3 /dev/pts/0

Далее перенаправим запущенный процесс в удаленный файл, использую  fd. Указываем pid нашего процесса 1418 и номер открытого файл дескрипотора 3.

root@vagrant:/mnt/data# cat /proc/1418/fd/3 > big/file.img

и снова проверяем объем занятого пространства на диске и объем удаленного файла. Теперь все в порядке, файл не занимает места.
root@vagrant:/mnt/data# df -h
Filesystem                         Size  Used Avail Use% Mounted on
/dev/mapper/ubuntu--vg-ubuntu--lv   31G  3.8G   26G  13% /

root@vagrant:/mnt/data# du -shc ./*
8.0K	./big
4.0K	./small
12K	total

*****************************************************************************************************
									Задание 3 (доработка)
В данном примере нет удаления файла и нет постоянной записи в файл. dd выполнится всего один раз. 
Запустить тот же пинг с перенаправлением в текстовый файл и попробуйте на нём.
*****************************************************************************************************
В этот раз воспользуемся текстовым файлом "newfile". 

vagrant@vagrant:~$ ls -l
total 8
-rw-rw-r-- 1 vagrant vagrant 157 Jan 11 18:15 newfile
-rw-rw-r-- 1 vagrant vagrant 157 Jan 11 18:18 newfile_out

Сейчас он занимает 4kb
vagrant@vagrant:~$ du -shc ./*
4.0K	./newfile
4.0K	./newfile_out
8.0K	total
vagrant@vagrant:~$ 

Направляем вывод ping ya.ru в текстовый файл newfile:
vagrant@vagrant:~$ ping ya.ru >newfile

Файл постепенно наполняется
vagrant@vagrant:~$ du -shc ./*
16K	./newfile
4.0K	./newfile_out
20K	total

vagrant@vagrant:~$ du -shc ./*
28K	./newfile
4.0K	./newfile_out
32K	total

vagrant@vagrant:~$ du -shc ./*
80K	./newfile
4.0K	./newfile_out
84K	total
vagrant@vagrant:

vagrant@vagrant:~$ du -shc ./*
100K	./newfile
4.0K	./newfile_out
104K	total

Со второй сессии терминала удаляем открытый файл:
vagrant@vagrant:~$ rm -f newfile

Теперь посмотрим удаленные процессы через lsof:
vagrant@vagrant:~$ sudo bash
root@vagrant:/home/vagrant# lsof | grep deleted
ping      3196                        vagrant    1w      REG              253,0   105763    1048608 /home/vagrant/newfile (deleted)
root@vagrant:/home/vagrant# 

Далее перенаправим запущенный процесс в удаленный файл, использую fd. Указываем pid нашего процесса 3196 и номер открытого файл дескрипотора 2.
root@vagrant:/home/vagrant# cat >/proc/3196/fd/2

Объем файла стал равен 0
root@vagrant:/home/vagrant# du -shc ./*
0	./newfile
4.0K	./newfile_out
4.0K	total
root@vagrant:/home/vagrant# 


4. Занимают ли зомби-процессы какие-то ресурсы в ОС (CPU, RAM, IO)?

В случае, когда по какой-либо причине родительский процесс не смог обработать код возврата от дочернего процесса, такой дочерний процесс становится зомби.
Зомби процессы Linux не выполняются и убить их нельзя, даже с помощью sigkill, они продолжают висеть в памяти, пока не будет завершён их родительский процесс.
Зомби не занимают памяти (как процессы-сироты), но блокируют записи в таблице процессов, размер которой ограничен для каждого пользователя и системы в целом.


5. В iovisor BCC есть утилита opensnoop:

root@vagrant:~# dpkg -L bpfcc-tools | grep sbin/opensnoop
/usr/sbin/opensnoop-bpfcc

На какие файлы вы увидели вызовы группы open за первую секунду работы утилиты? Воспользуйтесь пакетом bpfcc-tools для Ubuntu 20.04.

Устанавливаем пакет opensnoop:
root@vagrant:/# sudo apt-get install bpfcc-tools linux-headers-$(ivan -r)

В первую секунду видим вызовы на следюущие файлы:

root@vagrant:/# /usr/sbin/opensnoop-bpfcc
PID    COMM               FD ERR PATH
1035   vminfo              6   0 /var/run/utmp
635    dbus-daemon        -1   2 /usr/local/share/dbus-1/system-services
635    dbus-daemon        18   0 /usr/share/dbus-1/system-services
635    dbus-daemon        -1   2 /lib/dbus-1/system-services
635    dbus-daemon        18   0 /var/lib/snapd/dbus-1/system-services/
1035   vminfo              6   0 /var/run/utmp
635    dbus-daemon        -1   2 /usr/local/share/dbus-1/system-services
635    dbus-daemon        18   0 /usr/share/dbus-1/system-services
635    dbus-daemon        -1   2 /lib/dbus-1/system-services
635    dbus-daemon        18   0 /var/lib/snapd/dbus-1/system-services/
639    irqbalance          6   0 /proc/interrupts
639    irqbalance          6   0 /proc/stat
639    irqbalance          6   0 /proc/irq/20/smp_affinity
639    irqbalance          6   0 /proc/irq/0/smp_affinity
639    irqbalance          6   0 /proc/irq/1/smp_affinity
639    irqbalance          6   0 /proc/irq/8/smp_affinity
639    irqbalance          6   0 /proc/irq/12/smp_affinity
639    irqbalance          6   0 /proc/irq/14/smp_affinity
639    irqbalance          6   0 /proc/irq/15/smp_affinity
^Croot@vagrant:/# 


6. Какой системный вызов использует uname -a? Приведите цитату из man по этому системному вызову, где описывается альтернативное местоположение в /proc, где можно узнать версию ядра и релиз ОС.

uname использует системный вызов uname(2) для получения информации, относящейся к ядру.
uname -a использует системный вызов "uname (2)".

ivan@PSB141C02:~/Vagrant-congis$ uname -a
Linux PSB141C02 5.13.0-27-generic #29~20.04.1-Ubuntu SMP Fri Jan 14 00:32:30 UTC 2022 x86_64 x86_64 x86_64 GNU/Linux

ivan@PSB141C02:~/Vagrant-congis$ man 2 uname

UNAME(2)                                                                                  Linux Programmer's Manual                                                                                  UNAME(2)

NAME
       uname - get name and information about current kernel
	   
В man  альтернативное местоположение в /proc описывается в разделе NOTES:
Цитата:
"Part of the utsname information is also accessible via /proc/sys/kernel/{ostype, hostname, osrelease, version, domainname}."


7. Чем отличается последовательность команд через ; и через && в bash? 
Например:
root@netology1:~# test -d /tmp/some_dir; echo Hi
Hi
root@netology1:~# test -d /tmp/some_dir && echo Hi
root@netology1:~#
Есть ли смысл использовать в bash &&, если применить set -e?

Оператор точка с запятой (;) позволяет запускать несколько команд за один раз, и выполнение команды происходит последовательно.

Оператор AND (&&) будет выполнять вторую команду только в том случае, если результат первой команды равен “0” — программа выполнена успешно. 
Эта команда очень полезна при проверке состояния выполнения последней команды.

В нашем случае в первой команде после выполнения "test -d /tmp/some_dir" на экран будет выведена строчка "Hi".

При выполнении второй команды "test -d /tmp/some_dir && echo Hi" мы видим через  strace, что тест не проходит, поэтому вторая часть команды после && не выполняется.

ivan@PSB141C02:~/Vagrant-congis$ strace test -d /tmp/some_dir
execve("/usr/bin/test", ["test", "-d", "/tmp/some_dir"], 0x7ffdf94c5d50 /* 48 vars */) = 0
brk(NULL)                               = 0x55c7d4e66000
arch_prctl(0x3001 /* ARCH_??? */, 0x7fff7169c940) = -1 EINVAL (Недопустимый аргумент)
access("/etc/ld.so.preload", R_OK)      = -1 ENOENT (Нет такого файла или каталога)
..
..
и т.д.

В man set сказано, что выполненте команды set в сочетании с "-е" немедленно завершит работу, если команда завершается с ненулевым статусом.
Поэтому использовать set -e вместе с && нет смысла, т.к. при использовании && выполнение второй команды итак не будет продолжено при неуспешном результате первой команды. 


8. Из каких опций состоит режим bash set -euxo pipefail и почему его хорошо было бы использовать в сценариях?

ivan@PSB141C02:~/Vagrant-congis$ help set
set: set [-abefhkmnptuvxBCHP] [-o параметр] [--] [аргумент ...]
    Set or unset values of shell options and positional parameters.
    
    Change the value of shell attributes and positional parameters, or
    display the names and values of shell variables.
    
    Options:
      -e  Немедленно завершите работу, если команда завершается с ненулевым статусом.
      -u  Рассматривает неустановленные/незаданные переменные как ошибку при замене.
      -x  Выводит команды и их аргументы по мере их выполнения.
      -o option-name
          Установит переменную, соответствующую названию опции:
              pipefail - возвращаемое значение pipeline - это статус последней команды для выхода с ненулевым статусом или ноль при успешном выполнении.
			  
В сценарии его будет удобно использовать, если нам нужно завершить работу сценария при появлении ошибки. Опция -e остановит работу. 
Также будет удобно смотреть вывод команд по мере их выполнения - опция -х.
Опция -u повысит точность выполнения сценария, т.к. незаданные параметры будут восприняты как ошибки.


9. Используя -o stat для ps, определите, какой наиболее часто встречающийся статус у процессов в системе. В man ps ознакомьтесь (/PROCESS STATE CODES) что значат 
дополнительные к основной заглавной буквы статуса процессов. Его можно не учитывать при расчете (считать S, Ss или Ssl равнозначными).

ivan@PSB141C02:~/Vagrant-congis$ ps -o stat
STAT
Ss
R+

Наиболее часто в моей системе встречаются процессы со статусом Ss и R+. 
S - interruptible sleep, т.е. это процессы, ожидающие завершения события. Маленькая s после большой - это дополнительный параметр, который говорит, что процесс является лидером сессии.
R - running or runnable (on run queue) - запущенный или запускаемый процесс (в очереди выполнения). Дополнительный параметр + говорит о том, что процесс находится в группе процессов переднего плана. 


