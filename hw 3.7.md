## Домашнее задание к занятию "3.7. Компьютерные сети, лекция 2"

### 1. Проверьте список доступных сетевых интерфейсов на вашем компьютере. Какие команды есть для этого в Linux и в Windows?

```commandline
В Windows - ipconfig /all
В Linux - ip a, ifconfig -a, nwcli

В виртуалке U 20.04:
vagrant@vagrant:~$ ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:a2:6b:fd brd ff:ff:ff:ff:ff:ff
    inet 10.0.2.15/24 brd 10.0.2.255 scope global dynamic eth0
       valid_lft 50556sec preferred_lft 50556sec
    inet6 fe80::a00:27ff:fea2:6bfd/64 scope link 
       valid_lft forever preferred_lft forever
3: eth1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:15:ae:22 brd ff:ff:ff:ff:ff:ff
    inet 192.168.56.2/24 brd 192.168.56.255 scope global eth1
       valid_lft forever preferred_lft forever
    inet6 fe80::a00:27ff:fe15:ae22/64 scope link 
       valid_lft forever preferred_lft forever
4: eth2: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:36:13:06 brd ff:ff:ff:ff:ff:ff
    inet 192.168.56.3/24 brd 192.168.56.255 scope global eth2
       valid_lft forever preferred_lft forever
    inet6 fe80::a00:27ff:fe36:1306/64 scope link 
       valid_lft forever preferred_lft forever
_____________________________________________________________________

На основной Ubuntu 22.04:
~$ ip a
eno1: connected to Wired connection 1
        "Intel I219-V"
        ethernet (e1000e), 04:92:26:C0:AD:E0, hw, mtu 1500
        ip4 default
        inet4 192.168.88.202/24
        route4 192.168.88.0/24 metric 100
        route4 default via 192.168.88.1 metric 100
        route4 169.254.0.0/16 metric 1000
        inet6 fe80::687a:c257:9cd1:9a41/64
        route6 fe80::/64 metric 1024

kvnet: connected (externally) to kvnet
        "kvnet"
        tun, 72:BE:BE:86:30:F8, sw, mtu 1400
        inet4 172.26.127.18/24
        route4 172.26.127.0/24 metric 0
        route4 172.16.0.0/24 via 172.26.127.1 metric 1
        route4 192.168.0.0/24 via 172.26.127.1 metric 1
        inet6 fe80::70be:beff:fe86:30f8/64
        route6 fe80::/64 metric 256

vboxnet0: unmanaged
        "vboxnet0"
        ethernet (vboxnet), 0A:00:27:00:00:00, hw, mtu 1500

vmnet1: unmanaged
        "vmnet1"
        ethernet (unknown), 00:50:56:C0:00:01, hw, mtu 1500

vmnet8: unmanaged
        "vmnet8"
        ethernet (unknown), 00:50:56:C0:00:08, hw, mtu 1500

lo: unmanaged
        "lo"
        loopback (unknown), 00:00:00:00:00:00, sw, mtu 65536

DNS configuration:
        servers: 8.8.4.4 8.8.8.8
        interface: eno1

```

### 2. Какой протокол используется для распознавания соседа по сетевому интерфейсу? Какой пакет и команды есть в Linux для этого?

```commandline
Link Layer Discovery Protocol (LLDP) — протокол канального уровня
Пакет lldpd, команды lldpctl, lldpcli

$ lldpctl
-------------------------------------------------------------------------------
LLDP neighbors:
-------------------------------------------------------------------------------
Interface:    eno1, via: LLDP, RID: 2, Time: 0 day, 00:01:26
  Chassis:     
    ChassisID:    mac 0c:9d:92:53:ef:78
    SysName:      RT-AC68U
    SysDescr:      Linux 2.6.36.4brcmarm #1 SMP PREEMPT Sun Jul 24 17:39:39 EDT 2022 armv7l
    MgmtIP:       127.0.1.1
    MgmtIface:    1
    Capability:   Bridge, on
    Capability:   Router, on
    Capability:   Wlan, off
    Capability:   Station, off
  Port:        
    PortID:       mac 0c:9d:92:53:ef:78
    PortDescr:    vlan1
    TTL:          20
  Unknown TLVs:
    TLV:          OUI: F8,32,E4, SubType: 3, Len: 20 F7,2B,50,8E,0D,E9,1A,81,07,BE,C2,0A,5E,66,8C,BC,5A,ED,3C,03
    TLV:          OUI: F8,32,E4, SubType: 2, Len: 4 00,00,00,00
    TLV:          OUI: F8,32,E4, SubType: 18, Len: 4 00,00,00,00
    TLV:          OUI: F8,32,E4, SubType: 19, Len: 4 78,7C,00,00
-------------------------------------------------------------------------------
```  

