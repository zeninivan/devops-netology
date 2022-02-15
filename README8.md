Домашнее задание к занятию "3.6. Компьютерные сети, лекция 1"


1. Работа c HTTP через телнет.

    Подключитесь утилитой телнет к сайту stackoverflow.com telnet stackoverflow.com 80
    отправьте HTTP запрос

	GET /questions HTTP/1.0
	HOST: stackoverflow.com
	[press enter]
	[press enter]
	В ответе укажите полученный HTTP код, что он означает?

Результат:
ivan@HP-Pavilion-dv6:~$ telnet stackoverflow.com 80
Trying 151.101.1.69...
Connected to stackoverflow.com.
Escape character is '^]'.
GET /questions HTTP/1.0
HOST: stackoverflow.com

HTTP/1.1 301 Moved Permanently
cache-control: no-cache, no-store, must-revalidate
location: https://stackoverflow.com/questions
x-request-guid: ac64966d-b509-4abf-b040-34816210b4dd
feature-policy: microphone 'none'; speaker 'none'
content-security-policy: upgrade-insecure-requests; frame-ancestors 'self' https://stackexchange.com
Accept-Ranges: bytes
Date: Mon, 14 Feb 2022 14:11:45 GMT
Via: 1.1 varnish
Connection: close
X-Served-By: cache-hhn4029-HHN
X-Cache: MISS
X-Cache-Hits: 0
X-Timer: S1644847905.250695,VS0,VE156
Vary: Fastly-SSL
X-DNS-Prefetch-Control: off
Set-Cookie: prov=4e9fd2f4-6316-6d33-f9e8-fe5eb881f6df; domain=.stackoverflow.com; expires=Fri, 01-Jan-2055 00:00:00 GMT; path=/; HttpOnly
Connection closed by foreign host.
ivan@HP-Pavilion-dv6:~$

Здесь мы видим код состояния HTTP 301 или Moved Permanently (Перемещено навсегда) — стандартный код ответа HTTP, получаемый в ответ от сервера в ситуации, 
когда запрошенный ресурс был на постоянной основе перемещён в новое месторасположение, и указывающий на то, что текущие ссылки, использующие данный URL, 
должны быть обновлены. Адрес нового месторасположения ресурса указывается в поле Location получаемого в ответ заголовка пакета протокола HTTP. 

Т.е. в нашем случае новый адрес страницы:
https://stackoverflow.com/questions


2.  Повторите задание 1 в браузере, используя консоль разработчика F12.

    откройте вкладку Network
    отправьте запрос http://stackoverflow.com
    найдите первый ответ HTTP сервера, откройте вкладку Headers
    укажите в ответе полученный HTTP код.
    проверьте время загрузки страницы, какой запрос обрабатывался дольше всего?
    приложите скриншот консоли браузера в ответ.
 
 Первый ответ HTTP сервера - http://stackoverflow.com/   (добавлен скриншот) 
Состояние
301
Moved Permanently
ВерсияHTTP/1.1
Передано53,24 КБ (размер 177,19 КБ)

		Заголовки ответа:	
Accept-Ranges  			bytes
cache-control     			no-cache, no-store, must-revalidate
Connection		 			keep-alive
content-security-policy	upgrade-insecure-requests; frame-ancestors 'self' https://stackexchange.com
Date								Mon, 14 Feb 2022 15:59:30 GMT
feature-policy				microphone 'none'; speaker 'none'
location							https://stackoverflow.com/
Transfer-Encoding			chunked
Vary								Fastly-SSL
Via								1.1 varnish
X-Cache						MISS
X-Cache-Hits					0
X-DNS-Prefetch-Control	off
x-request-guid				9a1acf85-d289-4278-9d9c-a8cb5c09f3b8
X-Served-By					cache-hhn4031-HHN
X-Timer						S1644854371.641807,VS0,VE78

		Заголовки запроса:	
Accept							text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Encoding			gzip, deflate
Accept-Language			ru-RU,ru;q=0.8,en-US;q=0.5,en;q=0.3
Connection					keep-alive
Cookie							prov=b56da3ed-5d75-8181-60e4-42b32f03b737; _ga=GA1.2.943309599.1644175582
Host								stackoverflow.com
Upgrade-Insecure-Requests	1
User-Agent					Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:96.0) Gecko/20100101 Firefox/96.0

