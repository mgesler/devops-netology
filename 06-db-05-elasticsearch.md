# devops-netology  
Home work 06-db-05-elasticsearch

Домашнее задание к занятию "6.5. Elasticsearch"
Задача 1  
В этом задании вы потренируетесь в:

установке elasticsearch
первоначальном конфигурировании elastcisearch
запуске elasticsearch в docker
Используя докер образ centos:7 как базовый и документацию по установке и запуску Elastcisearch:

составьте Dockerfile-манифест для elasticsearch

соберите docker-образ и сделайте push в ваш docker.io репозиторий
запустите контейнер из получившегося образа и выполните запрос пути / c хост-машины
Требования к elasticsearch.yml:

данные path должны сохраняться в /var/lib
имя ноды должно быть netology_test
В ответе приведите:

текст Dockerfile манифеста
````
Т.к. репозитории elastic заблокированы с адресов РФ пришлось 
в docerfile использовать локальную копию дистрибутива. 

FROM centos:centos7
ENV PATH=/usr/lib:/usr/lib/jvm/jre-11/bin:$PATH

USER 0

RUN yum install java-11-openjdk -y
ENV JAVA_HOME=/elasticsearch-7.17.0/jdk/
ENV ES_HOME=/elasticsearch-7.17.0
COPY elasticsearch-7.17.0-linux-x86_64.tar.gz elasticsearch-7.17.0-linux-x86_64.tar.gz

RUN export ES_HOME="/var/lib/elasticsearch" && \
    tar -xzf elasticsearch-7.17.0-linux-x86_64.tar.gz && \
    rm -f elasticsearch-7.17.0-linux-x86_64.tar.gz* && \
    mv elasticsearch-7.17.0 ${ES_HOME} && \
    useradd -m -u 1000 elasticsearch && \
    chown elasticsearch:elasticsearch -R ${ES_HOME} && \
    yum install sudo && \
    yum clean all
