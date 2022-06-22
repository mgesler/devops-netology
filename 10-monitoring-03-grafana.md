# devops-netology  
Home work 10-monitoring-03-grafana

Домашнее задание к занятию "10.03. Grafana" 

Задание 1
Используя директорию help внутри данного домашнего задания - запустите связку prometheus-grafana.

Зайдите в веб-интерфейс графана, используя авторизационные данные, указанные в манифесте docker-compose.
````
admin
admin
````
Подключите поднятый вами prometheus как источник данных.
````
docker container inspect 49dbe5914f3f | grep IPAddress
     "SecondaryIPAddresses": null,
            "IPAddress": "",
            "IPAddress": "172.18.0.3",
 
где 49dbe5914f3f - id контейнера с prometheus

Переходим в web-интерфейс Grafana , выбираем configuration-data sources и там Prometheus
вбиваем адрес как http://172.18.0.3:9090
  

````
![](https://github.com/mgesler/devops-netology/blob/main/pic/prom1.jpg)

Решение домашнего задания - скриншот веб-интерфейса grafana со списком подключенных Datasource.

Задание 2
Изучите самостоятельно ресурсы:

promql-for-humans
understanding prometheus cpu metrics
Создайте Dashboard и в ней создайте следующие Panels:

Утилизация CPU для nodeexporter (в процентах, 100-idle)
CPULA 1/5/15
Количество свободной оперативной памяти
Количество места на файловой системе
Для решения данного ДЗ приведите promql запросы для выдачи этих метрик, а также скриншот получившейся Dashboard.

Задание 3
Создайте для каждой Dashboard подходящее правило alert (можно обратиться к первой лекции в блоке "Мониторинг").

Для решения ДЗ - приведите скриншот вашей итоговой Dashboard.

Задание 4
Сохраните ваш Dashboard.

Для этого перейдите в настройки Dashboard, выберите в боковом меню "JSON MODEL".

Далее скопируйте отображаемое json-содержимое в отдельный файл и сохраните его.

В решении задания - приведите листинг этого файла.

