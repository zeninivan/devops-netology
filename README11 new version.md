# Домашнее задание к занятию "3.9. Элементы безопасности информационных систем" #


#### 1. Установите Bitwarden плагин для браузера. Зарегестрируйтесь и сохраните несколько паролей.

Плагин Bitwarden успешно установлен в Firefox и зарегистрирован. Сохранены пароли и логины для GitHub, GitLab и Bitbucket.
*Добавлен скриншот "Снимок экрана ДЗ 3.9 упражнение 1".*

![](../../HomeTasks/ДЗ 3.9. Элементы безопасности/Снимок экрана ДЗ 3.9 упражнение 1.png)
***

#### 2. Установите Google authenticator на мобильный телефон. Настройте вход в Bitwarden акаунт через Google authenticator OTP.

Установлено приложение Authy. Добавил аккаунт Bitwarden и включил в настройках двухфакторную аутентификацию.
Two-step Login работает.

*Добавлен скриншот "Снимок экрана ДЗ 3.9 упражнение 1".*

![](../../HomeTasks/ДЗ 3.9. Элементы безопасности/Снимок экрана ДЗ 3.9 упражнение 2.png)
***

#### 3. Установите apache2, сгенерируйте самоподписанный сертификат, настройте тестовый сайт для работы по HTTPS.

Устанавливаем пакет apache2
```sudo apt install apache2```

Включаем mod_ssl с помощью команды a2enmod:
```sudo a2enmod ssl```

Чтобы активировать новую конфигурацию, перезапускаем apache2:
```sudo systemctl restart apache2```

Создаем файлы SSL-ключа и сертификата с помощью команды openssl:
```
ivan@HP-Pavilion-dv6:~$ sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/apache-selfsigned.key -out /etc/ssl/certs/apache-selfsigned.crt
[sudo] пароль для ivan: 
Generating a RSA private key
................................................................................+++++
....................................................................................................................................................................................+++++
writing new private key to '/etc/ssl/private/apache-selfsigned.key'
-----
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [AU]:RU
State or Province Name (full name) [Some-State]:Moscow
Locality Name (eg, city) []:Moscow
Organization Name (eg, company) [Internet Widgits Pty Ltd]:Company Name 
Organizational Unit Name (eg, section) []:Org
Common Name (e.g. server FQDN or YOUR name) []:www.example.com
Email Address []:
ivan@HP-Pavilion-dv6:~$
```
Настройка Apache для использования SSL:

Открываем новый файл в каталоге */etc/apache2/sites-available*.
```sudo vim /etc/apache2/sites-available/www.example.com.conf```
```
<VirtualHost *:443>
   ServerName www.example.com
   DocumentRoot /var/www/www.example.com

   SSLEngine on
   SSLCertificateFile /etc/ssl/certs/apache-selfsigned.crt
   SSLCertificateKeyFile /etc/ssl/private/apache-selfsigned.key
</VirtualHost>
```
Теперь создадим директорию и поместим в нее HTML-файл для целей тестирования:
```sudo mkdir /var/www/www.example.com```

Открываем новый *index.html* файл с помощью vim:
```sudo vim /var/www/www.example.com/index.html```

Включаем файл конфигурации с помощью инструмента *a2ensite*:
```
ivan@HP-Pavilion-dv6:~$ sudo a2ensite www.example.com.conf
Enabling site www.example.com.
To activate the new configuration, you need to run:
  systemctl reload apache2
ivan@HP-Pavilion-dv6:~$
```
Проверим наличие ошибок конфигурации:
```
ivan@HP-Pavilion-dv6:~$ sudo apache2ctl configtest
AH00558: apache2: Could not reliably determine the server's fully qualified domain name, using 127.0.1.1. Set the 'ServerName' directive globally to suppress this message
Syntax OK
ivan@HP-Pavilion-dv6:~$
```
Т.к. в примере используется сайт https://www.example.com, то нужно, чтобы сервер Apache не отправлял запрос в интернет, а направлял его на локальный хост машины.  
Для этого внесем изменения в файл */etc/hosts*, добавив:

```127.0.0.1       www.example.com```

