# otus_net
1. Работать будем в CentOs7. Загружаемся и проверяем сетевые интерфейсы ``` ip a ```
```
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 52:54:00:4d:77:d3 brd ff:ff:ff:ff:ff:ff
    inet 10.0.2.15/24 brd 10.0.2.255 scope global noprefixroute dynamic eth0
       valid_lft 85960sec preferred_lft 85960sec
    inet6 fe80::5054:ff:fe4d:77d3/64 scope link 
       valid_lft forever preferred_lft forever
```
2. Выбираем интерфейс eth0 и добавляем на него еще один IP   ``` ip addr add 10.0.2.25/24 dev eth0 ```
3. Проверяем настройки ``` ip a```
```
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 52:54:00:4d:77:d3 brd ff:ff:ff:ff:ff:ff
    inet 10.0.2.15/24 brd 10.0.2.255 scope global noprefixroute dynamic eth0
       valid_lft 85347sec preferred_lft 85347sec
    inet 10.0.2.25/24 brd 10.0.2.255 scope global secondary eth0
       valid_lft forever preferred_lft forever
    inet6 fe80::5054:ff:fe4d:77d3/64 scope link 
       valid_lft forever preferred_lft forever
```
4. Устанавливаем маршрут по умолчанию ``` ip route add default 10.0.2.5/24 dev eth0 ```
5. Проверяем настройки ``` ip r```
```
default via 10.0.2.5 dev eth0 proto dhcp metric 100 
10.0.2.0/24 dev eth0 proto kernel scope link src 10.0.2.15 metric 100 
```
6. Для изменения DNS необходимо внести строки в файл конфигурации сетевого интерфейса ``` /etc/sysconfig/network-scripts/ifcfg-eth0 ```
также туда необходимо внести изменения для того что бы наши сетевые настройки не пропали после перезагрузки. Приводим к виду:
```
DEVICE="eth0"
BOOTPROTO="static"
IPADDR0=10.0.2.25
PREFIX0=24
GATEWAY0=10.0.2.5
DNS1=8.8.8.8
ONBOOT="yes"
TYPE="Ethernet"
```
9. Для применения измений в файле необходимо перезагрузить демон сети ``` systemctl restart network ```