Дольше всего загружался документ html https://stackoverflow.com/ - загружено за 396 мс (добавлен скриншот)
Состояние
200
OK
ВерсияHTTP/2
Передано53,27 КБ (размер 177,19 КБ)

Скриншот приложен (Снимок 2022-02-14 упр 2).


3. Какой IP адрес у вас в интернете?

Мой ip в интернете  91.223.89.29. Информация получена с ресурса https://whoer.net/ru
Скриншот приложен (Снимок 2022-02-14 упр 3).


4. Какому провайдеру принадлежит ваш IP адрес? Какой автономной системе AS? Воспользуйтесь утилитой whois

Мой IP адрес принадлежит PAKT  - org-name:       P.A.K.T LLC
Автономная система - AS39087
origin:         AS39087

Результат вывода whois:

ivan@HP-Pavilion-dv6:~$ whois 91.223.89.29
% This is the RIPE Database query service.
% The objects are in RPSL format.
%
% The RIPE Database is subject to Terms and Conditions.
% See http://www.ripe.net/db/support/db-terms-conditions.pdf

% Note: this output has been filtered.
%       To receive output for a database update, use the "-B" flag.

% Information related to '91.223.89.0 - 91.223.89.255'

% Abuse contact for '91.223.89.0 - 91.223.89.255' is 'abuse@pakt.spb.ru'

inetnum:        91.223.89.0 - 91.223.89.255
netname:        RU-PAKT-20191111
country:        RU
org:            ORG-PL73-RIPE
admin-c:        LR3226-RIPE
tech-c:         LR3226-RIPE
status:         ALLOCATED PA
mnt-by:         PAKT-MNT
mnt-by:         RIPE-NCC-HM-MNT
created:        2021-12-09T14:08:39Z
last-modified:  2021-12-09T14:08:39Z
source:         RIPE

organisation:   ORG-PL73-RIPE
org-name:       P.A.K.T LLC
country:        RU
org-type:       LIR
address:        21A-11N, Sizova avenue
address:        197349
address:        St. Petersburg
address:        RUSSIAN FEDERATION
phone:          +78125958111
fax-no:         +78125958111
abuse-c:        AR16847-RIPE
admin-c:        LR3226-RIPE
mnt-ref:        LR-MNT
mnt-ref:        PAKT-MNT
mnt-ref:        RIPE-NCC-HM-MNT
mnt-by:         RIPE-NCC-HM-MNT
mnt-by:         PAKT-MNT
created:        2009-04-17T08:57:40Z
last-modified:  2021-12-30T08:34:25Z
source:         RIPE # Filtered

person:         Leonid Ryzhik
address:        14-2, apt 9N, Fomina street
address:        RU-194295, St. Petersburg, Russia
phone:          +7 812 5958122
nic-hdl:        LR3226-RIPE
mnt-by:         LR-MNT
created:        2007-11-04T17:11:21Z
last-modified:  2015-12-23T13:30:59Z
source:         RIPE # Filtered

% Information related to '91.223.89.0/24AS39087'

route:          91.223.89.0/24
origin:         AS39087
mnt-by:         PAKT-MNT
created:        2021-12-09T14:14:27Z
last-modified:  2021-12-09T14:14:27Z
source:         RIPE

% This query was served by the RIPE Database Query Service version 1.102.2 (ANGUS)


5. Через какие сети проходит пакет, отправленный с вашего компьютера на адрес 8.8.8.8? Через какие AS? Воспользуйтесь утилитой traceroute

С помощью traceroute видим, через какие сети и AS проходит пакет:

ivan@HP-Pavilion-dv6:~$ traceroute -An 8.8.8.8
traceroute to 8.8.8.8 (8.8.8.8), 30 hops max, 60 byte packets
 1  192.168.1.1 [*]  3.355 ms  4.246 ms  4.226 ms
 2  10.34.130.129 [*]  7.162 ms  7.154 ms  7.137 ms
 3  10.37.2.169 [*]  7.119 ms  7.106 ms  7.142 ms
 4  10.37.2.185 [*]  4.807 ms  6.032 ms  6.066 ms
 5  10.37.254.181 [*]  6.014 ms  5.997 ms  5.977 ms
 6  10.37.5.249 [*]  5.305 ms  2.764 ms  1.699 ms
 7  10.37.129.187 [*]  2.441 ms  2.949 ms  2.960 ms
 8  209.85.148.36 [AS15169]  5.677 ms  3.605 ms  5.613 ms
 9  74.125.244.132 [AS15169]  4.177 ms 74.125.244.180 [AS15169]  4.735 ms  4.744 ms
