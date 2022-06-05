# devops-netology  
Home work 08-ansible-02-playbook

Домашнее задание к занятию "08.01 Введение в Ansible"  
Подготовка к выполнению  
Установите ansible версии 2.10 или выше.  
Создайте свой собственный публичный репозиторий на github с произвольным именем.  
Скачайте playbook из репозитория с домашним заданием и перенесите его в свой репозиторий.  

Основная часть  
Приготовьте свой собственный inventory файл prod.yml.
````
Подключение будет производится к докер контейнеру (устанавливаю все в один)
---
elasticsearch:
  hosts:
    debian:
      ansible_connection: docker
      remote_addr: debian

kibana:
  hosts:
    debian:
      ansible_connection: docker
      remote_addr: debian
````
Допишите playbook: нужно сделать ещё один play, который устанавливает и настраивает kibana.
````

````
При создании tasks рекомендую использовать модули: get_url, template, unarchive, file.
Tasks должны: скачать нужной версии дистрибутив, выполнить распаковку в выбранную директорию, сгенерировать конфигурацию с параметрами.
Запустите ansible-lint site.yml и исправьте ошибки, если они есть.
````
При проверке ansible-lint возникали следующие ошибки:
1. fqcn-builtins: Use FQCN for builtin actions. - 
   требуется указание полной версии при обращении к встроенным модулям
   
   Пример: было set_fact , правильно ansible.builtin.set_fact  

2. risky-file-permissions: File permissions unset or incorrect. - 
   не задаются явно права при создании объектов файловой системы 
 
   Пример: не были заданы явно права при создании директории в таске, 
   правильно - указать явно, как вариант mode: 0755
 
