# Домашнее задание к занятию "5.3. Введение. Экосистема. Архитектура. Жизненный цикл Docker контейнера"

## Задача 1 
Сценарий выполения задачи:

создайте свой репозиторий на https://hub.docker.com;

выберете любой образ, который содержит веб-сервер Nginx;

создайте свой fork образа;

реализуйте функциональность: запуск веб-сервера в фоне с индекс-страницей, содержащей HTML-код ниже:

````
<html>
<head>
Hey, Netology
</head>
<body>
<h1>I’m DevOps Engineer!</h1>
</body>
</html>
````

Опубликуйте созданный форк в своем репозитории и предоставьте ответ в виде ссылки на https://hub.docker.com/username_repo.

### Ответ:
docker pull zeninivan/dev-netology1:first

````
ivan@HP-Pavilion-dv6:~$ docker images
REPOSITORY                TAG       IMAGE ID       CREATED         SIZE
zeninivan/dev-netology1   first     d90ffaae6e4a   8 seconds ago   196MB
nginx                     1.1       c706afe8b260   3 hours ago     196MB
nginx                     latest    0e901e68141f   6 days ago      142MB

ivan@HP-Pavilion-dv6:~$ docker ps
CONTAINER ID   IMAGE          COMMAND                  CREATED       STATUS       PORTS                               NAMES
e48cc72fc3f1   c706afe8b260   "/docker-entrypoint.…"   2 hours ago   Up 2 hours   0.0.0.0:80->80/tcp, :::80->80/tcp   gifted_swirles

ivan@HP-Pavilion-dv6:~$ docker exec -it e48cc72fc3f1 bash

root@e48cc72fc3f1:/etc/nginx# cd /usr/share/nginx/html
root@e48cc72fc3f1:/usr/share/nginx/html# cat index.html
<!DOCTYPE html>
<html>
<head>
Hey, Netology!
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>I'm DevOps Engineer!</h1>
</body>
</html>
root@e48cc72fc3f1:/usr/share/nginx/html# 
````

