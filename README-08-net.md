# devops-netology  
Home work 03-sysadmin-08-net  

Домашнее задание к занятию "3.8. Компьютерные сети, лекция 3"  

1. Подключитесь к публичному маршрутизатору в интернет. Найдите маршрут к вашему публичному IP  
telnet route-views.routeviews.org  
Username: rviews  
show ip route x.x.x.x/32  
show bgp x.x.x.x/32
````
Username: rviews
route-views>sh ip route 213.87.130.25
Routing entry for 213.87.128.0/19, supernet
  Known via "bgp 6447", distance 20, metric 0
  Tag 2497, type external
  Last update from 202.232.0.2 04:47:12 ago
  Routing Descriptor Blocks:
  * 202.232.0.2, from 202.232.0.2, 04:47:12 ago
      Route metric is 0, traffic share count is 1
      AS Hops 2
      Route tag 2497
      MPLS label: none
````
````
route-views>sh bgp 213.87.130.25
BGP routing table entry for 213.87.128.0/19, version 3163958647
Paths: (23 available, best #13, table default)
  Not advertised to any peer
  Refresh Epoch 1
  53767 14315 6453 6453 3356 8359
    162.251.163.2 from 162.251.163.2 (162.251.162.3)
      Origin IGP, localpref 100, valid, external
      Community: 14315:5000 53767:5000
      path 7FE0EC4D9328 RPKI State valid
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  3333 8359
    193.0.0.56 from 193.0.0.56 (193.0.0.56)
      Origin IGP, localpref 100, valid, external
      Community: 8359:5500 8359:16012 8359:55277
      path 7FE0852125C8 RPKI State valid
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  6939 8359
    64.71.137.241 from 64.71.137.241 (216.218.252.164)
      Origin IGP, localpref 100, valid, external
      path 7FE026158220 RPKI State valid
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  1351 8359
    132.198.255.253 from 132.198.255.253 (132.198.255.253)
      Origin IGP, localpref 100, valid, external
      path 7FE12018C9B0 RPKI State valid
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  20912 3257 3356 8359
    212.66.96.126 from 212.66.96.126 (212.66.96.126)
      Origin IGP, localpref 100, valid, external
      Community: 3257:8070 3257:30515 3257:50001 3257:53900 3257:53902 20912:65004
      path 7FE18D147E08 RPKI State valid
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 2
  8283 8359
    94.142.247.3 from 94.142.247.3 (94.142.247.3)
      Origin IGP, metric 0, localpref 100, valid, external
      Community: 8283:1 8283:101 8283:102 8283:103 8359:5500 8359:16012 8359:55277
      unknown transitive attribute: flag 0xE0 type 0x20 length 0x30
        value 0000 205B 0000 0000 0000 0001 0000 205B
              0000 0005 0000 0001 0000 205B 0000 0005
              0000 0002 0000 205B 0000 0005 0000 0003

      path 7FE0BEEC53A0 RPKI State valid
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  101 3356 8359
    209.124.176.223 from 209.124.176.223 (209.124.176.223)
      Origin IGP, localpref 100, valid, external
      Community: 101:20100 101:20110 101:22100 3356:2 3356:22 3356:100 3356:123 3356:507 3356:901 3356:2111 8359:5500 8359:16012 8359:55277
      Extended Community: RT:101:22100
      path 7FE13BE0D398 RPKI State valid
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  3356 8359
    4.68.4.46 from 4.68.4.46 (4.69.184.201)
      Origin IGP, metric 0, localpref 100, valid, external
      Community: 3356:2 3356:22 3356:100 3356:123 3356:507 3356:901 3356:2111 8359:5500 8359:16012 8359:55277
      path 7FE091719CD8 RPKI State valid
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  852 3356 8359
    154.11.12.212 from 154.11.12.212 (96.1.209.43)
      Origin IGP, metric 0, localpref 100, valid, external
      path 7FE0CCB3EA18 RPKI State valid
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  3549 3356 8359
    208.51.134.254 from 208.51.134.254 (67.16.168.191)
      Origin IGP, metric 0, localpref 100, valid, external
      Community: 3356:2 3356:22 3356:100 3356:123 3356:507 3356:901 3356:2111 3549:2581 3549:30840 8359:5500 8359:16012 8359:55277
      path 7FE0A30390F8 RPKI State valid
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  57866 3356 8359
    37.139.139.17 from 37.139.139.17 (37.139.139.17)
      Origin IGP, metric 0, localpref 100, valid, external
      Community: 3356:2 3356:22 3356:100 3356:123 3356:507 3356:901 3356:2111 8359:5500 8359:16012 8359:55277
      path 7FE165F16A70 RPKI State valid
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  20130 6939 8359
    140.192.8.16 from 140.192.8.16 (140.192.8.16)
      Origin IGP, localpref 100, valid, external
      path 7FE0C9EFC0A8 RPKI State valid
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  2497 8359
    202.232.0.2 from 202.232.0.2 (58.138.96.254)
      Origin IGP, localpref 100, valid, external, best
      path 7FE104C72EA0 RPKI State valid
      rx pathid: 0, tx pathid: 0x0
  Refresh Epoch 3
  3303 8359
    217.192.89.50 from 217.192.89.50 (138.187.128.158)
      Origin IGP, localpref 100, valid, external
      Community: 3303:1004 3303:1006 3303:1030 3303:3054 8359:5500 8359:16012 8359:55277
      path 7FE17375C6A0 RPKI State valid
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  4901 6079 8359
    162.250.137.254 from 162.250.137.254 (162.250.137.254)
      Origin IGP, localpref 100, valid, external
      Community: 65000:10100 65000:10300 65000:10400
      path 7FE0DA1B9948 RPKI State valid
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  7660 2516 1299 8359
    203.181.248.168 from 203.181.248.168 (203.181.248.168)
      Origin IGP, localpref 100, valid, external
      Community: 2516:1030 7660:9001
      path 7FE014C67DF0 RPKI State valid
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  3561 3910 3356 8359
    206.24.210.80 from 206.24.210.80 (206.24.210.80)
      Origin IGP, localpref 100, valid, external
      path 7FE0D743DAE8 RPKI State valid
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  3257 3356 8359
    89.149.178.10 from 89.149.178.10 (213.200.83.26)
      Origin IGP, metric 10, localpref 100, valid, external
      Community: 3257:8794 3257:30043 3257:50001 3257:54900 3257:54901
      path 7FE032F50A68 RPKI State valid
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  7018 3356 8359
    12.0.1.63 from 12.0.1.63 (12.0.1.63)
      Origin IGP, localpref 100, valid, external
      Community: 7018:5000 7018:37232
      path 7FE10F456908 RPKI State valid
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  49788 12552 8359
    91.218.184.60 from 91.218.184.60 (91.218.184.60)
      Origin IGP, localpref 100, valid, external
      Community: 12552:12000 12552:12100 12552:12101 12552:22000
      Extended Community: 0x43:100:1
      path 7FE156818510 RPKI State valid
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  1221 4637 3356 8359
    203.62.252.83 from 203.62.252.83 (203.62.252.83)
      Origin IGP, localpref 100, valid, external
      path 7FE104BB84F8 RPKI State valid
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  701 3356 8359
    137.39.3.55 from 137.39.3.55 (137.39.3.55)
      Origin IGP, localpref 100, valid, external
      path 7FE0852EBE28 RPKI State valid
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  19214 3257 3356 8359
    208.74.64.40 from 208.74.64.40 (208.74.64.40)
      Origin IGP, localpref 100, valid, external
      Community: 3257:8108 3257:30048 3257:50002 3257:51200 3257:51203
      path 7FE11C675F10 RPKI State valid
      rx pathid: 0, tx pathid: 0
````


2. Создайте dummy0 интерфейс в Ubuntu. Добавьте несколько статических маршрутов. Проверьте таблицу маршрутизации. 
> echo "dummy" >> /etc/modules
> echo "options dummy numdummies=2" > /etc/modprobe.d/dummy.conf  
> auto dummy0
> iface dummy0 inet static
>    address 10.2.2.2/32
>    pre-up ip link add dummy0 type dummy
>    post-down ip link del dummy0

````
ip address
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:73:60:cf brd ff:ff:ff:ff:ff:ff
    inet 10.0.2.15/24 brd 10.0.2.255 scope global dynamic eth0
       valid_lft 79230sec preferred_lft 79230sec
    inet6 fe80::a00:27ff:fe73:60cf/64 scope link
       valid_lft forever preferred_lft forever
3: dummy0: <BROADCAST,NOARP,UP,LOWER_UP> mtu 1500 qdisc noqueue state UNKNOWN group default qlen 1000
    link/ether 86:51:b8:48:95:18 brd ff:ff:ff:ff:ff:ff
    inet 10.2.2.2/32 brd 10.2.2.2 scope global dummy0
       valid_lft forever preferred_lft forever
    inet6 fe80::8451:b8ff:fe48:9518/64 scope link
       valid_lft forever preferred_lft forever
````
>   
> Статический маршрут: добавил в /etc/netplan/01-netcfg.yaml , в соотв интерфейс  
>  routes:  
>       - to: 172.16.0.0/24  
>         via: 10.0.2.3  
> 
> ip route
> default via 10.0.2.2 dev eth0 proto dhcp src 10.0.2.15 metric 100  
> 10.0.2.0/24 dev eth0 proto kernel scope link src 10.0.2.15  
> 10.0.2.2 dev eth0 proto dhcp scope link src 10.0.2.15 metric 100  
> 172.16.0.0/24 via 10.0.2.3 dev eth0 proto static onlink  
> 

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
