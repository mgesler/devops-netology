# devops-netology  
Home work 08-ansible-01-base

Домашнее задание к занятию "08.01 Введение в Ansible"  
Подготовка к выполнению  
Установите ansible версии 2.10 или выше.  
Создайте свой собственный публичный репозиторий на github с произвольным именем.  
Скачайте playbook из репозитория с домашним заданием и перенесите его в свой репозиторий.  

Основная часть  
Попробуйте запустить playbook на окружении из test.yml, зафиксируйте какое значение имеет факт some_fact для указанного хоста при выполнении playbook'a.
````
ansible-playbook site.yml  -i inventory/test.yml

TASK [Print fact] ***********************************************************************************
ok: [localhost] => {
    "msg": 12
}

````
Найдите файл с переменными (group_vars) в котором задаётся найденное в первом пункте значение и поменяйте его на 'all default fact'.
````
TASK [Print fact] ***********************************************************************************************************************************************
ok: [localhost] => {
    "msg": "all default fact"
}

````
Воспользуйтесь подготовленным (используется docker) или создайте собственное окружение для проведения дальнейших испытаний.
Проведите запуск playbook на окружении из prod.yml. Зафиксируйте полученные значения some_fact для каждого из managed host.
Добавьте факты в group_vars каждой из групп хостов так, чтобы для some_fact получились следующие значения: для deb - 'deb default fact', для el - 'el default fact'.
Повторите запуск playbook на окружении prod.yml. Убедитесь, что выдаются корректные значения для всех хостов.
````
TASK [Print OS] *******************************************************************************************
ok: [centos7] => {
    "msg": "CentOS"
}
ok: [ubuntu] => {
    "msg": "Ubuntu"
}

TASK [Print fact] *****************************************************************************************
ok: [centos7] => {
    "msg": "el"
}
ok: [ubuntu] => {
    "msg": "deb"
}


TASK [Print OS] *******************************************************************************************
ok: [centos7] => {
    "msg": "CentOS"
}
ok: [ubuntu] => {
    "msg": "Ubuntu"
}

TASK [Print fact] *****************************************************************************************
ok: [centos7] => {
    "msg": "el default fact"
}
ok: [ubuntu] => {
    "msg": "deb default fact"
}


````

При помощи ansible-vault зашифруйте факты в group_vars/deb и group_vars/el с паролем netology.
Запустите playbook на окружении prod.yml. При запуске ansible должен запросить у вас пароль. Убедитесь в работоспособности.
````
Чтобы зашифровать именно значение факта, а не весь файл, его содержащий, шифруем значение факта как строку
ansible-vault encrypt_string "deb default fact"
ansible-vault encrypt_string "el default fact"
подставляем полученное значение вместо значения соотв факта 
и при дальнейшем использовании добавляем ключ  
--ask-vault-password

ansible-playbook site.yml  -i inventory/prod.yml --ask-vault-password
Vault password: 

PLAY [Print os facts] ******************************************************************************************************************

TASK [Gathering Facts] *****************************************************************************************************************
ok: [ubuntu]
ok: [centos7]

TASK [Print OS] ************************************************************************************************************************
ok: [ubuntu] => {
    "msg": "Ubuntu"
}
ok: [centos7] => {
    "msg": "CentOS"
}

TASK [Print fact] **********************************************************************************************************************
ok: [centos7] => {
    "msg": "el default fact"
}
ok: [ubuntu] => {
    "msg": "deb default fact"
}

PLAY RECAP *****************************************************************************************************************************
centos7                    : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
ubuntu                     : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   

````