*Добавлен скриншот "Задание 1 снимок 1".*
![](https://github.com/zeninivan/devops-netology/blob/main/05-virt-03-docker-usage/Task_1_2022-06-03.png)

---

## Задача 2
Посмотрите на сценарий ниже и ответьте на вопрос: "Подходит ли в этом сценарии использование Docker контейнеров или лучше подойдет виртуальная машина, физическая машина? Может быть возможны разные варианты?"

Детально опишите и обоснуйте свой выбор.

Сценарий:

    Высоконагруженное монолитное java веб-приложение;
    Nodejs веб-приложение;
    Мобильное приложение c версиями для Android и iOS;
    Шина данных на базе Apache Kafka;
    Elasticsearch кластер для реализации логирования продуктивного веб-приложения - три ноды elasticsearch, два logstash и две ноды kibana;
    Мониторинг-стек на базе Prometheus и Grafana;
    MongoDB, как основное хранилище данных для java-приложения;
    Gitlab сервер для реализации CI/CD процессов и приватный (закрытый) Docker Registry.

### Ответ:
Высоконагруженное монолитное java веб-приложение:

	Монолитное веб-приложение представляет собой приложение, доставляемое через единое развертывание. В монолите все части находятся в одном и том же модуле (user interface, business logic, data access layer). 
	В монолите практически нет изоляции, все компоненты растут и меняются вместе. Исходя из архитектуры монолита, для его развертывания подойдет физический сервер либо виртуализация. Учитывая высоконагруженность, я бы 
	остановился на физической машине.

Nodejs веб-приложение:

	Node.js — это среда выполнения JavaScript, которую можно создать как на контейнерах, так и на виртуальных машинах, специально предназначенных для разработки быстрых и масштабируемых сетевых приложений.
	Учитывая современные тенденции и приемущества контейнеров в плане масштабируемости и потребления ресурсов системы, я бы выбрал контейнеры. 
	
Мобильное приложение c версиями для Android и iOS;

	В данном случае лучше всего подойдут виртуальные машины. Их проще и менее затратно эксплуатировать и будет в наличии GUI. 
	
Шина данных на базе Apache Kafka;

    Kafka позволяет с легкостью разграничить коммуникацию между различными микросервисами. С учетом огромного объема событий/запросов, которые Kafka может обрабатывать, будет возникать потребность в ее масштабировании.
    Я думаю контейнеры в данном случае будут более выигрышны.
	
Elasticsearch кластер для реализации логирования продуктивного веб-приложения - три ноды elasticsearch, два logstash и две ноды kibana;

	Elasticsearch можно установить на VM, отказоустойчивость решается на уровне кластера, logstash и kibana можно разместить как на VM, так и на контейнерах. 

Мониторинг-стек на базе Prometheus и Grafana;

	Prometheus получает метрики из разных сервисов и собирает их в одном месте. Grafana отображает данные из Prometheus в виде графиков и диаграмм, организованных в дашборды.
	Думаю здесь нет ограничений ни для VM, ни для контейнеров. Вопрос удобства и налчия ресурсов для эксплуатации той или иной системы.
	
MongoDB, как основное хранилище данных для java-приложения;

	Как основное хранилище данных я бы выбрал физический сервер, если позволяют финансы. На второе место поставлю VM.
	
Gitlab сервер для реализации CI/CD процессов и приватный (закрытый) Docker Registry:

	В данном случае подойдет контейнеризация. GitLab как сторона сервера, контролирует сторону Runner для выполнения серии задач CI.
	Исходя из предположения, что Runner использует сборку Docker, все переключения зависимостей и переключения среды осуществляются путем 
	переключения разных образов, то есть сборка, а затем использование образа сборки. Полная изоляция окружающей среды через контейнеры с образами.
	Различные задачи CI фактически выполняются в контейнерах с использованием образов.
	Вся важная конфиденциальная информация, такая как пароли учетных записей и т. д., не должна записываться в конфигурацию CI и помещаться непосредственно в переменные среды GitLab.
---

## Задача 3

Запустите первый контейнер из образа centos c любым тэгом в фоновом режиме, подключив папку /data из текущей рабочей директории на хостовой машине в /data контейнера;

Запустите второй контейнер из образа debian в фоновом режиме, подключив папку /data из текущей рабочей директории на хостовой машине в /data контейнера;

Подключитесь к первому контейнеру с помощью docker exec и создайте текстовый файл любого содержания в /data;

Добавьте еще один файл в папку /data на хостовой машине;

Подключитесь во второй контейнер и отобразите листинг и содержание файлов в /data контейнера.

### Ответ:

Проверяем наличие образов debian и centos
````
ivan@HP-Pavilion-dv6:~$ docker images
REPOSITORY                TAG       IMAGE ID       CREATED        SIZE
zeninivan/dev-netology1   first     d90ffaae6e4a   8 days ago     196MB
nginx                     latest    0e901e68141f   2 weeks ago    142MB
debian                    latest    4eacea30377a   2 weeks ago    124MB
centos                    latest    5d0da3dc9764   8 months ago   231MB
````

Создаем docker volume на хосте:
````
ivan@HP-Pavilion-dv6:~/devops-netology/05-virt-03-docker-usage/data$ docker volume create my-vol
my-vol
ivan@HP-Pavilion-dv6:~/devops-netology/05-virt-03-docker-usage/data$ docker volume ls
DRIVER    VOLUME NAME
local     my-vol
ivan@HP-Pavilion-dv6:~/devops-netology/05-virt-03-docker-usage/data$ docker volume inspect my-vol
[
    {
        "CreatedAt": "2022-06-11T14:50:54+03:00",
        "Driver": "local",
        "Labels": {},
        "Mountpoint": "/var/snap/docker/common/var-lib-docker/volumes/my-vol/_data",
        "Name": "my-vol",
        "Options": {},
        "Scope": "local"
    }
]
````

Запускаем первый контейнер из образа centos, подключив папку /data из хостовой машины в /data контейнера:
````
ivan@HP-Pavilion-dv6:~$ docker run -d --name my-vol -v /var/snap/docker/common/var-lib-docker/volumes/my-vol/_data:/data centos sleep infinity
17d7d965b8f0ce65321d60c1cec6f2909dc784a083e54308d2471c3dc8f81a72
ivan@HP-Pavilion-dv6:~$ docker ps
CONTAINER ID   IMAGE     COMMAND            CREATED         STATUS         PORTS     NAMES
17d7d965b8f0   centos    "sleep infinity"   7 seconds ago   Up 6 seconds             my-vol
````

Запускаем второй контейнер из образа debian, подключив папку /data из хостовой машины в /data контейнера:  
````
ivan@HP-Pavilion-dv6:~$ docker run -v /var/snap/docker/common/var-lib-docker/volumes/my-vol/_data:/data -d debian sleep infinity
62c22b53acf89e942db6cd33674bd349d85635aebbf8dacb0171049d502cba5a
````

Проверяем, что оба контейнера запущены:
````
ivan@HP-Pavilion-dv6:~$ docker ps
CONTAINER ID   IMAGE     COMMAND            CREATED          STATUS          PORTS     NAMES
62c22b53acf8   debian    "sleep infinity"   2 minutes ago    Up 2 minutes              jolly_lalande
17d7d965b8f0   centos    "sleep infinity"   10 minutes ago   Up 10 minutes             my-vol
````

Подключаемся к первому контейнеру (centos) с помощью docker exec и создаем текстовый файл в /data:
````
ivan@HP-Pavilion-dv6:~$ docker exec -it 17d7d965b8f0 bash
[root@17d7d965b8f0 /]# echo 123 > /data/testfile_centos
[root@17d7d965b8f0 /]# ls /data
testfile_centos
````

Добавляем файл в папку /data на хостовой машине:
````
ivan@HP-Pavilion-dv6:~$ sudo bash
[sudo] пароль для ivan: 
root@HP-Pavilion-dv6:/home/ivan# echo 12345 > /var/snap/docker/common/var-lib-docker/volumes/my-vol/_data/testfile_on_host_ubuntu
root@HP-Pavilion-dv6:/home/ivan# ls /var/snap/docker/common/var-lib-docker/volumes/my-vol/_data
testfile_centos  testfile_on_host_ubuntu
````

Подключаемся во второй контейнер (debian) и отображаем листинг и содержание файлов в /data контейнера:
````
ivan@HP-Pavilion-dv6:~$ docker exec -it 62c22b53acf8 bash
root@62c22b53acf8:/# cd data
root@62c22b53acf8:/data# ls -lah
total 8.0K
drwxr-xr-x 2 root root 4.0K Jun 11 11:50 .
drwxr-xr-x 1 root root 4.0K Jun 11 19:00 ..
root@62c22b53acf8:/data# ls -lah
total 12K
drwxr-xr-x 2 root root 4.0K Jun 11 19:09 .
drwxr-xr-x 1 root root 4.0K Jun 11 19:00 ..
-rw-r--r-- 1 root root    4 Jun 11 19:09 testfile_centos
root@62c22b53acf8:/data# ls -lah
total 16K
drwxr-xr-x 2 root root 4.0K Jun 11 19:12 .
drwxr-xr-x 1 root root 4.0K Jun 11 19:00 ..
-rw-r--r-- 1 root root    4 Jun 11 19:09 testfile_centos
-rw-r--r-- 1 root root    6 Jun 11 19:12 testfile_on_host_ubuntu
root@62c22b53acf8:/data# cat testfile_centos
123
root@62c22b53acf8:/data# cat testfile_on_host_ubuntu
12345
````

*Добавлен скриншот "Задание 3 снимок 1".*
![](https://github.com/zeninivan/devops-netology/blob/main/05-virt-03-docker-usage/Task_3_1_2022-06-11.png)

*Добавлен скриншот "Задание 3 снимок 2".*
![](https://github.com/zeninivan/devops-netology/blob/main/05-virt-03-docker-usage/Task_3_2_2022-06-11.png)

---

## Задача 4 (*)
Воспроизвести практическую часть лекции самостоятельно.

Соберите Docker образ с Ansible, загрузите на Docker Hub и пришлите ссылку вместе с остальными ответами к задачам.

### Ответ:

---