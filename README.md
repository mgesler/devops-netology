# devops-netology
Home work 03-sysadmin-01-terminal

Домашнее задание к занятию "3.1. Работа в терминале, лекция 1"  


1. Установите средство виртуализации Oracle VirtualBox.  
> Сделано  
2. Установите средство автоматизации Hashicorp Vagrant.  

В вашем основном окружении подготовьте удобный для дальнейшей работы терминал.    
> Выбран Windows Terminal в Windows  

3. С помощью базового файла конфигурации запустите Ubuntu 20.04 в VirtualBox посредством Vagrant:  

- Создайте директорию, в которой будут храниться конфигурационные файлы Vagrant. В ней выполните vagrant init. Замените содержимое Vagrantfile по умолчанию следующим:  

 Vagrant.configure("2") do |config|  
 	config.vm.box = "bento/ubuntu-20.04"  
 end  
> Сделано  
> vagrant init
> A `Vagrantfile` has been placed in this directory. You are now
> ready to `vagrant up` your first virtual environment! Please read
> the comments in the Vagrantfile as well as documentation on
> `vagrantup.com` for more information on using Vagrant.


- Выполнение в этой директории vagrant up установит провайдер VirtualBox для Vagrant, скачает необходимый образ и запустит виртуальную машину.  
>
> vagrant up
> Bringing machine 'default' up with 'virtualbox' provider...
> ==> default: Box 'bento/ubuntu-20.04' could not be found. Attempting to find and install...
>    default: Box Provider: virtualbox
>    default: Box Version: >= 0
>==> default: Loading metadata for box 'bento/ubuntu-20.04'
>     default: URL: https://vagrantcloud.com/bento/ubuntu-20.04
> ==> default: Adding box 'bento/ubuntu-20.04' (v202107.28.0) for provider: virtualbox
>    default: Downloading: https://vagrantcloud.com/bento/boxes/ubuntu-20.04/versions/202107.28.0/providers/virtualbox.box
>    default:
> ==> default: Successfully added box 'bento/ubuntu-20.04' (v202107.28.0) for 'virtualbox'!
> ==> default: Importing base box 'bento/ubuntu-20.04'...
> ==> default: Matching MAC address for NAT networking...
> ==> default: Checking if box 'bento/ubuntu-20.04' version '202107.28.0' is up to date...
> ==> default: Setting the name of the VM: files_default_1633011561489_58955
> Vagrant is currently configured to create VirtualBox synced folders with
> the `SharedFoldersEnableSymlinksCreate` option enabled. If the Vagrant
> guest is not trusted, you may want to disable this option.

- vagrant suspend выключит виртуальную машину с сохранением ее состояния (т.е., при следующем vagrant up будут запущены все процессы внутри, которые работали на момент вызова suspend), vagrant halt выключит виртуальную машину штатным образом.

5. Ознакомьтесь с графическим интерфейсом VirtualBox, посмотрите как выглядит виртуальная машина, которую создал для вас Vagrant, какие аппаратные ресурсы ей выделены. Какие ресурсы выделены по-умолчанию?    
> 2cpu, 1gb, 64Gb hdd

6. Ознакомьтесь с возможностями конфигурации VirtualBox через Vagrantfile: документация. Как добавить оперативной памяти или ресурсов процессора виртуальной машине?
>
Изменением/добавлением следующих параметров Vagrantfile
config.vm.provider "virtualbox" do |v|  
  v.memory = 1024    
  v.cpus = 2  
end
>
8. Команда vagrant ssh из директории, в которой содержится Vagrantfile, позволит вам оказаться внутри виртуальной машины без каких-либо дополнительных настроек. Попрактикуйтесь в выполнении обсуждаемых команд в терминале Ubuntu.
> Работает!  

9. Ознакомиться с разделами man bash, почитать о настройках самого bash:

какой переменной можно задать длину журнала history, и на какой строчке manual это описывается?
> 862 строка, HISTSIZE 
что делает директива ignoreboth в bash?
> ignorespace — не сохранять строки начинающиеся с символа <пробел>  
> ignoredups — не сохранять строки, совпадающие с последней выполненной командой  
> ignoreboth = и ‘ignorespace’ и ‘ignoredups’  

