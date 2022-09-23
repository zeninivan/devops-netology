# Домашнее задание к занятию "6.5. Elasticsearch"

## Задача 1
Используя докер образ centos:7 как базовый и документацию по установке и запуску Elastcisearch:

    составьте Dockerfile-манифест для elasticsearch
    соберите docker-образ и сделайте push в ваш docker.io репозиторий
    запустите контейнер из получившегося образа и выполните запрос пути / c хост-машины

Требования к elasticsearch.yml:

    данные path должны сохраняться в /var/lib
    имя ноды должно быть netology_test

В ответе приведите:

    текст Dockerfile манифеста
    ссылку на образ в репозитории dockerhub
    ответ elasticsearch на запрос пути / в json виде

### Решение:

Dockerfile-манифест:
````
ivan@HP-Pavilion-dv6:~/Elasticsearch_DZ$ cat Dockerfile
FROM centos:7

RUN yum install -y wget &&\
    yum clean all

WORKDIR /usr/share/elasticsearch

COPY ./elasticsearch-7.17.0-linux-x86_64.tar.gz /usr/share/elasticsearch/
RUN tar -xzf elasticsearch-7.17.0-linux-x86_64.tar.gz && \
    cp -rp elasticsearch-7.17.0/* ./ && \
    rm -rf elasticsearch-7.17.0*
COPY ./elasticsearch.yml /usr/share/elasticsearch/config/elasticsearch.yml

RUN groupadd elasticsearch && \
    useradd elasticsearch -g elasticsearch -p elasticsearch && \
    chown -R elasticsearch:elasticsearch /usr/share/elasticsearch

RUN mkdir /var/lib/elasticsearch && \
    chown -R elasticsearch:elasticsearch /var/lib/elasticsearch && \
    mkdir /var/log/elasticsearch && \
    chown -R elasticsearch:elasticsearch /var/log/elasticsearch

USER elasticsearch

EXPOSE 9200

CMD ["bin/elasticsearch"]
````

Конфиг elasticsearch.yml:

````
ivan@HP-Pavilion-dv6:~/Elasticsearch_DZ$ cat elasticsearch.yml
path.data: /var/lib/elasticsearch
path.logs: /var/log/elasticsearch
node.name: netology_test
cluster.name: "netology_test"
network.host: 127.0.0.1
transport.host: 127.0.0.1
http.host: 0.0.0.0
http.port: "9200"
discovery.type: single-node
````
Cоберите docker-образ и сделайте push в ваш docker.io репозиторий. Запустите контейнер из получившегося образа и выполните запрос пути / c хост-машины
````
docker build -t elasticsearch:v1 .
````
*Добавлен скриншот "DZ_6_5_Task_1_snapshot_1.png".*
![](https://github.com/zeninivan/devops-netology/blob/main/HomeTasks/Virtualization/%8.png)

Запускаем контейнер:

docker run -d -p 9200:9200 de07ba7798dc

curl -X GET 'http://localhost:9200/'

*Добавлен скриншот "DZ_6_5_Task_1_snapshot_2.png".*
![](https://github.com/zeninivan/devops-netology/blob/main/HomeTasks/Virtualization/%8.png)

---

## Задача 2
Добавьте в elasticsearch 3 индекса, в соответствии со таблицей. олучите список индексов и их статусов, используя API и приведите в ответе на задание.
Получите состояние кластера elasticsearch, используя API. Как вы думаете, почему часть индексов и кластер находится в состоянии yellow? Удалите все индексы.

### Решение:

Запускаем контейнер:

docker run -d -p 9200:9200 7417fac0d6a3

Добавление индексов:

curl -X PUT localhost:9200/ind-1 -H 'Content-Type: application/json' -d'{ "settings": { "number_of_shards": 1,  "number_of_replicas": 0 }}'

curl -X PUT localhost:9200/ind-2 -H 'Content-Type: application/json' -d'{ "settings": { "number_of_shards": 2,  "number_of_replicas": 1 }}'

curl -X PUT localhost:9200/ind-3 -H 'Content-Type: application/json' -d'{ "settings": { "number_of_shards": 4,  "number_of_replicas": 2 }}' 

Получите список индексов и их статусов:

````
ivan@HP-Pavilion-dv6:~$ curl -X GET 'http://localhost:9200/_cat/indices?v'
health status index            			uuid                   	pri rep  docs.count   docs.deleted  store.size  pri.store.size
green  open   .geoip_databases                  x7g-wDB_QbyTm9B1zgmP8g   1   0       41             0         38.8mb         38.8mb
green  open   ind-1            			gNECcwmpR6aqHspG2cmMaA   1   0       0              0          226b           226b
yellow open   ind-3            			ZyQr5LgoQMaZSFNoTPh8cQ   4   2       0              0          904b           904b
yellow open   ind-2            			BnoRpb6sQr6kof92D8cDjA   2   1       0              0          452b           452b
ivan@HP-Pavilion-dv6:~$
````
````
ivan@HP-Pavilion-dv6:~$ curl -X GET 'http://localhost:9200/_cluster/health/ind-1?pretty'
{
  "cluster_name" : "netology_test",
  "status" : "green",
  "timed_out" : false,
  "number_of_nodes" : 1,
  "number_of_data_nodes" : 1,
  "active_primary_shards" : 1,
  "active_shards" : 1,
  "relocating_shards" : 0,
  "initializing_shards" : 0,
  "unassigned_shards" : 0,
  "delayed_unassigned_shards" : 0,
  "number_of_pending_tasks" : 0,
  "number_of_in_flight_fetch" : 0,
  "task_max_waiting_in_queue_millis" : 0,
  "active_shards_percent_as_number" : 100.0
}
ivan@HP-Pavilion-dv6:~$ curl -X GET 'http://localhost:9200/_cluster/health/ind-2?pretty'
{
  "cluster_name" : "netology_test",
  "status" : "yellow",
  "timed_out" : false,
  "number_of_nodes" : 1,
  "number_of_data_nodes" : 1,
  "active_primary_shards" : 2,
  "active_shards" : 2,
  "relocating_shards" : 0,
  "initializing_shards" : 0,
  "unassigned_shards" : 2,
  "delayed_unassigned_shards" : 0,
  "number_of_pending_tasks" : 0,
  "number_of_in_flight_fetch" : 0,
  "task_max_waiting_in_queue_millis" : 0,
  "active_shards_percent_as_number" : 50.0
}
ivan@HP-Pavilion-dv6:~$ curl -X GET 'http://localhost:9200/_cluster/health/ind-3?pretty'
{
  "cluster_name" : "netology_test",
  "status" : "yellow",
  "timed_out" : false,
  "number_of_nodes" : 1,
  "number_of_data_nodes" : 1,
  "active_primary_shards" : 4,
  "active_shards" : 4,
  "relocating_shards" : 0,
  "initializing_shards" : 0,
  "unassigned_shards" : 8,
  "delayed_unassigned_shards" : 0,
  "number_of_pending_tasks" : 0,
  "number_of_in_flight_fetch" : 0,
  "task_max_waiting_in_queue_millis" : 0,
  "active_shards_percent_as_number" : 50.0
}
ivan@HP-Pavilion-dv6:~$
````
Получите состояние кластера elasticsearch, используя API:
````
ivan@HP-Pavilion-dv6:~$ curl -XGET localhost:9200/_cluster/health/?pretty=true
{
  "cluster_name" : "netology_test",
  "status" : "yellow",
  "timed_out" : false,
  "number_of_nodes" : 1,
  "number_of_data_nodes" : 1,
  "active_primary_shards" : 10,
  "active_shards" : 10,
  "relocating_shards" : 0,
  "initializing_shards" : 0,
  "unassigned_shards" : 10,
  "delayed_unassigned_shards" : 0,
  "number_of_pending_tasks" : 0,
  "number_of_in_flight_fetch" : 0,
  "task_max_waiting_in_queue_millis" : 0,
  "active_shards_percent_as_number" : 50.0
}
ivan@HP-Pavilion-dv6:~$
````
Как вы думаете, почему часть индексов и кластер находится в состоянии yellow?

Ответ:

    Индексы 2 и 3 находятся в статусе Yellow, т.к. у них значения "unassigned_shards" отличны от 0. Также для индексов 2 и 3 указано количество реплик, отличных от 0, а по факту мы имеем 1-нодовый кластер. 
    Поэтому состояние кластера - yellow.

Удалите все индексы:
````
ivan@HP-Pavilion-dv6:~$ curl -X DELETE 'http://localhost:9200/ind-1?pretty'
{
  "acknowledged" : true
}
ivan@HP-Pavilion-dv6:~$ curl -X DELETE 'http://localhost:9200/ind-2?pretty'
{
  "acknowledged" : true
}
ivan@HP-Pavilion-dv6:~$ curl -X DELETE 'http://localhost:9200/ind-3?pretty'
{
  "acknowledged" : true
}
ivan@HP-Pavilion-dv6:~$ curl -X GET 'http://localhost:9200/_cat/indices?v'
health status index            uuid                   pri rep docs.count docs.deleted store.size pri.store.size
green  open   .geoip_databases x7g-wDB_QbyTm9B1zgmP8g   1   0         41            0     38.8mb         38.8mb
ivan@HP-Pavilion-dv6:~$
````
---

## Задача 3
Создайте директорию {путь до корневой директории с elasticsearch в образе}/snapshots. Используя API зарегистрируйте данную директорию как snapshot repository c именем netology_backup. Приведите в ответе запрос API и результат вызова API для создания репозитория. Создайте индекс test с 0 реплик и 1 шардом и приведите в ответе список индексов. Создайте snapshot состояния кластера elasticsearch. Приведите в ответе список файлов в директории со snapshotами. Удалите индекс test и создайте индекс test-2. Приведите в ответе список индексов. Восстановите состояние кластера elasticsearch из snapshot, созданного ранее. Приведите в ответе запрос к API восстановления и итоговый список индексов.

Редактируем Dockerfile-манифест:

````
ivan@HP-Pavilion-dv6:~/Elasticsearch_DZ$ cat Dockerfile
FROM zeninivan/elasticsearch:v1

USER root

RUN mkdir /usr/share/elasticsearch/snapshots && \
         chown -R elasticsearch:elasticsearch /usr/share/elasticsearch/snapshots && \
         echo "path.repo: /usr/share/elasticsearch/snapshots" >> /usr/share/elasticsearch/config/elasticsearch.yml

USER elasticsearch
````

Собираем новый образ: 

 docker build -t elasticsearch:v2 .
 
 ````
 ivan@HP-Pavilion-dv6:~/Elasticsearch_DZ$ docker build -t elasticsearch:v2 .
Sending build context to Docker daemon  311.4MB
Step 1/4 : FROM zeninivan/elasticsearch:v1
 ---> 7417fac0d6a3
Step 2/4 : USER root
 ---> Running in 4c8e7a646dcb
Removing intermediate container 4c8e7a646dcb
 ---> 2ce1bb9a1201
Step 3/4 : RUN mkdir /usr/share/elasticsearch/snapshots &&          chown -R elasticsearch:elasticsearch /usr/share/elasticsearch/snapshots &&          echo "path.repo: /usr/share/elasticsearch/snapshots" >> /usr/share/elasticsearch/config/elasticsearch.yml
 ---> Running in 157567205bd3
Removing intermediate container 157567205bd3
 ---> d50f919352bd
Step 4/4 : USER elasticsearch
 ---> Running in 1ef58206be11
Removing intermediate container 1ef58206be11
 ---> e904440e0f31
Successfully built e904440e0f31
Successfully tagged elasticsearch:v2
ivan@HP-Pavilion-dv6:~/Elasticsearch_DZ$ 
````

*Добавлен скриншот "DZ_6_5_Task_3_snapshot_1.png".*
![](https://github.com/zeninivan/devops-netology/blob/main/HomeTasks/Virtualization/%8.png)

Запускаем контейнер:

docker run -d -p 9200:9200 e904440e0f31

Используя API зарегистрируйте данную директорию как snapshot repository c именем netology_backup.
Приведите в ответе запрос API и результат вызова API для создания репозитория.

curl -XPUT http://localhost:9200/_snapshot/netology_backup?pretty -H 'content-type: application/json' -d'{ "type": "fs", "settings": { "location": "/usr/share/elasticsearch/snapshots"}}'

````
ivan@HP-Pavilion-dv6:~/Elasticsearch_DZ$ curl -XPUT http://localhost:9200/_snapshot/netology_backup?pretty -H 'content-type: application/json' -d'{ "type": "fs", "settings": { "location": "/usr/share/elasticsearch/snapshots"}}'
{
  "acknowledged" : true
}
ivan@HP-Pavilion-dv6:~/Elasticsearch_DZ$ 
````

curl -XGET http://localhost:9200/_snapshot/netology_backup?pretty

````
ivan@HP-Pavilion-dv6:~/Elasticsearch_DZ$ curl -XGET http://localhost:9200/_snapshot/netology_backup?pretty
{
  "netology_backup" : {
    "type" : "fs",
    "settings" : {
      "location" : "/usr/share/elasticsearch/snapshots"
    }
  }
}
ivan@HP-Pavilion-dv6:~/Elasticsearch_DZ$ 
````

Создайте индекс test с 0 реплик и 1 шардом и приведите в ответе список индексов.

curl -XPUT http://localhost:9200/test -H 'Content-Type: application/json' -d'{ "settings": { "number_of_shards": 1,  "number_of_replicas": 0 }}'
curl -XGET 'http://localhost:9200/_cat/indices?v'

````
ivan@HP-Pavilion-dv6:~/Elasticsearch_DZ$ curl -XPUT http://localhost:9200/test -H 'Content-Type: application/json' -d'{ "settings": { "number_of_shards": 1,  "number_of_replicas": 0 }}'
{"acknowledged":true,"shards_acknowledged":true,"index":"test"}ivan@HP-Pavilion-dv6:~/Elasticsearch_DZ$ 
ivan@HP-Pavilion-dv6:~/Elasticsearch_DZ$ curl -XGET 'http://localhost:9200/_cat/indices?v'
health status index            uuid                   pri rep docs.count docs.deleted store.size pri.store.size
green  open   .geoip_databases j2jiey-tSImqYCwSwl0zzw   1   0         41            0     38.8mb         38.8mb
green  open   test             h899f-8lSrOdRqyR74ZcUg   1   0          0            0       226b           226b
ivan@HP-Pavilion-dv6:~/Elasticsearch_DZ$
````

Создайте snapshot состояния кластера elasticsearch.

curl -XPUT http://localhost:9200/_snapshot/netology_backup/first_snapshot?wait_for_completion=true
````
ivan@HP-Pavilion-dv6:~/Elasticsearch_DZ$ curl -XPUT http://localhost:9200/_snapshot/netology_backup/first_snapshot?wait_for_completion=true
{"snapshot":{"snapshot":"first_snapshot","uuid":"Ajfzr_xzSnK3vLx9uUBz7Q","repository":"netology_backup","version_id":7170099,"version":"7.17.0","indices":[".ds-.logs-deprecation.elasticsearch-default-2022.09.23-000001",".geoip_databases",".ds-ilm-history-5-2022.09.23-000001","test"],"data_streams":["ilm-history-5",".logs-deprecation.elasticsearch-default"],"include_global_state":true,"state":"SUCCESS","start_time":"2022-09-23T16:55:25.568Z","start_time_in_millis":1663952125568,"end_time":"2022-09-23T16:55:26.969Z","end_time_in_millis":1663952126969,"duration_in_millis":1401,"failures":[],"shards":{"total":4,"failed":0,"successful":4},"feature_states":[{"feature_name":"geoip","indices":[".geoip_databases"]}]}}ivan@HP-Pavilion-dv6:~/Elasticsearch_DZ$ 
````

curl -XGET 'http://localhost:9200/_snapshot?pretty'
````
ivan@HP-Pavilion-dv6:~/Elasticsearch_DZ$ curl -XGET 'http://localhost:9200/_snapshot?pretty'
{
  "netology_backup" : {
    "type" : "fs",
    "uuid" : "HnUg62RtROeBN90_Fo0eGw",
    "settings" : {
      "location" : "/usr/share/elasticsearch/snapshots"
    }
  }
}
ivan@HP-Pavilion-dv6:~/Elasticsearch_DZ$ 
````

Приведите в ответе список файлов в директории со snapshotами.

Подключаемся к контейнеру:

docker exec -it 465b38b30713 bash
````
ivan@HP-Pavilion-dv6:~/Elasticsearch_DZ$ docker exec -it 465b38b30713 bash
[elasticsearch@465b38b30713 elasticsearch]$ ll /usr/share/elasticsearch/snapshots
total 48
-rw-r--r-- 1 elasticsearch elasticsearch  1426 Sep 23 16:55 index-0
-rw-r--r-- 1 elasticsearch elasticsearch     8 Sep 23 16:55 index.latest
drwxr-xr-x 6 elasticsearch elasticsearch  4096 Sep 23 16:55 indices
-rw-r--r-- 1 elasticsearch elasticsearch 29222 Sep 23 16:55 meta-Ajfzr_xzSnK3vLx9uUBz7Q.dat
-rw-r--r-- 1 elasticsearch elasticsearch   713 Sep 23 16:55 snap-Ajfzr_xzSnK3vLx9uUBz7Q.dat
[elasticsearch@465b38b30713 elasticsearch]$
````

Удалите индекс test и создайте индекс test-2. Приведите в ответе список индексов.

curl -X DELETE 'http://localhost:9200/test?pretty'

curl -X PUT localhost:9200/test-2?pretty -H 'Content-Type: application/json' -d'{ "settings": { "number_of_shards": 1,  "number_of_replicas": 0 }}'

curl -XGET 'http://localhost:9200/_cat/indices?v'
````
[elasticsearch@465b38b30713 elasticsearch]$ curl -X DELETE 'http://localhost:9200/test?pretty'
{
  "acknowledged" : true
}
[elasticsearch@465b38b30713 elasticsearch]$ curl -X PUT localhost:9200/test-2?pretty -H 'Content-Type: application/json' -d'{ "settings": { "number_of_shards": 1,  "number_of_replicas": 0 }}'
{
  "acknowledged" : true,
  "shards_acknowledged" : true,
  "index" : "test-2"
}
[elasticsearch@465b38b30713 elasticsearch]$ curl -XGET 'http://localhost:9200/_cat/indices?v'
health status index            uuid                   pri rep docs.count docs.deleted store.size pri.store.size
green  open   .geoip_databases j2jiey-tSImqYCwSwl0zzw   1   0         41            0     38.8mb         38.8mb
green  open   test-2           oa2sNqC2SB6Dh0BgQlWSzg   1   0          0            0       226b           226b
[elasticsearch@465b38b30713 elasticsearch]$
````

Восстановите состояние кластера elasticsearch из snapshot, созданного ранее.

curl -X POST localhost:9200/_snapshot/netology_backup/first_snapshot_restore?pretty -H 'Content-Type: application/json' -d'{"include_global_state":true}'

````
[elasticsearch@465b38b30713 elasticsearch]$ curl -X POST localhost:9200/_snapshot/netology_backup/first_snapshot_restore?pretty -H 'Content-Type: application/json' -d'{"include_global_state":true}'
{
  "accepted" : true
}
[elasticsearch@465b38b30713 elasticsearch]$
````

Приведите в ответе запрос к API восстановления и итоговый список индексов.

curl -XGET 'http://localhost:9200/_cat/indices?v'
````
[elasticsearch@465b38b30713 elasticsearch]$ curl -XGET 'http://localhost:9200/_cat/indices?v'
health status index            uuid                   pri rep docs.count docs.deleted store.size pri.store.size
green  open   .geoip_databases j2jiey-tSImqYCwSwl0zzw   1   0         41            0     38.8mb         38.8mb
green  open   test-2           oa2sNqC2SB6Dh0BgQlWSzg   1   0          0            0       226b           226b
[elasticsearch@465b38b30713 elasticsearch]$
---