Этот файл выступает в роли локального DNS сервера. Если компьютер находит ip для домена в нем, то запрос в интернет уже не отправляется. 
````
ivan@HP-Pavilion-dv6:~$ cat /etc/hosts
127.0.0.1	localhost
127.0.1.1	HP-Pavilion-dv6
127.0.0.1	www.example.com
````

Перезагрузим Apache для реализации изменений:
```sudo systemctl reload apache2```

Вводим в браузере: https://127.0.0.1/ и получаем страничку **"it worked!"**

*Добавлен скриншот экрана "Снимок экрана ДЗ 3.9 упражнение 3".*

![](../../HomeTasks/ДЗ 3.9. Элементы безопасности/Снимок экрана ДЗ 3.9 упражнение 3.png)
***


#### 4. Проверьте на TLS уязвимости произвольный сайт в интернете (кроме сайтов МВД, ФСБ, МинОбр, НацБанк, РосКосмос, РосАтом, РосНАНО и любых госкомпаний, объектов КИИ, ВПК ... и тому подобное). 

Копируем с Гитхаб скрипт:
```git clone --depth 1 https://github.com/drwetter/testssl.sh.git```

Проверяем сайта https://www.google.com/ на уязвимость:

```ivan@HP-Pavilion-dv6:~/testssl.sh$ ./testssl.sh -U --sneaky https://www.google.com/

    testssl.sh       3.1dev from https://testssl.sh/dev/
    (7b38198 2022-02-17 09:04:23 -- )

      This program is free software. Distribution and
             modification under GPLv2 permitted.
      USAGE w/o ANY WARRANTY. USE IT AT YOUR OWN RISK!
       Please file bugs @ https://testssl.sh/bugs/

 Using "OpenSSL 1.0.2-chacha (1.0.2k-dev)" [~183 ciphers]
 on HP-Pavilion-dv6:./bin/openssl.Linux.x86_64
 (built: "Jan 18 17:12:17 2019", platform: "linux-x86_64")

Testing all IPv4 addresses (port 443): 74.125.205.147 74.125.205.103 74.125.205.105 74.125.205.106 74.125.205.99 74.125.205.104
--------------------------------------------------------------------------------------------------------------------------------------------
 Start 2022-02-25 22:15:52        -->> 74.125.205.147:443 (www.google.com) <<--

 Further IP addresses:   74.125.205.104 74.125.205.99 74.125.205.106 74.125.205.105 74.125.205.103 2a00:1450:4010:c02::63 2a00:1450:4010:c02::67 2a00:1450:4010:c02::69 2a00:1450:4010:c02::93 
 rDNS (74.125.205.147):  le-in-f147.1e100.net.
 Service detected:       HTTP

 Testing vulnerabilities 

 Heartbleed (CVE-2014-0160)                not vulnerable (OK), no heartbeat extension
 CCS (CVE-2014-0224)                       not vulnerable (OK)
 Ticketbleed (CVE-2016-9244), experiment.  not vulnerable (OK)
 ROBOT                                     not vulnerable (OK)
 Secure Renegotiation (RFC 5746)           supported (OK)
 Secure Client-Initiated Renegotiation     not vulnerable (OK)
 CRIME, TLS (CVE-2012-4929)                not vulnerable (OK)
 BREACH (CVE-2013-3587)                    potentially NOT ok, "br gzip" HTTP compression detected. - only supplied "/" tested
                                           Can be ignored for static pages or if no secrets in the page
 POODLE, SSL (CVE-2014-3566)               not vulnerable (OK)
 TLS_FALLBACK_SCSV (RFC 7507)              Downgrade attack prevention supported (OK)
 SWEET32 (CVE-2016-2183, CVE-2016-6329)    VULNERABLE, uses 64 bit block ciphers
 FREAK (CVE-2015-0204)                     not vulnerable (OK)
 DROWN (CVE-2016-0800, CVE-2016-0703)      not vulnerable on this host and port (OK)
                                           make sure you don't use this certificate elsewhere with SSLv2 enabled services
                                           https://censys.io/ipv4?q=A5E4E5FA13750D3C19178C797191A5C04CBA3CA71C4F6A9B89F93CA785F1517F could help you to find out
 LOGJAM (CVE-2015-4000), experimental      not vulnerable (OK): no DH EXPORT ciphers, no DH key detected with <= TLS 1.2
 BEAST (CVE-2011-3389)                     TLS1: ECDHE-ECDSA-AES128-SHA ECDHE-ECDSA-AES256-SHA ECDHE-RSA-AES128-SHA ECDHE-RSA-AES256-SHA AES128-SHA AES256-SHA DES-CBC3-SHA 
                                           VULNERABLE -- but also supports higher protocols  TLSv1.1 TLSv1.2 (likely mitigated)
 LUCKY13 (CVE-2013-0169), experimental     potentially VULNERABLE, uses cipher block chaining (CBC) ciphers with TLS. Check patches
 Winshock (CVE-2014-6321), experimental    not vulnerable (OK)
 RC4 (CVE-2013-2566, CVE-2015-2808)        no RC4 ciphers detected (OK)

 Done 2022-02-25 22:16:35 [  47s] -->> 74.125.205.147:443 (www.google.com) <<--

--------------------------------------------------------------------------------------------------------------------------------------------

*часть текста пропущено*

 Done 2022-02-25 22:20:34 [ 286s] -->> 74.125.205.104:443 (www.google.com) <<--
--------------------------------------------------------------------------------------------------------------------------------------------
Done testing now all IP addresses (on port 443): 74.125.205.147 74.125.205.103 74.125.205.105 74.125.205.106 74.125.205.99 74.125.205.104
ivan@HP-Pavilion-dv6:~/testssl.sh$
```
***

