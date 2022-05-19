# Домашнее задание к занятию "5.2. Применение принципов IaaC в работе с виртуальными машинами"

## Задача 1 
Опишите своими словами основные преимущества применения на практике IaaC паттернов.

Какой из принципов IaaC является основополагающим?

###Ответ:
Основными приемуществами применения паттернов IaaC являются автоматизация, ускорение и минимизация ошибок в процессе создания/развертывания инфрастуктуры для новых продуктов ПО
за счет применения готовых шаблонов. Повышение стабильности конфигурации инфрастуктуры в процессе ее создания и масштабирования за счет ухода от ручных произвольных настроек. 
Подобно принципу, согласно которому один и тот же исходный код создает один и тот же двоичный файл, модель IaaC при каждом применении формирует одну и ту же среду. 
Инфраструктура, представленная в виде кода, может быть проверена и протестирована для предотвращения распространенных проблем развертывания.

На мой взгляд, основополагающим принципом IaaC является принцип идемпотентности, который позволяет получать одинаковые свойства объекта или операции при повторном, многократном выполнении
одних и тех же действий, связанных с созданием/настройкой инфраструктуры. Т.е. мы избегаем ситуации - «на моём компьютере всё работало».
IaaC является ключевой методикой DevOps.
---

## Задача 2
Чем Ansible выгодно отличается от других систем управление конфигурациями?

Какой, на ваш взгляд, метод работы систем конфигурации более надёжный push или pull?

###Ответ:
Ansible поддерживает работу с системами Linux и Windows, а также FreeBSD, Solaris, MacOS. Ansible использует существующую SSH инфраструктуру. 
В отличие от своих аналогов — Chef, Puppet и SaltStack, не требует установки агентов на удаленные системы, 
которыми необходимо управлять. Ansible хорошо зарекомендовал себя за простоту и возможность легко расширяться. 

На мой взгляд, нельзя однозначно ответить, какой режим, push или pull более надежен. У каждого подхода есть свои плюсы и минусы.
Следует использовать то, что больше подходит к конкретному случаю или комбинировать.

В основе подхода pull лежит тот факт, что все изменения применяются изнутри кластера. Внутри кластера есть оператор, который регулярно проверяет связанные репозитории. 
Если в них происходят какие-либо изменения, состояние кластера обновляется изнутри. Обычно считается, что подобный процесс весьма безопасен, поскольку ни у одного внешнего 
клиента нет доступа к правам администратора кластера.
В push-подходе внешняя система (преимущественно CD-пайплайны) запускает развертывания в кластер после коммита в Git-репозиторий или в случае успешного выполнения предыдущего CI-пайплайна. 
В этом подходе система обладает доступом в кластер.
---

## Задача 3 
Установить на личный компьютер:

    VirtualBox

    Vagrant

    Ansible
Приложить вывод команд установленных версий каждой из программ, оформленный в markdown.

###Ответ:
````
ivan@HP-Pavilion-dv6:~$ vboxmanage --version
6.1.32r149290

ivan@HP-Pavilion-dv6:~$ vagrant --version
Vagrant 2.2.6

ivan@HP-Pavilion-dv6:~$ ansible --version
ansible 2.9.6
  config file = /etc/ansible/ansible.cfg
  configured module search path = ['/home/ivan/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
  ansible python module location = /usr/lib/python3/dist-packages/ansible
  executable location = /usr/bin/ansible
  python version = 3.8.10 (default, Mar 15 2022, 12:22:08) [GCC 9.4.0]
````  
---

## Задача 4 (*)

Воспроизвести практическую часть лекции самостоятельно.

Создать виртуальную машину.

Зайти внутрь ВМ, убедиться, что Docker установлен с помощью команды

docker ps

###Ответ:
````
ivan@HP-Pavilion-dv6:~/virt-homeworks/05-virt-02-control-systems/src/vagrant$ vagrant up
Bringing machine 'server1.netology' up with 'virtualbox' provider...
==> server1.netology: Importing base box 'bento/ubuntu-20.04'...
==> server1.netology: Matching MAC address for NAT networking...
==> server1.netology: Checking if box 'bento/ubuntu-20.04' version '202112.19.0' is up to date...
==> server1.netology: Setting the name of the VM: server1.netology
==> server1.netology: Clearing any previously set network interfaces...
==> server1.netology: Preparing network interfaces based on configuration...
    server1.netology: Adapter 1: nat
    server1.netology: Adapter 2: hostonly
==> server1.netology: Forwarding ports...
    server1.netology: 22 (guest) => 20011 (host) (adapter 1)
    server1.netology: 22 (guest) => 2222 (host) (adapter 1)
==> server1.netology: Running 'pre-boot' VM customizations...
==> server1.netology: Booting VM...
==> server1.netology: Waiting for machine to boot. This may take a few minutes...
    server1.netology: SSH address: 127.0.0.1:2222
    server1.netology: SSH username: vagrant
    server1.netology: SSH auth method: private key
    server1.netology: 
    server1.netology: Vagrant insecure key detected. Vagrant will automatically replace
    server1.netology: this with a newly generated keypair for better security.
    server1.netology: 
    server1.netology: Inserting generated public key within guest...
    server1.netology: Removing insecure key from the guest if it's present...
    server1.netology: Key inserted! Disconnecting and reconnecting using new SSH key...
==> server1.netology: Machine booted and ready!
==> server1.netology: Checking for guest additions in VM...
==> server1.netology: Setting hostname...
==> server1.netology: Configuring and enabling network interfaces...
==> server1.netology: Mounting shared folders...
    server1.netology: /vagrant => /home/ivan/virt-homeworks/05-virt-02-control-systems/src/vagrant
==> server1.netology: Running provisioner: ansible...
Vagrant has automatically selected the compatibility mode '2.0'
according to the Ansible version installed (2.9.6).

Alternatively, the compatibility mode can be specified in your Vagrantfile:
https://www.vagrantup.com/docs/provisioning/ansible_common.html#compatibility_mode

    server1.netology: Running ansible-playbook...

PLAY [nodes] *******************************************************************

TASK [Gathering Facts] *********************************************************
ok: [server1.netology]

TASK [Create directory for ssh-keys] *******************************************
ok: [server1.netology]

TASK [Adding rsa-key in /root/.ssh/authorized_keys] ****************************
changed: [server1.netology]

TASK [Checking DNS] ************************************************************
changed: [server1.netology]

TASK [Installing tools] ********************************************************
ok: [server1.netology] => (item=['git', 'curl'])

TASK [Installing docker] *******************************************************
changed: [server1.netology]

TASK [Add the current user to docker group] ************************************
changed: [server1.netology]

PLAY RECAP *********************************************************************
server1.netology           : ok=7    changed=4    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   


ivan@HP-Pavilion-dv6:~/virt-homeworks/05-virt-02-control-systems/src/vagrant$ vagrant ssh
Welcome to Ubuntu 20.04.3 LTS (GNU/Linux 5.4.0-91-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Thu 19 May 2022 08:07:03 PM UTC

  System load:  0.08               Users logged in:          0
  Usage of /:   13.6% of 30.88GB   IPv4 address for docker0: 172.17.0.1
  Memory usage: 24%                IPv4 address for eth0:    10.0.2.15
  Swap usage:   0%                 IPv4 address for eth1:    192.168.56.11
  Processes:    108


This system is built by the Bento project by Chef Software
More information can be found at https://github.com/chef/bento
Last login: Thu May 19 19:54:00 2022 from 10.0.2.2
vagrant@server1:~$ docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
vagrant@server1:~$ 
````


---
