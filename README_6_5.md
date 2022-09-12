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
ivan@HP-Pavilion-dv6:~/.docker$ cat Dockerfile
FROM centos:7

RUN yum install -y wget
RUN yum install -y java-11-openjdk wget curl perl-Digest-SHA

RUN wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-7.17.3-linux-x86_64.tar.gz

RUN mkdir /usr/elasticsearch && \
    mv elasticsearch-7.17.3-linux-x86_64.tar.gz /usr/elasticsearch && \
    cd /usr/elasticsearch && \
    tar -xzf elasticsearch-7.17.3-linux-x86_64.tar.gz

COPY elasticsearch.yml /usr/elasticsearch/elasticsearch-7.17.3/config/elasticsearch.yml

RUN groupadd elasticsearch && \
    useradd elasticsearch -g elasticsearch -p elasticsearch && \
    cd /opt && \
    chown -R elasticsearch:elasticsearch /usr/elasticsearch

RUN mkdir /var/lib/elasticsearch_data && \
    chown -R elasticsearch:elasticsearch /var/lib/elasticsearch_data && \
    mkdir /var/lib/elasticsearch_logs && \
    chown -R elasticsearch:elasticsearch /var/lib/elasticsearch_logs

USER elasticsearch

EXPOSE 9200

CMD ["/usr/elasticsearch/elasticsearch-7.17.3/bin/elasticsearch"]
````

К сожалению, при попытке создания образа получаю следующую ошибку:

````
ivan@HP-Pavilion-dv6:~/.docker$ docker build -t netology/elastic:v.1 .
Sending build context to Docker daemon  3.584kB
Step 1/11 : FROM centos:7
 ---> eeb6ee3f44bd
...
...
Complete!
Removing intermediate container 3c4904291d70
 ---> f60f7065fc63
Step 4/11 : RUN wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-7.17.3-linux-x86_64.tar.gz
 ---> Running in 214c1bd9e04a
--2022-09-12 18:49:51--  https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-7.17.3-linux-x86_64.tar.gz
Resolving artifacts.elastic.co (artifacts.elastic.co)... 34.120.127.130, 2600:1901:0:1d7::
Connecting to artifacts.elastic.co (artifacts.elastic.co)|34.120.127.130|:443... connected.
HTTP request sent, awaiting response... 403 Forbidden
2022-09-12 18:49:51 ERROR 403: Forbidden.
````

При попытке использовать VPN получаю другую ошибку:
````
ivan@HP-Pavilion-dv6:~/.docker$ docker build -t netology/elst:v1 .
Sending build context to Docker daemon  3.584kB
Step 1/11 : FROM centos:7
 ---> eeb6ee3f44bd
Step 2/11 : RUN yum install -y wget
 ---> Running in a0162ba3377a
Loaded plugins: fastestmirror, ovl
Determining fastest mirrors
Could not retrieve mirrorlist http://mirrorlist.centos.org/?release=7&arch=x86_64&repo=os&infra=container error was
14: curl#6 - "Could not resolve host: mirrorlist.centos.org; Unknown error"


 One of the configured repositories failed (Unknown),
 and yum doesn't have enough cached data to continue. At this point the only
 safe thing yum can do is fail. There are a few ways to work "fix" this:

     1. Contact the upstream for the repository and get them to fix the problem.

     2. Reconfigure the baseurl/etc. for the repository, to point to a working
        upstream. This is most often useful if you are using a newer
        distribution release than is supported by the repository (and the
        packages for the previous distribution release still work).

     3. Run the command with the repository temporarily disabled
            yum --disablerepo=<repoid> ...

     4. Disable the repository permanently, so yum won't use it by default. Yum
        will then just ignore the repository until you permanently enable it
        again or use --enablerepo for temporary usage:

            yum-config-manager --disable <repoid>
        or
            subscription-manager repos --disable=<repoid>

     5. Configure the failing repository to be skipped, if it is unavailable.
        Note that yum will try to contact the repo. when it runs most commands,
        so will have to try and fail each time (and thus. yum will be be much
        slower). If it is a very temporary problem though, this is often a nice
        compromise:

            yum-config-manager --save --setopt=<repoid>.skip_if_unavailable=true

Cannot find a valid baseurl for repo: base/7/x86_64
The command '/bin/sh -c yum install -y wget' returned a non-zero code: 1
````

Думаю, что надо использовать скачанный tar c elasticsearch, но не успеваю разобраться, как переделать Dockerfile 
под локальную версию архива elasticsearch.

Продлите, пожалуйста, время на доработку данного Д.З. 


---
