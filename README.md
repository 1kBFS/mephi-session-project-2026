# Раздел 1
## 1.1 Настройка сети

> Выберите динамическую настройку IP-адреса сетевого интерфейса (зависит от установки, например, eth0) по протоколу DHCP.


Интерфейс ens18 получил адрес 192.168.1.43/24 по DHCP ->  настройка по DHCP работает. Это можно понять по фалгу dynamic и параметрам(valid_lft, preffered_left) - оставшееся время аренды.

```
[pavel@localhost ~]$ ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host noprefixroute 
       valid_lft forever preferred_lft forever
2: ens18: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether bc:24:11:68:d0:a2 brd ff:ff:ff:ff:ff:ff
    altname enp0s18
    inet 192.168.1.43/24 brd 192.168.1.255 scope global dynamic noprefixroute ens18
       valid_lft 23744sec preferred_lft 23744sec
    inet6 fe80::be24:11ff:fe68:d0a2/64 scope link noprefixroute 
       valid_lft forever preferred_lft forever
[pavel@localhost ~]$ 
```

## 1.2. Настройка имени хоста
> Установите имя хоста mephi-2026.domain.local.

```
[pavel@localhost ~]$ sudo hostnamectl set-hostname mephi-2026.domain.local
[sudo] пароль для pavel: 
[pavel@localhost ~]$ cat /etc/hostname
mephi-2026.domain.local
[pavel@localhost ~]$ 
```
После перелогина имя сменилось и в сессии терминала
```
[pavel@mephi-2026 ~]$ echo 'hello'
hello
[pavel@mephi-2026 ~]$ 
```

# 1.3. Проверка сетевой связности

[ping.out](artifacts/ping.out)

Сетевая связность есть:

```
[pavel@mephi-2026 ~]$ ping -c 4 8.8.8.8 > ping.out 
[pavel@mephi-2026 ~]$ cat ping.out 
PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
64 bytes from 8.8.8.8: icmp_seq=1 ttl=109 time=16.3 ms
64 bytes from 8.8.8.8: icmp_seq=2 ttl=109 time=16.6 ms
64 bytes from 8.8.8.8: icmp_seq=3 ttl=109 time=16.4 ms
64 bytes from 8.8.8.8: icmp_seq=4 ttl=109 time=16.1 ms

--- 8.8.8.8 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3006ms
rtt min/avg/max/mdev = 16.059/16.341/16.571/0.184 ms
[pavel@mephi-2026 ~]$ 
```

# Раздел 2

## 2.1. Обновление дистрибутива

> Выполните обновление пакетов дистрибутива.

```
[pavel@mephi-2026 ~]$ sudo dnf update
[sudo] пароль для pavel: 
RedOS - Updates                                                        9.6 MB/s | 108 MB     00:11    
Последняя проверка окончания срока действия метаданных: 0:01:24 назад, Вс 25 окт 2026 21:48:07.
Зависимости разрешены.
=======================================================================================================
 Пакет                                           Архитектура
                                                         Версия                         Репозиторий
                                                                                                 Размер
=======================================================================================================
Обновление:

...
Обновлен:
  acl-2.4.0-1.red80.x86_64                                                                             
  amd-gpu-firmware-20260810-1.red80.noarch                                                             
  amd-ucode-firmware-20260810-1.red80.noarch                                                           
  atheros-firmware-20260810-1.red80.noarch                                                             
  audit-3.1.3-5.red80.x86_64                                                                           
  audit-libs-3.1.3-5.red80.x86_64                                                                      
  brcmfmac-firmware-20260810-1.red80.noarch                                                            
  c-ares-1.34.6-1.red80.x86_64                                                                         
  ca-certificates-2026.2.90_v9.0.317-1.red80.noarch                                                    
  cirrus-audio-firmware-20260810-1.red80.noarch                                                        
  cups-client-1:2.4.19-3.red80.x86_64                                                                  
  cups-filesystem-1:2.4.19-3.red80.noarch                                                              
  cups-libs-1:2.4.19-3.red80.x86_64                                                                    
  curl-8.22.0-1.red80.x86_64                                                                           
  dracut-059-18.red80.x86_64                                                                           
  dracut-config-rescue-059-18.red80.x86_64                                                             
  expat-2.8.4-1.red80.x86_64        
...
Установлен:
  gawk-all-langpacks-5.3.2-1.red80.x86_64         kernel-lt-6.12.101-1.red80.x86_64                   
  libcares-1.34.6-1.red80.x86_64                  libkcapi-hasher-1.5.1-2.red80.x86_64                
  libpkgconf-1.9.5-4.red80.x86_64                 libusbx-1.0.30-1.red80.x86_64         
```