В каких сценариях использования применимы скобки {} и на какой строчке man bash это описано?
> строка 257 списки (list)  
> 

10. С учётом ответа на предыдущий вопрос, как создать однократным вызовом touch 100000 файлов? Получится ли аналогичным образом создать 300000? Если нет, то почему?
> touch myfile{0001..0003}  
> 100000 получится, а 300000 тыс нет, надо будет увеличивать размер стека командой ulimit. Но точное соотношение "количество создаваемых файлов" - "размер стека" для меня неизвестно.    
11. В man bash поищите по /\[\[. Что делает конструкция [[ -d /tmp ]]
>Вычисляется выражение заключенное в [[]] с единым кодом возврата 1 или 0,  [[ -d /tmp ]] проверяет существование каталога /tmp  

12. Основываясь на знаниях о просмотре текущих (например, PATH) и установке новых переменных; командах, которые мы рассматривали, добейтесь в выводе type -a bash в виртуальной машине наличия первым пунктом в списке:

bash is /tmp/new_path_directory/bash
bash is /usr/local/bin/bash
bash is /bin/bash
(прочие строки могут отличаться содержимым и порядком) В качестве ответа приведите команды, которые позволили вам добиться указанного вывода или соответствующие скриншоты.
>export PATH="/tmp/new_path_directory:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin"  
> и копирование файла bash в соотв каталог
>
13. Чем отличается планирование команд с помощью batch и at?
>команда at используется для назначения одноразового задания на заданное время, а команда batch — для назначения одноразовых задач, которые должны выполняться, когда загрузка системы становится меньше 0,8.  








vagrant up
Bringing machine 'default' up with 'virtualbox' provider...
==> default: Box 'bento/ubuntu-20.04' could not be found. Attempting to find and install...
    default: Box Provider: virtualbox
    default: Box Version: >= 0
==> default: Loading metadata for box 'bento/ubuntu-20.04'
    default: URL: https://vagrantcloud.com/bento/ubuntu-20.04
==> default: Adding box 'bento/ubuntu-20.04' (v202107.28.0) for provider: virtualbox
    default: Downloading: https://vagrantcloud.com/bento/boxes/ubuntu-20.04/versions/202107.28.0/providers/virtualbox.box
    default:
==> default: Successfully added box 'bento/ubuntu-20.04' (v202107.28.0) for 'virtualbox'!
==> default: Importing base box 'bento/ubuntu-20.04'...
==> default: Matching MAC address for NAT networking...
==> default: Checking if box 'bento/ubuntu-20.04' version '202107.28.0' is up to date...
==> default: Setting the name of the VM: files_default_1633011561489_58955
Vagrant is currently configured to create VirtualBox synced folders with
the `SharedFoldersEnableSymlinksCreate` option enabled. If the Vagrant
guest is not trusted, you may want to disable this option. For more
information on this option, please refer to the VirtualBox manual:

  https://www.virtualbox.org/manual/ch04.html#sharedfolders

This option can be disabled globally with an environment variable:

  VAGRANT_DISABLE_VBOXSYMLINKCREATE=1

or on a per folder basis within the Vagrantfile:

  config.vm.synced_folder '/host/path', '/guest/path', SharedFoldersEnableSymlinksCreate: false
==> default: Clearing any previously set network interfaces...
==> default: Preparing network interfaces based on configuration...
    default: Adapter 1: nat
==> default: Forwarding ports...
    default: 22 (guest) => 2222 (host) (adapter 1)
==> default: Booting VM...
==> default: Waiting for machine to boot. This may take a few minutes...
    default: SSH address: 127.0.0.1:2222
    default: SSH username: vagrant
    default: SSH auth method: private key
Timed out while waiting for the machine to boot. This means that
Vagrant was unable to communicate with the guest machine within
the configured ("config.vm.boot_timeout" value) time period.

If you look above, you should be able to see the error(s) that
Vagrant had when attempting to connect to the machine. These errors
are usually good hints as to what may be wrong.

If you're using a custom box, make sure that networking is properly
working and you're able to connect to the machine. It is a common
problem that networking isn't setup properly in these boxes.
Verify that authentication configurations are also setup properly,
as well.

If the box appears to be booting properly, you may want to increase
the timeout ("config.vm.boot_timeout") value.















