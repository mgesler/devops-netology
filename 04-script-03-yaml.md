# Домашнее задание к занятию "4.3. Языки разметки JSON и YAML"


## Обязательная задача 1
Мы выгрузили JSON, который получили через API запрос к нашему сервису:
```
    { "info" : "Sample JSON output from our service\t",
        "elements" :[
            { "name" : "first",
            "type" : "server",
            "ip" : 7175 
            }
            { "name" : "second",
            "type" : "proxy",
            "ip : 71.78.22.43
            }
        ]
    }
```
  Нужно найти и исправить все ошибки, которые допускает наш сервис
> "ip**"**: **"**71.78.22.43**"**

## Обязательная задача 2
В прошлый рабочий день мы создавали скрипт, позволяющий опрашивать веб-сервисы и получать их IP. К уже реализованному функционалу нам нужно добавить возможность записи JSON и YAML файлов, описывающих наши сервисы. Формат записи JSON по одному сервису: `{ "имя сервиса" : "его IP"}`. Формат записи YAML по одному сервису: `- имя сервиса: его IP`. Если в момент исполнения скрипта меняется IP у сервиса - он должен так же поменяться в yml и json файле.

### Ваш скрипт:
```python
#!/usr/bin/env python3

import socket
import http.client
import time

import json
import yaml


hosts = {'drive.google.com':'', 'mail.google.com':'', 'google.com':''}
#seconds between iterations
pause1 = 2
#iterations count
count1 =10

#Initialization
for host in hosts:
    hosts[host]=socket.gethostbyname(host)
    print ('http://'+host+' - '+hosts[host])

for counter1 in range(count1):
    time.sleep(pause1)
    print(counter1)
    for host in hosts:
            ip=socket.gethostbyname(host)
            if ip!=hosts[host]:
                    print ('[ERROR] http://'+host+' IP mismatch: '+hosts[host]+' - '+ip)
                    try:
                            h1 = http.client.HTTPConnection(host)
                            hosts[host]=ip
                            with open("hosts.json",'w') as jsn1:
                                    json_data=json.dumps(hosts)
                                    jsn1.write(json_data)
                            with open("hosts.yaml",'w') as yaml1:
                                    yaml.dump(hosts,yaml1)
                    except:
                        print('connection error')

```

### Вывод скрипта при запуске при тестировании:
```
http://drive.google.com - 64.233.165.194
http://mail.google.com - 173.194.222.83
http://google.com - 108.177.14.101
0
[ERROR] http://google.com IP mismatch: 108.177.14.101 - 64.233.162.113
1
[ERROR] http://google.com IP mismatch: 64.233.162.113 - 64.233.162.100
2
3
4
5
6
7
8
9
```

### json-файл(ы), который(е) записал ваш скрипт:
```json
{"drive.google.com": "64.233.165.194", "mail.google.com": "173.194.222.83", "google.com": "64.233.162.100"}
```

### yml-файл(ы), который(е) записал ваш скрипт:
```yaml
drive.google.com: 64.233.165.194
google.com: 64.233.162.100
mail.google.com: 173.194.222.83
```

## Дополнительное задание (со звездочкой*) - необязательно к выполнению

Так как команды в нашей компании никак не могут прийти к единому мнению о том, какой формат разметки данных использовать: JSON или YAML, нам нужно реализовать парсер из одного формата в другой. Он должен уметь:
   * Принимать на вход имя файла
   * Проверять формат исходного файла. Если файл не json или yml - скрипт должен остановить свою работу
   * Распознавать какой формат данных в файле. Считается, что файлы *.json и *.yml могут быть перепутаны
   * Перекодировать данные из исходного формата во второй доступный (из JSON в YAML, из YAML в JSON)
   * При обнаружении ошибки в исходном файле - указать в стандартном выводе строку с ошибкой синтаксиса и её номер
   * Полученный файл должен иметь имя исходного файла, разница в наименовании обеспечивается разницей расширения файлов

### Ваш скрипт:
```python
???
```

### Пример работы скрипта:
???