10  72.14.232.85 [AS15169]  5.442 ms  5.410 ms 216.239.48.163 [AS15169]  8.669 ms
11  216.239.62.107 [AS15169]  10.661 ms 142.251.61.221 [AS15169]  10.175 ms 142.250.208.25 [AS15169]  10.511 ms
12  * * 172.253.79.115 [AS15169]  8.586 ms
13  * * *
14  * * *
15  * * *
16  * * *
17  * * *
18  * * *
19  * * *
20  * * *
21  * * *
22  8.8.8.8 [AS15169]  10.949 ms  14.402 ms *
ivan@HP-Pavilion-dv6:~$ 
 
 
 6. Повторите задание 5 в утилите mtr. На каком участке наибольшая задержка - delay?
 
 Запустим утилиту mtr
 ivan@HP-Pavilion-dv6:~$ mtr -zn 8.8.8.8
 
 Значение отображаются в реальном времени. Наибольшая задержка Avg 12.1  на 12м прыжке - AS15169  142.250.209.35

Скриншот приложен (Снимок 2022-02-15 упр 6).
 
 
 7. Какие DNS сервера отвечают за доменное имя dns.google? Какие A записи? воспользуйтесь утилитой dig.
 
 ivan@HP-Pavilion-dv6:~$ dig +trace @8.8.8.8 dns.google
; <<>> DiG 9.16.1-Ubuntu <<>> +trace @8.8.8.8 dns.google
; (1 server found)
;; global options: +cmd
.			86431	IN	NS	g.root-servers.net.
.			86431	IN	NS	j.root-servers.net.
.			86431	IN	NS	i.root-servers.net.
.			86431	IN	NS	e.root-servers.net.
.			86431	IN	NS	a.root-servers.net.
.			86431	IN	NS	l.root-servers.net.
.			86431	IN	NS	c.root-servers.net.
.			86431	IN	NS	d.root-servers.net.
.			86431	IN	NS	m.root-servers.net.
.			86431	IN	NS	b.root-servers.net.
.			86431	IN	NS	f.root-servers.net.
.			86431	IN	NS	k.root-servers.net.
.			86431	IN	NS	h.root-servers.net.
.			86431	IN	RRSIG	NS 8 0 518400 20220228050000 20220215040000 9799 . Z09bmklPvZuQJUQ/xTmPSxDGWedNCcH1ZyC3v8+OqOyf0V7oQTu5w4bI cAHTGF4m0BVH2zFw/VVKV5MSYT22HIdl3iuB4U7aMyPmcBH1kMz4RZHC X3GWShtHUKPkDF5xA65ZvHjcy8yxbchHR4b+Q3hJCJf/s+ND1TZKQb50 4fYLt7k37TBGuQFv8gKQyvWw0j7rQVXezAS81E7jbCP3cGm5QjnYxckM LlxfIEGn/KUbThdDvvBm1scSEvwiHSX3mBVAwDnBhYQ4pwqPpI4mct4x xeXPHzXhLqJywZ2NTqyJdJfbg6Mt8b9VbcqaPMhjsPMyvc3Agauwrqf7 9QaFzA==
;; Received 525 bytes from 8.8.8.8#53(8.8.8.8) in 3 ms

google.			172800	IN	NS	ns-tld3.charlestonroadregistry.com.
google.			172800	IN	NS	ns-tld1.charlestonroadregistry.com.
google.			172800	IN	NS	ns-tld5.charlestonroadregistry.com.
google.			172800	IN	NS	ns-tld2.charlestonroadregistry.com.
google.			172800	IN	NS	ns-tld4.charlestonroadregistry.com.
google.			86400	IN	DS	6125 8 2 80F8B78D23107153578BAD3800E9543500474E5C30C29698B40A3DB2 3ED9DA9F
google.			86400	IN	RRSIG	DS 8 1 86400 20220228050000 20220215040000 9799 . cmL6YgLmSECVjBzcYl+Kr5D8pXFVis+5wJxa9EH2EldnQgOtS93HcKpF BXsGXqBmWY/p8DmOtKPRfjQ3ZEyKnEox3idU32FKwoBkKWKw79N0lJMM /FKjrbhLSB531vahUSY/P0+tPTu21f2Kp5WaI0B399YeRY1eTGIsv/M7 JQZmwXGhzRpLKT+Bc79jZtgUcPkVwbPzHjxoeoLKi9FMjNmcQqcYnPRz M8Gc2U16ZGBsuNcjjeWYoF+2HMiOcSzfi2nLc9d3sFktm2p+UOH6GKre J4YCVkjvJQL/aUKidFdW8D5kbs0oE3BPcVenhP9AxNyIMTH1blgT7Phb dHQbCA==
;; Received 730 bytes from 202.12.27.33#53(m.root-servers.net) in 67 ms