## 2.2. Установка пакетов из репозиториев

```
sudo dnf install nginx libcap-ng-utils

Установлен:
  gperftools-libs-2.9.1-3.red80.x86_64             libcap-ng-utils-0.8.3-3.red80.x86_64               
  libunwind-1.6.2-1.red80.x86_64                   nginx-1:1.30.4-2.red80.x86_64                      
  nginx-core-1:1.30.4-2.red80.x86_64               nginx-filesystem-1:1.30.4-2.red80.noarch           
  nginx-mimetypes-2.1.53-3.red80.noarch            redos-indexhtml-8-7.red80.noarch                   
  redos-logos-70.0.11-9.red80.noarch              

Выполнено!

```

## 2.3

```
sudo dnf download --destdir /tmp tcpdump

Последняя проверка окончания срока действия метаданных: 0:20:53 назад, Вс 25 окт 2026 21:48:07.
(1/2): tcpdump-4.99.5-1.red80.i686.rpm                                 3.2 MB/s | 530 kB     00:00    
(2/2): tcpdump-4.99.5-1.red80.x86_64.rpm                               2.7 MB/s | 523 kB     00:00   
```

Установим пакет:

```
[pavel@mephi-2026 ~]$ sudo rpm -ivh /tmp/tcpdump-4.99.5-1.red80.x86_64.rpm
Verifying...                          ################################# [100%]
Подготовка...               ################################# [100%]
Обновление / установка...
   1:tcpdump-14:4.99.5-1.red80        ################################# [100%]
[pavel@mephi-2026 ~]$ 

```
Пакет установлен, утилита работает

```
[pavel@mephi-2026 ~]$ rpm -q tcpdump
tcpdump-4.99.5-1.red80.x86_64
[pavel@mephi-2026 ~]$ tcpdump --version
tcpdump version 4.99.5
libpcap version 1.10.1 (with TPACKET_V3)
OpenSSL 3.5.7 9 Jun 2026
64-bit build, 64-bit time_t
[pavel@mephi-2026 ~]$ 
```


Сохраненная история транзакций менеджера пакетов в [dnf.out](artifacts/dnf.out)

# Раздел 3

## 3.1 Создание ФС

> Подключите второй диск (например, /dev/sdb). На втором диске создайте один раздел для хранения данных проекта, занимающий всё пространство диска.
Отформатируйте раздел как файловую систему ext4 с меткой тома MEPHI_WEB

Добавил диск на 5Гб
```
[pavel@mephi-2026 ~]$ lsblk
NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINTS
sda      8:0    0   32G  0 disk 
├─sda1   8:1    0    1M  0 part 
├─sda2   8:2    0    1G  0 part /boot
├─sda3   8:3    0    2G  0 part [SWAP]
└─sda4   8:4    0 28,9G  0 part /
sdb      8:16   0    5G  0 disk 
sr0     11:0    1    6G  1 rom  
zram0  252:0    0  1,9G  0 disk [SWAP]
```

Создадим таблицу GPT с одним разделом на весь диск

```
[pavel@mephi-2026 ~]$ sudo parted /dev/sdb mklabel gpt
Информация: Не забудьте обновить /etc/fstab.

[pavel@mephi-2026 ~]$ sudo parted -a opt /dev/sdb mkpart primary ext4 0% 100%
Информация: Не забудьте обновить /etc/fstab.

```

