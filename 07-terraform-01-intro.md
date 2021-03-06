# devops-netology  
Home work 07-terraform-01-intro

Домашнее задание к занятию "7.1. Инфраструктура как код"  
Задача 1. Выбор инструментов.  

Легенда
Через час совещание на котором менеджер расскажет о новом проекте. Начать работу над которым надо будет уже сегодня. На данный момент известно, что это будет сервис, который ваша компания будет предоставлять внешним заказчикам. Первое время, скорее всего, будет один внешний клиент, со временем внешних клиентов станет больше.

Так же по разговорам в компании есть вероятность, что техническое задание еще не четкое, что приведет к большому количеству небольших релизов, тестирований интеграций, откатов, доработок, то есть скучно не будет.

Вам, как девопс инженеру, будет необходимо принять решение об инструментах для организации инфраструктуры. На данный момент в вашей компании уже используются следующие инструменты:

остатки Сloud Formation,
некоторые образы сделаны при помощи Packer,
год назад начали активно использовать Terraform,
разработчики привыкли использовать Docker,
уже есть большая база Kubernetes конфигураций,
для автоматизации процессов используется Teamcity,
также есть совсем немного Ansible скриптов,
и ряд bash скриптов для упрощения рутинных задач.
Для этого в рамках совещания надо будет выяснить подробности о проекте, что бы в итоге определиться с инструментами:

Какой тип инфраструктуры будем использовать для этого проекта: изменяемый или не изменяемый?
> Я бы предусмотрел гибридный: под определенные задачи, даже в рамках одного проекта, бывает лучше использовать 
> условно-неизменяемую инфраструктуру (полное отсутствие изменений тяжело достигнуть на практике), под другие изменяемую.
 
Будет ли центральный сервер для управления инфраструктурой?
> Без центрального сервера управления. Это определяется набором используемых инструментов: Terraform, Ansible
 
Будут ли агенты на серверах?
> Нет, так же определяется набором используемых инструментов

Будут ли использованы средства для управления конфигурацией или инициализации ресурсов?
> Да, как уже было упомянуто в предыдущих вопросах: Ansible, Terraform. Packer для создания образов.

В связи с тем, что проект стартует уже сегодня, в рамках совещания надо будет определиться со всеми этими вопросами.

В результате задачи необходимо
Ответить на четыре вопроса представленных в разделе "Легенда".
> См. выше

Какие инструменты из уже используемых вы хотели бы использовать для нового проекта?
>Ansible, Terraform, Packer для развертывания инфраструктуры и управления конфигурациями
>И планировал бы отказ от Сloud Formation и скриптов Bash, в целях унификации, если это возможно.
>Для выбора средства оркестрации необходимо более внимательно ознакомиться с ТЗ проекта.

Хотите ли рассмотреть возможность внедрения новых инструментов для этого проекта?
>Это будет зависеть от специфики места размещения инфраструктуры. Облачные провайдеры могут иметь свои инструментарии.

Если для ответа на эти вопросы недостаточно информации, то напишите какие моменты уточните на совещании.
> 1 Где планируется развернуть инфраструктуру для проекта (выбор облачного провайдера и т.п.) 
> в т.ч. с учетом бюджетов и требований к географической распределенности?  
> 2 Какие нагрузки на инфраструктуру предусмотрены ТЗ и бизнес-планом? От этого, в т.ч., будет зависеть выбор инструмента оркестрации.
> 3 Попрошу SLA/OLA на ознакомление, для резервирования необходимых ресурсов. 

Задача 2. Установка терраформ.  
Официальный сайт: https://www.terraform.io/

Установите терраформ при помощи менеджера пакетов используемого в вашей операционной системе. В виде результата этой задачи приложите вывод команды terraform --version.
````
terraform --version
Terraform v1.1.6
on linux_amd64
+ provider terraform-registry.storage.yandexcloud.net/hashicorp/local v2.2.2
+ provider terraform-registry.storage.yandexcloud.net/hashicorp/null v3.1.1
+ provider terraform-registry.storage.yandexcloud.net/yandex-cloud/yandex v0.72.0
````


Задача 3. Поддержка легаси кода.  
В какой-то момент вы обновили терраформ до новой версии, например с 0.12 до 0.13. А код одного из проектов настолько устарел, что не может работать с версией 0.13. В связи с этим необходимо сделать так, чтобы вы могли одновременно использовать последнюю версию терраформа установленную при помощи штатного менеджера пакетов и устаревшую версию 0.12.

В виде результата этой задачи приложите вывод --version двух версий терраформа доступных на вашем компьютере или виртуальной машине.
````
Terraform разных версий, в виде одного бинарного файла, можно скачивать, например, отсюда:
https://hashicorp-releases.website.yandexcloud.net/terraform/

Просто размещая эти бинарные исполняемые файлы в разных каталогах и запуская их непосредственно оттуда,
можно получить произвольное количество версий terraform в рамках одного экземпляра ОС 
Есть так же вариант с переименованием соотв бинарных файлов.

cd /terraform1
./terraform --version
Terraform v1.1.6
on linux_amd64

cd /terraform2
./terraform --version
Terraform v1.1.0
on linux_amd64

````