Посмотрите при помощи ansible-doc список плагинов для подключения. Выберите подходящий для работы на control node.
````
ansible-doc -l -t connection
[WARNING]: Collection frr.frr does not support Ansible version 2.12.2
[WARNING]: Collection ibm.qradar does not support Ansible version 2.12.2
[WARNING]: Collection splunk.es does not support Ansible version 2.12.2
ansible.netcommon.httpapi      Use httpapi to run command on network appliances                                                    
ansible.netcommon.libssh       (Tech preview) Run tasks using libssh for ssh connection                                            
ansible.netcommon.napalm       Provides persistent connection using NAPALM                                                         
ansible.netcommon.netconf      Provides a persistent connection using the netconf protocol                                         
ansible.netcommon.network_cli  Use network_cli to run command on network appliances                                                
ansible.netcommon.persistent   Use a persistent unix socket for connection                                                         
community.aws.aws_ssm          execute via AWS Systems Manager                                                                     
community.docker.docker        Run tasks in docker containers                                                                      
community.docker.docker_api    Run tasks in docker containers                                                                      
community.docker.nsenter       execute on host running controller container                                                        
community.general.chroot       Interact with local chroot                                                                          
community.general.funcd        Use funcd to connect to target                                                                      
community.general.iocage       Run tasks in iocage jails                                                                           
community.general.jail         Run tasks in jails                                                                                  
community.general.lxc          Run tasks in lxc containers via lxc python library                                                  
community.general.lxd          Run tasks in lxc containers via lxc CLI                                                             
community.general.qubes        Interact with an existing QubesOS AppVM                                                             
community.general.saltstack    Allow ansible to piggyback on salt minions                                                          
community.general.zone         Run tasks in a zone instance                                                                        
community.libvirt.libvirt_lxc  Run tasks in lxc containers via libvirt                                                             
community.libvirt.libvirt_qemu Run tasks on libvirt/qemu virtual machines                                                          
community.okd.oc               Execute tasks in pods running on OpenShift                                                          
community.vmware.vmware_tools  Execute tasks inside a VM via VMware Tools                                                          
containers.podman.buildah      Interact with an existing buildah container                                                         
containers.podman.podman       Interact with an existing podman container                                                          
debops.debops.lxc_ssh          connect via ssh and lxc to remote lxc guest                                                         
kubernetes.core.kubectl        Execute tasks in pods running on Kubernetes                                                         
local                          execute on controller                                                                               
paramiko_ssh                   Run tasks via python ssh (paramiko)                                                                 
psrp                           Run tasks over Microsoft PowerShell Remoting Protocol                                               
ssh                            connect via SSH client binary                                                                       
winrm                          Run tasks over Microsoft's WinRM      

на control node логично использовать local, но допустимо использование и других plugin, 
в зависимости от условий задачи.          
````

В prod.yml добавьте новую группу хостов с именем local, в ней разместите localhost с необходимым типом подключения.
Запустите playbook на окружении prod.yml. При запуске ansible должен запросить у вас пароль. 
Убедитесь что факты some_fact для каждого из хостов определены из верных group_vars.
````
ansible-playbook site.yml  -i inventory/prod.yml --ask-vault-password
Vault password: 

PLAY [Print os facts] ******************************************************************************************************************

TASK [Gathering Facts] *****************************************************************************************************************
ok: [localhost]
ok: [ubuntu]
ok: [centos7]

TASK [Print OS] ************************************************************************************************************************
ok: [localhost] => {
    "msg": "Debian"
}
ok: [centos7] => {
    "msg": "CentOS"
}
ok: [ubuntu] => {
    "msg": "Ubuntu"
}

TASK [Print fact] **********************************************************************************************************************
ok: [localhost] => {
    "msg": "all default fact"
}
ok: [ubuntu] => {
    "msg": "deb default fact"
}
ok: [centos7] => {
    "msg": "el default fact"
}

PLAY RECAP *****************************************************************************************************************************
centos7                    : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
localhost                  : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
ubuntu                     : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   

````

Заполните README.md ответами на вопросы. Сделайте git push в ветку master. В ответе отправьте ссылку на ваш открытый репозиторий с изменённым playbook и заполненным README.md.возможно вам понадобится доработать elasticsearch.yml в части директивы path.repo и перезапустить elasticsearch