#### 5. Установите на Ubuntu ssh сервер, сгенерируйте новый приватный ключ. Скопируйте свой публичный ключ на другой сервер. Подключитесь к серверу по SSH-ключу.

Установка sshd сервера
````
sudo apt install openssh-server
systemctl start sshd.service
systemctl enable sshd.service
````

Генерим ключи

*id_rsa - приватный ключ*

*id_rsa.pub - публичный ключ*
````
ivan@HP-Pavilion-dv6:~$ ssh-keygen
Generating public/private rsa key pair.
Enter file in which to save the key (/home/ivan/.ssh/id_rsa): 
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/ivan/.ssh/id_rsa
Your public key has been saved in /home/ivan/.ssh/id_rsa.pub
The key fingerprint is:
SHA256:IeE4zWy/rtI+o4lF4ywICq6OqLvx/sRUEx3piXCGvAA ivan@HP-Pavilion-dv6
The key's randomart image is:
+---[RSA 3072]----+
|E. . .o..o       |
|  . +*ooo        |
|   .o=Xo..       |
|    .+.+o.       |
|o   +   S        |
|=. * .   .       |
|+.. *.  .        |
|+o =..+.         |
|X=+.++o+.        |
+----[SHA256]-----+
ivan@HP-Pavilion-dv6:~$
````

Копируем публичный ключ на удаленный сервер (в данном случае на виртуальную машину)
вручную в файл *authorized_keys*

Выведем содержимое ключа *id_rsa.pub* на локальном компьютере, используя следующую команду:
````
ivan@HP-Pavilion-dv6:~/Vagrant$ cat ~/.ssh/id_rsa.pub
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQC+N6BvpFXD5BK4jUl8RZADBI6d84cIOtTNQDNTnzfW/cdkOCIR3bIZckc+piaq8Uud4pJa9cwmqk5htMmpdT+W3GCSiLtGA0b8fxBN1Y0HFHW80BzvTh+Tmp9lVJHAWXGX78KLSx3pYe3r9FDRAWNbbd2fPuWK9uqaU6XqeYApIaYSE8wgkT/Z3F0ONxVKTE2+avcrE+hm7+sNwi9EOlac5ydXOPGXo9hGtgccGefAabVJEqYUf+1H3dE2KjxEM2qRJfpC+2Xo7TSXrhXDEqCjQMEhSQwIy0fHbRsXu8gOkHaG6hYqVHK0i9poYWGi85DoUiz/gqTdffRdgq62MWOwzhI+uU9YmiwvCg7wfl4nB9u4C2poGigAjJ9J+UQabiSwEMCW/qQ4HnTPx8XkdBtXf9lX3IL5oxdVW2aYOaf/SLTwxcf0U4xtuhoRjVxbiPC00NMnd463D/S3H68uKqpeJxu5pR25vgrp57pz8Y9zeRB2we4zAwqrhX5GlVSdRZc= ivan@HP-Pavilion-dv6
ivan@HP-Pavilion-dv6:~/Vagrant$
````

