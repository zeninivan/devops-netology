Домашнее задание к занятию "3.1. Работа в терминале, лекция 1"


1. Установите средство виртуализации Oracle VirtualBox. 

sudo apt install virtualbox

2. Установите средство автоматизации Hashicorp Vagrant.

sudo apt install vagrant

3. В вашем основном окружении подготовьте удобный для дальнейшей работы терминал. 

Для работы я использовал стандартный терминал в Ubuntu 20.04.3 LTS.
Установка дополнительного пакета linux-headers-generic (debian-based) не потребовалась. 

4. С помощью базового файла конфигурации запустите Ubuntu 20.04 в VirtualBox посредством Vagrant:

vagrant init - выполнен.
Замена содержимого Vagrantfile - выполнено.
ivan@PSB141C02:~/Vagrant-congis$ vi Vagrantfile

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://vagrantcloud.com/search.
  config.vm.box = "bento/ubuntu-20.04"

vagrant up - выполнен.
vagrant suspend - выполнен.


5. Ознакомьтесь с графическим интерфейсом VirtualBox, посмотрите как выглядит виртуальная машина, которую создал для вас Vagrant, какие аппаратные ресурсы ей выделены. Какие ресурсы выделены по-умолчанию?

По-умолчанию было выделено:
Оперативная память: 1024мб
Процессоры: 2
Жесткий диск SATA порт 0: 64Гб
Видеопамять: 4Мб (в последствии добавил до 32Мб и сменил тип графич контроллера на  VMSVGA)


6. Ознакомьтесь с возможностями конфигурации VirtualBox через Vagrantfile: документация. Как добавить оперативной памяти или ресурсов процессора виртуальной машине?

В конфигурационный файл Vagrantfile добавляем команды:

config.vm.provider "virtualbox" do |vb|  
vb.memory = 1024  
vb.cpus = 2
end


7. Команда vagrant ssh из директории, в которой содержится Vagrantfile, позволит вам оказаться внутри виртуальной машины без каких-либо дополнительных настроек. Попрактикуйтесь в выполнении обсуждаемых команд в терминале Ubuntu. 

