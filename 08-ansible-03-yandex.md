# devops-netology  
Home work 08-ansible-03-yandex
# Домашнее задание к занятию "08.03 Использование Yandex Cloud"


## Подготовка к выполнению
1. Создайте свой собственный (или используйте старый) публичный репозиторий на github с произвольным именем.
2. Скачайте [playbook](./playbook/) из репозитория с домашним заданием и перенесите его в свой репозиторий.

## Основная часть
1. Допишите playbook: нужно сделать ещё один play, который устанавливает и настраивает kibana.
````
- name: Install Kibana
  hosts: kibana
  handlers:
    - name: restart Kibana
      become: true
      service:
        name: kibana
        state: restarted
  tasks:
    - name: "Download Kibana's rpm"
      get_url:
        url: "http://10.2.25.120/kibana-{{ version }}-x86_64.rpm"
        dest: "/tmp/kibana-{{ version }}-x86_64.rpm"
      register: download_kibana
      until: download_kibana is succeeded
    - name: Install Kibana
      become: true
      yum:
        name: "/tmp/kibana-{{ version }}-x86_64.rpm"
        state: present
    - name: Configure Kibana
      become: true
      template:
        src: kibana.yml.j2
        dest: /etc/kibana/kibana.yml
      notify: restart Kibana
      
````
2. При создании tasks рекомендую использовать модули: `get_url`, `template`, `yum`, `apt`.
3. Tasks должны: скачать нужной версии дистрибутив, выполнить распаковку в выбранную директорию, сгенерировать конфигурацию с параметрами.
4. Приготовьте свой собственный inventory файл `prod.yml`.
5. Запустите `ansible-lint site.yml` и исправьте ошибки, если они есть.
````
ansible-lint site.yml
WARNING  Overriding detected file kind 'yaml' with 'playbook' for given positional argument: site.yml
WARNING  Listing 21 violation(s) that are fatal
fqcn-builtins: Use FQCN for builtin actions.
site.yml:5 Task/Handler: restart Elasticsearch

fqcn-builtins: Use FQCN for builtin actions.
site.yml:11 Task/Handler: Download Elasticsearch's rpm

risky-file-permissions: File permissions unset or incorrect.
site.yml:11 Task/Handler: Download Elasticsearch's rpm

fqcn-builtins: Use FQCN for builtin actions.
site.yml:17 Task/Handler: Install Elasticsearch

fqcn-builtins: Use FQCN for builtin actions.
site.yml:22 Task/Handler: Configure Elasticsearch

risky-file-permissions: File permissions unset or incorrect.
site.yml:22 Task/Handler: Configure Elasticsearch

fqcn-builtins: Use FQCN for builtin actions.
site.yml:32 Task/Handler: restart Kibana

fqcn-builtins: Use FQCN for builtin actions.
site.yml:38 Task/Handler: Download Kibana's rpm

risky-file-permissions: File permissions unset or incorrect.
site.yml:38 Task/Handler: Download Kibana's rpm

fqcn-builtins: Use FQCN for builtin actions.
site.yml:44 Task/Handler: Install Kibana

fqcn-builtins: Use FQCN for builtin actions.
site.yml:49 Task/Handler: Configure Kibana

risky-file-permissions: File permissions unset or incorrect.
site.yml:49 Task/Handler: Configure Kibana

fqcn-builtins: Use FQCN for builtin actions.
site.yml:59 Task/Handler: restart filebeat

fqcn-builtins: Use FQCN for builtin actions.
site.yml:66 Task/Handler: Download filebeat's rpm

risky-file-permissions: File permissions unset or incorrect.
site.yml:66 Task/Handler: Download filebeat's rpm

fqcn-builtins: Use FQCN for builtin actions.
site.yml:72 Task/Handler: Install filebeat

fqcn-builtins: Use FQCN for builtin actions.
site.yml:78 Task/Handler: Configure filebeat