Теперь можно создать или изменить файл *authorized_keys* на удаленной (виртуальной машине).

```echo public_key_string >> ~/.ssh/authorized_keys```

В вышеуказанной команде надо замените public_key_string результатом команды ```cat ~/.ssh/id_rsa.pub```, выполненной на локальном компьютере. 

Подключаемся к виртуальной машине стандартным способом и добавляем public_key_string:
````
vagrant@vagrant:~/.ssh$ echo ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQC+N6BvpFXD5BK4jUl8RZADBI6d84cIOtTNQDNTnzfW/cdkOCIR3bIZckc+piaq8Uud4pJa9cwmqk5htMmpdT+W3GCSiLtGA0b8fxBN1Y0HFHW80BzvTh+Tmp9lVJHAWXGX78KLSx3pYe3r9FDRAWNbbd2fPuWK9uqaU6XqeYApIaYSE8wgkT/Z3F0ONxVKTE2+avcrE+hm7+sNwi9EOlac5ydXOPGXo9hGtgccGefAabVJEqYUf+1H3dE2KjxEM2qRJfpC+2Xo7TSXrhXDEqCjQMEhSQwIy0fHbRsXu8gOkHaG6hYqVHK0i9poYWGi85DoUiz/gqTdffRdgq62MWOwzhI+uU9YmiwvCg7wfl4nB9u4C2poGigAjJ9J+UQabiSwEMCW/qQ4HnTPx8XkdBtXf9lX3IL5oxdVW2aYOaf/SLTwxcf0U4xtuhoRjVxbiPC00NMnd463D/S3H68uKqpeJxu5pR25vgrp57pz8Y9zeRB2we4zAwqrhX5GlVSdRZc= ivan@HP-Pavilion-dv6 >> ~/.ssh/authorized_keys
````

Проверяем:
````
vagrant@vagrant:~/.ssh$ cat authorized_keys
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC5brTwdEHerCWtHyB5zw0BEOLVDvqh9tPtAxCeCYKPoc5l9mjrL8koASfXXydN3ytXFwyfFEVmvF2dcfnkLvIi7EoKKxtakwzEuMBMU/u1vkEL+VtzCzzyve2hPq/r8IpLab9BYABDn4cS/FGpBo7+sk/LYWes7+AsEdnCqXhUhojvsp1bbUMfVotDqzIalnrmG6y0hFHXjGlCBmDe00m0hcHiPXiozxl7uJjiMhyOruaKAbnzwUtQ/36XuTQCZAYOVSHMsjwihTa1XhusymKM2ma5VJFTyOfI/h5KusEvuFQVs7N11Ma/mOk8bt1nzKfi+nAiq0nYYCfdKjqQB5MH vagrant
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQC+N6BvpFXD5BK4jUl8RZADBI6d84cIOtTNQDNTnzfW/cdkOCIR3bIZckc+piaq8Uud4pJa9cwmqk5htMmpdT+W3GCSiLtGA0b8fxBN1Y0HFHW80BzvTh+Tmp9lVJHAWXGX78KLSx3pYe3r9FDRAWNbbd2fPuWK9uqaU6XqeYApIaYSE8wgkT/Z3F0ONxVKTE2+avcrE+hm7+sNwi9EOlac5ydXOPGXo9hGtgccGefAabVJEqYUf+1H3dE2KjxEM2qRJfpC+2Xo7TSXrhXDEqCjQMEhSQwIy0fHbRsXu8gOkHaG6hYqVHK0i9poYWGi85DoUiz/gqTdffRdgq62MWOwzhI+uU9YmiwvCg7wfl4nB9u4C2poGigAjJ9J+UQabiSwEMCW/qQ4HnTPx8XkdBtXf9lX3IL5oxdVW2aYOaf/SLTwxcf0U4xtuhoRjVxbiPC00NMnd463D/S3H68uKqpeJxu5pR25vgrp57pz8Y9zeRB2we4zAwqrhX5GlVSdRZc= ivan@HP-Pavilion-dv6
vagrant@vagrant:~/.ssh$ ^C
````

