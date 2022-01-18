Домашнее задание к занятию "3.2. Работа в терминале, лекция 2"

1. Какого типа команда cd? Попробуйте объяснить, почему она именно такого типа; опишите ход своих мыслей, если считаете что она могла бы быть другого типа.

cd - это встроенная команда.
Т.е. оболочка выполняет ее сама, вместо того, чтобы интерпретировать ее как запрос на загрузку или запуск другой программы.
Встроенная команда может повлиять на внутреннее состояние оболочки, а внешняя программа не сможет изменить текущий каталог оболочки.
Смена текущей директории в рамках дочернего процесса или дочерней командной оболочки не повлияет на текущую директорию родительской командной оболочки.
Поэтому нет смысла делать команду cd  внешней и плодить дочерние процессы или оболочки.


2. Какая альтернатива без pipe команде grep <some_string> <some_file> | wc -l? man grep поможет в ответе на этот вопрос. Ознакомьтесь с документом о других подобных некорректных вариантах использования pipe.

Вместо конструкции "grep | wc -l" можно использовать "grep -c"

ivan@PSB141C02:~/devops-netology$ grep 19: README3.md
drwxr-xr-x 3 root    root    4.0K Dec 19 19:42 ..
drwx------ 2 vagrant vagrant 4.0K Dec 19 19:42 .cache
-rw-r--r-- 1 vagrant vagrant    0 Dec 19 19:42 .sudo_as_admin_successful
-rw-r--r-- 1 vagrant vagrant    6 Dec 19 19:42 .vbox_version
-rw-r--r-- 1 root    root     180 Dec 19 19:44 .wget-hsts
ivan@PSB141C02:~/devops-netology$ grep 19: README3.md -c
5
ivan@PSB141C02:~/devops-netology$ grep 19: README3.md |wc -l
5
ivan@PSB141C02:~/devops-netology$ 


3. Какой процесс с PID 1 является родителем для всех процессов в вашей виртуальной машине Ubuntu 20.04?

Родителем  является процесс "systemd". Посмотреть можно с помощью команды pstree c ключом -p

vagrant@vagrant:~$ pstree -p
systemd(1)─┬─VBoxService(842)─┬─{VBoxService}(843)
           │                  ├─{VBoxService}(844)
           │                  ├─{VBoxService}(845)
           │                  ├─{VBoxService}(846)
           │                  ├─{VBoxService}(847)
           │                  ├─{VBoxService}(849)
           │                  ├─{VBoxService}(851)
           │                  └─{VBoxService}(852)
           ├─accounts-daemon(631)─┬─{accounts-daemon}(639)
           │                      └─{accounts-daemon}(686)
           ├─agetty(673)
           ├─atd(658)
           ├─cron(656)


4. Как будет выглядеть команда, которая перенаправит вывод stderr ls на другую сессию терминала?

vagrant@vagrant:~$ ls -l /dev/stderr
lrwxrwxrwx 1 root root 15 Jan 10 19:07 /dev/stderr -> /proc/self/fd/2
vagrant@vagrant:~$ bash 2>/dev/null
lsof -p $$
COMMAND  PID    USER   FD   TYPE DEVICE SIZE/OFF    NODE NAME
bash    1425 vagrant  cwd    DIR  253,0     4096 1051845 /home/vagrant
bash    1425 vagrant  rtd    DIR  253,0     4096       2 /
bash    1425 vagrant  txt    REG  253,0  1183448 1835491 /usr/bin/bash
bash    1425 vagrant  mem    REG  253,0  3035952 1835290 /usr/lib/locale/locale-archive
bash    1425 vagrant  mem    REG  253,0  2029224 1841468 /usr/lib/x86_64-linux-gnu/libc-2.31.so
bash    1425 vagrant  mem    REG  253,0    18816 1841486 /usr/lib/x86_64-linux-gnu/libdl-2.31.so
bash    1425 vagrant  mem    REG  253,0   192032 1841679 /usr/lib/x86_64-linux-gnu/libtinfo.so.6.2
bash    1425 vagrant  mem    REG  253,0    27002     682 /usr/lib/x86_64-linux-gnu/gconv/gconv-modules.cache
bash    1425 vagrant  mem    REG  253,0   191472 1841428 /usr/lib/x86_64-linux-gnu/ld-2.31.so
bash    1425 vagrant    0u   CHR  136,0      0t0       3 /dev/pts/0
bash    1425 vagrant    1u   CHR  136,0      0t0       3 /dev/pts/0
bash    1425 vagrant    2w   CHR    1,3      0t0       6 /dev/null