Появился раздел:

```
[pavel@mephi-2026 ~]$ lsblk /dev/sdb                                      
NAME   MAJ:MIN RM SIZE RO TYPE MOUNTPOINTS
sdb      8:16   0   5G  0 disk 
└─sdb1   8:17   0   5G  0 part 
[pavel@mephi-2026 ~]$ 
```

Отформатируем раздел:
```
[pavel@mephi-2026 ~]$ sudo mkfs.ext4 -L MEPHI_WEB /dev/sdb1
mke2fs 1.46.3 (27-Jul-2021)
Discarding device blocks: done                            
Creating filesystem with 1310208 4k blocks and 327680 inodes
Filesystem UUID: 5f261297-4824-4145-8b81-290551f32620
Superblock backups stored on blocks: 
	32768, 98304, 163840, 229376, 294912, 819200, 884736

Allocating group tables: done                            
Writing inode tables: done                            
Creating journal (16384 blocks): done
Writing superblocks and filesystem accounting information: done 
```

## 3.2 Монтирование ФС

> Создайте точку монтирования /mephi-web.
Настройте автоматическое монтирование созданного раздела при начальной загрузке, используя метку тома в качестве идентификатора файловой системы.
Выполните ручное монтирование раздела.

Создаем точку монтирования
```
sudo mkdir /mephi-web
```

Обновим fstab, чтобы запись не зависела от имени устройства

```
[pavel@mephi-2026 ~]$ sudo echo "LABEL=MEPHI_WEB   /mephi-web   ext4   defaults   0 2" | sudo tee /etc/fstab 

[pavel@mephi-2026 ~]$ sudo systemctl daemon-reload

```

Монтируем:

```
[pavel@mephi-2026 ~]$ sudo mount -L MEPHI_WEB /mephi-web
```

Проверяем:
```
[pavel@mephi-2026 ~]$ mount | grep mephi-web
/dev/sdb1 on /mephi-web type ext4 (rw,relatime,seclabel)
```

# Раздел 4

## 4.1 Управление сервисами

```
sudo systemctl enable --now nginx

Created symlink /etc/systemd/system/multi-user.target.wants/nginx.service → /usr/lib/systemd/system/nginx.service.
```

## 4.2 Журналирование
```
[pavel@mephi-2026 ~]$ sudo journalctl -b -u nginx
окт 25 23:03:50 mephi-2026.domain.local systemd[1]: Starting nginx.service - The nginx HTTP and revers>
окт 25 23:03:50 mephi-2026.domain.local nginx[69438]: nginx: the configuration file /etc/nginx/nginx.c>
окт 25 23:03:50 mephi-2026.domain.local nginx[69438]: nginx: configuration file /etc/nginx/nginx.conf >
окт 25 23:03:50 mephi-2026.domain.local systemd[1]: Started nginx.service - The nginx HTTP and reverse>
lines 1-4/4 (END)
```

Данные журнала в [файле](artifacts/journalctl.out)

# Раздел 5. Управление доступом

## DAC

Создаем пользователей

```
[pavel@mephi-2026 ~]$ sudo groupadd developers
[pavel@mephi-2026 ~]$ sudo groupadd -g 4444 curators
[pavel@mephi-2026 ~]$ sudo useradd -m -u 5501 -G developers user1
useradd: пользователь «user1» уже существует
[pavel@mephi-2026 ~]$ sudo usermod -u 5501 -aG developers user1
[pavel@mephi-2026 ~]$ sudo useradd -m -u 5502 -G developers user2
[pavel@mephi-2026 ~]$ sudo useradd -m -u 5503 -G developers user3
[pavel@mephi-2026 ~]$ sudo useradd -m -G curators curator1
[pavel@mephi-2026 ~]$ sudo useradd -m -G curators curator2
[pavel@mephi-2026 ~]$ id user1
uid=5501(user1) gid=1001(students) группы=1001(students),1002(developers)
[pavel@mephi-2026 ~]$ id curator1
uid=5504(curator1) gid=5504(curator1) группы=5504(curator1),4444(curators)
[pavel@mephi-2026 ~]$ 
```