COPY --chown=elasticsearch:elasticsearch config/* /var/lib/elasticsearch/config/

USER 1000

ENV ES_HOME="/var/lib/elasticsearch" \
    ES_PATH_CONF="/var/lib/elasticsearch/config"
WORKDIR ${ES_HOME}

ADD elasticsearch.yml /elasticsearch-7.17.0/config/

CMD ["sh", "-c", "${ES_HOME}/bin/elasticsearch"]




elasticsearch.yml файл

cluster.name: netology_test
discovery.type: single-node
node.name: netology_test
path.data: /var/lib/data
path.logs: /var/lib/logs
path.repo: /elasticsearch-7.17.0/snapshots
network.host: 0.0.0.0
http.port: 9200
discovery.seed_hosts: ["127.0.0.1", "[::1]"]
````

ссылку на образ в репозитории dockerhub
````
https://hub.docker.com/repository/docker/nollon/elastic-netology
````
ответ elasticsearch на запрос пути / в json виде
````
 curl http://127.0.0.1:9200
{
  "name" : "netology_test",
  "cluster_name" : "netology_test",
  "cluster_uuid" : "vUHDrCraQ2WbFpUAZ3ljyQ",
  "version" : {
    "number" : "7.17.0",
    "build_flavor" : "default",
    "build_type" : "tar",
    "build_hash" : "bee86328705acaa9a6daede7140defd4d9ec56bd",
    "build_date" : "2022-01-28T08:36:04.875279988Z",
    "build_snapshot" : false,
    "lucene_version" : "8.11.1",
    "minimum_wire_compatibility_version" : "6.8.0",
    "minimum_index_compatibility_version" : "6.0.0-beta1"
  },
  "tagline" : "You Know, for Search"
}


````
Подсказки:

возможно вам понадобится установка пакета perl-Digest-SHA для корректной работы пакета shasum
при сетевых проблемах внимательно изучите кластерные и сетевые настройки в elasticsearch.yml
при некоторых проблемах вам поможет docker директива ulimit
elasticsearch в логах обычно описывает проблему и пути ее решения
Далее мы будем работать с данным экземпляром elasticsearch.

Задача 2
В этом задании вы научитесь:

создавать и удалять индексы
изучать состояние кластера
обосновывать причину деградации доступности данных
Ознакомтесь с документацией и добавьте в elasticsearch 3 индекса, в соответствии со таблицей:

Имя	Количество реплик	Количество шард
ind-1	0	1
ind-2	1	2
ind-3	2	4
Получите список индексов и их статусов, используя API и приведите в ответе на задание.
````
Создание индексов:
curl -X PUT localhost:9200/ind-1 -H 'Content-Type: application/json' -d'{ "settings": { "number_of_shards": 1,  "number_of_replicas": 0 }}'
curl -X PUT localhost:9200/ind-2 -H 'Content-Type: application/json' -d'{ "settings": { "number_of_shards": 2,  "number_of_replicas": 1 }}'
curl -X PUT localhost:9200/ind-3 -H 'Content-Type: application/json' -d'{ "settings": { "number_of_shards": 4,  "number_of_replicas": 2 }}' 

````
Получите состояние кластера elasticsearch, используя API.
````
Список индексов:
 curl -X GET 'http://localhost:9200/_cat/indices?v'
health status index            uuid                   pri rep docs.count docs.deleted store.size pri.store.size
green  open   .geoip_databases UD6TjDHxS0aRV0HumqdxvQ   1   0         40            0     37.9mb         37.9mb
green  open   ind-1            U7NNRTHGTwuE0ld6HmB0ew   1   0          0            0       226b           226b
yellow open   ind-3            eJeCnCLaSdW0UcU0ebkjyQ   4   2          0            0       904b           904b
yellow open   ind-2            SHOnaDhnRa-zURyAgBjYBw   2   1          0            0       452b           452b

Статус индексов:
 curl -X GET 'http://localhost:9200/_cluster/health/ind-1?pretty'
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
curl -X GET 'http://localhost:9200/_cluster/health/ind-2?pretty'
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
 curl -X GET 'http://localhost:9200/_cluster/health/ind-3?pretty'
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


````
Как вы думаете, почему часть индексов и кластер находится в состоянии yellow?
````
Одна нода не позволяет создать доп реплики.
````
Удалите все индексы.
````
Удаление индексов

 curl -X DELETE localhost:9200/ind-1?pretty
{
  "acknowledged" : true
}

 curl -X DELETE localhost:9200/ind-2?pretty
{
  "acknowledged" : true
}
 curl -X DELETE localhost:9200/ind-3?pretty
{
  "acknowledged" : true
}
````
Важно

При проектировании кластера elasticsearch нужно корректно рассчитывать количество реплик и шард, иначе возможна потеря данных индексов, вплоть до полной, при деградации системы.

Задача 3
В данном задании вы научитесь:

создавать бэкапы данных
восстанавливать индексы из бэкапов
Создайте директорию {путь до корневой директории с elasticsearch в образе}/snapshots.

Используя API зарегистрируйте данную директорию как snapshot repository c именем netology_backup.

Приведите в ответе запрос API и результат вызова API для создания репозитория.
````
curl -X PUT "localhost:9200/_snapshot/netology_backup?pretty" -H 'Content-Type: application/json' -d '{"type": "fs","settings": {"location": "/elasticsearch-7.17.0/snapshots","compress": true}}'
{
  "acknowledged" : true
}
 curl localhost:9200/_snapshot/netology_backup?pretty
{
  "netology_backup" : {
    "type" : "fs",
    "settings" : {
      "compress" : "true",
      "location" : "/elasticsearch-7.17.0/snapshots"
    }
  }
}

````
Создайте индекс test с 0 реплик и 1 шардом и приведите в ответе список индексов.
````
curl -X PUT localhost:9200/test -H 'Content-Type: application/json' -d '{"settings": {"number_of_shards": 1, "number_of_replicas": 0}}'
{"acknowledged":true,"shards_acknowledged":true,"index":"test"}


curl -X GET 'http://localhost:9200/_cat/indices?v'
health status index            uuid                   pri rep docs.count docs.deleted store.size pri.store.size
green  open   .geoip_databases UD6TjDHxS0aRV0HumqdxvQ   1   0         40            0     37.9mb         37.9mb
green  open   test             CexSg3snS9KD6Cp3pSPKXQ   1   0          0            0       226b           226b


````
Создайте snapshot состояния кластера elasticsearch.
````
 curl -X PUT localhost:9200/_snapshot/netology_backup/elasticsearch?wait_for_completion=true
{"snapshot":{"snapshot":"elasticsearch","uuid":"WD-sAIR-QxaCFa1xn__BYA","repository":"netology_backup","version_id":7170099,"version":"7.17.0","indices":[".ds-ilm-history-5-2022.04.25-000001",".ds-.logs-deprecation.elasticsearch-default-2022.04.25-000001",".geoip_databases","test"],"data_streams":["ilm-history-5",".logs-deprecation.elasticsearch-default"],"include_global_state":true,"state":"SUCCESS","start_time":"2022-04-25T14:13:08.038Z","start_time_in_millis":1650895988038,"end_time":"2022-04-25T14:13:09.647Z","end_time_in_millis":1650895989647,"duration_in_millis":1609,"failures":[],"shards":{"total":4,"failed":0,"successful":4},"feature_states":[{"feature_name":"geoip","indices":[".geoip_databases"]}]}}
````
Приведите в ответе список файлов в директории со snapshotами.
````
docker container ls
CONTAINER ID   IMAGE                            COMMAND                  CREATED       STATUS       PORTS                                       NAMES
200631b01502   nollon/elastic-netology:latest   "/elasticsearch-7.17…"   3 hours ago   Up 3 hours   0.0.0.0:9200->9200/tcp, :::9200->9200/tcp   upbeat_wu
root@msk-dock01:~/homework/elastic# docker exec 200631b01502 ls -l /elasticsearch-7.17.0/snapshots
total 28
-rw-r--r-- 1 elasticsearch elasticsearch 1425 Apr 25 14:13 index-0
-rw-r--r-- 1 elasticsearch elasticsearch    8 Apr 25 14:13 index.latest
drwxr-xr-x 6 elasticsearch elasticsearch 4096 Apr 25 14:13 indices
-rw-r--r-- 1 elasticsearch elasticsearch 9742 Apr 25 14:13 meta-WD-sAIR-QxaCFa1xn__BYA.dat
-rw-r--r-- 1 elasticsearch elasticsearch  453 Apr 25 14:13 snap-WD-sAIR-QxaCFa1xn__BYA.dat

````
Удалите индекс test и создайте индекс test-2. Приведите в ответе список индексов.
````
 curl -X DELETE localhost:9200/test?pretty
{
  "acknowledged" : true
}
curl -X PUT localhost:9200/test2 -H 'Content-Type: application/json' -d '{"settings": {"number_of_shards": 1, "number_of_replicas": 0}}'
{"acknowledged":true,"shards_acknowledged":true,"index":"test2"}

curl -X GET 'http://localhost:9200/_cat/indices?v'
health status index            uuid                   pri rep docs.count docs.deleted store.size pri.store.size
green  open   .geoip_databases UD6TjDHxS0aRV0HumqdxvQ   1   0         40            0     37.9mb         37.9mb
green  open   test2            vdNxOgDpTYag7jdLdKfKGA   1   0          0            0       226b           226b

````
Восстановите состояние кластера elasticsearch из snapshot, созданного ранее.

Приведите в ответе запрос к API восстановления и итоговый список индексов.
````
curl -X POST localhost:9200/_snapshot/netology_backup/elasticsearch/_restore?pretty -H 'Content-Type: application/json' -d'{"indices": "test", "include_global_state":true}'
{
  "accepted" : true
}

curl -X GET 'http://localhost:9200/_cat/indices?v'
health status index            uuid                   pri rep docs.count docs.deleted store.size pri.store.size
green  open   .geoip_databases _AGJZI6kTueY7Gg8_tea3g   1   0         40            0     37.9mb         37.9mb
green  open   test2            vdNxOgDpTYag7jdLdKfKGA   1   0          0            0       226b           226b
green  open   test             saYktOXaSAeMOzK6GnWzFQ   1   0          0            0       226b           226b

````
Подсказки:

возможно вам понадобится доработать elasticsearch.yml в части директивы path.repo и перезапустить elasticsearch