ivan@PSB141C02:~/Vagrant-congis$ vagrant ssh
Welcome to Ubuntu 20.04.3 LTS (GNU/Linux 5.4.0-91-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Thu 06 Jan 2022 12:05:34 PM UTC

  System load:  0.91               Processes:             125
  Usage of /:   11.9% of 30.88GB   Users logged in:       0
  Memory usage: 20%                IPv4 address for eth0: 10.0.2.15
  Swap usage:   0%


This system is built by the Bento project by Chef Software
More information can be found at https://github.com/chef/bento
Last login: Wed Jan  5 12:25:29 2022 from 10.0.2.2
vagrant@vagrant:~$ type -a bash


8. Ознакомиться с разделами man bash, почитать о настройках самого bash:

-какой переменной можно задать длину журнала history, и на какой строчке manual это описывается?
 
HISTFILESIZE - максимальное количество строк, содержащихся в файле истории. Когда этой переменной присваивается значение, файл истории при необходимости усекается, чтобы содержать не более указанного количества строк
, удаляя самые старые записи. Файл истории также обрезается до этого размера после его записи при выходе из командной оболочки. Если значение равно 0, файл истории усекается до нулевого размера. 
Это описано в man bash на строках 619-622.

HISTSIZE - количество команд, которые нужно запомнить в истории команд. Если значение равно 0, команды не сохраняются в списке истории. Числовые значения меньше нуля приводят к тому, что каждая
команда сохраняется в списке истории (ограничений нет). Оболочка устанавливает значение по умолчанию равным 500 после чтения любых файлов запуска.
Это описано в man bash на строках 628-630.

 -что делает директива ignoreboth в bash?

Значение ignoreboth - это сокращение от ignorespace и ignoredups. 
ignorespace - строки, начинающиеся с пробела, не сохраняются в списке истории.
ignoredups - строки, соответствующие предыдущей записи истории, не сохраняются.
Это описано в man bash в разделе HISTCONTROL на строках 611-616.


9. В каких сценариях использования применимы скобки {} и на какой строчке man bash это описано?

скобки {} - нужны для зарезервированных слов.
Зарезервированными являются слова, имеющие специальное значение для командного интерпретатора. 
Функция зарезервированного слова опциональна. 
В  man bash описание данной функции дано на строках 307-312.


10. С учётом ответа на предыдущий вопрос, как создать однократным вызовом touch 100000 файлов? 

touch testfile{1..100000}

Получится ли аналогичным образом создать 300000? Если нет, то почему?
Нет, не получится, слишком длинный список аргументов.

ivan@PSB141C02:~/Testfile$ touch testfile{1..300000}
bash: /usr/bin/touch: Слишком длинный список аргументов


11. В man bash поищите по /\[\[. Что делает конструкция [[ -d /tmp ]]

[[ expression ]]
Возвращает статус 0 или 1 в зависимости от оценки условного выражения. Выражения состоят из основных элементов, описанных ниже в разделе УСЛОВНЫЕ ВЫРАЖЕНИЯ. 
Т.е. в данном примере проверяется наличие катаолга /tmp и выводится статус 0 или 1.


12. Основываясь на знаниях о просмотре текущих (например, PATH) и установке новых переменных; командах, которые мы рассматривали, добейтесь в выводе type -a bash в виртуальной машине наличия первым пунктом в списке:

bash is /tmp/new_path_directory/bash
bash is /usr/local/bin/bash
bash is /bin/bash

(прочие строки могут отличаться содержимым и порядком) В качестве ответа приведите команды, которые позволили вам добиться указанного вывода или соответствующие скриншоты.

This system is built by the Bento project by Chef Software
More information can be found at https://github.com/chef/bento
Last login: Wed Jan  5 12:25:29 2022 from 10.0.2.2
vagrant@vagrant:~$ type -a bash
bash is /usr/bin/bash
bash is /bin/bash
vagrant@vagrant:~$ ls -lh
total 0
vagrant@vagrant:~$ mkdir /tmp/new_path_directory/
vagrant@vagrant:~$ ls -lh
total 0
vagrant@vagrant:~$ cd /tmp/new_path_directory/
vagrant@vagrant:/tmp/new_path_directory$ ls -lh
total 0
vagrant@vagrant:/tmp/new_path_directory$ cd
vagrant@vagrant:~$ ls -lha
total 40K
drwxr-xr-x 4 vagrant vagrant 4.0K Jan  5 12:27 .
drwxr-xr-x 3 root    root    4.0K Dec 19 19:42 ..
-rw------- 1 vagrant vagrant    5 Jan  5 12:27 .bash_history
-rw-r--r-- 1 vagrant vagrant  220 Feb 25  2020 .bash_logout
-rw-r--r-- 1 vagrant vagrant 3.7K Feb 25  2020 .bashrc
drwx------ 2 vagrant vagrant 4.0K Dec 19 19:42 .cache
-rw-r--r-- 1 vagrant vagrant  807 Feb 25  2020 .profile
drwx------ 2 vagrant root    4.0K Jan  3 18:58 .ssh
-rw-r--r-- 1 vagrant vagrant    0 Dec 19 19:42 .sudo_as_admin_successful
-rw-r--r-- 1 vagrant vagrant    6 Dec 19 19:42 .vbox_version
-rw-r--r-- 1 root    root     180 Dec 19 19:44 .wget-hsts
vagrant@vagrant:~$ dir
vagrant@vagrant:~$ type -a bash
bash is /usr/bin/bash
bash is /bin/bash
vagrant@vagrant:~$ cp /bin/bash /tmp/new_path_directory/
vagrant@vagrant:~$ type -a bash
bash is /usr/bin/bash
bash is /bin/bash
vagrant@vagrant:~$ echo $PATH
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin
vagrant@vagrant:~$ PATH=/tmp/new_path_directory/:$PATH
vagrant@vagrant:~$ type -a bash
bash is /tmp/new_path_directory/bash
bash is /usr/bin/bash
bash is /bin/bash
vagrant@vagrant:~$ 


13. Чем отличается планирование команд с помощью batch и at?

at — это утилита командной строки, которая позволяет вам планировать выполнение команд в определенное время. Задания, созданные с помощью at , выполняются только один раз.

batch или его псевдоним at -b планирует задания и выполняет их в пакетной очереди, если позволяет уровень загрузки системы. 
По умолчанию задания выполняются, когда средняя загрузка системы ниже 1,5. Значение нагрузки можно указать при вызове демона atd . 
Если средняя загрузка системы выше указанной, задания будут ждать в очереди.


14. Завершите работу виртуальной машины чтобы не расходовать ресурсы компьютера и/или батарею ноутбука.

ivan@PSB141C02:~/Vagrant-congis$ vagrant suspend
==> default: Saving VM state and suspending execution...
ivan@PSB141C02:~/Vagrant-congis$ 
































