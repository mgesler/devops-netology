# devops-netology  
Home work 05-virt-05-docker-swarm

Домашнее задание к занятию "5.5. Оркестрация кластером Docker контейнеров на примере Docker Swarm"

Задача 1
Дайте письменые ответы на следующие вопросы:

В чём отличие режимов работы сервисов в Docker Swarm кластере: replication и global?

````
В Replicated режиме сервис запускается в стольких экземплярах, сколько будет задано.
В Global режиме сервис запускается в количестве экремпляров равному количеству доступных нод, 
соответствующих требованиям сервиса и доступным ресурсам, 
по одному экземпляру на каждую из нод. 
````
Какой алгоритм выбора лидера используется в Docker Swarm кластере?
````
Raft
````
Что такое Overlay Network?
````
Распределенная, шифрованная, сеть поверх имеющихся сетей между нодами swarm.
Общая рекомендация: располагать ноды в одной внутренней сети, для минимизации сетевых задержек.
````

Задача 2
Создать ваш первый Docker Swarm кластер в Яндекс.Облаке

Для получения зачета, вам необходимо предоставить скриншот из терминала (консоли), с выводом команды:

docker node ls
````
[root@node01 centos]# docker node ls
ID                            HOSTNAME             STATUS    AVAILABILITY   MANAGER STATUS   ENGINE VERSION
rj8cflea282cah57vf8ce9doc *   node01.netology.yc   Ready     Active         Leader           20.10.14
w1y1ayh0rwlobo0b1lslq9mtz     node02.netology.yc   Ready     Active         Reachable        20.10.14
nrxmi11osvhbqp90u5uzo9h8i     node03.netology.yc   Ready     Active         Reachable        20.10.14
rj4tqzrc43t8qv05emse7uw3v     node04.netology.yc   Ready     Active                          20.10.14
t9oucojlc983glf196a7kbe7b     node05.netology.yc   Ready     Active                          20.10.14
ri2ceexlorgau0lc2eent89vh     node06.netology.yc   Ready     Active                          20.10.14
````



Задача 3
Создать ваш первый, готовый к боевой эксплуатации кластер мониторинга, состоящий из стека микросервисов.

Для получения зачета, вам необходимо предоставить скриншот из терминала (консоли), с выводом команды:

docker service ls
````
[root@node01 centos]# docker service ls
ID             NAME                                MODE         REPLICAS   IMAGE                                          PORTS
gdq9cxctfy19   swarm_monitoring_alertmanager       replicated   1/1        stefanprodan/swarmprom-alertmanager:v0.14.0
kn0mzi0l62g5   swarm_monitoring_caddy              replicated   0/1        stefanprodan/caddy:latest                      *:3000->3000/tcp, *:9090->9090/tcp, *:9093-9094->9093-9094/tcp
kgzu95ijjzjn   swarm_monitoring_cadvisor           global       6/6        google/cadvisor:latest
xpvtae8qw5xo   swarm_monitoring_dockerd-exporter   global       6/6        stefanprodan/caddy:latest
aqnsl5heqdul   swarm_monitoring_grafana            replicated   0/1        stefanprodan/swarmprom-grafana:5.3.4
exkospe82h83   swarm_monitoring_node-exporter      global       6/6        stefanprodan/swarmprom-node-exporter:v0.16.0
toh8r2h5bgl6   swarm_monitoring_prometheus         replicated   1/1        stefanprodan/swarmprom-prometheus:v2.5.0
evlttiz841jm   swarm_monitoring_unsee              replicated   1/1        cloudflare/unsee:v0.8.0
````