### 2. Какая технология используется для разделения L2 коммутатора на несколько виртуальных сетей? Какой пакет и команды есть в Linux для этого? Приведите пример конфига.

```commandline
Технология VLAN, стандарт IEEE 802.1q
Пакет vlan

vagrant@vagrant:~$ sudo modprobe 8021q
vagrant@vagrant:~$ sudo ip link add link eth1 eth1.5 type vlan id 5
vagrant@vagrant:~$ sudo ip address add 172.16.0.3/24 dev eth1.5
vagrant@vagrant:~$ sudo ip link set eth1.5 up
vagrant@vagrant:~$ ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:a2:6b:fd brd ff:ff:ff:ff:ff:ff
    inet 10.0.2.15/24 brd 10.0.2.255 scope global dynamic eth0
       valid_lft 48328sec preferred_lft 48328sec
    inet6 fe80::a00:27ff:fea2:6bfd/64 scope link 
       valid_lft forever preferred_lft forever
3: eth1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:15:ae:22 brd ff:ff:ff:ff:ff:ff
    inet 192.168.56.2/24 brd 192.168.56.255 scope global eth1
       valid_lft forever preferred_lft forever
    inet6 fe80::a00:27ff:fe15:ae22/64 scope link 
       valid_lft forever preferred_lft forever
4: eth2: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:36:13:06 brd ff:ff:ff:ff:ff:ff
    inet 192.168.56.3/24 brd 192.168.56.255 scope global eth2
       valid_lft forever preferred_lft forever
    inet6 fe80::a00:27ff:fe36:1306/64 scope link 
       valid_lft forever preferred_lft forever
5: eth1.5@eth1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default qlen 1000
    link/ether 08:00:27:15:ae:22 brd ff:ff:ff:ff:ff:ff
    inet 172.16.0.3/24 scope global eth1.5
       valid_lft forever preferred_lft forever
    inet6 fe80::a00:27ff:fe15:ae22/64 scope link 
       valid_lft forever preferred_lft forever

```


### 3. Какие типы агрегации интерфейсов есть в Linux? Какие опции есть для балансировки нагрузки? Приведите пример конфига.

```commandline
Из интернета:

mode=0 (balance-rr)
Этот режим используется по-умолчанию, если в настройках не указано другое. balance-rr обеспечивает балансировку нагрузки и отказоустойчивость. 
В данном режиме пакеты отправляются "по кругу" от первого интерфейса к последнему и сначала. Если выходит из строя один из интерфейсов, 
пакеты отправляются на остальные оставшиеся.При подключении портов к разным коммутаторам, требует их настройки.

mode=1 (active-backup)
При active-backup один интерфейс работает в активном режиме, остальные в ожидающем. Если активный падает, управление передается одному из ожидающих. 
Не требует поддержки данной функциональности от коммутатора.

mode=2 (balance-xor)
Передача пакетов распределяется между объединенными интерфейсами по формуле ((MAC-адрес источника) XOR (MAC-адрес получателя)) % число интерфейсов. 
Один и тот же интерфейс работает с определённым получателем. Режим даёт балансировку нагрузки и отказоустойчивость.

mode=3 (broadcast)
Происходит передача во все объединенные интерфейсы, обеспечивая отказоустойчивость.

mode=4 (802.3ad)
Это динамическое объединение портов. В данном режиме можно получить значительное увеличение пропускной способности как входящего 
так и исходящего трафика, используя все объединенные интерфейсы. 
Требует поддержки режима от коммутатора, а так же (иногда) дополнительную настройку коммутатора.

mode=5 (balance-tlb)
Адаптивная балансировка нагрузки. При balance-tlb входящий трафик получается только активным интерфейсом, 
исходящий - распределяется в зависимости от текущей загрузки каждого интерфейса. 
Обеспечивается отказоустойчивость и распределение нагрузки исходящего трафика. Не требует специальной поддержки коммутатора.

mode=6 (balance-alb)
Адаптивная балансировка нагрузки (более совершенная). Обеспечивает балансировку нагрузки как исходящего (TLB, transmit load balancing), 
так и входящего трафика (для IPv4 через ARP). Не требует специальной поддержки коммутатором, но требует возможности изменять MAC-адрес устройства.


```

```commandline
vagrant@vagrant:~$ cat /etc/netplan/50-vagrant.yaml 
---
network:
  version: 2
  renderer: networkd
  ethernets:
    eth1:
      dhcp4: no
    eth2:
      dhcp4: no
  bonds:
    bond0:
      interfaces: [eth1, eth2]
      addresses: [192.168.56.200/24]
      gateway4: 192.168.56.1
      parameters:
        mode: balance-tlb
        transmit-hash-policy: layer2
        
vagrant@vagrant:~$ sudo netplan apply

vagrant@vagrant:~$ sudo ip a show dev bond0
6: bond0: <BROADCAST,MULTICAST,MASTER,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default qlen 1000
    link/ether 7e:db:18:c7:ba:ce brd ff:ff:ff:ff:ff:ff
    inet 192.168.56.200/24 brd 192.168.56.255 scope global bond0
       valid_lft forever preferred_lft forever
    inet6 fe80::7cdb:18ff:fec7:bace/64 scope link 
       valid_lft forever preferred_lft forever
```

