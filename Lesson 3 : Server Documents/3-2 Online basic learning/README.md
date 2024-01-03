# Linux Development - Online basic learning



## 📣 Introduction to Internet related terms


**People nowadays are inseparable from the Internet, especially after the popularity of smart phone applications, the younger generation has been following Internet activities since they were born! However, we don't just "know how to use it", information personnel still need to understand the physical connection methods at the bottom of these networks, as well as network software related concepts, so that if there is a problem with the network in the future, or if you want to engage in network information The industry will not cause the network connection to crash just because some small details were not noticed! Therefore, let's first introduce the common connection methods and related hardware in the local network, and then introduce the network architecture (the concept of OSI seven-layer stacking), as well as the currently applied TCP/IP protocol! It would be great to know! No need to delve into the more internal theories!**





### 🎓 network components

**Generally speaking, in computer classrooms within companies or schools, we use a switch as the center and connect all devices together with physical network lines. Sometimes an additional wireless access point (AP) is provided to provide connection for smartphones or laptops. This switch will then be connected to a router or Linux firewall with more than two network interfaces, and then connected to the Internet through the assistance of an Internet Service Provider (ISP). The relevant connection diagram looks a bit like this:**



* **Node: A node is mainly a device with a network address (IP address). Therefore, the general PC, network server, router/firewall, network printer, etc. in the above diagram can be used individually. called a node. Is the switch (swich/hub) in the middle a node? Basically, the base switch does not have IP, so it does not count as a node.**


* **Network server (server): In terms of the direction of the network connection, the host that provides data to "respond" to the user can be called a server. For example, Google is a WWW server, and Bird Station is also a web server. Of course, there must be "services" and related "data" provided on the server!**


* **Workstation (workstation) or client (client): Any device that can input on the computer network can be a workstation. In terms of the direction of connection initiation, if the connection is actively initiated to "request" data, it can be called Is the client. For example, if a general PC opens a browser and requests news information from Google, then the general PC is the client.**


* **Router or gateway: A device with more than two network interfaces that can connect more than two different network segments. For example, an IP sharer is a common router device. Brother Niao prefers to use a power-saving Linux server as a firewall or router! (You will find that among the node devices in the picture above, only the "router/communication gateway" and "wireless AP" have more than two Internet connection lines, and the others only have one line. This means that these two nodes have More than two network interfaces)**


* **Wireless AP (Access point): Wireless AP also has two interfaces. In fact, it has the same function as the router above and can communicate with two different interfaces for transmission. One of the interfaces uses a wired network to connect to the upper router, and the other interface uses the wireless base station function to provide clients (smartphones, laptops) with wireless network cards to connect.**