******************************
****Доработка к заданию 4:****
******************************

Добрый день. Попробую по-другому решить это задание.

Открываем две ssh сессии терминала к виртуальной машине:

В первой сессии имеем:  
vagrant@vagrant:~$ tty
/dev/pts/0

Во второй сессии имеем:
vagrant@vagrant:~$ tty
/dev/pts/2

Используя /dev/stderr, находим ссылку на свой собственный поток ошибок, /dev/stderr будет “свой” у каждого процесса

vagrant@vagrant:~$ ls -l /dev/stderr
lrwxrwxrwx 1 root root 15 Jan 14 18:01 /dev/stderr -> /proc/self/fd/2
vagrant@vagrant:~$ ls -l /proc/self/fd/2
lrwx------ 1 vagrant vagrant 64 Jan 18 15:59 /proc/self/fd/2 -> /dev/pts/0

Далее, с помощью ">" перенаправляем поток на вторую сессию терминала /dev/pts/2:
vagrant@vagrant:~$ ls -l /proc/self/fd/2>/dev/pts/2
vagrant@vagrant:~$ 

Во второй сессии терминала появится вывод на экран:
vagrant@vagrant:~$ lrwx------ 1 vagrant vagrant 64 Jan 18 16:01 /proc/self/fd/2 -> /dev/pts/0


5. Получится ли одновременно передать команде файл на stdin и вывести ее stdout в другой файл? Приведите работающий пример.

Да, получится:

vagrant@vagrant:~$ ls -lh
total 8.0K
-rw-rw-r-- 1 vagrant vagrant 157 Jan 11 18:15 newfile
-rw-rw-r-- 1 vagrant vagrant 491 Jan 11 18:04 newfile_out
vagrant@vagrant:~$ cat newfile

SYNOPSIS
       grep [OPTION...] PATTERNS [FILE...]
       grep [OPTION...] -e PATTERNS ... [FILE...]
       grep [OPTION...] -f PATTERN_FILE ... [FILE...]
vagrant@vagrant:~$ cat <newfile >newfile_out
vagrant@vagrant:~$ cat newfile_out

SYNOPSIS
       grep [OPTION...] PATTERNS [FILE...]
       grep [OPTION...] -e PATTERNS ... [FILE...]
       grep [OPTION...] -f PATTERN_FILE ... [FILE...]
vagrant@vagrant:~$ 


6. Получится ли находясь в графическом режиме, вывести данные из PTY в какой-либо из эмуляторов TTY? Сможете ли вы наблюдать выводимые данные? 

Да, получится. У меня настроен запуск вирутальной машины с поддержкой GUI (в конфигурации файла Vagrantfile прописано vb.gui = true)
  config.vm.provider "virtualbox" do |vb|
  #   # Display the VirtualBox GUI when booting the machine
   vb.gui = true
В открывшемся окне виртуальной машины видим:
vagrant@vagrant:~$ tty
/dev/tty1

Далее, в псевдо-терминале PTY запускаем удаленное подключение ssh к виртуальной машине:
ivan@PSB141C02:~/Vagrant-congis$ vagrant ssh
vagrant@vagrant:~$ tty
/dev/pts/2

и перенаправляем вывод приветственного сообщения "Hello from pts2 to tty1" на эмулятор TTY:
vagrant@vagrant:~$ tty
/dev/pts/2
vagrant@vagrant:~$ echo Hello from pts2 to tty1 >/dev/tty1