### 4. Сколько IP адресов в сети с маской /29 ? Сколько /29 подсетей можно получить из сети с маской /24. Приведите несколько примеров /29 подсетей внутри сети 10.10.10.0/24.

```commandline
В сети с маской /29 8 адресов, из них 6 для хостов
Из сети с маской /24 можно получить 32 подсети с маской /29 (256 делим на 8)

Примеры:
10.10.10.0/29
10.10.10.8/29
10.10.10.16/29
...
10.10.10.240/29
10.10.10.248/29
```
### 5. Задача: вас попросили организовать стык между 2-мя организациями. Диапазоны 10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16 уже заняты. Из какой подсети допустимо взять частные IP адреса? Маску выберите из расчета максимум 40-50 хостов внутри подсети.

```commandline
Можно использовать подсеть CGN(Carrier-Grade NAT) - 100.64.0.0/10
Для 40-50 хостов подойдет подсеть с маской /26 на 64 адреса, 62 хоста
Например 100.67.10.0/26

~$ ipcalc 100.67.10.0/26
Address:   100.67.10.0          01100100.01000011.00001010.00 000000
Netmask:   255.255.255.192 = 26 11111111.11111111.11111111.11 000000
Wildcard:  0.0.0.63             00000000.00000000.00000000.00 111111
=>
Network:   100.67.10.0/26       01100100.01000011.00001010.00 000000
HostMin:   100.67.10.1          01100100.01000011.00001010.00 000001
HostMax:   100.67.10.62         01100100.01000011.00001010.00 111110
Broadcast: 100.67.10.63         01100100.01000011.00001010.00 111111
Hosts/Net: 62                    Class A
```

### 6. Как проверить ARP таблицу в Linux, Windows? Как очистить ARP кеш полностью? Как из ARP таблицы удалить только один нужный IP?

```commandline
В Windows используется команда arp -a:
c:\>arp -a

Interface: 192.168.56.1 --- 0x3
  Internet Address      Physical Address      Type
  192.168.56.255        ff-ff-ff-ff-ff-ff     static
  224.0.0.22            01-00-5e-00-00-16     static
  224.0.0.251           01-00-5e-00-00-fb     static
  224.0.0.252           01-00-5e-00-00-fc     static
  224.0.0.253           01-00-5e-00-00-fd     static
  239.192.152.143       01-00-5e-40-98-8f     static
  239.255.102.18        01-00-5e-7f-66-12     static
  239.255.255.250       01-00-5e-7f-ff-fa     static
  239.255.255.253       01-00-5e-7f-ff-fd     static
  255.255.255.255       ff-ff-ff-ff-ff-ff     static

Interface: 192.168.0.106 --- 0x10
  Internet Address      Physical Address      Type
  192.168.0.11          68-05-ca-1f-7e-b9     dynamic
  192.168.0.21          68-05-ca-5a-c9-58     dynamic
  192.168.0.34          00-15-5d-00-20-03     dynamic
  192.168.0.35          ac-1f-6b-b0-c9-c5     dynamic
  192.168.0.40          00-15-5d-00-20-04     dynamic
  
В Linux - arp, ip neighbour:
~$ arp
Address                  HWtype  HWaddress           Flags Mask            Iface
192.168.56.200           ether   7e:db:18:c7:ba:ce   C                     vboxnet0
192.168.88.1             ether   c4:ad:34:0c:2e:53   C                     eno1
172.26.127.1             ether   44:45:53:53:01:00   C                     kvnet 

Очистить весь кеш можно командой sudo ip -s neighbour flush all:
~$ sudo ip -s neighbour flush all

*** Round 1, deleting 2 entries ***
*** Flush is complete after 1 round ***

Удалить конкретный IP:
~$ sudo arp -d 172.26.127.1

или

~$ sudo ip neighbour
192.168.88.1 dev eno1 lladdr c4:ad:34:0c:2e:53 REACHABLE
172.26.127.1 dev kvnet lladdr 44:45:53:53:01:00 REACHABLE
nikita@uwork-comp:~$ sudo ip -s neighbour delete 192.168.88.1 dev eno1

В Windows:
C:\WINDOWS\system32>arp -d inet_addr 192.168.0.50

C:\WINDOWS\system32>netsh interface IP delete arpcache

```