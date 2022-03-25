# Курсовая работа по итогам модуля "DevOps и системное администрирование" #

#### 1. Создайте виртуальную машину Linux.
Для выполнения курсовой работы используется ранее созданная VM Vagrant Default.
```
Welcome to Ubuntu 20.04.3 LTS (GNU/Linux 5.4.0-91-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Sun 13 Mar 2022 10:40:10 AM UTC

  System load:  0.07               Processes:             132
  Usage of /:   14.0% of 30.88GB   Users logged in:       1
  Memory usage: 11%                IPv4 address for eth0: 10.0.2.15
  Swap usage:   0%


This system is built by the Bento project by Chef Software
More information can be found at https://github.com/chef/bento
Last login: Sun Mar 13 10:35:54 2022 from 10.0.2.2
````
***

#### 2.Установите ufw и разрешите к этой машине сессии на порты 22 и 443, при этом трафик на интерфейсе localhost (lo) должен ходить свободно на все порты.

UFW установлен и настроен.

*Добавлен скриншот "Задание 2 Снимок 1".*

![](https://github.com/zeninivan/devops-netology/blob/main/%D0%9A%D1%83%D1%80%D1%81%D0%BE%D0%B2%D0%B0%D1%8F%20%D1%80%D0%B0%D0%B1%D0%BE%D1%82%D0%B0%20%D1%81%D0%BA%D1%80%D0%B8%D0%BD%D1%88%D0%BE%D1%82%D1%8B/%D0%97%D0%B0%D0%B4%D0%B0%D0%BD%D0%B8%D0%B5%202%20%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%201.png)
***

#### 3. Установите hashicorp vault (инструкция по ссылке).

Hashicorp vault установлен.  

*Добавлен скриншот "Задание 3".*

![](https://github.com/zeninivan/devops-netology/blob/main/%D0%9A%D1%83%D1%80%D1%81%D0%BE%D0%B2%D0%B0%D1%8F%20%D1%80%D0%B0%D0%B1%D0%BE%D1%82%D0%B0%20%D1%81%D0%BA%D1%80%D0%B8%D0%BD%D1%88%D0%BE%D1%82%D1%8B/%D0%97%D0%B0%D0%B4%D0%B0%D0%BD%D0%B8%D0%B5%203.png)
***

#### 4. Cоздайте центр сертификации по инструкции (ссылка) и выпустите сертификат для использования его в настройке веб-сервера nginx (срок жизни сертификата - месяц).

Центр сертификации создан и выпущены сертификаты:

*Добавлен скриншот "Задание 4 снимок 1".*

![](https://github.com/zeninivan/devops-netology/blob/main/%D0%9A%D1%83%D1%80%D1%81%D0%BE%D0%B2%D0%B0%D1%8F%20%D1%80%D0%B0%D0%B1%D0%BE%D1%82%D0%B0%20%D1%81%D0%BA%D1%80%D0%B8%D0%BD%D1%88%D0%BE%D1%82%D1%8B/%D0%97%D0%B0%D0%B4%D0%B0%D0%BD%D0%B8%D0%B5%204%20%D1%81%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%201.png)

````
vagrant@vagrant:~$ sudo apt-get install jq
Reading package lists... Done
Building dependency tree       
Reading state information... Done
The following additional packages will be installed:
  libjq1 libonig5
The following NEW packages will be installed:
  jq libjq1 libonig5
0 upgraded, 3 newly installed, 0 to remove and 0 not upgraded.
Need to get 313 kB of archives.
After this operation, 1062 kB of additional disk space will be used.
Do you want to continue? [Y/n] Y
Get:1 http://us.archive.ubuntu.com/ubuntu focal/universe amd64 libonig5 amd64 6.9.4-1 [142 kB]
Get:2 http://us.archive.ubuntu.com/ubuntu focal-updates/universe amd64 libjq1 amd64 1.6-1ubuntu0.20.04.1 [121 kB]
Get:3 http://us.archive.ubuntu.com/ubuntu focal-updates/universe amd64 jq amd64 1.6-1ubuntu0.20.04.1 [50.2 kB]
Fetched 313 kB in 4s (89.0 kB/s)
Selecting previously unselected package libonig5:amd64.
(Reading database ... 40687 files and directories currently installed.)
Preparing to unpack .../libonig5_6.9.4-1_amd64.deb ...
Unpacking libonig5:amd64 (6.9.4-1) ...
Selecting previously unselected package libjq1:amd64.
Preparing to unpack .../libjq1_1.6-1ubuntu0.20.04.1_amd64.deb ...
Unpacking libjq1:amd64 (1.6-1ubuntu0.20.04.1) ...
Selecting previously unselected package jq.
Preparing to unpack .../jq_1.6-1ubuntu0.20.04.1_amd64.deb ...
Unpacking jq (1.6-1ubuntu0.20.04.1) ...
Setting up libonig5:amd64 (6.9.4-1) ...
Setting up libjq1:amd64 (1.6-1ubuntu0.20.04.1) ...
Setting up jq (1.6-1ubuntu0.20.04.1) ...
Processing triggers for man-db (2.9.1-1) ...
Processing triggers for libc-bin (2.31-0ubuntu9.2) ...
vagrant@vagrant:~$

vagrant@vagrant:~$ vault server -dev -dev-root-token-id root
==> Vault server configuration:

             Api Address: http://127.0.0.1:8200
                     Cgo: disabled
         Cluster Address: https://127.0.0.1:8201
              Go Version: go1.17.5
              Listener 1: tcp (addr: "127.0.0.1:8200", cluster address: "127.0.0.1:8201", max_request_duration: "1m30s", max_request_size: "33554432", tls: "disabled")
               Log Level: info
                   Mlock: supported: true, enabled: false
           Recovery Mode: false
                 Storage: inmem
                 Version: Vault v1.9.3
             Version Sha: 7dbdd57243a0d8d9d9e07cd01eb657369f8e1b8a

==> Vault server started! Log data will stream in below:


You may need to set the following environment variable:

    $ export VAULT_ADDR='http://127.0.0.1:8200'

The unseal key and root token are displayed below in case you want to
seal/unseal the Vault or re-authenticate.
Unseal Key: boculfMKoK8sauad66VSsTQNuDF0h0uXW+YV8bXzIEU=
Root Token: root
Development mode should NOT be used in production installations!

vagrant@vagrant:~$ export VAULT_ADDR=http://127.0.0.1:8200
vagrant@vagrant:~$ export VAULT_TOKEN=root
````

Корневой сертификат выпущен:

````
ivan@HP-Pavilion-dv6:~/Vagrant$ cat CA_cert.crt
-----BEGIN CERTIFICATE-----
MIIDpzCCAo+gAwIBAgIUTPjgxkf2N2G3fQKAWYIDivz+1VwwDQYJKoZIhvcNAQEL
BQAwFjEUMBIGA1UEAxMLZXhhbXBsZS5jb20wHhcNMjIwMzEzMTUyMDE3WhcNMjIw
NDEyMTUyMDQ3WjAWMRQwEgYDVQQDEwtleGFtcGxlLmNvbTCCASIwDQYJKoZIhvcN
AQEBBQADggEPADCCAQoCggEBAM/kgE6BXk7dzXFUas0GWwlvPFJYy7E5TvypPd7F
7oJNDoYi5BC0bYbHlHqX43wmvF9DE/CW/bT6yy9aIgZFhJzpoMw6QEdRTPs00l/6
CPB/a0wFNsaUul1PgITkxukjwUC5irAV48YS3roj4YRszk48CnOSLOInJWiD9Jla
M4YFM2XWhhs8sNYkJX33HWz7wvibznEXsFjGrmbQHmhsJitIIUNI8L7ymsK+spCD
+vQZ5Il9nN3o7yA19qUYJLzcnrYp4o3uIuWGFTu6PYpShmEVIV2EwwC054N2Cqlu
q1PvMhuZhwQgxm+GGJrZ8Y/HFFH7BXWMAZF/9iDc877Zr8cCAwEAAaOB7DCB6TAO
BgNVHQ8BAf8EBAMCAQYwDwYDVR0TAQH/BAUwAwEB/zAdBgNVHQ4EFgQUxpluJjNy
sGHH/ZZmLkDlK2Y3QmYwHwYDVR0jBBgwFoAUxpluJjNysGHH/ZZmLkDlK2Y3QmYw
OwYIKwYBBQUHAQEELzAtMCsGCCsGAQUFBzAChh9odHRwOi8vMTI3LjAuMC4xOjgy
MDAvdjEvcGtpL2NhMBYGA1UdEQQPMA2CC2V4YW1wbGUuY29tMDEGA1UdHwQqMCgw
JqAkoCKGIGh0dHA6Ly8xMjcuMC4wLjE6ODIwMC92MS9wa2kvY3JsMA0GCSqGSIb3
DQEBCwUAA4IBAQCIWMzV0Ca53MMxJ25+TgasRaq3xOlvPs9Mcvr6lzozAG0NnHty
67Tc7eIj7HnUk792/+zk5OTKMhGMV3fxcsoKtX1DZMISHXJqwooTG2saM72+qTng
0TXGSqHCk09k9WWfreCPIRGN4f4OjqoozvIAdL+J2fBDM5FVlhaTSJQws8byBOWv
sUxg7uA/XcUfB4d5teN8DFQyY17WQ2E4tcCwEQV30Tv4/Tq7Ivm4NBYZv3kb+xU3
9XZLll4a6cqO+wDNL3YiL5nCy88SePWzw2jjeDr7RQjYSUXgeAdJM59Qy+kYjUh/
qtCJQLYwoBgCBcr26PCfIQjOQHQqGwhY9UxM
ivan@HP-Pavilion-dv6:~/Vagrant$ 
````
Выпуск intermediate CA и private key и парсинг из json в файлы сертификатов:
````
vagrant@vagrant:~$ vault secrets enable -path=pki_int pki
Success! Enabled the pki secrets engine at: pki_int/
vagrant@vagrant:~$ 
vagrant@vagrant:~$ vault secrets tune -max-lease-ttl=720h pki_int
Success! Tuned the secrets engine at: pki_int/
vagrant@vagrant:~$ 
vault write -format=json pki_int/intermediate/generate/internal \
     common_name="example.com Intermediate Authority" \
     | jq -r '.data.csr' > pki_intermediate.csr
vault write -format=json pki/root/sign-intermediate csr=@pki_intermediate.csr \
     format=pem_bundle ttl="720h" \
     | jq -r '.data.certificate' > intermediate.cert.pem
vagrant@vagrant:~$ vault write pki_int/intermediate/set-signed certificate=@intermediate.cert.pem
Success! Data written to: pki_int/intermediate/set-signed
vagrant@vagrant:~$ 
vagrant@vagrant:~$ vault write pki_int/roles/example-dot-com \
>      allowed_domains="example.com" \
>      allow_subdomains=true \
>      max_ttl="720h"
Success! Data written to: pki_int/roles/example-dot-com
vagrant@vagrant:~$ 
````
Запрос сертификата:
````
vault write -format=json pki_int/issue/example-dot-com common_name="test.example.com" ttl="72h"
````
Получены сертификаты в формате json:
````
{
  "request_id": "af95d9c9-0fb1-b4d1-c901-49268de64695",
  "lease_id": "",
  "lease_duration": 0,
  "renewable": false,
  "data": {
    "ca_chain": [
      "-----BEGIN CERTIFICATE-----\nMIIDpjCCAo6gAwIBAgIUZ6QkprRmxS8ufiMrv2oDI3aMs0wwDQYJKoZIhvcNAQEL\nBQAwFjEUMBIGA1UEAxMLZXhhbXBsZS5jb20wHhcNMjIwMzEzMTgyNTI0WhcNMjIw\nNDEyMTgyNTU0WjAtMSswKQYDVQQDEyJleGFtcGxlLmNvbSBJbnRlcm1lZGlhdGUg\nQXV0aG9yaXR5MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAqOWujfHE\nWoXwc397cLkRIoV4U9Utq7O5rkbjkkApxhmIu73EXpn8V6b8JVdd3Oj/wLajfqsI\n6oNpTMUPPc5aLk5JO/vuNXBsuFE17n5LqXjahU6pCKIPaIZ/CIXIDH7YVfXQ6SRN\nmdrn/im5b0cdJOr/96V/gLYWmNTGnc2f4Lf+hwFdKLgNm9VaP/UfkT/qqVte8lby\ns08PnqgjyOGOEfEygxpklQquLX+ZVVHtSvyW24jFBGDGlbPpLfx2tTtqvr07GPm4\n6XjQFqC6VRZZO431UpSUxokNf9E2My3gyj8kTn+CUunuB47DFKrddDV1HLEoxk+3\nc1L0Dc3zHkfr9QIDAQABo4HUMIHRMA4GA1UdDwEB/wQEAwIBBjAPBgNVHRMBAf8E\nBTADAQH/MB0GA1UdDgQWBBR+lns8VyVIcCFyfG5HnrFogrHwyjAfBgNVHSMEGDAW\ngBTO2+l1S/SywbcpoK4A5xhjFxwocjA7BggrBgEFBQcBAQQvMC0wKwYIKwYBBQUH\nMAKGH2h0dHA6Ly8xMjcuMC4wLjE6ODIwMC92MS9wa2kvY2EwMQYDVR0fBCowKDAm\noCSgIoYgaHR0cDovLzEyNy4wLjAuMTo4MjAwL3YxL3BraS9jcmwwDQYJKoZIhvcN\nAQELBQADggEBAG4h7FVEa9s4CbNIVUBVyF8KJwITTmSVz8Bjkd2ikNnZPmp5WBo6\njz03KNdUfutkZE5+ld158t56qFKz7eQfPj731QY3UnRX0J8GfngKKB/zZW0potMK\nmYEYrjt1dsBwBJSTE/iskpuCB9pWGuSuxGMwVZmT8rDQrwy/X7tjROlnIE07VZiy\ncAVG7rbpPbNMj0O2u0KoOWp4cNmcwus20BnFc3XfYwQDfqiZ9z5uJqLfdRJuL/KT\nF66ppm5oEc5iima+oKLmV05cK1MtTPT02hKlIXjGUQG2upchgCWJQAVFJX4gX4gX\nPnYCLji8H/LqKLDJUbyUv4cWEQLjAKhV/us=\n-----END CERTIFICATE-----"
    ],
    "certificate": "-----BEGIN CERTIFICATE-----\nMIIDZjCCAk6gAwIBAgIUaQ7TQ+ywNsPac91yjVwgWkC/growDQYJKoZIhvcNAQEL\nBQAwLTErMCkGA1UEAxMiZXhhbXBsZS5jb20gSW50ZXJtZWRpYXRlIEF1dGhvcml0\neTAeFw0yMjAzMTQxOTM5MzRaFw0yMjAzMTcxOTQwMDNaMBsxGTAXBgNVBAMTEHRl\nc3QuZXhhbXBsZS5jb20wggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIBAQDJ\nTzZ9TRULGqrgkbHvNVYJ0gpRJ6X6voABcwBJAcjx907PK+z4sfotEWL23qTzRp26\nxupvA4yTKVPb9MMYfI/Y3r0Z0EKo3sKGpRFVJ38ReMySIxvV4I0qC9SoDHIMhiQT\naynQydum42961cplutgGQXKBQ7nF3iq31j/d+zvT77H9PCAOpN7ra+elR+FmfOk/\nT91LOhy095fwQOHvuTIC7O2IWu8h1wzndWNr1H3OPsiKj4jb4BvSs2GENjJzg808\nKc9omtsMl3SJoMKGWEHFlx0dgcwTohSbhLLpVIoR9OXZWZOGbBhWGqVV6EpEPV98\n0tHV/0mH5NscD+g1jAADAgMBAAGjgY8wgYwwDgYDVR0PAQH/BAQDAgOoMB0GA1Ud\nJQQWMBQGCCsGAQUFBwMBBggrBgEFBQcDAjAdBgNVHQ4EFgQUa3tTm31BSochB5Wa\n4aNxQgF8cpEwHwYDVR0jBBgwFoAUfpZ7PFclSHAhcnxuR56xaIKx8MowGwYDVR0R\nBBQwEoIQdGVzdC5leGFtcGxlLmNvbTANBgkqhkiG9w0BAQsFAAOCAQEAQWmWyJeK\nqQAG5VaTXnekSlOOazpeYeGNj53MAGcGAfkMs6XolcNfY9490doDubLQy7qdnFNs\niwN5YQi/Vs9VBhL/8wI3nB6lmrB1tMA7cUikiHmkcABCNx+jQaGDZgir2MEKCw1M\ncQUItnv5mwgl9SBJFHyviCHE1b/Zxt161LNXY9rv8pX6EATeHNlpJm0VKXvjAy1l\nmNeubmlqOMvypzwFgVuR9YCuaobYC65KeusMPrbcMz3zS8A/69+41rlNb12PGgqP\nyCYOK3TopqZlypXqzwokJ9YvoERwF0CgRFaSfObQHPUsxd67lmx8GuzxNy6NyQ5z\nrSBlwFOtMfynWw==\n-----END CERTIFICATE-----",
    "expiration": 1647546003,
    "issuing_ca": "-----BEGIN CERTIFICATE-----\nMIIDpjCCAo6gAwIBAgIUZ6QkprRmxS8ufiMrv2oDI3aMs0wwDQYJKoZIhvcNAQEL\nBQAwFjEUMBIGA1UEAxMLZXhhbXBsZS5jb20wHhcNMjIwMzEzMTgyNTI0WhcNMjIw\nNDEyMTgyNTU0WjAtMSswKQYDVQQDEyJleGFtcGxlLmNvbSBJbnRlcm1lZGlhdGUg\nQXV0aG9yaXR5MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAqOWujfHE\nWoXwc397cLkRIoV4U9Utq7O5rkbjkkApxhmIu73EXpn8V6b8JVdd3Oj/wLajfqsI\n6oNpTMUPPc5aLk5JO/vuNXBsuFE17n5LqXjahU6pCKIPaIZ/CIXIDH7YVfXQ6SRN\nmdrn/im5b0cdJOr/96V/gLYWmNTGnc2f4Lf+hwFdKLgNm9VaP/UfkT/qqVte8lby\ns08PnqgjyOGOEfEygxpklQquLX+ZVVHtSvyW24jFBGDGlbPpLfx2tTtqvr07GPm4\n6XjQFqC6VRZZO431UpSUxokNf9E2My3gyj8kTn+CUunuB47DFKrddDV1HLEoxk+3\nc1L0Dc3zHkfr9QIDAQABo4HUMIHRMA4GA1UdDwEB/wQEAwIBBjAPBgNVHRMBAf8E\nBTADAQH/MB0GA1UdDgQWBBR+lns8VyVIcCFyfG5HnrFogrHwyjAfBgNVHSMEGDAW\ngBTO2+l1S/SywbcpoK4A5xhjFxwocjA7BggrBgEFBQcBAQQvMC0wKwYIKwYBBQUH\nMAKGH2h0dHA6Ly8xMjcuMC4wLjE6ODIwMC92MS9wa2kvY2EwMQYDVR0fBCowKDAm\noCSgIoYgaHR0cDovLzEyNy4wLjAuMTo4MjAwL3YxL3BraS9jcmwwDQYJKoZIhvcN\nAQELBQADggEBAG4h7FVEa9s4CbNIVUBVyF8KJwITTmSVz8Bjkd2ikNnZPmp5WBo6\njz03KNdUfutkZE5+ld158t56qFKz7eQfPj731QY3UnRX0J8GfngKKB/zZW0potMK\nmYEYrjt1dsBwBJSTE/iskpuCB9pWGuSuxGMwVZmT8rDQrwy/X7tjROlnIE07VZiy\ncAVG7rbpPbNMj0O2u0KoOWp4cNmcwus20BnFc3XfYwQDfqiZ9z5uJqLfdRJuL/KT\nF66ppm5oEc5iima+oKLmV05cK1MtTPT02hKlIXjGUQG2upchgCWJQAVFJX4gX4gX\nPnYCLji8H/LqKLDJUbyUv4cWEQLjAKhV/us=\n-----END CERTIFICATE-----",
    "private_key": "-----BEGIN RSA PRIVATE KEY-----\nMIIEpAIBAAKCAQEAyU82fU0VCxqq4JGx7zVWCdIKUSel+r6AAXMASQHI8fdOzyvs\n+LH6LRFi9t6k80adusbqbwOMkylT2/TDGHyP2N69GdBCqN7ChqURVSd/EXjMkiMb\n1eCNKgvUqAxyDIYkE2sp0MnbpuNvetXKZbrYBkFygUO5xd4qt9Y/3fs70++x/Twg\nDqTe62vnpUfhZnzpP0/dSzoctPeX8EDh77kyAuztiFrvIdcM53Vja9R9zj7Iio+I\n2+Ab0rNhhDYyc4PNPCnPaJrbDJd0iaDChlhBxZcdHYHME6IUm4Sy6VSKEfTl2VmT\nhmwYVhqlVehKRD1ffNLR1f9Jh+TbHA/oNYwAAwIDAQABAoIBAQCirHcs1ABAQ+F3\nrWRrF9+Z+fhKUk7HC+/mu+asGFwoY590vFs3MKMojhc5xProd9T33MwOv4B2Xvwc\nD3MkM2wOZRfMZ0WmrrPlGDikZlFBbitpoCNbNqT8KClFTyFOS4uVgZB93tC30KwC\nSAbRJCZzD6oXGQJjCb/dZK2hlOZowKWZYxoI6GnVMXJZ93dofj3P9JJpgucF2w0f\nvsi9u5giQ+TTLJwhyv9Kq2Bhgdbg1nWq3sdqZfvd6W/faJJNj+etV9MIZ4Wwd1iF\n/Zv2Re7Wc1wqIzWppFeCOkLU84hltYQYunAQ+I0e1h8AoyyP/Q6hzMY8C7kWNoVm\ncFgc2PLxAoGBAPp52qkSufwGqxoIQkWA7OzOhjS6gTlAwU//eu4xnFH5GwBVzEa4\nr9fAw4ldl+zArplmOAEcGemX02/xbXsFOId0nUbmVsXOOmRRZmUi2EQjSBLjqs7Q\nNy5A4ko5KAS155zaBSZK2yfLJ7lKxkK5P/2n/Z8Kr67NCUcYnIBK+fsLAoGBAM2/\nxb4Zyzrr2lLdCdsuxiOdIKNtLAezLa09xflUAaavOiyzUkLi+2W24zhvDKfbLrDo\nPBTzvyWl1CyQAxZtMhsiJN4qOk7uh/H3oHFuQsOBEdgiZfVif+063y0L0FJdhFm+\n6kUQyucenBiGxje7SFWD5Xldlfikzqd0qt9hjWnpAoGBAPErbuyoSUdvLEQOe7Ds\naDPCztnqUg5MWVWrijPatMcA8YyrD9twbG9y/VNAOM4O4I53K6l140VVmJIKhf2T\nk1Bpah4gHqCq5vI8pjjvCgjhZ744U/h55we0Fa6dxfhzJaWTDq5GGSoBpCf25VrN\nf92+aKc/5NSMO0inW9jzWCrtAoGAQB3d+oLBQWhUTfRR6PrnhhumGyefS/r7ZfxV\nIICcTxxWDa4IGY3wd98JagG5OOnl3/1PE9xtmcbWmth6DdgTgD8grBcOuqA8vxvC\n5PZOWexz6h22FkUOxpfNCpWe4rv/zZPgH4u/H0z7qez+AkobnYKP1UVjweth9u81\nfI1C61kCgYAzB7DVRDLi3QR5ZtOghZcTktOTZ2RkNJFLjdQUeLJSyGKiJemxT4BU\nOlcVbjZsZZvdGJG9zP6vSe0qNYRN496nYPXRtl5dDWMNMmlO131OJkK82l1tjx7V\n38p0LqS6WoFw/ql94wqwnCW+mj+wtOxyD1CiYIZWgUxvw/Io+qYFxg==\n-----END RSA PRIVATE KEY-----",
    "private_key_type": "rsa",
    "serial_number": "69:0e:d3:43:ec:b0:36:c3:da:73:dd:72:8d:5c:20:5a:40:bf:82:ba"
  },
  "warnings": null
}
````
Парсинг сертификатов в файлы:
````
cat mycrt.json | jq -r .data.certificate > test.example.com.crt.pem
//cat mycrt.json | jq -r .data.issuing_ca >> test.example.com.crt.pem //не нужен, не используется
cat mycrt.json | jq -r .data.private_key > test.example.com.crt.key
cat mycrt.json | jq -r .data.ca_chain[] >> test.example.com.crt.pem 
````
Получены файлы сертификатов:

*Добавлен скриншот "Задание 4 снимок 2".*

![](https://github.com/zeninivan/devops-netology/blob/main/%D0%9A%D1%83%D1%80%D1%81%D0%BE%D0%B2%D0%B0%D1%8F%20%D1%80%D0%B0%D0%B1%D0%BE%D1%82%D0%B0%20%D1%81%D0%BA%D1%80%D0%B8%D0%BD%D1%88%D0%BE%D1%82%D1%8B/%D0%97%D0%B0%D0%B4%D0%B0%D0%BD%D0%B8%D0%B5%204%20%D1%81%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%202.png)

***

#### 5. Установите корневой сертификат созданного центра сертификации в доверенные в хостовой системе.

Добавление корневого сертификата в доверенные на хостовой машине Ubuntu:
````
ivan@HP-Pavilion-dv6:~/Vagrant$ sudo cp test.example.com.crt /usr/local/share/ca-certificates/
[sudo] пароль для ivan: 
ivan@HP-Pavilion-dv6:~/Vagrant$ awk -v cmd='openssl x509 -noout -subject' ' /BEGIN/{close(cmd)};{print | cmd}' < /etc/ssl/certs/ca-certificates.crt | grep -i example.com
subject=CN = example.com
ivan@HP-Pavilion-dv6:~/Vagrant$ sudo gcr-viewer /home/ivan/Vagrant/test.example.com.crt
ivan@HP-Pavilion-dv6:~/Vagrant$ sudo update-ca-certificates
Updating certificates in /etc/ssl/certs...
rehash: warning: skipping test.example.com.pem,it does not contain exactly one certificate or CRL
1 added, 0 removed; done.
Running hooks in /etc/ca-certificates/update.d...
done.
ivan@HP-Pavilion-dv6:~/Vagrant$ awk -v cmd='openssl x509 -noout -subject' ' /BEGIN/{close(cmd)};{print | cmd}' < /etc/ssl/certs/ca-certificates.crt | grep -i example.com
subject=CN = example.com
subject=CN = test.example.com
subject=CN = example.com Intermediate Authority
````

Добавление корневого сертификата в браузере "Firefox":

*Добавлен скриншот "Задание 5 снимок 1".*

![](https://github.com/zeninivan/devops-netology/blob/main/%D0%9A%D1%83%D1%80%D1%81%D0%BE%D0%B2%D0%B0%D1%8F%20%D1%80%D0%B0%D0%B1%D0%BE%D1%82%D0%B0%20%D1%81%D0%BA%D1%80%D0%B8%D0%BD%D1%88%D0%BE%D1%82%D1%8B/%D0%97%D0%B0%D0%B4%D0%B0%D0%BD%D0%B8%D0%B5%205%20%D1%81%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%201.png)

*Добавлен скриншот "Задание 5 снимок 2".*

![](https://github.com/zeninivan/devops-netology/blob/main/%D0%9A%D1%83%D1%80%D1%81%D0%BE%D0%B2%D0%B0%D1%8F%20%D1%80%D0%B0%D0%B1%D0%BE%D1%82%D0%B0%20%D1%81%D0%BA%D1%80%D0%B8%D0%BD%D1%88%D0%BE%D1%82%D1%8B/%D0%97%D0%B0%D0%B4%D0%B0%D0%BD%D0%B8%D0%B5%205%20%D1%81%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%202.png)
***

#### 6.Установите nginx.
Ngnix установлен.

*Добавлен скриншот "Задание 6 установлен Nginx".*

![](https://github.com/zeninivan/devops-netology/blob/main/%D0%9A%D1%83%D1%80%D1%81%D0%BE%D0%B2%D0%B0%D1%8F%20%D1%80%D0%B0%D0%B1%D0%BE%D1%82%D0%B0%20%D1%81%D0%BA%D1%80%D0%B8%D0%BD%D1%88%D0%BE%D1%82%D1%8B/%D0%97%D0%B0%D0%B4%D0%B0%D0%BD%D0%B8%D0%B5%206%20%D1%83%D1%81%D1%82%D0%B0%D0%BD%D0%BE%D0%B2%D0%BB%D0%B5%D0%BD%20Nginx.png)
***

#### 7. По инструкции (ссылка) настройте nginx на https, используя ранее подготовленный сертификат:
Ngnix запущен, проверка синтаксиса проходит успешно.

*Добавлен скриншот "Задание 7 снимок 1".*

![](https://github.com/zeninivan/devops-netology/blob/main/%D0%9A%D1%83%D1%80%D1%81%D0%BE%D0%B2%D0%B0%D1%8F%20%D1%80%D0%B0%D0%B1%D0%BE%D1%82%D0%B0%20%D1%81%D0%BA%D1%80%D0%B8%D0%BD%D1%88%D0%BE%D1%82%D1%8B/%D0%97%D0%B0%D0%B4%D0%B0%D0%BD%D0%B8%D0%B5%207%20%D1%81%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%201.png)

Конфиг nginx.conf на данный момент выглядит следующим образом:
````
user www-data;
worker_processes auto;
pid /run/nginx.pid;
include /etc/nginx/modules-enabled/*.conf;

events {
        worker_connections 768;
        # multi_accept on;
}

http {

        ##
        # Basic Settings
        ##

        sendfile on;
        tcp_nopush on;
        tcp_nodelay on;
        keepalive_timeout 65;
        types_hash_max_size 2048;
        # server_tokens off;

        server_names_hash_bucket_size 64;
        # server_name_in_redirect off;

        include /etc/nginx/mime.types;
        default_type application/octet-stream;

        ##
        # SSL Settings
        ##

server {
    listen              443 ssl;
    server_name         test.example.com;
    ssl_certificate     /vagrant/test.example.com.crt.pem;
    ssl_certificate_key /vagrant/test.example.com.crt.key;
    ssl_protocols       TLSv1 TLSv1.1 TLSv1.2;
    ssl_ciphers         HIGH:!aNULL:!MD5;
}

        ssl_protocols TLSv1 TLSv1.1 TLSv1.2 TLSv1.3; # Dropping SSLv3, ref: POODLE
        ssl_prefer_server_ciphers on;

        ##
        # Logging Settings
        ##
        access_log /var/log/nginx/access.log;
        error_log /var/log/nginx/error.log;

        ##
        # Gzip Settings
        ##

        gzip on;

        # gzip_vary on;
        # gzip_proxied any;
        # gzip_comp_level 6;
        # gzip_buffers 16 8k;
        # gzip_http_version 1.1;
        # gzip_types text/plain text/css application/json application/javascript text/xml application/xml application/xml+rss text/javascript;

        ##
        # Virtual Host Configs
        ##

        include /etc/nginx/conf.d/*.conf;
        include /etc/nginx/sites-enabled/*;
}


#mail {
#       # See sample authentication script at:
#       # http://wiki.nginx.org/ImapAuthenticateWithApachePhpScript
#
#       # auth_http localhost/auth.php;
#       # pop3_capabilities "TOP" "USER";
#       # imap_capabilities "IMAP4rev1" "UIDPLUS";
#
#       server {
#               listen     localhost:110;
#               protocol   pop3;
#               proxy      on;
#       }
#
#       server {
#               listen     localhost:143;
#               protocol   imap;
#               proxy      on;
#       }
#}
````
***

#### 8. Откройте в браузере на хосте https адрес страницы, которую обслуживает сервер nginx.

В файл etc/hosts добавлена связка ip-домен.
````
ivan@HP-Pavilion-dv6:/etc$ cat hosts
127.0.0.1	localhost
127.0.1.1	HP-Pavilion-dv6

192.168.43.22	test.example.com

# The following lines are desirable for IPv6 capable hosts
::1     ip6-localhost ip6-loopback
fe00::0 ip6-localnet
ff00::0 ip6-mcastprefix
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters
ivan@HP-Pavilion-dv6:/etc$
````
Проверка curl -I проходит успешно.

````
ivan@HP-Pavilion-dv6:/etc$ curl -I test.example.com
HTTP/1.1 200 OK
Server: nginx/1.18.0 (Ubuntu)
Date: Fri, 25 Mar 2022 14:19:17 GMT
Content-Type: text/html
Content-Length: 612
Last-Modified: Tue, 22 Mar 2022 20:32:12 GMT
Connection: keep-alive
ETag: "623a324c-264"
Accept-Ranges: bytes
````
Страница https://test.example.com успешно открывается.

*Добавлен скриншот "Задание 8 снимок 1".*
![](https://github.com/zeninivan/devops-netology/blob/main/%D0%9A%D1%83%D1%80%D1%81%D0%BE%D0%B2%D0%B0%D1%8F%20%D1%80%D0%B0%D0%B1%D0%BE%D1%82%D0%B0%20%D1%81%D0%BA%D1%80%D0%B8%D0%BD%D1%88%D0%BE%D1%82%D1%8B/%D0%97%D0%B0%D0%B4%D0%B0%D0%BD%D0%B8%D0%B5%208%20%D1%81%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%201.png)

*Добавлен скриншот "Задание 8 снимок 2".*
![](https://github.com/zeninivan/devops-netology/blob/main/%D0%9A%D1%83%D1%80%D1%81%D0%BE%D0%B2%D0%B0%D1%8F%20%D1%80%D0%B0%D0%B1%D0%BE%D1%82%D0%B0%20%D1%81%D0%BA%D1%80%D0%B8%D0%BD%D1%88%D0%BE%D1%82%D1%8B/%D0%97%D0%B0%D0%B4%D0%B0%D0%BD%D0%B8%D0%B5%208%20%D1%81%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%202.png)

*Добавлен скриншот "Задание 8 снимок 3".*
![](https://github.com/zeninivan/devops-netology/blob/main/%D0%9A%D1%83%D1%80%D1%81%D0%BE%D0%B2%D0%B0%D1%8F%20%D1%80%D0%B0%D0%B1%D0%BE%D1%82%D0%B0%20%D1%81%D0%BA%D1%80%D0%B8%D0%BD%D1%88%D0%BE%D1%82%D1%8B/%D0%97%D0%B0%D0%B4%D0%B0%D0%BD%D0%B8%D0%B5%208%20%D1%81%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%203.png)
***

#### 9. Создайте скрипт, который будет генерировать новый сертификат в vault:

* генерируем новый сертификат так, чтобы не переписывать конфиг nginx;
* перезапускаем nginx для применения нового сертификата.

Создан скрипт MyScript_CA.sh
````
vagrant@vagrant:~$ cat MyScript_CA.sh
vault secrets enable pki
vault secrets tune -max-lease-ttl=720h pki
vault write -format=json pki/root/generate/internal common_name="example.com" ttl=720h > CA_cert.json
cat CA_cert.json | jq -r .data.certificate > rootCA.pem
vault write pki/config/urls issuing_certificates="$VAULT_ADDR/v1/pki/ca" crl_distribution_points="$VAULT_ADDR/v1/pki/crl"
vault secrets enable -path=pki_int pki
vault secrets tune -max-lease-ttl=720h pki_int
vault write -format=json pki_int/intermediate/generate/internal common_name="example.com Intermediate Authority" | jq -r '.data.csr' > pki_intermediate.csr
vault write -format=json pki/root/sign-intermediate csr=@pki_intermediate.csr format=pem_bundle ttl="720h" | jq -r '.data.certificate' > intermediate.cert.pem
vault write pki_int/intermediate/set-signed certificate=@intermediate.cert.pem
vault write pki_int/roles/example-dot-com allowed_domains="example.com" allow_subdomains=true max_ttl="720h"
vault write -format=json pki_int/issue/example-dot-com common_name="test.example.com" ttl="168h" > test.example.com.crt
cat test.example.com.crt | jq -r .data.certificate > test.example.com.crt.pem
cat test.example.com.crt | jq -r .data.ca_chain[] >> test.example.com.crt.pem
cat test.example.com.crt | jq -r .data.private_key > test.example.com.crt.key
sudo service nginx restart
vagrant@vagrant:~$ 
````
Скрипт успешно выполняется.

*Добавлен скриншот "Задание 9 снимок 1".*
![](https://github.com/zeninivan/devops-netology/blob/main/%D0%9A%D1%83%D1%80%D1%81%D0%BE%D0%B2%D0%B0%D1%8F%20%D1%80%D0%B0%D0%B1%D0%BE%D1%82%D0%B0%20%D1%81%D0%BA%D1%80%D0%B8%D0%BD%D1%88%D0%BE%D1%82%D1%8B/%D0%97%D0%B0%D0%B4%D0%B0%D0%BD%D0%B8%D0%B5%209%20%D1%81%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%201.png)

*Добавлен скриншот "Задание 9 снимок 2".*
![](https://github.com/zeninivan/devops-netology/blob/main/%D0%9A%D1%83%D1%80%D1%81%D0%BE%D0%B2%D0%B0%D1%8F%20%D1%80%D0%B0%D0%B1%D0%BE%D1%82%D0%B0%20%D1%81%D0%BA%D1%80%D0%B8%D0%BD%D1%88%D0%BE%D1%82%D1%8B/%D0%97%D0%B0%D0%B4%D0%B0%D0%BD%D0%B8%D0%B5%209%20%D1%81%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%202.png)

***
#### 10. Поместите скрипт в crontab, чтобы сертификат обновлялся какого-то числа каждого месяца в удобное для вас время.

В crontab был настроен тестовый запуск скрипта на указанное время. Также настроен сбор лога по выполнению скрипта:
````
vagrant@vagrant:~$ crontab -l
05 14 * * * /home/vagrant/MyScript_CA.sh >> /home/vagrant/MyScript_CA.log 2>&1
````
В назначенное время скрипт запустился, лог сформировался.
````
vagrant@vagrant:~$ cat MyScript_CA.log
/home/vagrant/MyScript_CA.sh: 1: vault: not found
/home/vagrant/MyScript_CA.sh: 2: vault: not found
/home/vagrant/MyScript_CA.sh: 3: vault: not found
/home/vagrant/MyScript_CA.sh: 5: vault: not found
/home/vagrant/MyScript_CA.sh: 6: vault: not found
/home/vagrant/MyScript_CA.sh: 7: vault: not found
/home/vagrant/MyScript_CA.sh: 8: vault: not found
/home/vagrant/MyScript_CA.sh: 9: vault: not found
/home/vagrant/MyScript_CA.sh: 10: vault: not found
/home/vagrant/MyScript_CA.sh: 11: vault: not found
/home/vagrant/MyScript_CA.sh: 12: vault: not found
sudo: service: command not found
vagrant@vagrant:~$
````
***