на экране эмулятора TTY видим приветствие:
vagrant@vagrant:~$ Hello from pts2 to tty1


7. Выполните команду bash 5>&1. К чему она приведет? Что будет, если вы выполните echo netology > /proc/$$/fd/5? Почему так происходит?

Команда bash 5>&1 приведет к открытию файла и выделению ему файлового дескриптора с номером 5 и перенаправит его поток в stdout.

vagrant@vagrant:~$ bash 5>&1
vagrant@vagrant:~$ tty
/dev/pts/2
vagrant@vagrant:~$ ls -l /dev/stdout
lrwxrwxrwx 1 root root 15 Jan 11 15:22 /dev/stdout -> /proc/self/fd/1
vagrant@vagrant:~$ lsof -p $$
COMMAND  PID    USER   FD   TYPE DEVICE SIZE/OFF    NODE NAME
bash    2100 vagrant  cwd    DIR  253,0     4096 1051845 /home/vagrant
bash    2100 vagrant  rtd    DIR  253,0     4096       2 /
bash    2100 vagrant  txt    REG  253,0  1183448 1835491 /usr/bin/bash
bash    2100 vagrant  mem    REG  253,0    51832 1841607 /usr/lib/x86_64-linux-gnu/libnss_files-2.31.so
bash    2100 vagrant  mem    REG  253,0  3035952 1835290 /usr/lib/locale/locale-archive
bash    2100 vagrant  mem    REG  253,0  2029224 1841468 /usr/lib/x86_64-linux-gnu/libc-2.31.so
bash    2100 vagrant  mem    REG  253,0    18816 1841486 /usr/lib/x86_64-linux-gnu/libdl-2.31.so
bash    2100 vagrant  mem    REG  253,0   192032 1841679 /usr/lib/x86_64-linux-gnu/libtinfo.so.6.2
bash    2100 vagrant  mem    REG  253,0    27002     682 /usr/lib/x86_64-linux-gnu/gconv/gconv-modules.cache
bash    2100 vagrant  mem    REG  253,0   191472 1841428 /usr/lib/x86_64-linux-gnu/ld-2.31.so
bash    2100 vagrant    0u   CHR  136,2      0t0       5 /dev/pts/2
bash    2100 vagrant    1u   CHR  136,2      0t0       5 /dev/pts/2
bash    2100 vagrant    2u   CHR  136,2      0t0       5 /dev/pts/2
bash    2100 vagrant    5u   CHR  136,2      0t0       5 /dev/pts/2
bash    2100 vagrant  255u   CHR  136,2      0t0       5 /dev/pts/2
vagrant@vagrant:~$ 

Если выполнить echo netology > /proc/$$/fd/5, то произойдет вывод строчки "netology" в текущей сессии терминала, т.к. поток с файловым дескриптором 5 перенаправляется на stdout.
Если выполнить ту же команду с дескриптором 4, то вывод строчки "netology" не произойдет, т.к. в нашей сессии нет файла с дескриптором 4.
То же самое можно сделать с дескриптором 2 и мы увидим вывод строчки "netology".

vagrant@vagrant:~$ echo netology > /proc/$$/fd/5
netology
vagrant@vagrant:~$ echo netology > /proc/$$/fd/4
bash: /proc/2100/fd/4: No such file or directory
vagrant@vagrant:~$ echo netology > /proc/$$/fd/2
netology
vagrant@vagrant:~$ 


8. Получится ли в качестве входного потока для pipe использовать только stderr команды, не потеряв при этом отображение stdout на pty? 
Напоминаем: по умолчанию через pipe передается только stdout команды слева от | на stdin команды справа. Это можно сделать, поменяв стандартные потоки местами 
через промежуточный новый дескриптор, который вы научились создавать в предыдущем вопросе.

Вывод данных команды cat /proc/uptime будет являться входным для нового дескриптора 5. 
Данные нового дескриптора 5 перенаправляем в поток stderr.
Поток stderr перенаправляем в stdout.
Поток stdout перенаправляем в новый дескриптор 5.
Получаем команду (справа от | укажем wc -m, т.е. вывести количество символов):
cat /proc/uptime 5>&2 2>&1 1>&5 | wc -m

