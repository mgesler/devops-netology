# devops-netology  
Home work 03-sysadmin-09-security  

Домашнее задание к занятию "3.9. Элементы безопасности информационных систем"    

1. Установите Bitwarden плагин для браузера. Зарегестрируйтесь и сохраните несколько паролей.  
![](https://github.com/mgesler/devops-netology/blob/main/pic/pic/bitwarden1.jpg)

2. Установите Google authenticator на мобильный телефон. Настройте вход в Bitwarden акаунт через Google authenticator OTP.
![](https://github.com/mgesler/devops-netology/blob/main/pic/bitwarden2.jpg)
3. Установите apache2, сгенерируйте самоподписанный сертификат, настройте тестовый сайт для работы по HTTPS.
> apt install apache2  
> a2enmod ssl  
> openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/apache-selfsigned.key -out /etc/ssl/certs/apache-selfsigned.crt -subj "/C=RU/ST=Moscow/L=Moscow/O=Netology/OU=Students/CN=www.example.com"    
> touch /etc/apache2/sites-available/localhostsite.conf  
>  a2ensite localhostsite.conf  
> apache2ctl configtest  
> systemctl reload apache2  
> apt install links  
> links  
> ![](https://github.com/mgesler/devops-netology/blob/main/pic/ssl-cert-error.jpg)
> it worked!  


4. Проверьте на TLS уязвимости произвольный сайт в интернете (кроме сайтов МВД, ФСБ, МинОбр, НацБанк, РосКосмос, РосАтом, РосНАНО и любых госкомпаний, объектов КИИ, ВПК ... и тому подобное).
````
./testssl.sh -e --fast --parallel https://client.ukguyg.ru/

###########################################################
    testssl.sh       3.1dev from https://testssl.sh/dev/
    (3827521 2021-12-27 17:11:36 -- )

      This program is free software. Distribution and
             modification under GPLv2 permitted.
      USAGE w/o ANY WARRANTY. USE IT AT YOUR OWN RISK!

       Please file bugs @ https://testssl.sh/bugs/

###########################################################

 Using "OpenSSL 1.0.2-chacha (1.0.2k-dev)" [~183 ciphers]
 on vagrant:./bin/openssl.Linux.x86_64
 (built: "Jan 18 17:12:17 2019", platform: "linux-x86_64")


 Start 2022-01-03 13:40:00        -->> 37.140.192.225:443 (client.ukguyg.ru) <<--

 rDNS (37.140.192.225):  server68.hosting.reg.ru.
 Service detected:       HTTP



 Testing all 183 locally available ciphers against the server, ordered by encryption strength


Hexcode  Cipher Suite Name (OpenSSL)       KeyExch.   Encryption  Bits     Cipher Suite Name (IANA/RFC)
-----------------------------------------------------------------------------------------------------------------------------
 xc030   ECDHE-RSA-AES256-GCM-SHA384       ECDH 256   AESGCM      256      TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384
 xc028   ECDHE-RSA-AES256-SHA384           ECDH 256   AES         256      TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384
 xc014   ECDHE-RSA-AES256-SHA              ECDH 256   AES         256      TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA
 xc077   ECDHE-RSA-CAMELLIA256-SHA384      ECDH 256   Camellia    256      TLS_ECDHE_RSA_WITH_CAMELLIA_256_CBC_SHA384
 x9d     AES256-GCM-SHA384                 RSA        AESGCM      256      TLS_RSA_WITH_AES_256_GCM_SHA384
 x3d     AES256-SHA256                     RSA        AES         256      TLS_RSA_WITH_AES_256_CBC_SHA256
 x35     AES256-SHA                        RSA        AES         256      TLS_RSA_WITH_AES_256_CBC_SHA
 xc0     CAMELLIA256-SHA256                RSA        Camellia    256      TLS_RSA_WITH_CAMELLIA_256_CBC_SHA256
 x84     CAMELLIA256-SHA                   RSA        Camellia    256      TLS_RSA_WITH_CAMELLIA_256_CBC_SHA
 xc02f   ECDHE-RSA-AES128-GCM-SHA256       ECDH 256   AESGCM      128      TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256
 xc027   ECDHE-RSA-AES128-SHA256           ECDH 256   AES         128      TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256
 xc013   ECDHE-RSA-AES128-SHA              ECDH 256   AES         128      TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA
 xc076   ECDHE-RSA-CAMELLIA128-SHA256      ECDH 256   Camellia    128      TLS_ECDHE_RSA_WITH_CAMELLIA_128_CBC_SHA256
 x9c     AES128-GCM-SHA256                 RSA        AESGCM      128      TLS_RSA_WITH_AES_128_GCM_SHA256
 x3c     AES128-SHA256                     RSA        AES         128      TLS_RSA_WITH_AES_128_CBC_SHA256
 x2f     AES128-SHA                        RSA        AES         128      TLS_RSA_WITH_AES_128_CBC_SHA
 xba     CAMELLIA128-SHA256                RSA        Camellia    128      TLS_RSA_WITH_CAMELLIA_128_CBC_SHA256
 x41     CAMELLIA128-SHA                   RSA        Camellia    128      TLS_RSA_WITH_CAMELLIA_128_CBC_SHA


 Done 2022-01-03 13:40:03 [   5s] -->> 37.140.192.225:443 (client.ukguyg.ru) <<--
````
````
./testssl.sh -U --sneaky https://client.ukguyg.ru/

###########################################################
    testssl.sh       3.1dev from https://testssl.sh/dev/
    (3827521 2021-12-27 17:11:36 -- )

      This program is free software. Distribution and
             modification under GPLv2 permitted.
      USAGE w/o ANY WARRANTY. USE IT AT YOUR OWN RISK!

       Please file bugs @ https://testssl.sh/bugs/

###########################################################

 Using "OpenSSL 1.0.2-chacha (1.0.2k-dev)" [~183 ciphers]
 on vagrant:./bin/openssl.Linux.x86_64
 (built: "Jan 18 17:12:17 2019", platform: "linux-x86_64")


 Start 2022-01-03 14:05:22        -->> 37.140.192.225:443 (client.ukguyg.ru) <<--

 rDNS (37.140.192.225):  server68.hosting.reg.ru.
 Service detected:       HTTP


 Testing vulnerabilities

 Heartbleed (CVE-2014-0160)                not vulnerable (OK), no heartbeat extension
 CCS (CVE-2014-0224)                       not vulnerable (OK)
 Ticketbleed (CVE-2016-9244), experiment.  not vulnerable (OK)
 ROBOT                                     not vulnerable (OK)
 Secure Renegotiation (RFC 5746)           supported (OK)
 Secure Client-Initiated Renegotiation     not vulnerable (OK)
 CRIME, TLS (CVE-2012-4929)                not vulnerable (OK)
 BREACH (CVE-2013-3587)                    no gzip/deflate/compress/br HTTP compression (OK)  - only supplied "/" tested
 POODLE, SSL (CVE-2014-3566)               not vulnerable (OK)
 TLS_FALLBACK_SCSV (RFC 7507)              No fallback possible (OK), no protocol below TLS 1.2 offered
 SWEET32 (CVE-2016-2183, CVE-2016-6329)    not vulnerable (OK)
 FREAK (CVE-2015-0204)                     not vulnerable (OK)
 DROWN (CVE-2016-0800, CVE-2016-0703)      not vulnerable on this host and port (OK)
                                           make sure you don't use this certificate elsewhere with SSLv2 enabled services
                                           https://censys.io/ipv4?q=AFDAE1326D34F0883695E732298DC03126E2F0CD7EF59CAB7203D95EB56802A0 could help you to find out
 LOGJAM (CVE-2015-4000), experimental      not vulnerable (OK): no DH EXPORT ciphers, no DH key detected with <= TLS 1.2
 BEAST (CVE-2011-3389)                     not vulnerable (OK), no SSL3 or TLS1
 LUCKY13 (CVE-2013-0169), experimental     potentially VULNERABLE, uses cipher block chaining (CBC) ciphers with TLS. Check patches
 Winshock (CVE-2014-6321), experimental    not vulnerable (OK)
 RC4 (CVE-2013-2566, CVE-2015-2808)        no RC4 ciphers detected (OK)


 Done 2022-01-03 14:05:51 [  30s] -->> 37.140.192.225:443 (client.ukguyg.ru) <<--
````


5. Установите на Ubuntu ssh сервер, сгенерируйте новый приватный ключ. Скопируйте свой публичный ключ на другой сервер. Подключитесь к серверу по SSH-ключу.
````
apt install openssh-server
systemctl start sshd.service
systemctl enable sshd.service

ssh-copy-id netology@89.225.65.18
/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/netology/.ssh/id_rsa.pub"
The authenticity of host '89.225.65.18 (89.225.65.18)' can't be established.
ECDSA key fingerprint is SHA256:NweWINcNfWmF81ZPQJPi8hedMH82Exc8Y04+PSEtxZY.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
netology@89.225.65.18's password:

Number of key(s) added: 1

Now try logging into the machine, with:   "ssh 'netology@89.225.65.18'"
and check to make sure that only the key(s) you wanted were added.

ssh 89.225.65.18
Enter passphrase for key '/netology/.ssh/id_rsa':
Linux pve01 5.4.78-2-pve #1 SMP PVE 5.4.78-2 (Thu, 03 Dec 2020 14:26:17 +0100) x86_64

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
Last login: Sat Jan  1 14:31:05 2022
netology@pve01:~#

````
6. Переименуйте файлы ключей из задания 5. Настройте файл конфигурации SSH клиента, так чтобы вход на удаленный сервер осуществлялся по имени сервера.
````
touch ~/.ssh/config && chmod 600 ~/.ssh/config
cp id_rsa some_server.key

vi ~/.ssh/config

Host my_server
HostName 89.225.65.18
IdentityFile ~/.ssh/some_server.key
User netology

Host *
IdentityFile ~/.ssh/id_rsa

 ssh my_server
Enter passphrase for key '/netology/.ssh/some_server.key':
Linux pve01 5.4.78-2-pve #1 SMP PVE 5.4.78-2 (Thu, 03 Dec 2020 14:26:17 +0100) x86_64

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
Last login: Mon Jan  3 21:45:24 2022 from 90.15.1.31
netology@pve01:~#

````
8. Соберите дамп трафика утилитой tcpdump в формате pcap, 100 пакетов. Откройте файл pcap в Wireshark.
````
apt install tcpdump
tcpdump -c 100 -i eth0 -w 1.pcap
![](https://github.com/mgesler/devops-netology/blob/main/pic/wireshark.jpg)