dns.google.		10800	IN	NS	ns4.zdns.google.
dns.google.		10800	IN	NS	ns3.zdns.google.
dns.google.		10800	IN	NS	ns2.zdns.google.
dns.google.		10800	IN	NS	ns1.zdns.google.
dns.google.		3600	IN	DS	56044 8 2 1B0A7E90AA6B1AC65AA5B573EFC44ABF6CB2559444251B997103D2E4 0C351B08
dns.google.		3600	IN	RRSIG	DS 8 2 3600 20220305171116 20220211171116 39106 google. Cu27ometfUKfhQzo2Ks8qNROOkbXU7YpXQlredYRmxiEBggHy37QY1TM ZhY6RjUJTvOI5LtzKbPNV87jJ8rIJfXJFcgm62qLCqK7WnddD/z2wHcO AidLBWIT7uHaQORe3+4LDGkK8rtiw5tbflqL8NcCQay8O3fjRpIwWfFH Zyo=
;; Received 506 bytes from 216.239.38.105#53(ns-tld4.charlestonroadregistry.com) in 7 ms

dns.google.		900	IN	A	8.8.8.8
dns.google.		900	IN	A	8.8.4.4
dns.google.		900	IN	RRSIG	A 8 2 900 20220307120732 20220213120732 25800 dns.google. Kap8xUB2kuHwdtW+zn+rUsoEVQgtUhFV8D9ora+4xHxhmehohTvJ1ES9 cs1oq7igHDlfRTkw9F6LIhPoNXtGrPM8EANzL2MJ6DAZOTOuqsgHYiJ1 b6whTQUAuEUB6VxQ25ESSAg51RTvxfI+52P5ufb7Z9N0FzEk0z87viUv ovQ=
;; Received 241 bytes from 216.239.34.114#53(ns2.zdns.google) in 39 ms

Корневые DNS сервера перечислены в верхней части вывода (с точкой):
g.root-servers.net.
j.root-servers.net.
i.root-servers.net.
e.root-servers.net.
a.root-servers.net.
l.root-servers.net.
c.root-servers.net.
d.root-servers.net.
m.root-servers.net.
b.root-servers.net.
f.root-servers.net.
k.root-servers.net.
h.root-servers.net.

За зону google отвечает следующая группа NS серверов:
ns-tld3.charlestonroadregistry.com.
ns-tld1.charlestonroadregistry.com.
ns-tld5.charlestonroadregistry.com.
ns-tld2.charlestonroadregistry.com.
ns-tld4.charlestonroadregistry.com.

A записи находим в нижней части вывода:
A	8.8.8.8
A	8.8.4.4


8. Проверьте PTR записи для IP адресов из задания 7. Какое доменное имя привязано к IP? воспользуйтесь утилитой dig.

ivan@HP-Pavilion-dv6:~$ dig -x 8.8.8.8

; <<>> DiG 9.16.1-Ubuntu <<>> -x 8.8.8.8
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 30859
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 65494
;; QUESTION SECTION:
;8.8.8.8.in-addr.arpa.		IN	PTR

;; ANSWER SECTION:
8.8.8.8.in-addr.arpa.	1425	IN	PTR	dns.google.

;; Query time: 47 msec
;; SERVER: 127.0.0.53#53(127.0.0.53)
;; WHEN: Вт фев 15 12:19:19 MSK 2022
;; MSG SIZE  rcvd: 73

ivan@HP-Pavilion-dv6:~$ 

В поле ANSWER SECTION видим, что к ip адресу 8.8.8.8 привязано доменное имя dns.google.
;; ANSWER SECTION:
8.8.8.8.in-addr.arpa.	1425	IN	PTR	dns.google.