vagrant@vagrant:~$ cat /proc/uptime 5>&2 2>&1 1>&5 | wc -m
889.91 1601.36
0


9. Что выведет команда cat /proc/$$/environ? Как еще можно получить аналогичный по содержанию вывод?

Команда cat /proc/$$/environ выведет переменные окружения в Linux - это специальные переменные, определенные оболочкой и используемые программами во время выполнения.

Аналогичный вывод можно получить с помощью комады "env" (с разделением переменных по строкам):

vagrant@vagrant:~$ env
SHELL=/bin/bash
PWD=/home/vagrant
LOGNAME=vagrant
XDG_SESSION_TYPE=tty
MOTD_SHOWN=pam
HOME=/home/vagrant
LANG=en_US.UTF-8
LS_COLORS=rs=0:di=01;34:ln=01;36:mh=00:pi=40;33:so=01;35:do=01;35:bd=40;33;01:cd=40;33;01:or=40;31;01:mi=00:su=37;41:sg=30;43:ca=30;41:tw=30;42:ow=34;42:st=37;44:ex=01;32:*.tar=01;31:*.tgz=01;31:*.arc=01;31:*.arj=01;31:*.taz=01;31:*.lha=01;31:*.lz4=01;31:*.lzh=01;31:*.lzma=01;31:*.tlz=01;31:*.txz=01;31:*.tzo=01;31:*.t7z=01;31:*.zip=01;31:*.z=01;31:*.dz=01;31:*.gz=01;31:*.lrz=01;31:*.lz=01;31:*.lzo=01;31:*.xz=01;31:*.zst=01;31:*.tzst=01;31:*.bz2=01;31:*.bz=01;31:*.tbz=01;31:*.tbz2=01;31:*.tz=01;31:*.deb=01;31:*.rpm=01;31:*.jar=01;31:*.war=01;31:*.ear=01;31:*.sar=01;31:*.rar=01;31:*.alz=01;31:*.ace=01;31:*.zoo=01;31:*.cpio=01;31:*.7z=01;31:*.rz=01;31:*.cab=01;31:*.wim=01;31:*.swm=01;31:*.dwm=01;31:*.esd=01;31:*.jpg=01;35:*.jpeg=01;35:*.mjpg=01;35:*.mjpeg=01;35:*.gif=01;35:*.bmp=01;35:*.pbm=01;35:*.pgm=01;35:*.ppm=01;35:*.tga=01;35:*.xbm=01;35:*.xpm=01;35:*.tif=01;35:*.tiff=01;35:*.png=01;35:*.svg=01;35:*.svgz=01;35:*.mng=01;35:*.pcx=01;35:*.mov=01;35:*.mpg=01;35:*.mpeg=01;35:*.m2v=01;35:*.mkv=01;35:*.webm=01;35:*.ogm=01;35:*.mp4=01;35:*.m4v=01;35:*.mp4v=01;35:*.vob=01;35:*.qt=01;35:*.nuv=01;35:*.wmv=01;35:*.asf=01;35:*.rm=01;35:*.rmvb=01;35:*.flc=01;35:*.avi=01;35:*.fli=01;35:*.flv=01;35:*.gl=01;35:*.dl=01;35:*.xcf=01;35:*.xwd=01;35:*.yuv=01;35:*.cgm=01;35:*.emf=01;35:*.ogv=01;35:*.ogx=01;35:*.aac=00;36:*.au=00;36:*.flac=00;36:*.m4a=00;36:*.mid=00;36:*.midi=00;36:*.mka=00;36:*.mp3=00;36:*.mpc=00;36:*.ogg=00;36:*.ra=00;36:*.wav=00;36:*.oga=00;36:*.opus=00;36:*.spx=00;36:*.xspf=00;36:
SSH_CONNECTION=10.0.2.2 55012 10.0.2.15 22
LESSCLOSE=/usr/bin/lesspipe %s %s
XDG_SESSION_CLASS=user
TERM=xterm-256color
LESSOPEN=| /usr/bin/lesspipe %s
USER=vagrant
SHLVL=2
XDG_SESSION_ID=9
XDG_RUNTIME_DIR=/run/user/1000
SSH_CLIENT=10.0.2.2 55012 22
XDG_DATA_DIRS=/usr/local/share:/usr/share:/var/lib/snapd/desktop
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin
DBUS_SESSION_BUS_ADDRESS=unix:path=/run/user/1000/bus
SSH_TTY=/dev/pts/1
_=/usr/bin/env


