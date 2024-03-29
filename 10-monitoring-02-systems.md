# devops-netology  
Home work 10-monitoring-02-systems
# Домашнее задание к занятию "10.02. Системы мониторинга"

## Обязательные задания

1. Опишите основные плюсы и минусы pull и push систем мониторинга.
````
Push +
параллельная отправка результата мониторинга на несколько серверов
гибкость расписания и прочих параметров отправки с агента
использование протокола UDP, как более оптимального, в данном случае

Pull +
Контроль подлинности данных (спорный плюс, есть масса других способов контроля)
Использование прокси
Потенциальная возможность прямой отладки агентов

Фактически, сильные стороны одной системы это слабые стороны другой. 

````

3. Какие из ниже перечисленных систем относятся к push модели, а какие к pull? А может есть гибридные?

````
    - Prometheus Дефолтно pull, но может работать и как push
    - TICK push 
    - Zabbix Гибрид, умеет штатно обе системы
    - VictoriaMetrics Гибрид
    - Nagios pull
````

4. Склонируйте себе [репозиторий](https://github.com/influxdata/sandbox/tree/master) и запустите TICK-стэк, 
используя технологии docker и docker-compose.

В виде решения на это упражнение приведите выводы команд с вашего компьютера (виртуальной машины):

    - curl http://localhost:8086/ping
![](https://github.com/mgesler/devops-netology/blob/main/pic/mon1.jpg)
    - curl http://localhost:8888
![](https://github.com/mgesler/devops-netology/blob/main/pic/mon2.jpg)
    - curl http://localhost:9092/kapacitor/v1/ping
![](https://github.com/mgesler/devops-netology/blob/main/pic/mon3.jpg)

А также скриншот веб-интерфейса ПО chronograf (`http://localhost:8888`). 
![](https://github.com/mgesler/devops-netology/blob/main/pic/mon4.jpg)

P.S.: если при запуске некоторые контейнеры будут падать с ошибкой - проставьте им режим `Z`, например
`./data:/var/lib:Z`

4. Перейдите в веб-интерфейс Chronograf (`http://localhost:8888`) и откройте вкладку `Data explorer`.

    - Нажмите на кнопку `Add a query`
    - Изучите вывод интерфейса и выберите БД `telegraf.autogen`
    - В `measurments` выберите mem->host->telegraf_container_id , а в `fields` выберите used_percent. 
    Внизу появится график утилизации оперативной памяти в контейнере telegraf.
    - Вверху вы можете увидеть запрос, аналогичный SQL-синтаксису. 
    Поэкспериментируйте с запросом, попробуйте изменить группировку и интервал наблюдений.

Для выполнения задания приведите скриншот с отображением метрик утилизации места на диске 
(disk->host->telegraf_container_id) из веб-интерфейса.

````
Возможно, поменялась версия, но нет "disk" в меню и если руками его в запрос вписать, 
выдается соотв ошибка об отсутствии.
Снял другую метрику, смысл тот же, в плане алгоритма действий. 
````
![](https://github.com/mgesler/devops-netology/blob/main/pic/mon5-mem.jpg)
![](https://github.com/mgesler/devops-netology/blob/main/pic/mon6.jpg)

5. Изучите список [telegraf inputs](https://github.com/influxdata/telegraf/tree/master/plugins/inputs). 
Добавьте в конфигурацию telegraf следующий плагин - [docker](https://github.com/influxdata/telegraf/tree/master/plugins/inputs/docker):
```
[[inputs.docker]]
  endpoint = "unix:///var/run/docker.sock"
```

Дополнительно вам может потребоваться донастройка контейнера telegraf в `docker-compose.yml` дополнительного volume и 
режима privileged:
```
  telegraf:
    image: telegraf:1.4.0
    privileged: true
    volumes:
      - ./etc/telegraf.conf:/etc/telegraf/telegraf.conf:Z
      - /var/run/docker.sock:/var/run/docker.sock:Z
    links:
      - influxdb
    ports:
      - "8092:8092/udp"
      - "8094:8094"
      - "8125:8125/udp"
```

После настройке перезапустите telegraf, обновите веб интерфейс и приведите скриншотом список `measurments` в 
веб-интерфейсе базы telegraf.autogen . Там должны появиться метрики, связанные с docker.

Ф![](https://github.com/mgesler/devops-netology/blob/main/pic/mon7-docker.jpg)