````
![](https://github.com/mgesler/devops-netology/blob/main/pic/lint.jpg)


Попробуйте запустить playbook на этом окружении с флагом --check.
````
ansible-playbook site.yml -i inventory/prod.yml --check

PLAY [Install Java] *******************************************************************************************************************

TASK [Gathering Facts] ****************************************************************************************************************
ok: [debian]

TASK [Set facts for Java 11 vars] *****************************************************************************************************
ok: [debian]

TASK [Upload .tar.gz file containing binaries from local storage] *********************************************************************
changed: [debian]

TASK [Ensure installation dir exists] *************************************************************************************************
ok: [debian]

TASK [Extract java in the installation directory] *************************************************************************************
skipping: [debian]

TASK [Export environment variables] ***************************************************************************************************
changed: [debian]

PLAY [Install Elasticsearch] **********************************************************************************************************

TASK [Gathering Facts] ****************************************************************************************************************
ok: [debian]

TASK [Upload .tar.gz file containing binaries from local storage] *********************************************************************
changed: [debian]

TASK [Create directrory for Elasticsearch] ********************************************************************************************
ok: [debian]

TASK [Extract Elasticsearch in the installation directory] ****************************************************************************
skipping: [debian]

TASK [Set environment Elastic] ********************************************************************************************************
changed: [debian]

PLAY [Install Kibana] *****************************************************************************************************************

TASK [Gathering Facts] ****************************************************************************************************************
ok: [debian]

TASK [Upload .tar.gz file containing binaries from local storage] *********************************************************************
changed: [debian]

TASK [Create directrory for Kibana] ***************************************************************************************************
ok: [debian]

TASK [Extract Kibana in the installation directory] ***********************************************************************************
skipping: [debian]

TASK [Set environment Kibana] *********************************************************************************************************
changed: [debian]

PLAY RECAP ****************************************************************************************************************************
debian                     : ok=13   changed=6    unreachable=0    failed=0    skipped=3    rescued=0    ignored=0   
````
Запустите playbook на prod.yml окружении с флагом --diff. Убедитесь, что изменения на системе произведены.
````
ansible-playbook site.yml -i inventory/prod.yml --diff

PLAY [Install Java] *******************************************************************************************************************

TASK [Gathering Facts] ****************************************************************************************************************
ok: [debian]

TASK [Set facts for Java 11 vars] *****************************************************************************************************
ok: [debian]

TASK [Upload .tar.gz file containing binaries from local storage] *********************************************************************
diff skipped: source file size is greater than 104448
changed: [debian]

TASK [Ensure installation dir exists] *************************************************************************************************
--- before
+++ after
@@ -1,4 +1,4 @@
 {
     "path": "/opt/jdk/11.0.15.1",
-    "state": "absent"
+    "state": "directory"
 }

changed: [debian]

TASK [Extract java in the installation directory] *************************************************************************************
changed: [debian]

TASK [Export environment variables] ***************************************************************************************************
--- before
+++ after: /root/.ansible/tmp/ansible-local-8608915cc4f9o/tmpilmr8cp8/jdk.sh.j2
@@ -0,0 +1,5 @@
+# Warning: This file is Ansible Managed, manual changes will be overwritten on next playbook run.
+#!/usr/bin/env bash
+
+export JAVA_HOME=/opt/jdk/11.0.15.1
+export PATH=$PATH:$JAVA_HOME/bin
\ No newline at end of file

changed: [debian]

PLAY [Install Elasticsearch] **********************************************************************************************************

TASK [Gathering Facts] ****************************************************************************************************************
ok: [debian]

TASK [Upload .tar.gz file containing binaries from local storage] *********************************************************************
diff skipped: source file size is greater than 104448
changed: [debian]

TASK [Create directrory for Elasticsearch] ********************************************************************************************
--- before
+++ after
@@ -1,4 +1,4 @@
 {
     "path": "/opt/elastic/7.10.1",
-    "state": "absent"
+    "state": "directory"
 }

changed: [debian]

TASK [Extract Elasticsearch in the installation directory] ****************************************************************************
changed: [debian]

TASK [Set environment Elastic] ********************************************************************************************************
--- before
+++ after: /root/.ansible/tmp/ansible-local-8608915cc4f9o/tmpl1953v4_/elk.sh.j2
@@ -0,0 +1,5 @@
+# Warning: This file is Ansible Managed, manual changes will be overwritten on next playbook run.
+#!/usr/bin/env bash
+
+export ES_HOME=/opt/elastic/7.10.1
+export PATH=$PATH:$ES_HOME/bin
\ No newline at end of file

changed: [debian]

PLAY [Install Kibana] *****************************************************************************************************************

TASK [Gathering Facts] ****************************************************************************************************************
ok: [debian]

TASK [Upload .tar.gz file containing binaries from local storage] *********************************************************************
diff skipped: source file size is greater than 104448
changed: [debian]

TASK [Create directrory for Kibana] ***************************************************************************************************
--- before
+++ after
@@ -1,4 +1,4 @@
 {
     "path": "/opt/kibana/7.10.1",
-    "state": "absent"
+    "state": "directory"
 }

changed: [debian]

TASK [Extract Kibana in the installation directory] ***********************************************************************************
changed: [debian]

TASK [Set environment Kibana] *********************************************************************************************************
--- before
+++ after: /root/.ansible/tmp/ansible-local-8608915cc4f9o/tmpzg7i6com/kib.sh.j2
@@ -0,0 +1,5 @@
+# Warning: This file is Ansible Managed, manual changes will be overwritten on next playbook run.
+#!/usr/bin/env bash
+
+export KB_HOME=/opt/kibana/7.10.1
+export PATH=$PATH:$KB_HOME/bin
\ No newline at end of file

changed: [debian]

PLAY RECAP ****************************************************************************************************************************
debian                     : ok=16   changed=12   unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
````
Повторно запустите playbook с флагом --diff и убедитесь, что playbook идемпотентен.
````
ansible-playbook site.yml -i inventory/prod.yml --diff

PLAY [Install Java] *******************************************************************************************************************

TASK [Gathering Facts] ****************************************************************************************************************
ok: [debian]

TASK [Set facts for Java 11 vars] *****************************************************************************************************
ok: [debian]

TASK [Upload .tar.gz file containing binaries from local storage] *********************************************************************
ok: [debian]

TASK [Ensure installation dir exists] *************************************************************************************************
ok: [debian]

TASK [Extract java in the installation directory] *************************************************************************************
skipping: [debian]

TASK [Export environment variables] ***************************************************************************************************
ok: [debian]

PLAY [Install Elasticsearch] **********************************************************************************************************

TASK [Gathering Facts] ****************************************************************************************************************
ok: [debian]

TASK [Upload .tar.gz file containing binaries from local storage] *********************************************************************
ok: [debian]

TASK [Create directrory for Elasticsearch] ********************************************************************************************
ok: [debian]

TASK [Extract Elasticsearch in the installation directory] ****************************************************************************
skipping: [debian]

TASK [Set environment Elastic] ********************************************************************************************************
ok: [debian]

PLAY [Install Kibana] *****************************************************************************************************************

TASK [Gathering Facts] ****************************************************************************************************************
ok: [debian]

TASK [Upload .tar.gz file containing binaries from local storage] *********************************************************************
ok: [debian]

TASK [Create directrory for Kibana] ***************************************************************************************************
ok: [debian]

TASK [Extract Kibana in the installation directory] ***********************************************************************************
skipping: [debian]

TASK [Set environment Kibana] *********************************************************************************************************
ok: [debian]

PLAY RECAP ****************************************************************************************************************************
debian                     : ok=13   changed=0    unreachable=0    failed=0    skipped=3    rescued=0    ignored=0   
````
Подготовьте README.md файл по своему playbook. В нём должно быть описано: что делает playbook, какие у него есть параметры и теги.

````
GROUP VARS
java_oracle_jdk_package - имя файла дистрибутива Java
java_jdk_version -версия Java, которую будем использовать

elastic_home - домашний каталог Elasticsearch
kibana_home - домашний каталога Kibana
elastic_version - версия Elasticsearch
kibana_version - версия Kibana

В качестве окружения используется докер контейнер в него устанавливаются Java, ElasticSearch, Kibana.
Используется локальное копирование дистрибутивов, т.к. соотв. репозитории закрыты для ip-адресов РФ.
В принципе, можно было сэмулировать скачивание, подняв локальный web-сервер и сделав нужные записи в файле hosts.

Происходит установка Elasticsearch и Kibana в подготовленные каталоги.

Для Elasticsearch устанавливаются переменные окружения.
Для Kibana устанавливаются переменные окружения, путь до Elasticsearch, в данном случае, совпадает с дефолтным. 


````