Каталог проекта

```
[pavel@mephi-2026 ~]$ sudo mkdir -p /data/mephi-2026
[pavel@mephi-2026 ~]$ sudo chown root:developers /data/mephi-2026
[pavel@mephi-2026 ~]$ sudo chmod 2770 /data/mephi-2026
[pavel@mephi-2026 ~]$ 
```
2770 = владелец и группа имеют полный досту, остальные - нет
setgid - новые файлы будут наследовать группу developers


ACL для кураторов и разработчиков:

```
[pavel@mephi-2026 ~]$ sudo setfacl -m  g:developers:rwx,g:curators:r-x,o::--- /data/mephi-2026
[pavel@mephi-2026 ~]$ sudo setfacl -d -m g:developers:rwx,g:curators:r-x,o::--- /data/mephi-2026
[pavel@mephi-2026 ~]$ 
```

Проверка:

```
[pavel@mephi-2026 ~]$ getfacl /data/mephi-2026
getfacl: Removing leading '/' from absolute path names
# file: data/mephi-2026
# owner: root
# group: developers
# flags: -s-
user::rwx
group::rwx
group:developers:rwx
group:curators:r-x
mask::rwx
other::---
default:user::rwx
default:group::rwx
default:group:developers:rwx
default:group:curators:r-x
default:mask::rwx
default:other::---
```

## 5.2. Привилегии (уменьшение количества set-UID-программ)

Текущее состояние: сейчас нет ни SUID-бита, ни file capabilities
```
[pavel@mephi-2026 ~]$ ls -l $(which tcpdump)
-rwxr-xr-x. 1 root root 1299064 июл 30  2025 /usr/sbin/tcpdump
[pavel@mephi-2026 ~]$ getcap $(which tcpdump)
[pavel@mephi-2026 ~]$ 
```

Установим capbilities:

```
[pavel@mephi-2026 ~]$ sudo setcap cap_net_raw,cap_net_admin=eip /usr/sbin/tcpdump
[pavel@mephi-2026 ~]$ getcap /usr/sbin/tcpdump
/usr/sbin/tcpdump cap_net_admin,cap_net_raw=eip
[pavel@mephi-2026 ~]$ 
```

Проверка

```
[pavel@mephi-2026 ~]$  sudo -u user1 tcpdump 
tcpdump: verbose output suppressed, use -v[v]... for full protocol decode
listening on ens18, link-type EN10MB (Ethernet), snapshot length 262144 bytes
23:16:53.779552 IP mephi-2026.domain.local.ssh > 192.168.1.142.65111: Flags [P.], seq 4025661081:4025661197, ack 2384190342, win 713, options [nop,nop,TS val 994078810 ecr 172865840], length 116
23:16:53.779680 IP mephi-2026.domain.local.ssh > 192.168.1.142.65111: Flags [P.], seq 116:152, ack 1, win 713, options [nop,nop,TS val 994078810 ecr 172865840], length 36
23:16:53.779782 IP mephi-2026.domain.local.ssh > 192.168.1.142.65111: Flags [P.], seq 152:204, ack 1, win 713, options [nop,nop,TS val 994078810 ecr 172865
```

## 5.3

```
[pavel@mephi-2026 ~]$ getenforce
Enforcing
[pavel@mephi-2026 ~]$ sestatus
SELinux status:                 enabled
SELinuxfs mount:                /sys/fs/selinux
SELinux root directory:         /etc/selinux
Loaded policy name:             targeted
Current mode:                   enforcing
Mode from config file:          enforcing
Policy MLS status:              enabled
Policy deny_unknown status:     allowed
Memory protection checking:     actual (secure)
Max kernel policy version:      33
[pavel@mephi-2026 ~]$ 
```