Пробуем установить аутентификацию без пароля.
Подключаемся по нестандартному ключу. Успешно.
````
ivan@HP-Pavilion-dv6:~/Vagrant$ ssh -i /home/ivan/Vagrant/.vagrant/machines/default/virtualbox/private_key vagrant@127.0.0.1 -p 2222
Welcome to Ubuntu 20.04.3 LTS (GNU/Linux 5.4.0-91-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Sat 26 Feb 2022 01:17:55 PM UTC

  System load:  0.0                Processes:             119
  Usage of /:   12.3% of 30.88GB   Users logged in:       0
  Memory usage: 23%                IPv4 address for eth0: 10.0.2.15
  Swap usage:   0%

This system is built by the Bento project by Chef Software
More information can be found at https://github.com/chef/bento
Last login: Sat Feb 26 13:03:31 2022 from 10.0.2.2
vagrant@vagrant:~$ 
````
***

#### 6. Переименуйте файлы ключей из задания 5. Настройте файл конфигурации SSH клиента, так чтобы вход на удаленный сервер осуществлялся по имени сервера.

Создаем файл *~/.ssh/config*, указываем имя хоста и путь к файлу ключа.
````
ivan@HP-Pavilion-dv6:~/.ssh$ vim config
ivan@HP-Pavilion-dv6:~/.ssh$ cat config
Host Test_VM_connect
  HostName 127.0.0.1
  User vagrant
  Port 2222
  IdentityFile ~/.ssh/id_rsa
ivan@HP-Pavilion-dv6:~/.ssh$ cd ../
````
Пробуем подключиться на удаленный сервер по имени хоста Test_VM_connect:
````
ivan@HP-Pavilion-dv6:~/Vagrant$ ssh Test_VM_connect
Welcome to Ubuntu 20.04.3 LTS (GNU/Linux 5.4.0-91-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Sun 27 Feb 2022 03:31:23 PM UTC

  System load:  0.0                Processes:             121
  Usage of /:   12.3% of 30.88GB   Users logged in:       0
  Memory usage: 24%                IPv4 address for eth0: 10.0.2.15
  Swap usage:   0%

This system is built by the Bento project by Chef Software
More information can be found at https://github.com/chef/bento
Last login: Sat Feb 26 13:17:55 2022 from 10.0.2.2
vagrant@vagrant:~$ 
````
***

#### 7. Соберите дамп трафика утилитой tcpdump в формате pcap, 100 пакетов. Откройте файл pcap в Wireshark.

Проверяем список доступных интерфейсов
````
ivan@HP-Pavilion-dv6:~/Vagrant$ tcpdump -D
1.wlo1 [Up, Running]
2.lo [Up, Running, Loopback]
3.any (Pseudo-device that captures on all interfaces) [Up, Running]
4.eno1 [Up]
5.bluetooth-monitor (Bluetooth Linux Monitor) [none]
6.nflog (Linux netfilter log (NFLOG) interface) [none]
7.nfqueue (Linux netfilter queue (NFQUEUE) interface) [none]
ivan@HP-Pavilion-dv6:~/Vagrant$
````
Собираем дамп трафика (100 пакетов) в файл 0001.pcap
````
ivan@HP-Pavilion-dv6:~/Vagrant$ sudo tcpdump -w 0001.pcap -i wlo1 -c 100
tcpdump: listening on wlo1, link-type EN10MB (Ethernet), capture size 262144 bytes
100 packets captured
108 packets received by filter
0 packets dropped by kernel
ivan@HP-Pavilion-dv6:~/Vagrant$
````
Устанавливаем Wireshark и открываем собранный tcpdump 0001.pcap
```sudo apt install wireshark```

*Добавлен скриншот "Снимок экрана ДЗ 3.9 упражнение 7".*

![](../../HomeTasks/ДЗ 3.9. Элементы безопасности/Снимок экрана ДЗ 3.9 упражнение 7.png)