10. Используя man, опишите что доступно по адресам /proc/<PID>/cmdline, /proc/<PID>/exe.

/proc/[pid]/cmdline
Этот файл, доступный только для чтения, содержит полную командную строку (путь) для указанного процесса PID
В  man proc это описано на строчках 178-180.

/proc/[pid]/exe
В Linux 2.2 и более поздних версиях этот файл представляет собой ссылку для указанного [pid], содержащую фактический путь к выполняемой команде. Попытка открыть ее приведет
к открытию исполняемого файла. Вы даже можете ввести /proc/[pid]/exe, чтобы запустить другую копию того же исполняемого файла, который запускается процессом [pid]. 
В  man proc это описано на строчках 219-235.


11. Узнайте, какую наиболее старшую версию набора инструкций SSE поддерживает ваш процессор с помощью /proc/cpuinfo.

Выведем содержимое файла /proc/cpuinfo с помощью less или cat и отфильтруем по grep sse. Получим:
ivan@PSB141C02:~$ cat /proc/cpuinfo | grep sse
flags		: fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts acpi mmx fxsr sse sse2 ss ht tm pbe syscall nx pdpe1gb rdtscp lm constant_tsc art arch_perfmon pebs bts rep_good nopl xtopology tsc_reliable nonstop_tsc cpuid aperfmperf tsc_known_freq pni pclmulqdq dtes64 monitor ds_cpl vmx est tm2 ssse3 sdbg cx16 xtpr pdcm sse4_1 sse4_2 x2apic movbe popcnt tsc_deadline_timer aes xsave rdrand lahf_lm 3dnowprefetch cpuid_fault cat_l2 ibrs ibpb stibp tpr_shadow vnmi flexpriority ept vpid ept_ad fsgsbase tsc_adjust smep erms mpx rdt_a rdseed smap clflushopt intel_pt sha_ni xsaveopt xsavec xgetbv1 xsaves dtherm ida arat pln pts md_clear arch_capabilities
flags		: fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts acpi mmx fxsr sse sse2 ss ht tm pbe syscall nx pdpe1gb rdtscp lm constant_tsc art arch_perfmon pebs bts rep_good nopl xtopology tsc_reliable nonstop_tsc cpuid aperfmperf tsc_known_freq pni pclmulqdq dtes64 monitor ds_cpl vmx est tm2 ssse3 sdbg cx16 xtpr pdcm sse4_1 sse4_2 x2apic movbe popcnt tsc_deadline_timer aes xsave rdrand lahf_lm 3dnowprefetch cpuid_fault cat_l2 ibrs ibpb stibp tpr_shadow vnmi flexpriority ept vpid ept_ad fsgsbase tsc_adjust smep erms mpx rdt_a rdseed smap clflushopt intel_pt sha_ni xsaveopt xsavec xgetbv1 xsaves dtherm ida arat pln pts md_clear arch_capabilities
ivan@PSB141C02:~$ 

Самая старшая версия набора инструкций SSE -  sse4_2.


12. При открытии нового окна терминала и vagrant ssh создается новая сессия и выделяется pty. Это можно подтвердить командой tty, которая упоминалась в лекции 3.2. Однако:

vagrant@netology1:~$ ssh localhost 'tty'
not a tty
Почитайте, почему так происходит, и как изменить поведение.