Настроим NGINX

```bash
[pavel@mephi-2026 ~]$ echo "<h1>MEPHI WEB</h1>" | sudo tee /mephi-web/index.html
<h1>MEPHI WEB</h1>
[pavel@mephi-2026 ~]$ sudo nano /etc/nginx/nginx.conf
[pavel@mephi-2026 ~]$ sudo vi /etc/nginx/nginx.conf
[pavel@mephi-2026 ~]$ cat /etc/nginx/nginx.conf | grep server -C 5

    server {
        listen       80;
        listen       [::]:80;
        server_name  _;
        root         /mephi-web; # <-----

        # Load configuration files for the default server block.
        include /etc/nginx/default.d/*.conf;
    }
[pavel@mephi-2026 ~]$ sudo nginx -t
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
[pavel@mephi-2026 ~]$ sudo systemctl restart nginx
```

Настроим правило:

```
[pavel@mephi-2026 ~]$ sudo semanage fcontext -a -t httpd_sys_content_t "/mephi-web(/.*)?"
[pavel@mephi-2026 ~]$ sudo restorecon -Rv /mephi-web
Relabeled /mephi-web from system_u:object_r:unlabeled_t:s0 to system_u:object_r:httpd_sys_content_t:s0
Relabeled /mephi-web/lost+found from system_u:object_r:unlabeled_t:s0 to system_u:object_r:httpd_sys_content_t:s0
Relabeled /mephi-web/index.html from unconfined_u:object_r:unlabeled_t:s0 to unconfined_u:object_r:httpd_sys_content_t:s0
[pavel@mephi-2026 ~]$ 
```

Проверка:

```
[pavel@mephi-2026 ~]$ ls -Zd /mephi-web/
system_u:object_r:httpd_sys_content_t:s0 /mephi-web/

[pavel@mephi-2026 ~]$ curl http://localhost
<h1>MEPHI WEB</h1>
```

# Раздел 6

## 6.1

Настроим правило
```
[pavel@mephi-2026 ~]$ echo "-:(curators):LOCAL" | sudo tee /etc/security/access.conf 
-:(curators):LOCAL
[pavel@mephi-2026 ~]$ sudo tail -n 2 /etc/security/access.conf 
-:(curators):LOCAL
[pavel@mephi-2026 ~]$ 
```
Подключим модуль PAM

```
sudo sed -i '/^account[[:space:]]\+required[[:space:]]\+pam_nologin.so/a account    required     pam_access.so' /etc/pam.d/login
```

## 6.2

Для вновь созданных пользователей:
```
[pavel@mephi-2026 ~]$ sudo sed -i 's/^PASS_MAX_DAYS.*/PASS_MAX_DAYS   90/' /etc/login.defs
[pavel@mephi-2026 ~]$ grep ^PASS_ /etc/login.defs
PASS_MAX_DAYS   90
PASS_MIN_DAYS	0
PASS_WARN_AGE	7
[pavel@mephi-2026 ~]$ 
```
Для сущесивующих

```
for u in pavel user1 user2 user3 curator1 curator2; do sudo chage -M 90 $u; done
```

Также изменена настройка `minlen` длины пароля в [pwquality.conf](artifacts/pwquality.conf)

# Раздел 7

```
[pavel@mephi-2026 ~]$ echo "Hello from Student: 356453" | sudo tee /mephi-web/index.html
[sudo] пароль для pavel: 
Попробуйте ещё раз.
[sudo] пароль для pavel: 
Hello from Student: 356453
[pavel@mephi-2026 ~]$ sudo restorecon -v /mephi-web/index.html
[pavel@mephi-2026 ~]$ 
[pavel@mephi-2026 ~]$ sudo chmod 644 /mephi-web/index.html
[pavel@mephi-2026 ~]$ curl http://localhost
Hello from Student: 356453
[pavel@mephi-2026 ~]$ 

```