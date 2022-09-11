# devops-netology  
Home work 08-ansible-05-testing
# Домашнее задание к занятию "08.05 Тестирование Roles"

### Важно!!!
По [ссылке](https://github.com/netology-code/mnt-homeworks/tree/MNT-13/08-ansible-05-testing) можно найти обновленное домашнее задание к занятию "Тестирование Roles". В этом задании используется другой стек технологий, позволяющий заменить ELK, в связи с запретом на установку стека ELK из официальных источников.
Все новые продукты являются аналогами старых, что позволяет не только безболезненно выполнить задания, но и познакомиться с возможными будущими заменами в стандартном ELK стеке внутри той компаниий, где вы будете работать.
Желаем успехов в выполнении задания!
## Подготовка к выполнению
1. Установите molecule: `pip3 install "molecule==3.4.0"`
2. Соберите локальный образ на основе [Dockerfile](./Dockerfile)

## Основная часть

Наша основная цель - настроить тестирование наших ролей. Задача: сделать сценарии тестирования для kibana, filebeat. Ожидаемый результат: все сценарии успешно проходят тестирование ролей.

### Molecule

1. Запустите  `molecule test` внутри корневой директории elasticsearch-role, посмотрите на вывод команды.
````
# molecule test
INFO     default scenario test matrix: dependency, lint, cleanup, destroy, syntax, create, prepare, converge, idempotence, side_effect, verify, cleanup, destroy
INFO     Performing prerun with role_name_check=0...
INFO     Set ANSIBLE_LIBRARY=/root/.cache/ansible-compat/785bcd/modules:/root/.ansible/plugins/modules:/usr/share/ansible/plugins/modules
INFO     Set ANSIBLE_COLLECTIONS_PATH=/root/.cache/ansible-compat/785bcd/collections:/root/.ansible/collections:/usr/share/ansible/collections
INFO     Set ANSIBLE_ROLES_PATH=/root/.cache/ansible-compat/785bcd/roles:/root/.ansible/roles:/usr/share/ansible/roles:/etc/ansible/roles
INFO     Using /root/.cache/ansible-compat/785bcd/roles/myspace1.elc_role symlink to current repository in order to enable Ansible to find the role using its expected full name.
INFO     Running default > dependency
WARNING  Skipping, missing the requirements file.
WARNING  Skipping, missing the requirements file.
INFO     Running default > lint
INFO     Lint is disabled.
INFO     Running default > cleanup
WARNING  Skipping, cleanup playbook not configured.
INFO     Running default > destroy
INFO     Sanity checks: 'docker'

PLAY [Destroy] *****************************************************************

TASK [Destroy molecule instance(s)] ********************************************
changed: [localhost] => (item=instance)

TASK [Wait for instance(s) deletion to complete] *******************************
ok: [localhost] => (item=instance)

TASK [Delete docker networks(s)] ***********************************************

PLAY RECAP *********************************************************************
localhost                  : ok=2    changed=1    unreachable=0    failed=0    skipped=1    rescued=0    ignored=0

INFO     Running default > syntax

playbook: /root/homework/08-ansible-05-testing/elc_role/molecule/default/converge.yml
INFO     Running default > create

PLAY [Create] ******************************************************************

TASK [Log into a Docker registry] **********************************************
skipping: [localhost] => (item=None)
skipping: [localhost]

TASK [Check presence of custom Dockerfiles] ************************************
ok: [localhost] => (item={'image': 'docker.io/pycontribs/centos:7', 'name': 'instance', 'pre_build_image': True})

TASK [Create Dockerfiles from image names] *************************************
skipping: [localhost] => (item={'image': 'docker.io/pycontribs/centos:7', 'name': 'instance', 'pre_build_image': True})

TASK [Discover local Docker images] ********************************************
ok: [localhost] => (item={'changed': False, 'skipped': True, 'skip_reason': 'Conditional result was False', 'item': {'image': 'docker.io/pycontribs/centos:7', 'name': 'instance', 'pre_build_image': True}, 'ansible_loop_var': 'item', 'i': 0, 'ansible_index_var': 'i'})

TASK [Build an Ansible compatible image (new)] *********************************
skipping: [localhost] => (item=molecule_local/docker.io/pycontribs/centos:7)

TASK [Create docker network(s)] ************************************************

TASK [Determine the CMD directives] ********************************************
ok: [localhost] => (item={'image': 'docker.io/pycontribs/centos:7', 'name': 'instance', 'pre_build_image': True})

TASK [Create molecule instance(s)] *********************************************
changed: [localhost] => (item=instance)

TASK [Wait for instance(s) creation to complete] *******************************
FAILED - RETRYING: [localhost]: Wait for instance(s) creation to complete (300 retries left).
FAILED - RETRYING: [localhost]: Wait for instance(s) creation to complete (299 retries left).
changed: [localhost] => (item={'failed': 0, 'started': 1, 'finished': 0, 'ansible_job_id': '322765941837.7618', 'results_file': '/root/.ansible_async/322765941837.7618', 'changed': True, 'item': {'image': 'docker.io/pycontribs/centos:7', 'name': 'instance', 'pre_build_image': True}, 'ansible_loop_var': 'item'})

PLAY RECAP *********************************************************************
localhost                  : ok=5    changed=2    unreachable=0    failed=0    skipped=4    rescued=0    ignored=0

INFO     Running default > prepare
WARNING  Skipping, prepare playbook not configured.
INFO     Running default > converge

PLAY [Converge] ****************************************************************

TASK [Gathering Facts] *********************************************************
ok: [instance]

TASK [Include elc-role] ********************************************************

TASK [elc_role : Upload tar.gz Elasticsearch from remote URL] ******************
changed: [instance]

TASK [elc_role : Create directrory for Elasticsearch] **************************
changed: [instance]

TASK [elc_role : Extract Elasticsearch in the installation directory] **********
changed: [instance]

TASK [elc_role : Set environment Elastic] **************************************
changed: [instance]

PLAY RECAP *********************************************************************
instance                   : ok=5    changed=4    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

INFO     Running default > idempotence

PLAY [Converge] ****************************************************************

TASK [Gathering Facts] *********************************************************
ok: [instance]

TASK [Include elc-role] ********************************************************

TASK [elc_role : Upload tar.gz Elasticsearch from remote URL] ******************
ok: [instance]

TASK [elc_role : Create directrory for Elasticsearch] **************************
ok: [instance]

TASK [elc_role : Extract Elasticsearch in the installation directory] **********
skipping: [instance]

TASK [elc_role : Set environment Elastic] **************************************
ok: [instance]

PLAY RECAP *********************************************************************
instance                   : ok=4    changed=0    unreachable=0    failed=0    skipped=1    rescued=0    ignored=0

INFO     Idempotence completed successfully.
INFO     Running default > side_effect
WARNING  Skipping, side effect playbook not configured.
INFO     Running default > verify
INFO     Running Ansible Verifier

PLAY [Verify] ******************************************************************

TASK [Check Elastic version] ***************************************************
ok: [instance]

PLAY RECAP *********************************************************************
instance                   : ok=1    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

INFO     Verifier completed successfully.
INFO     Running default > cleanup
WARNING  Skipping, cleanup playbook not configured.
INFO     Running default > destroy

PLAY [Destroy] *****************************************************************

TASK [Destroy molecule instance(s)] ********************************************
changed: [localhost] => (item=instance)

TASK [Wait for instance(s) deletion to complete] *******************************
FAILED - RETRYING: [localhost]: Wait for instance(s) deletion to complete (300 retries left).
changed: [localhost] => (item=instance)

TASK [Delete docker networks(s)] ***********************************************

PLAY RECAP *********************************************************************
localhost                  : ok=2    changed=2    unreachable=0    failed=0    skipped=1    rescued=0    ignored=0

INFO     Pruning extra files from scenario ephemeral directory
````
2. Перейдите в каталог с ролью kibana-role и создайте сценарий тестирования по умолчаню при помощи `molecule init scenario --driver-name docker`.
3. Добавьте несколько разных дистрибутивов (centos:8, ubuntu:latest) для инстансов и протестируйте роль, исправьте найденные ошибки, если они есть.
````
 molecule test
INFO     default scenario test matrix: dependency, lint, cleanup, destroy, syntax, create, prepare, converge, idempotence, side_effect, verify, cleanup, destroy
INFO     Performing prerun with role_name_check=0...
INFO     Set ANSIBLE_LIBRARY=/root/.cache/ansible-compat/002ddc/modules:/root/.ansible/plugins/modules:/usr/share/ansible/plugins/modules
INFO     Set ANSIBLE_COLLECTIONS_PATH=/root/.cache/ansible-compat/002ddc/collections:/root/.ansible/collections:/usr/share/ansible/collections
INFO     Set ANSIBLE_ROLES_PATH=/root/.cache/ansible-compat/002ddc/roles:/root/.ansible/roles:/usr/share/ansible/roles:/etc/ansible/roles
INFO     Using /root/.cache/ansible-compat/002ddc/roles/myspace1.kibana_role symlink to current repository in order to enable Ansible to find the role using its expected full name.
INFO     Running default > dependency
WARNING  Skipping, missing the requirements file.
WARNING  Skipping, missing the requirements file.
INFO     Running default > lint
INFO     Lint is disabled.
INFO     Running default > cleanup
WARNING  Skipping, cleanup playbook not configured.
INFO     Running default > destroy
INFO     Sanity checks: 'docker'

PLAY [Destroy] *****************************************************************

TASK [Destroy molecule instance(s)] ********************************************
changed: [localhost] => (item=centos7)
changed: [localhost] => (item=ubuntu)

TASK [Wait for instance(s) deletion to complete] *******************************
ok: [localhost] => (item=centos7)
ok: [localhost] => (item=ubuntu)

TASK [Delete docker networks(s)] ***********************************************

PLAY RECAP *********************************************************************
localhost                  : ok=2    changed=1    unreachable=0    failed=0    skipped=1    rescued=0    ignored=0

INFO     Running default > syntax

playbook: /root/homework/08-ansible-04-role/1/kibana_role/molecule/default/converge.yml
INFO     Running default > create

PLAY [Create] ******************************************************************

TASK [Log into a Docker registry] **********************************************
skipping: [localhost] => (item=None)
skipping: [localhost] => (item=None)
skipping: [localhost]

TASK [Check presence of custom Dockerfiles] ************************************
ok: [localhost] => (item={'image': 'docker.io/pycontribs/centos:7', 'name': 'centos7', 'pre_build_image': True})
ok: [localhost] => (item={'image': 'docker.io/pycontribs/ubuntu:latest', 'name': 'ubuntu', 'pre_build_image': True})

TASK [Create Dockerfiles from image names] *************************************
skipping: [localhost] => (item={'image': 'docker.io/pycontribs/centos:7', 'name': 'centos7', 'pre_build_image': True})
skipping: [localhost] => (item={'image': 'docker.io/pycontribs/ubuntu:latest', 'name': 'ubuntu', 'pre_build_image': True})

TASK [Discover local Docker images] ********************************************
ok: [localhost] => (item={'changed': False, 'skipped': True, 'skip_reason': 'Conditional result was False', 'item': {'image': 'docker.io/pycontribs/centos:7', 'name': 'centos7', 'pre_build_image': True}, 'ansible_loop_var': 'item', 'i': 0, 'ansible_index_var': 'i'})
ok: [localhost] => (item={'changed': False, 'skipped': True, 'skip_reason': 'Conditional result was False', 'item': {'image': 'docker.io/pycontribs/ubuntu:latest', 'name': 'ubuntu', 'pre_build_image': True}, 'ansible_loop_var': 'item', 'i': 1, 'ansible_index_var': 'i'})

TASK [Build an Ansible compatible image (new)] *********************************
skipping: [localhost] => (item=molecule_local/docker.io/pycontribs/centos:7)
skipping: [localhost] => (item=molecule_local/docker.io/pycontribs/ubuntu:latest)

TASK [Create docker network(s)] ************************************************

TASK [Determine the CMD directives] ********************************************
ok: [localhost] => (item={'image': 'docker.io/pycontribs/centos:7', 'name': 'centos7', 'pre_build_image': True})
ok: [localhost] => (item={'image': 'docker.io/pycontribs/ubuntu:latest', 'name': 'ubuntu', 'pre_build_image': True})

TASK [Create molecule instance(s)] *********************************************
changed: [localhost] => (item=centos7)
changed: [localhost] => (item=ubuntu)

TASK [Wait for instance(s) creation to complete] *******************************
FAILED - RETRYING: [localhost]: Wait for instance(s) creation to complete (300 retries left).
FAILED - RETRYING: [localhost]: Wait for instance(s) creation to complete (299 retries left).
FAILED - RETRYING: [localhost]: Wait for instance(s) creation to complete (298 retries left).
FAILED - RETRYING: [localhost]: Wait for instance(s) creation to complete (297 retries left).
changed: [localhost] => (item={'failed': 0, 'started': 1, 'finished': 0, 'ansible_job_id': '478298441815.16706', 'results_file': '/root/.ansible_async/478298441815.16706', 'changed': True, 'item': {'image': 'docker.io/pycontribs/centos:7', 'name': 'centos7', 'pre_build_image': True}, 'ansible_loop_var': 'item'})
FAILED - RETRYING: [localhost]: Wait for instance(s) creation to complete (300 retries left).
FAILED - RETRYING: [localhost]: Wait for instance(s) creation to complete (299 retries left).
FAILED - RETRYING: [localhost]: Wait for instance(s) creation to complete (298 retries left).
changed: [localhost] => (item={'failed': 0, 'started': 1, 'finished': 0, 'ansible_job_id': '475778383830.16732', 'results_file': '/root/.ansible_async/475778383830.16732', 'changed': True, 'item': {'image': 'docker.io/pycontribs/ubuntu:latest', 'name': 'ubuntu', 'pre_build_image': True}, 'ansible_loop_var': 'item'})

PLAY RECAP *********************************************************************
localhost                  : ok=5    changed=2    unreachable=0    failed=0    skipped=4    rescued=0    ignored=0

INFO     Running default > prepare
WARNING  Skipping, prepare playbook not configured.
INFO     Running default > converge

PLAY [Converge] ****************************************************************

TASK [Gathering Facts] *********************************************************
ok: [ubuntu]
ok: [centos7]

TASK [Include kibana_role] *****************************************************

TASK [kibana_role : Upload tar.gz Kibana from remote URL] **********************
changed: [ubuntu]
changed: [centos7]

TASK [kibana_role : Create directrory for Kibana (/opt/kibana/7.10.1)] *********
changed: [ubuntu]
changed: [centos7]

TASK [kibana_role : Extract Kibana in the installation directory] **************
changed: [ubuntu]
changed: [centos7]

TASK [kibana_role : Set environment Kibana] ************************************
changed: [centos7]
changed: [ubuntu]

PLAY RECAP *********************************************************************
centos7                    : ok=5    changed=4    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
ubuntu                     : ok=5    changed=4    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

INFO     Running default > idempotence

PLAY [Converge] ****************************************************************

TASK [Gathering Facts] *********************************************************
ok: [ubuntu]
ok: [centos7]

TASK [Include kibana_role] *****************************************************

TASK [kibana_role : Upload tar.gz Kibana from remote URL] **********************
ok: [centos7]
ok: [ubuntu]

TASK [kibana_role : Create directrory for Kibana (/opt/kibana/7.10.1)] *********
ok: [centos7]
ok: [ubuntu]

TASK [kibana_role : Extract Kibana in the installation directory] **************
skipping: [ubuntu]
skipping: [centos7]

TASK [kibana_role : Set environment Kibana] ************************************
ok: [centos7]
ok: [ubuntu]

PLAY RECAP *********************************************************************
centos7                    : ok=4    changed=0    unreachable=0    failed=0    skipped=1    rescued=0    ignored=0
ubuntu                     : ok=4    changed=0    unreachable=0    failed=0    skipped=1    rescued=0    ignored=0

INFO     Idempotence completed successfully.
INFO     Running default > side_effect
WARNING  Skipping, side effect playbook not configured.
INFO     Running default > verify
INFO     Running Ansible Verifier

PLAY [Verify] ******************************************************************

TASK [Example assertion] *******************************************************
ok: [centos7] => {
    "changed": false,
    "msg": "All assertions passed"
}
ok: [ubuntu] => {
    "changed": false,
    "msg": "All assertions passed"
}

PLAY RECAP *********************************************************************
centos7                    : ok=1    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
ubuntu                     : ok=1    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

INFO     Verifier completed successfully.
INFO     Running default > cleanup
WARNING  Skipping, cleanup playbook not configured.
INFO     Running default > destroy

PLAY [Destroy] *****************************************************************

TASK [Destroy molecule instance(s)] ********************************************
changed: [localhost] => (item=centos7)
changed: [localhost] => (item=ubuntu)

TASK [Wait for instance(s) deletion to complete] *******************************
FAILED - RETRYING: [localhost]: Wait for instance(s) deletion to complete (300 retries left).
FAILED - RETRYING: [localhost]: Wait for instance(s) deletion to complete (299 retries left).
changed: [localhost] => (item=centos7)
changed: [localhost] => (item=ubuntu)

TASK [Delete docker networks(s)] ***********************************************

PLAY RECAP *********************************************************************
localhost                  : ok=2    changed=2    unreachable=0    failed=0    skipped=1    rescued=0    ignored=0

INFO     Pruning extra files from scenario ephemeral directory
````
4. Добавьте несколько assert'ов в verify.yml файл, для  проверки работоспособности kibana-role (проверка, что web отвечает, проверка логов, etc). Запустите тестирование роли повторно и проверьте, что оно прошло успешно.
````
hosts: all
  gather_facts: false
  tasks:
  - name: Show  Kibana version
    shell:
      cmd: /opt/kibana/7.10.1/bin/kibana --version --allow-root

TASK [Show  Kibana version] ****************************************************
ok: [centos7]
ok: [ubuntu]
````
5. Повторите шаги 2-4 для filebeat-role.
6. Добавьте новый тег на коммит с рабочим сценарием в соответствии с семантическим версионированием.

### Tox

1. Запустите `docker run --privileged=True -v <path_to_repo>:/opt/elasticsearch-role -w /opt/elasticsearch-role -it <image_name> /bin/bash`, 
где path_to_repo - путь до корня репозитория с elasticsearch-role на вашей файловой системе

````
Не работает в таком виде, я так и не смог победить ошибку (см. скриншот ниже)
````

2. Внутри контейнера выполните команду `tox`, посмотрите на вывод.
3. Добавьте файл `tox.ini` в корень репозитория каждой своей роли.
4. Создайте облегчённый сценарий для `molecule`. Проверьте его на исполнимость.
5. Пропишите правильную команду в `tox.ini` для того чтобы запускался облегчённый сценарий.
````

````
6. Запустите `docker` контейнер так, чтобы внутри оказались обе ваши роли.
7. Зайдти поочерёдно в каждую из них и запустите команду `tox`. Убедитесь, что всё отработало успешно.
````
Т.к. в контейнере с podman не заработало, пришлось запускать на "железе" и на docker.
Из нерешенного - почему-то не определяется статус сервиса при перезапуске в соотв таске... 


tox
py39-ansible30 installed: ansible==3.0.0,ansible-base==2.10.17,ansible-compat==2.2.0,ansible-lint==5.1.3,arrow==1.2.2,attrs==22.1.0,bcrypt==4.0.0,binaryornot==0.4.4,bracex==2.3.post1,Cerberus==1.3.2,certifi==2022.6.15,cffi==1.15.1,chardet==5.0.0,charset-normalizer==2.1.1,click==8.1.3,click-help-colors==0.9.1,commonmark==0.9.1,cookiecutter==2.1.1,cryptography==37.0.4,distro==1.7.0,docker==6.0.0,enrich==1.2.7,idna==3.3,Jinja2==3.1.2,jinja2-time==0.2.0,jmespath==1.0.1,jsonschema==4.14.0,lxml==4.9.1,MarkupSafe==2.1.1,molecule==3.4.0,molecule-docker==1.1.0,packaging==21.3,paramiko==2.11.0,pathspec==0.9.0,pluggy==0.13.1,pycparser==2.21,Pygments==2.13.0,PyNaCl==1.5.0,pyparsing==3.0.9,pyrsistent==0.18.1,python-dateutil==2.8.2,python-slugify==6.1.2,PyYAML==5.4.1,requests==2.28.1,rich==12.5.1,ruamel.yaml==0.17.21,ruamel.yaml.clib==0.2.6,selinux==0.2.1,six==1.16.0,subprocess-tee==0.3.5,tenacity==8.0.1,text-unidecode==1.3,urllib3==1.26.12,wcmatch==8.4,websocket-client==1.4.0,yamllint==1.26.3
py39-ansible30 run-test-pre: PYTHONHASHSEED='3179525851'
py39-ansible30 run-test: commands[0] | molecule test -s alternative --destroy always
INFO     alternative scenario test matrix: destroy, create, converge, destroy
INFO     Performing prerun...
INFO     Guessed /root/mnt-homeworks-ansible as project root directory
WARNING  Computed fully qualified role name of elasticsearch_role does not follow current galaxy requirements.
Please edit meta/main.yml and assure we can correctly determine full role name:

galaxy_info:
role_name: my_name  # if absent directory name hosting role is used instead
namespace: my_galaxy_namespace  # if absent, author is used instead

Namespace: https://galaxy.ansible.com/docs/contributing/namespaces.html#galaxy-namespace-limitations
Role: https://galaxy.ansible.com/docs/contributing/creating_role.html#role-names

As an alternative, you can add 'role-name' to either skip_list or warn_list.

INFO     Using /root/.cache/ansible-lint/e44800/roles/elasticsearch_role symlink to current repository in order to enable Ansible to find the role using its expected full name.
INFO     Added ANSIBLE_ROLES_PATH=~/.ansible/roles:/usr/share/ansible/roles:/etc/ansible/roles:/root/.cache/ansible-lint/e44800/roles
INFO     Running alternative > destroy
INFO     Sanity checks: 'docker'
[WARNING]: Collection community.docker does not support Ansible version 2.10.17

PLAY [Destroy] *****************************************************************

TASK [Destroy molecule instance(s)] ********************************************
changed: [localhost] => (item=instance1)

TASK [Wait for instance(s) deletion to complete] *******************************
FAILED - RETRYING: Wait for instance(s) deletion to complete (300 retries left).
ok: [localhost] => (item=instance1)

TASK [Delete docker networks(s)] ***********************************************

PLAY RECAP *********************************************************************
localhost                  : ok=2    changed=1    unreachable=0    failed=0    skipped=1    rescued=0    ignored=0

INFO     Running alternative > create
[WARNING]: Collection community.docker does not support Ansible version 2.10.17

PLAY [Create] ******************************************************************

TASK [Log into a Docker registry] **********************************************
skipping: [localhost] => (item=None)
skipping: [localhost]

TASK [Check presence of custom Dockerfiles] ************************************
ok: [localhost] => (item={'image': 'docker.io/pycontribs/centos:7', 'name': 'instance1', 'pre_build_image': True})

TASK [Create Dockerfiles from image names] *************************************
skipping: [localhost] => (item={'image': 'docker.io/pycontribs/centos:7', 'name': 'instance1', 'pre_build_image': True})

TASK [Discover local Docker images] ********************************************
ok: [localhost] => (item={'changed': False, 'skipped': True, 'skip_reason': 'Conditional result was False', 'item': {'image': 'docker.io/pycontribs/centos:7', 'name': 'instance1', 'pre_build_image': True}, 'ansible_loop_var': 'item', 'i': 0, 'ansible_index_var': 'i'})

TASK [Build an Ansible compatible image (new)] *********************************
skipping: [localhost] => (item=molecule_local/docker.io/pycontribs/centos:7)

TASK [Create docker network(s)] ************************************************

TASK [Determine the CMD directives] ********************************************
ok: [localhost] => (item={'image': 'docker.io/pycontribs/centos:7', 'name': 'instance1', 'pre_build_image': True})

TASK [Create molecule instance(s)] *********************************************
changed: [localhost] => (item=instance1)

TASK [Wait for instance(s) creation to complete] *******************************
FAILED - RETRYING: Wait for instance(s) creation to complete (300 retries left).
changed: [localhost] => (item={'started': 1, 'finished': 0, 'ansible_job_id': '610139331289.36159', 'results_file': '/root/.ansible_async/610139331289.36159', 'changed': True, 'failed': False, 'item': {'image': 'docker.io/pycontribs/centos:7', 'name': 'instance1', 'pre_build_image': True}, 'ansible_loop_var': 'item'})

PLAY RECAP *********************************************************************
localhost                  : ok=5    changed=2    unreachable=0    failed=0    skipped=4    rescued=0    ignored=0

INFO     Running alternative > converge

PLAY [Converge] ****************************************************************

TASK [Gathering Facts] *********************************************************
[WARNING]: Collection community.docker does not support Ansible version 2.10.17
ok: [instance1]

TASK [Include mnt-homeworks-ansible] *******************************************

TASK [elasticsearch_role : Fail if unsupported system detected] ****************
skipping: [instance1]

TASK [elasticsearch_role : include_tasks] **************************************
included: /root/mnt-homeworks-ansible/tasks/download_yum.yml for instance1

TASK [elasticsearch_role : Download Elasticsearch's rpm] ***********************
ok: [instance1 -> localhost]

TASK [elasticsearch_role : Copy Elasticsearch to managed node] *****************
[WARNING]: Collection community.docker does not support Ansible version 2.10.17
changed: [instance1]

TASK [elasticsearch_role : include_tasks] **************************************
included: /root/mnt-homeworks-ansible/tasks/install_yum.yml for instance1

TASK [elasticsearch_role : Install Elasticsearch] ******************************
[WARNING]: Collection community.docker does not support Ansible version 2.10.17
changed: [instance1]

TASK [elasticsearch_role : Configure Elasticsearch] ****************************
[WARNING]: Collection community.docker does not support Ansible version 2.10.17
changed: [instance1]

RUNNING HANDLER [elasticsearch_role : restart Elasticsearch] *******************
[WARNING]: Collection community.docker does not support Ansible version 2.10.17
fatal: [instance1]: FAILED! => {"changed": false, "msg": "Service is in unknown state", "status": {}}

NO MORE HOSTS LEFT *************************************************************

PLAY RECAP *********************************************************************
instance1                  : ok=7    changed=3    unreachable=0    failed=1    skipped=1    rescued=0    ignored=0

CRITICAL Ansible return code was 2, command was: ['ansible-playbook', '--inventory', '/root/.cache/molecule/mnt-homeworks-ansible/alternative/inventory', '--skip-tags', 'molecule-notest,notest', '/root/mnt-homeworks-ansible/molecule/alternative/converge.yml']
WARNING  An error occurred during the test sequence action: 'converge'. Cleaning up.
INFO     Running alternative > cleanup
WARNING  Skipping, cleanup playbook not configured.
INFO     Running alternative > destroy
[WARNING]: Collection community.docker does not support Ansible version 2.10.17

PLAY [Destroy] *****************************************************************

TASK [Destroy molecule instance(s)] ********************************************
changed: [localhost] => (item=instance1)

TASK [Wait for instance(s) deletion to complete] *******************************
FAILED - RETRYING: Wait for instance(s) deletion to complete (300 retries left).
changed: [localhost] => (item=instance1)

TASK [Delete docker networks(s)] ***********************************************

PLAY RECAP *********************************************************************
localhost                  : ok=2    changed=2    unreachable=0    failed=0    skipped=1    rescued=0    ignored=0

INFO     Pruning extra files from scenario ephemeral directory

````
8. Добавьте новый тег на коммит с рабочим сценарием в соответствии с семантическим версионированием.

После выполнения у вас должно получится два сценария molecule и один tox.ini файл в каждом репозитории. Ссылки на репозитории являются ответами на домашнее задание. Не забудьте указать в ответе теги решений Tox и Molecule заданий.