По умолчанию, когда вы запускаете команду на удаленном компьютере с помощью ssh, TTY не выделяется для удаленного сеанса. Это позволяет передавать данные без необходимости работать с причудами TTY.
Это среда, предусмотренная для команды, выполняемой на локальном компьютере (computerone).
Можно попробовать следующий вариант подключения:

ssh localhost -t "ssh vagrant@vagrant"

vagrant@vagrant:~$ ssh localhost -t "ssh vagrant@vagrant"
vagrant@localhost's password: 
The authenticity of host 'vagrant (127.0.1.1)' can't be established.
ECDSA key fingerprint is SHA256:RztZ38lZsUpiN3mQrXHa6qtsUgsttBXWJibL2nAiwdQ.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'vagrant' (ECDSA) to the list of known hosts.
vagrant@vagrant's password: 
Welcome to Ubuntu 20.04.3 LTS (GNU/Linux 5.4.0-91-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Fri 14 Jan 2022 04:53:29 PM UTC

  System load:  0.0                Processes:             123
  Usage of /:   12.0% of 30.88GB   Users logged in:       1
  Memory usage: 21%                IPv4 address for eth0: 10.0.2.15
  Swap usage:   0%


This system is built by the Bento project by Chef Software
More information can be found at https://github.com/chef/bento
Last login: Fri Jan 14 16:53:08 2022 from 127.0.0.1
vagrant@vagrant:~$ tty
/dev/pts/2
vagrant@vagrant:~$ 


13. Бывает, что есть необходимость переместить запущенный процесс из одной сессии в другую. Попробуйте сделать это, воспользовавшись reptyr. Например, так можно перенести в screen процесс, который вы запустили по ошибке в обычной SSH-сессии.

Получилось с 20го раза :). 
Открыл две сессии ssh к удаленной машине. На одной запустил ping, со второй перехватывал процесс командой: reptyr [pid]

Ssh Сессия 1:
root@vagrant:/home/vagrant# ping mail.ru
PING mail.ru (217.69.139.202) 56(84) bytes of data.
64 bytes from mail.ru (217.69.139.202): icmp_seq=1 ttl=63 time=74.1 ms
64 bytes from mail.ru (217.69.139.202): icmp_seq=2 ttl=63 time=62.5 ms
..
..
..
[1]+  Stopped                 ping mail.ru
root@vagrant:/home/vagrant# 

Ssh Сессия 2:
root@vagrant:/home/vagrant# reptyr 1915
64 bytes from mail.ru (217.69.139.202): icmp_seq=150 ttl=63 time=66.4 ms
64 bytes from mail.ru (217.69.139.202): icmp_seq=151 ttl=63 time=124 ms
..
..
..
64 bytes from mail.ru (217.69.139.202): icmp_seq=447 ttl=63 time=80.4 ms
64 bytes from mail.ru (217.69.139.202): icmp_seq=448 ttl=63 time=68.6 ms
^C
--- mail.ru ping statistics ---
448 packets transmitted, 436 received, 2.67857% packet loss, time 451393ms
rtt min/avg/max/mdev = 57.150/80.640/1143.816/59.349 ms, pipe 2
root@vagrant:/home/vagrant# 

В файле  
root@vagrant:/home/vagrant# /etc/sysctl.d/10-ptrace.conf
пришлось заменить 1 на 0 параметр
kernel.yama.ptrace_scope = 0

Reptyr сработал только под root пользователем.


14. sudo echo string > /root/new_file не даст выполнить перенаправление под обычным пользователем, так как перенаправлением занимается процесс shell'а, который запущен без sudo под вашим пользователем. 
Для решения данной проблемы можно использовать конструкцию echo string | sudo tee /root/new_file. Узнайте что делает команда tee и почему в отличие от sudo echo команда с sudo tee будет работать.

stdout команды слева от | на stdin команды справа

Команда tee принимает данные из одного источника (в данном случае из echo string) и через | перенаправляет их на stdin команды справа и сохраняет(записывает) их в файл, указанный в примере (/root/new_file).
Команда имеет права на запись, т.к. запущена из под root пользователя.

