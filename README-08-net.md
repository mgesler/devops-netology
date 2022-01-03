# devops-netology  
Home work 03-sysadmin-08-net  

Домашнее задание к занятию "3.8. Компьютерные сети, лекция 3"  

1. Подключитесь к публичному маршрутизатору в интернет. Найдите маршрут к вашему публичному IP  
telnet route-views.routeviews.org  
Username: rviews  
show ip route x.x.x.x/32  
show bgp x.x.x.x/32    
> сервис не работает, согласно лекции можно не выполнять этот пункт.  

2. Создайте dummy0 интерфейс в Ubuntu. Добавьте несколько статических маршрутов. Проверьте таблицу маршрутизации. 
> echo "dummy" >> /etc/modules
> echo "options dummy numdummies=2" > /etc/modprobe.d/dummy.conf  
> auto dummy0
> iface dummy0 inet static
>    address 10.2.2.2/32
>    pre-up ip link add dummy0 type dummy
>    post-down ip link del dummy0
>   
> Статический маршрут: добавил в /etc/netplan/01-netcfg.yaml , в соотв интерфейс  
>  routes:  
>       - to: 172.16.0.0/24  
>         via: 10.0.2.3  

3. Проверьте открытые TCP порты в Ubuntu, какие протоколы и приложения используют эти порты? Приведите несколько примеров.   
> Например, ss -tlpn | grep LISTEN  покажет протокол и pid процесса, который использует порт.
>tcp    LISTEN   0        4096        127.0.0.53%lo:53             0.0.0.0:*      users:(("systemd-resolve",pid=563,fd=13))  
>tcp    LISTEN   0        128               0.0.0.0:22             0.0.0.0:*      users:(("sshd",pid=711,fd=3))  
>tcp    LISTEN   0        4096            127.0.0.1:8125           0.0.0.0:*      users:(("netdata",pid=683,fd=58))  
>tcp    LISTEN   0        4096              0.0.0.0:19999          0.0.0.0:*      users:(("netdata",pid=683,fd=4))  
>tcp    LISTEN   0        4096              0.0.0.0:111            0.0.0.0:*      users:(("rpcbind",pid=561,fd=4),("systemd",pid=1,fd=35))  
>tcp    LISTEN   0        128                  [::]:22                [::]:*      users:(("sshd",pid=711,fd=4))  
>tcp    LISTEN   0        4096                [::1]:8125              [::]:*      users:(("netdata",pid=683,fd=57))  
>tcp    LISTEN   0        4096                 [::]:111               [::]:*      users:(("rpcbind",pid=561,fd=6),("systemd",pid=1,fd=37))  
> 
>Так же можно использовать lsof -i -P -n | grep LISTEN | grep TCP
>systemd      1            root   35u  IPv4  16410      0t0  TCP *:111 (LISTEN)  
>systemd      1            root   37u  IPv6  16414      0t0  TCP *:111 (LISTEN)  
>rpcbind    561            _rpc    4u  IPv4  16410      0t0  TCP *:111 (LISTEN)  
>rpcbind    561            _rpc    6u  IPv6  16414      0t0  TCP *:111 (LISTEN)  
>systemd-r  563 systemd-resolve   13u  IPv4  20414      0t0  TCP 127.0.0.53:53 (LISTEN)  
>netdata    683         netdata    4u  IPv4  23880      0t0  TCP *:19999 (LISTEN)  
>netdata    683         netdata   57u  IPv6  24392      0t0  TCP [::1]:8125 (LISTEN)  
>netdata    683         netdata   58u  IPv4  24393      0t0  TCP 127.0.0.1:8125 (LISTEN)  
>sshd       711            root    3u  IPv4  22774      0t0  TCP *:22 (LISTEN)    
> sshd       711            root    4u  IPv6  22776      0t0  TCP *:22 (LISTEN)  
> 
> 
> 

4. Проверьте используемые UDP сокеты в Ubuntu, какие протоколы и приложения используют эти порты?  
> Можно использовать команды из п.3 с другими ключами  
> ss -ulpn | grep LISTEN
> lsof -i -P -n | grep LISTEN | grep UDP
> 

5. Используя diagrams.net, создайте L3 диаграмму вашей домашней сети или любой другой сети, с которой вы работали.  
>
![](https://github.com/mgesler/devops-netology/blob/main/mynet1.png)