risky-file-permissions: File permissions unset or incorrect.
site.yml:78 Task/Handler: Configure filebeat

fqcn-builtins: Use FQCN for builtin actions.
site.yml:84 Task/Handler: Set filebeat systemwork

fqcn-builtins: Use FQCN for builtin actions.
site.yml:92 Task/Handler: Kibana dashboard

yaml: no new line character at the end of file (yaml[new-line-at-end-of-file])
site.yml:99

You can skip specific rules or tags by adding them to your configuration file:
# .config/ansible-lint.yml
warn_list:  # or 'skip_list' to silence them completely
  - experimental  # all rules tagged as experimental
  - fqcn-builtins  # Use FQCN for builtin actions.
  - yaml  # Violations reported by yamllint.

Finished with 15 failure(s), 6 warning(s) on 1 files.


Исправлено:

ansible-lint site.yml
WARNING  Overriding detected file kind 'yaml' with 'playbook' for given positional argument: site.yml


````
6. Попробуйте запустить playbook на этом окружении с флагом `--check`.
````
ansible-playbook site.yml -i inventory/prod.yml --check

PLAY [Install Elasticsearch] **********************************************************************************************************

TASK [Gathering Facts] ****************************************************************************************************************
ok: [els]

TASK [Download Elasticsearch's rpm] ***************************************************************************************************
ok: [els]

TASK [Install Elasticsearch] **********************************************************************************************************
ok: [els]

TASK [Configure Elasticsearch] ********************************************************************************************************
changed: [els]

RUNNING HANDLER [restart Elasticsearch] ***********************************************************************************************
changed: [els]

PLAY [Install Kibana] *****************************************************************************************************************

TASK [Gathering Facts] ****************************************************************************************************************
ok: [kbn]

TASK [Download Kibana's rpm] **********************************************************************************************************
ok: [kbn]

TASK [Install Kibana] *****************************************************************************************************************
ok: [kbn]

TASK [Configure Kibana] ***************************************************************************************************************
changed: [kbn]

RUNNING HANDLER [restart Kibana] ******************************************************************************************************
changed: [kbn]

PLAY [Install filebeat] ***************************************************************************************************************

TASK [Gathering Facts] ****************************************************************************************************************
ok: [fbt]

TASK [Download filebeat rpm] **********************************************************************************************************
ok: [fbt]

TASK [Install filebeat] ***************************************************************************************************************
ok: [fbt]

TASK [Configure filebeat] *************************************************************************************************************
changed: [fbt]

TASK [filebeat modules] ***************************************************************************************************************
skipping: [fbt]

TASK [Kibana dashboard] ***************************************************************************************************************
skipping: [fbt]

RUNNING HANDLER [restart filebeat] ****************************************************************************************************
changed: [fbt]

PLAY RECAP ****************************************************************************************************************************
els                        : ok=5    changed=2    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
fbt                        : ok=5    changed=2    unreachable=0    failed=0    skipped=2    rescued=0    ignored=0   
kbn                        : ok=5    changed=2    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
````
7. Запустите playbook на `prod.yml` окружении с флагом `--diff`. Убедитесь, что изменения на системе произведены.
````
ansible-playbook site.yml -i inventory/prod.yml --diff

PLAY [Install Elasticsearch] **********************************************************************************************************

TASK [Gathering Facts] ****************************************************************************************************************
ok: [els]

TASK [Download Elasticsearch's rpm] ***************************************************************************************************
ok: [els]

TASK [Install Elasticsearch] **********************************************************************************************************
ok: [els]

TASK [Configure Elasticsearch] ********************************************************************************************************
ok: [els]

PLAY [Install Kibana] *****************************************************************************************************************

TASK [Gathering Facts] ****************************************************************************************************************
ok: [kbn]

TASK [Download Kibana's rpm] **********************************************************************************************************
ok: [kbn]

TASK [Install Kibana] *****************************************************************************************************************
ok: [kbn]

TASK [Configure Kibana] ***************************************************************************************************************
--- before
+++ after
@@ -1,4 +1,4 @@
 {
-    "mode": "0660",
+    "mode": "0755",
     "path": "/etc/kibana/kibana.yml"
 }

changed: [kbn]

RUNNING HANDLER [restart Kibana] ******************************************************************************************************
changed: [kbn]

PLAY [Install filebeat] ***************************************************************************************************************

TASK [Gathering Facts] ****************************************************************************************************************
ok: [fbt]

TASK [Download filebeat rpm] **********************************************************************************************************
ok: [fbt]

TASK [Install filebeat] ***************************************************************************************************************
ok: [fbt]

TASK [Configure filebeat] *************************************************************************************************************
--- before
+++ after
@@ -1,4 +1,4 @@
 {
-    "mode": "0600",
+    "mode": "0755",
     "path": "/etc/filebeat/filebeat.yml"
 }

changed: [fbt]

TASK [filebeat modules] ***************************************************************************************************************
ok: [fbt]

TASK [Kibana dashboard] ***************************************************************************************************************
ok: [fbt]

RUNNING HANDLER [restart filebeat] ****************************************************************************************************
changed: [fbt]

PLAY RECAP ****************************************************************************************************************************
els                        : ok=4    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
fbt                        : ok=7    changed=2    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
kbn                        : ok=5    changed=2    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0 

````
8. Повторно запустите playbook с флагом `--diff` и убедитесь, что playbook идемпотентен.
````
PLAY RECAP ****************************************************************************************************************************
els                        : ok=4    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
fbt                        : ok=6    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
kbn                        : ok=4    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0 
````
9. Проделайте шаги с 1 до 8 для создания ещё одного play, который устанавливает и настраивает filebeat.
````
- name: Install filebeat
  hosts: filebeat
  handlers:
    - name: restart filebeat
      become: true
      ansible.builtin.systemd:
        name: filebeat
        state: restarted
        enabled: true
  tasks:
    - name: "Download filebeat rpm"
      ansible.builtin.get_url:
        url: "http://10.2.25.120/filebeat-{{ version }}-x86_64.rpm"
        dest: "/tmp/filebeat-{{ version }}-x86_64.rpm"
        mode: 0755
      register: download_filebeat
      until: download_filebeat is succeeded
    - name: Install filebeat
      become: true
      ansible.builtin.yum:
        name: "/tmp/filebeat-{{ version }}-x86_64.rpm"
        state: present
      notify: restart filebeat
    - name: Configure filebeat
      become: true
      ansible.builtin.template:
        src: filebeat.yml.j2
        dest: /etc/filebeat/filebeat.yml
        mode: 0755
      notify: restart filebeat
    - name: filebeat modules
      become: true
      ansible.builtin.command:
        cmd: filebeat modules enable system
        chdir: /usr/share/filebeat/bin
      register: filebeat_modules
      changed_when: filebeat_modules.stdout != 'already enabled'

    - name: Kibana dashboard
      become: true
      ansible.builtin.command:
        cmd: filebeat setup
        chdir: /usr/share/filebeat/bin
      register: filebeat_setup
      changed_when: false
      until: filebeat_setup is succeeded
````
10. Подготовьте README.md файл по своему playbook. В нём должно быть описано: что делает playbook, какие у него есть параметры и теги.
````
В связи с санкционными органичениями web-сервер для скачивания дистрибутивов эмулируется в отдельном контейнере.

Плэйбук состоит из плеев, скачивающих/устанавливающих/запускающих Elasticsearch, Kibana и Filebeat соответственно.
Считается, что java уже установлена (если необходимо).

Основные параметры: 
version - версия ПО, входящего в ELK стек.

````
11. Готовый playbook выложите в свой репозиторий, в ответ предоставьте ссылку на него.
[playbook](https://github.com/mgesler/devops-netology/tree/main/08-ansible-03-yandex)