# devops-netology
Home work 05-virt-01-basics

Домашнее задание к занятию "5.1. Основы виртуализации"
Задача 1
Вкратце опишите, как вы поняли - в чем основное отличие паравиртуализации и виртуализации на основе ОС.
````
При паравиртуализации гипервизор работает отдельным слоем, поверх операционной системы хоста.
Ядро гостевой ОС никак не модифицируется гипервизором.

Виртуализация на основе ОС не позволяет запускать гостевые ВМ с ядром ОС отличным от ОС гипервизора.
Фактически, ВМ является изолированной частью ОС гипервизора, в этом случае.

````
Задача 2
Выберите тип один из вариантов использования организации физических серверов, в зависимости от условий использования.

Организация серверов:

физические сервера
паравиртуализация
виртуализация уровня ОС
Условия использования:

Высоконагруженная база данных, чувствительная к отказу
Различные Java-приложения
Windows системы для использования Бухгалтерским отделом
Системы, выполняющие высокопроизводительные расчеты на GPU
Опишите, почему вы выбрали к каждому целевому использованию такую организацию.

````
Высоконагруженная база данных, чувствительная к отказу:
 - физические серверы в кластере/виртуализация уровня ОС, как с вариант с наименьшими накладными расходами

Различные Java-приложения:
 - паравиртуализация, возможность давать любую ОС для VM, не зависеть о версии ядра гипервизора 
 
Windows системы для использования Бухгалтерским отделом:
 - паравиртуализация, например, используем proxmox (kvm) для этой задачи.
 требуется ОС Windows, возможность снимать полную копию VM/делать снэпшоты, отсутствие платного лицензирования гипервизоров   

Системы, выполняющие высокопроизводительные расчеты на GPU:
 - физические серверы, для получения доступа к неэмулированным аппаратным ресурсам.

Опишите, почему вы выбрали к каждому целевому использованию такую организацию.
````
Задача 3
Как вы думаете, возможно ли совмещать несколько типов виртуализации на одном сервере? Приведите пример такого совмещения.
````
Можно. Пример: Proxmox - паравиртуализация (kvm)+виртуализация уровня ОС (LXC контейнеры)
````