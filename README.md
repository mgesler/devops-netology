# devops-netology
Home work 03-sysadmin-03-os

Домашнее задание к занятию "3.3. Операционные системы, лекция 1"

1. Какой системный вызов делает команда cd? В прошлом ДЗ мы выяснили, что cd не является самостоятельной программой, это shell builtin, поэтому запустить strace непосредственно на cd не получится. Тем не менее, вы можете запустить strace на /bin/bash -c 'cd /tmp'. В этом случае вы увидите полный список системных вызовов, которые делает сам bash при старте. Вам нужно найти тот единственный, который относится именно к cd. Обратите внимание, что strace выдаёт результат своей работы в поток stderr, а не в stdout.
> chdir("/tmp")                           = 0

2. Попробуйте использовать команду file на объекты разных типов на файловой системе. Например:  
vagrant@netology1:~$ file /dev/tty  
/dev/tty: character special (5/0)  
vagrant@netology1:~$ file /dev/sda  
/dev/sda: block special (8/0)  
vagrant@netology1:~$ file /bin/bash  
/bin/bash: ELF 64-bit LSB shared object, x86-64  
Используя strace выясните, где находится база данных file на основании которой она делает свои догадки.  
> Один из этих файлов:
>openat(AT_FDCWD, "/etc/magic.mgc", O_RDONLY) = -1 ENOENT (No such file or directory)
>openat(AT_FDCWD, "/etc/magic", O_RDONLY) = 6
>openat(AT_FDCWD, "/usr/share/misc/magic.mgc", O_RDONLY) = 6

3. Предположим, приложение пишет лог в текстовый файл. Этот файл оказался удален (deleted в lsof), однако возможности сигналом сказать приложению переоткрыть файлы или просто перезапустить приложение – нет. 
Так как приложение продолжает писать в удаленный файл, место на диске постепенно заканчивается. Основываясь на знаниях о перенаправлении потоков предложите способ обнуления открытого удаленного файла (чтобы освободить место на файловой системе).  
> Допустим, приложение пишет лог в 1.txt
> lsof | grep 1.txt - получим файловый дескриптор и PID
> mc        18903                          root   10w      REG              253,0        1         18 /1.txt
> echo > /proc/18903/fd/10

4. Занимают ли зомби-процессы какие-то ресурсы в ОС (CPU, RAM, IO)?
> Нет
5. В iovisor BCC есть утилита opensnoop:
root@vagrant:~# dpkg -L bpfcc-tools | grep sbin/opensnoop
/usr/sbin/opensnoop-bpfcc
На какие файлы вы увидели вызовы группы open за первую секунду работы утилиты? Воспользуйтесь пакетом bpfcc-tools для Ubuntu 20.04. Дополнительные сведения по установке.
Какой системный вызов использует uname -a? Приведите цитату из man по этому системному вызову, где описывается альтернативное местоположение в /proc, где можно узнать версию ядра и релиз ОС.
root@vagrant:/home/vagrant# /usr/sbin/opensnoop-bpfcc
*
PID    COMM               FD ERR PATH  
879    vminfo              4   0 /var/run/utmp  
567    dbus-daemon        -1   2 /usr/local/share/dbus-1/system-services  
567    dbus-daemon        18   0 /usr/share/dbus-1/system-services  
567    dbus-daemon        -1   2 /lib/dbus-1/system-services  
567    dbus-daemon        18   0 /var/lib/snapd/dbus-1/system-services/  
586    irqbalance          6   0 /proc/interrupts  
586    irqbalance          6   0 /proc/stat  
586    irqbalance          6   0 /proc/irq/20/smp_affinity  
586    irqbalance          6   0 /proc/irq/0/smp_affinity  
586    irqbalance          6   0 /proc/irq/1/smp_affinity  
586    irqbalance          6   0 /proc/irq/8/smp_affinity  
586    irqbalance          6   0 /proc/irq/12/smp_affinity  
586    irqbalance          6   0 /proc/irq/14/smp_affinity  
586    irqbalance          6   0 /proc/irq/15/smp_affinity  
567    dbus-daemon        -1   2 /usr/local/share/dbus-1/system-services  
567    dbus-daemon        18   0 /usr/share/dbus-1/system-services  
567    dbus-daemon        -1   2 /lib/dbus-1/system-services  
567    dbus-daemon        18   0 /var/lib/snapd/dbus-1/system-services/  
879    vminfo              4   0 /var/run/utmp  
*

>Part of the utsname information is also accessible via
>       /proc/sys/kernel/{ostype, hostname, osrelease, version,
>       domainname}.

7. Чем отличается последовательность команд через ; и через && в bash? Например:
root@netology1:~# test -d /tmp/some_dir; echo Hi
Hi
root@netology1:~# test -d /tmp/some_dir && echo Hi
root@netology1:~#
Есть ли смысл использовать в bash &&, если применить set -e?
> ; - Последовательное безусловное выполнение 
> && - Последовательное выполнение при успешном выполнении предыдущей команды
> set -e Exit immediately if a command exits with a non-zero status. Конечно, имеет смысл использовать && ведь команда может завершиться и с нулевым статусом (успешно)

8. Из каких опций состоит режим bash set -euxo pipefail и почему его хорошо было бы использовать в сценариях?
> -e Если одна из команд скрипта завершается неуспешно, то весь скрипт немедленно завершается.
> -u Если работа с переменными завершается неуспешно, то весь скрирт немедленно завершается.
> -x Выполняемые команды отображаются на консоли
> pipefail - если одна из команд в pipe завершилась неуспешно, то весь pipe завершается с кодом ошибки этой команды.  
> Результат работы скрипта будет более предсказуем, если можно обрабатывать ошибки хотя бы на таком уровне 
9. Используя -o stat для ps, определите, какой наиболее часто встречающийся статус у процессов в системе. В man ps ознакомьтесь (/PROCESS STATE CODES) что значат дополнительные к основной заглавной буквы статуса процессов. Его можно не учитывать при расчете (считать S, Ss или Ssl равнозначными).

>S -  interruptible sleep, наиболее часто встречающийся 