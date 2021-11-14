# devops-netology
Home work 03-sysadmin-04-os

Домашнее задание к занятию "3.4. Операционные системы, лекция 2"

1. На лекции мы познакомились с node_exporter. В демонстрации его исполняемый файл запускался в background. 
Этого достаточно для демо, но не для настоящей production-системы, где процессы должны находиться под внешним управлением. 
Используя знания из лекции по systemd, создайте самостоятельно простой unit-файл для node_exporter:
поместите его в автозагрузку, предусмотрите возможность добавления опций к запускаемому процессу через внешний файл (посмотрите, например, на systemctl cat cron), 
удостоверьтесь, что с помощью systemctl процесс корректно стартует, завершается, а после перезагрузки автоматически поднимается.

>[Unit]  
> Description=Node Exporter  
> Wants=network-online.target  
> After=network-online.target  

> [Service]  
> User=node_exporter  
> Group=node_exporter  
> ExecStart=/usr/local/bin/node_exporter
> EnvironmentFile=/etc/default/node_exporter  

> [Install]  
> WantedBy=default.target  
> 
> service node_exporter start  
> service node_exporter status  
>● node_exporter.service - Node Exporter  
>     Loaded: loaded (/etc/systemd/system/node_exporter.service; disabled; vendor preset: enabled)  
>     Active: active (running) since Sun 2021-11-14 20:26:04 UTC; 10s ago  
> 
> service node_exporter stop    
> service node_exporter status  
> ● node_exporter.service - Node Exporter  
>  Loaded: loaded (/etc/systemd/system/node_exporter.service; disabled; vendor preset: enabled)  
> Active: inactive (dead)
> 
> Переменные можем передавать через файл , указанный в EnvironmentFile

2. Ознакомьтесь с опциями node_exporter и выводом /metrics по-умолчанию. Приведите несколько опций, 
которые вы бы выбрали для базового мониторинга хоста по CPU, памяти, диску и сети.
> CPU  
> node_cpu_seconds_total    
> node_cpu_seconds_total  
> node_cpu_seconds_total 
> node_load5  
> 
> RAM  
> node_memory_MemAvailable_bytes   
> node_memory_MemFree_bytes  
> 
> Disk  
> node_disk_io_time_seconds_total   
> node_disk_read_bytes_total   
> node_filesystem_free_bytes  
> 
> Network  
> node_network_receive_errs_total   
> node_network_receive_bytes_total  
> node_network_transmit_bytes_total  
> node_network_transmit_errs_total  
>  

3. Установите в свою виртуальную машину Netdata. Воспользуйтесь готовыми пакетами для установки (sudo apt install -y netdata). После успешной установки:

в конфигурационном файле /etc/netdata/netdata.conf в секции [web] замените значение с localhost на bind to = 0.0.0.0,  
добавьте в Vagrantfile проброс порта Netdata на свой локальный компьютер и сделайте vagrant reload:  
config.vm.network "forwarded_port", guest: 19999, host: 19999  
После успешной перезагрузки в браузере на своем ПК (не в виртуальной машине) вы должны суметь зайти на localhost:19999. Ознакомьтесь с метриками, которые по умолчанию собираются Netdata и с комментариями, которые даны к этим метрикам.  
> ![](https://myoctocat.com/assets/images/base-octocat.svg)

4. Можно ли по выводу dmesg понять, осознает ли ОС, что загружена не на настоящем оборудовании, а на системе виртуализации?
>root@vagrant:/home/vagrant# dmesg |grep virtual  
> [    0.002151] CPU MTRRs all blank - virtualized system.    
> [    0.058830] Booting paravirtualized kernel on KVM    
> [    2.370105] systemd[1]: Detected virtualization oracle.  
> 
5. Как настроен sysctl fs.nr_open на системе по-умолчанию? Узнайте, что означает этот параметр. Какой другой существующий лимит не позволит достичь такого числа (ulimit --help)?
>This denotes the maximum number of file-handles a process can  allocate. Default value is 1024*1024 (1048576) 
> root@vagrant:/home/vagrant# ulimit -Sn    
> 1024  
> root@vagrant:/home/vagrant# ulimit -Hn  
> 1048576  

5. Запустите любой долгоживущий процесс (не ls, который отработает мгновенно, а, например, sleep 1h) в отдельном неймспейсе процессов; покажите, что ваш процесс работает под PID 1 через nsenter. Для простоты работайте в данном задании под root (sudo -i). Под обычным пользователем требуются дополнительные опции (--map-root-user) и т.д.
> unshare -f --pid --mount-proc sleep 1h &  
> ps -ax --forest | grep sleep  
>  15249 pts/0    S      0:00                          \_ unshare -f --pid --mount-proc sleep 1h  
>  15250 pts/0    S      0:00                          |   \_ sleep 1h  
> nsenter --target 15250 -p --mount
>  ps    
>    PID TTY          TIME CMD  
>      1 pts/0    00:00:00 sleep  
>     12 pts/0    00:00:00 bash  
>     21 pts/0    00:00:00 ps  
> 
>
6. Найдите информацию о том, что такое :(){ :|:& };:. Запустите эту команду в своей виртуальной машине Vagrant с Ubuntu 20.04 (это важно, поведение в других ОС не проверялось). Некоторое время все будет "плохо", после чего (минуты) – ОС должна стабилизироваться. Вызов dmesg расскажет, какой механизм помог автоматической стабилизации. Как настроен этот механизм по-умолчанию, и как изменить число процессов, которое можно создать в сессии?  
> для понятности заменим : именем f и отформатируем код.

> f() {  
>  f | f &  
>}  
>f  

> таким образом это функция, которая параллельно пускает два своих экземпляра. Каждый пускает ещё по два и т.д.   
> При отсутствии лимита на число процессов машина быстро исчерпывает физическую память и уходит в своп.  