# devops-netology  
Home work 10-monitoring-04-elk
# Домашнее задание к занятию "10.04. ELK"

## Задание 1

Вам необходимо поднять в докере:
- elasticsearch(hot и warm ноды)
- logstash
- kibana
- filebeat

и связать их между собой.

Logstash следует сконфигурировать для приёма по tcp json сообщений.

Filebeat следует сконфигурировать для отправки логов docker вашей системы в logstash.

Результатом выполнения данного задания должны быть:
- скриншот `docker ps` через 5 минут после старта всех контейнеров (их должно быть 5)
![](https://github.com/mgesler/devops-netology/blob/main/pic/5minutes.jpg)
Версии ПО были заменены на имеющиеся в hub.docker.com для обхода "санкционных ограничений"
- скриншот интерфейса kibana
![](https://github.com/mgesler/devops-netology/blob/main/pic/kibana.jpg)
- docker-compose манифест (если вы не использовали директорию help)
- ваши yml конфигурации для стека (если вы не использовали директорию help)

## Задание 2

Перейдите в меню [создания index-patterns  в kibana](http://localhost:5601/app/management/kibana/indexPatterns/create)
и создайте несколько index-patterns из имеющихся.
![](https://github.com/mgesler/devops-netology/blob/main/pic/kibana1.jpg)
Перейдите в меню просмотра логов в kibana (Discover) и самостоятельно изучите как отображаются логи и как производить 
поиск по логам.