* **Network Interface Card (NIC): A device built in or plugged into the host. It mainly provides network connection cards. Currently, most Ethernet network cards with RJ-45 connectors are used (later (Explanation), in network media above 25Gbps, most Ethernet networks using optical fiber connectors. Generally, a node has more than one network card to achieve network connection function. In addition, when you need a wireless network temporarily, you can also set it up through a wireless network card with a USB interface!**


#### LAN and WAN

**Among the wireless APs you come into contact with, you should often see the names of two ports (the jacks on the back of the IP sharer), one is LAN and the other is WAN. What are these two things? Basically, the original definition is this:**




**Local Area Network (LAN):**

* **The transmission distance between nodes is relatively short, such as within a building or within a school campus. More expensive wiring materials can be used, such as fiber optics or high-quality network cables (CAT 6). The network speed is faster, the connection quality is better and more reliable, so it can be applied to cluster systems, distributed systems, cloud load sharing systems, etc. for scientific computing.**




**Wide Area Network (WAN):**

* **Transmission distances are long, such as between cities, and therefore the connection media used require cheaper equipment, such as frequently used telephone lines. Due to the poor quality of the wire, the network speed is slower and the reliability is lower. Most network applications are similar to email, FTP, WWW browsing and other functions.**





#### Under Linux, use ethtool to observe the mechanisms supported by the physical network card.


**We can observe/set the physical network card through the ethtool command provided by Linux! Basically, the actual limitations of the network card in the virtual machine probably follow the physical network card, and it is not easy to modify the related mechanism. Therefore, the main thing below is to connect to the KVM host to find the physical network card, and then observe whether the relevant protocol mechanism mentioned above appears?**


```

# 1. 在 KVM host 實體主機上面，先找到實體網卡的名稱
[root@cloud ~]# ip link show
....
2: eno1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP mode DEFAULT group default qlen 1000
    link/ether fc:34:97:3b:eb:86 brd ff:ff:ff:ff:ff:ff
    altname enp0s31f6
....
# 我這部主機的實體網路卡名稱為 eno1 喔！透過 ip link show 去找出來的網卡名稱
# 由於網卡名稱與匯流排位址有關，所以每部主機的網卡名稱都不會相同喔！請自己找出來！

# 2. 使用 ethtool 觀察目前的連線狀態與機制
[root@cloud ~]# ethtool eno1
Settings for eno1:
        Supported ports: [ TP ]
        Supported link modes:   10baseT/Half 10baseT/Full
                                100baseT/Half 100baseT/Full
                                1000baseT/Full
        Supported pause frame use: No
        Supports auto-negotiation: Yes
        Supported FEC modes: Not reported
        Advertised link modes:  10baseT/Half 10baseT/Full
                                100baseT/Half 100baseT/Full
                                1000baseT/Full
        Advertised pause frame use: No
        Advertised auto-negotiation: Yes
        Advertised FEC modes: Not reported
        Speed: 1000Mb/s
        Duplex: Full
        Auto-negotiation: on
        Port: Twisted Pair
        PHYAD: 1
        Transceiver: internal
        MDI-X: off (auto)
        Supports Wake-on: pumbg
        Wake-on: g
        Current message level: 0x00000007 (7)
                               drv probe link
        Link detected: yes

```


**As shown above, we can see that the supported connection mode can be up to 1000baseT/Full, so this network card supports speeds of 1Gbps. Then looking down, the current speed is 1000Mb/s, and it is full duplex (Duplex: Full), and also has automatic negotiation (Autonegotiation: on). The network cable used is twisted pair (Twisted Pair), this is 8P8C of wire! The last Link detected: yes means that the network card is currently connected. Looking at it one by one, it’s actually quite clear!**

**Now, let us take a look, what will the network card parameters in the virtual machine look like?**


```

# 1. 確認一下我們啟動的是 test1 虛擬機器，用來作為測試。demo1 非必要，不要再啟用囉！
[root@cloud ~]# virsh create /kvm/xml/test1.xml
[root@cloud ~]# virsh list
 Id   Name    State
-----------------------
 1    test1   running

[root@cloud ~]# ssh vbird@192.168.10.52
# 請依據您的環境來連線到虛擬機裡面去！後續也是這樣做即可！

# 2. 同樣的，在虛擬機器裡面，先找出網卡名稱
[root@localhost ~]# ip link show
2: enp1s0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP mode DEFAULT group default qlen 1000
    link/ether 52:54:00:ed:63:aa brd ff:ff:ff:ff:ff:ff

# 2. 就來測試一下，到底虛擬網卡的機制長怎樣？
[root@localhost ~]# ethtool enp1s0
Settings for enp1s0:
        Supported ports: [  ]
        Supported link modes:   Not reported
        Supported pause frame use: No
        Supports auto-negotiation: No
        Supported FEC modes: Not reported
        Advertised link modes:  Not reported
        Advertised pause frame use: No
        Advertised auto-negotiation: No
        Advertised FEC modes: Not reported
        Speed: Unknown!
        Duplex: Unknown! (255)
        Auto-negotiation: off
        Port: Other
        PHYAD: 0
        Transceiver: internal
        Link detected: yes

```



**Except for Link detected, which appears normal, everything else... is all weird. If you want to change the speed and full-duplex type, you can! Sometimes, when some third-party software is executed, the network speed is required to be above 1Gbps before it is allowed to run! So! At this time, you have to make modifications! Use root to make changes in the virtual machine!**


```

# 基本語法
[root@localhost ~]# ethtool -s dev [speed NN] [duplex full|half] [mdix auto|on|off] [autoneg on|off]

# 注意，是在虛擬機器改喔！將速度改為 1000 且為全雙工
[root@localhost ~]# ethtool -s enp1s0 speed 1000 duplex full
[root@localhost ~]# ethtool enp1s0
Settings for enp1s0:
        ....
        Speed: 1000Mb/s
        Duplex: Full
        ....

```





### 🎓 OSI seven-layer protocol


**Just like language, if there is no consistent standard, then everyone will speak their own language, and no one can understand anyone else's language ~ then communication will be impossible! The same is true in the Internet world. In addition to the above-mentioned hardware standards (Ethernet), there is also information on the so-called network OSI seven-layer protocol (Open System Interconnection, OSI), which is intended to guide operating systems that want to join the network. At the time of writing, the network connection thread model needs to be written. This OSI seven-layer protocol only proposes a model and concept needed to join the network. As long as the operating system can be designed with reference to the specifications of the OSI seven-layer protocol, it can theoretically join the network with the OSI seven-layer protocol as the common standard. The meaning of road world. (A bit like the current dominant language is still English, so when you go abroad, everyone uses English to communicate, which means the same thing.)**




#### Seven layers of stacking? What is the purpose?


**The network is not just the network, why is it divided into 7 layers? In addition to the clear definition of each layer, which makes the design more convenient, debugging is also more convenient when designing at different levels. If you have ever designed a procedural program, you should know that if all the data is written in the main program, it will be difficult to maintain when the main program becomes larger and larger in the future! At the same time, if a colleague wants to assist you in the maintenance and development of your program, it will cause a lot of trouble... Everyone has different coding habits~**

**Therefore, split the main program into several functions and call them only when you need to use them! Each function has specific input/output parameters, so when you want to use it, fill in the fixed parameters, and you can also expect what data the function will output! This makes it easy to maintain! If a colleague wants to help with a function that is not so well written, he only needs to read the content of the function, and does not need to read all the main programs in full! What a pleasure! That’s one of the benefits of layering!**


&nbsp; <img src="./Images/Explanation of the layered responsible content of the OSI seven-layer protocol.png" alt="Explanation of the layered responsible content of the OSI seven-layer protocol"/>




### 🎓 routing concept (gateway)


**We now know that computer systems in a local area network (LAN) can directly exchange data through broadcasts between network cards. But what if the target host you want to transmit is not in the LAN? For example, when you have to connect to Google to check information, how will the packet data go? Basically, it’s the same as human parcel circulation! You have to take your home package to the post office at the intersection, or through a convenience store like TA-Q-BIN at the intersection. After filling in your information, give it to their postman for transmission ~ within the local network This post office/TA-Q-BIN is the so-called communication gateway or router!**


**In other words, when the destination of your packet transmission is within the local network, the packet can be directly delivered to the destination host for collection through broadcast. When the destination of your packet transmission is not within the local network, the packet will be transmitted to the default communication gateway through your own routing table, and the device will actively transmit it for you! Therefore, this communication gateway is an important link between your local network and the entire Internet society! Generally speaking, it is the IP sharer at our home/business!**


**Generally speaking, the IP address range of the router is usually within your local network. The more common router IP address is usually the last (or first) of the available IP address range. This is better to keep with Resolution ~ Since the router is also in your local network, as long as you set the default router address, your packets can usually flow freely on the Internet. Of course, some special mechanisms, such as pppoe dial-up through network cards, are more special through point-to-point connections. In any case, the network parameters of our host system must be preset. All you need is a communication gateway to be able to connect to the Internet.**






#### Observe the status of the local routing table


**Whether it is broadcasting or transmitting packets from your host system through a communication gateway, the transmission of this packet is carried out through the routing table on your local machine! If the routing table is designed incorrectly, your packets may not be successfully transmitted to the correct destination. So how do you observe the routing table? It's very simple, just pass the route command:**

```

# 1. 使用 route -n 觀察虛擬機器的路由表
[root@localhost ~]# route -n
Kernel IP routing table
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
0.0.0.0         192.168.10.254  0.0.0.0         UG    100    0        0 enp1s0
192.168.10.0    0.0.0.0         255.255.255.0   U     100    0        0 enp1s0

```


**Destination: Actually it means Network IP address!**

**Gateway: It is the Gateway IP address of the interface. If it is 0.0.0.0, it means direct broadcast without additional
gateway IP address.**

**Genmask: It is Netmask, which can be combined with the Destination field to form a local network domain.**

**Flags: Flags, the following are common flag meanings:**

* **U: indicates that the route is available**  
* **G: Indicates that the route needs to be forwarded through the gateway.**  
* **H: It means that the route is a host, not an entire network domain.**  

**Iface: Basically, it is the network card interface.**





#### Add/delete routing information


**In fact, communication does not necessarily need to be on the same network segment! You can also force our hosts to communicate with each other through custom routes! However, it should be noted that the network connection may be: "You can connect to the past, but the other party cannot connect back..."! Therefore, the network settings must be done in both directions! Now, let's "manually" adjust the design to see if we can add/delete certain routes out of thin air? It’s also easy to handle using the route command!**


```

# 1. 在虛擬機上面，針對 192.168.20.0/24 進行直接廣播傳遞資訊
[root@localhost ~]# route add -net 192.168.20.0 netmask 255.255.255.0 dev enp1s0

# 2. 在虛擬機上面，針對 192.168.30.5 進行直接廣播傳遞資訊
[root@localhost ~]# route add -host 192.168.30.5  dev enp1s0

[root@localhost ~]# route -n
Kernel IP routing table
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
0.0.0.0         192.168.10.254  0.0.0.0         UG    100    0        0 enp1s0
192.168.10.0    0.0.0.0         255.255.255.0   U     100    0        0 enp1s0
192.168.20.0    0.0.0.0         255.255.255.0   U     0      0        0 enp1s0  <==針對網域
192.168.30.5    0.0.0.0         255.255.255.255 UH    0      0        0 enp1s0  <==針對單一主機

# 3. 刪除上面兩條新建的路由
[root@localhost ~]# route del -net 192.168.20.0 netmask 255.255.255.0 dev enp1s0
[root@localhost ~]# route del -host 192.168.30.5  dev enp1s0

```





#### Use traceroute to query the communication gateway passed

Sometimes, you may be interested in checking the route of how the postman delivers the goods to the other party. Then can you know how your packet is delivered to the destination? We can track it through traceroute. However, many communication gateway firewalls currently have the function of turning off tracking. Therefore, you cannot query many communication gateways! That's normal too. Usually we can do a simple ICMP query without any problems. If we are worried about something going wrong, we can also use UDP (port 53) to do a simple query! It won’t get stuck for too long!


```

# 在虛擬機器上面，查詢你到 168.95.1.1 這部系統中間，到底經過幾個通訊閘？
[root@localhost ~]# yum -y install traceroute
[root@localhost ~]# traceroute -I 168.95.1.1 -n
 1  192.168.10.254  0.161 ms  0.139 ms  0.137 ms
 2  192.168.201.254  0.357 ms  0.356 ms  0.355 ms
 3  172.20.168.254  1.348 ms  1.534 ms  1.744 ms
 4  * * *
 5  120.114.50.201  1.099 ms  1.141 ms  1.152 ms
 6  * * *
 7  * * *
 8  * * *
 9  * * *
10  * * *
11  * * *
12  168.95.1.1  2.430 ms  2.428 ms  2.426 ms

[root@localhost ~]# traceroute -U 168.95.1.1 -n
 1  192.168.10.254  0.258 ms  0.221 ms  0.194 ms
 2  192.168.201.254  0.268 ms  0.232 ms  0.219 ms
 3  172.20.168.254  1.155 ms  1.222 ms  1.322 ms
 4  * * *
 5  120.114.50.201  0.982 ms  0.972 ms  0.974 ms
 6  * * *
 7  * * *
 8  * * *
 9  * * *
10  * * *
11  * * *
12  * * *
13  168.95.1.1  2.429 ms  2.546 ms  2.333 ms

```
