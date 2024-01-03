# Basic Learning for RockyLinux 9


## 📣 Installation and adjustment of the first virtual machine


**After setting up the basic KVM host system in the previous chapter, we are now ready to build the first VM system. We do not use the native qemu-kvm command to create a VM. Instead, we use the libvirtd environment with supervisory functions and qemu-related peripheral hardware simulation functions to set up and create various virtual hardware related to it. After the creation is completed, we can actually install another set of Linux through this virtual machine! That is to say, another logically completely independent Linux system is generated on Linux!**






## 📋 libvirtd and virsh on KVM host


#### Linux core kvm functions and parameters

**As mentioned earlier, KVM is actually part of the Linux core, so is the kvm module loaded into the current core? In addition, are there any additional parameters for KVM that can be modified? Let’s take a look:**



```

# 先看看目前的核心有沒有載入 kvm 的相關模組了？
[root@cloud ~]# lsmod | grep kvm
kvm_intel             364544  0
kvm                  1056768  1 kvm_intel
irqbypass              16384  1 kvm
# 有喔！有 kvm 以及 kvm_intel 這兩個已經被載入的模組！

[root@cloud ~]# modinfo kvm
filename:       /lib/modules/5.14.0-284.18.1.el9_2.x86_64/kernel/arch/x86/kvm/kvm.ko.xz
license:        GPL
author:         Qumranet
rhelversion:    9.2
srcversion:     46781597FA45B5FA677EBFF
depends:        irqbypass
retpoline:      Y
intree:         Y
name:           kvm
vermagic:       5.14.0-284.18.1.el9_2.x86_64 SMP preempt mod_unload modversions
....
parm:           tdp_mmu:bool
parm:           mmio_caching:bool
parm:           nx_huge_pages:bool
....

[root@cloud ~]# modinfo kvm_intel
filename:       /lib/modules/5.14.0-284.18.1.el9_2.x86_64/kernel/arch/x86/kvm/kvm-intel.ko.xz
license:        GPL
author:         Qumranet
rhelversion:    9.2
srcversion:     D8E06AE51DCBA93DB578DF9
alias:          cpu:type:x86,ven*fam*mod*:feature:*0085*
depends:        kvm
retpoline:      Y
intree:         Y
name:           kvm_intel
vermagic:       5.14.0-284.18.1.el9_2.x86_64 SMP preempt mod_unload modversions
....
parm:           enable_apicv:bool
parm:           enable_ipiv:bool
parm:           nested:bool
parm:           pml:bool
....

```







**You can see that there are currently two modules with the kvm keyword loaded, and you can also see module-related information through modinfo. The parm behind the module information is the module parameters that can be modified! For example, kvm_intel has a nested parameter, which can be enabled (1) or disabled (0). If we need to modify this parameter, we need to process the files in /etc/modprobe.d! qemu-kvm-common provides a configuration file named /etc/modprobe.d/kvm.conf. You can take a look:**


```

# 修改 kvm.conf 之後，卸載再掛載 kvm_intel 看看：
[root@cloud ~]# vim /etc/modprobe.d/kvm.conf
# For Intel
options kvm_intel nested=1

[root@cloud ~]# modprobe -r kvm_intel
[root@cloud ~]# modprobe kvm_intel
[root@cloud ~]# cat /sys/module/kvm_intel/parameters/nested
Y
# 確實修訂為 Y 了！

```







#### libvirtd service observation and virsh simple operation


**libvirtd provides quite a few services! At present, we mostly use the concept of "start the libvirtd service only when it is useful". Therefore, the main service to be started will be "libvirtd.socket"! In any case, just check it by observation:**


```

# 看看 libvirtd 有沒有啟動！？
[root@cloud ~]# systemctl status libvirtd.socket
○ libvirtd.socket - Libvirt local socket
     Loaded: loaded (/usr/lib/systemd/system/libvirtd.socket; enabled; preset: disabled)
     Active: inactive (dead)
   Triggers: ● libvirtd.service
     Listen: /run/libvirt/libvirt-sock (Stream)
# 特別注意，觀察的是 libvirtd.socket 而不是 libvirtd.service 喔！
# 很奇怪的是，在某些時刻，服務就是啟動不了！沒關係，我們手動來重新啟動它！

[root@cloud ~]# systemctl start libvirtd.socket

```




**Basically, just make sure it is enabled! Because we do not have a graphical interface, the focus should be on the operation of the virsh command! This virsh is the tool software used by libvirtd to manage VMs! You can use "virsh --help" to check the usage of virsh! It has so many functions that it will make you dizzy...so Brother Bird is not going to introduce the detailed usage of virsh in detail here. I will only introduce a few commonly used commands by Brother Bird. First, let us analyze, if libvirtd creates a VM, what will be its optimal hardware settings?**


```

# 檢查本機的相關虛擬化能力
[root@cloud ~]# virsh capabilities
<capabilities>

  <host>   <!--跟本機 CPU 較為相關的參數部份-->
    <uuid>f113f623-9a64-7d4e-ae37-184224474e59</uuid>
    <cpu>
      <arch>x86_64</arch>
      <model>Skylake-Client-noTSX-IBRS</model>
      <vendor>Intel</vendor>
      <microcode version='244'/>
      <signature family='6' model='165' stepping='3'/>
      <counter name='tsc' frequency='3095999000' scaling='no'/>
      <topology sockets='1' dies='1' cores='6' threads='2'/>
      <maxphysaddr mode='emulate' bits='39'/>
      <feature name='ds'/>
      ....
    </cpu>
    ....
  </host>  <!--結束跟本機 CPU 較為相關的參數部份-->

  <guest>  <!--跟用戶端 (Guest) 相關性較高的資料，這是 32 位元部份-->
    <os_type>hvm</os_type>
    <arch name='i686'>
      <wordsize>32</wordsize>
      <emulator>/usr/libexec/qemu-kvm</emulator>
      <machine maxCpus='240' deprecated='yes'>pc-i440fx-rhel7.6.0</machine>
      <machine canonical='pc-i440fx-rhel7.6.0' maxCpus='240' deprecated='yes'>pc</machine>
      ....
      <domain type='qemu'/>
      <domain type='kvm'/>
    </arch>
    ....
  </guest>

  <guest>  <!--跟用戶端 (Guest) 相關性較高的資料，這是 64 位元部份-->
    <os_type>hvm</os_type>
    <arch name='x86_64'>
      <wordsize>64</wordsize>
      <emulator>/usr/libexec/qemu-kvm</emulator>
      <machine maxCpus='240' deprecated='yes'>pc-i440fx-rhel7.6.0</machine>
      <machine canonical='pc-i440fx-rhel7.6.0' maxCpus='240' deprecated='yes'>pc</machine>
      ....
      <machine maxCpus='710'>pc-q35-rhel9.2.0</machine>
      <machine canonical='pc-q35-rhel9.2.0' maxCpus='710'>q35</machine>
      ....
      <domain type='qemu'/>
      <domain type='kvm'/>
    </arch>
    ....
  </guest>

</capabilities>

```








**From the table above, we can see the main CPU models, parameters, etc. You can list the above parameters in the subsequent hardware configuration file. In the <guest> project, we can also check the currently supported virtual machine chipset models! Generally, the default hardware is the older pc-i440fx chip, but the new one should use q35! You can find out whether your virtual machine supports the relevant chips here!**


```

# 檢查支援的 q35 晶片組，內部硬體模擬為何
[root@cloud ~]# virsh domcapabilities --machine q35 --arch x86_64
<domainCapabilities>
  <path>/usr/libexec/qemu-kvm</path>   <== KVM 主程式的路徑
  <domain>kvm</domain>
  <machine>pc-q35-rhel9.2.0</machine>
  <arch>x86_64</arch>
  <vcpu max='710'/>
  <iothreads supported='yes'/>
  <os supported='yes'>
    <enum name='firmware'>
      <value>efi</value>   <==主要使用 EFI 的開機引導方式
    </enum>
    <loader supported='yes'>
      <value>/usr/share/edk2/ovmf/OVMF_CODE.secboot.fd</value>  <== EFI 引導程式放置位置
      ....
    </loader>
  </os>
  <cpu>
    <mode name='host-passthrough' supported='yes'>  <==支援直接使用本機CPU
      <enum name='hostPassthroughMigratable'>
        <value>on</value>
        <value>off</value>
      </enum>
    </mode>
    <mode name='maximum' supported='yes'>
      <enum name='maximumMigratable'>
        <value>on</value>
        <value>off</value>
      </enum>
    </mode>
    <mode name='host-model' supported='yes'>
      <model fallback='forbid'>Skylake-Client-IBRS</model>
      <vendor>Intel</vendor>
      <feature policy='require' name='ss'/>
      ....
    </mode>
    <mode name='custom' supported='yes'>  <==有支援的其他模擬的 CPU 類型
      <model usable='yes' deprecated='yes' vendor='unknown'>qemu64</model>
      <model usable='yes' deprecated='yes' vendor='unknown'>qemu32</model>
      <model usable='no' deprecated='yes' vendor='AMD'>phenom</model>
      ....
    </mode>
  </cpu>
  ....
  <devices>  <==其他支援的週邊裝置
    <disk supported='yes'>  <==針對硬碟的支援類型
      <enum name='diskDevice'>
        <value>disk</value>
        <value>cdrom</value>
        <value>floppy</value>
        <value>lun</value>
      </enum>
      <enum name='bus'>     <==針對匯流排的支援類型
        <value>fdc</value>
        <value>scsi</value>
        <value>virtio</value>
        <value>usb</value>
        <value>sata</value>
      </enum>
    </disk>
    <graphics supported='yes'> <==針對圖形顯示的支援類型
      <enum name='type'>
        <value>vnc</value>
        <value>egl-headless</value>
      </enum>
    </graphics>
    <video supported='yes'>
      <enum name='modelType'>
        <value>vga</value>
        <value>cirrus</value>
        <value>virtio</value>
        <value>none</value>
        <value>bochs</value>
        <value>ramfb</value>
      </enum>
    </video>
  </devices>
  ....
</domainCapabilities>

```







## 📋 qemu configure network and disk

**/kvm/xml：主要放置各種 XML 規範的設定檔，包括網路、虛擬機器的設定等等**  
**/kvm/iso：主要放置來自網路上的各種 ISO 映像檔，例如 9.0, 9.2 的 RockyLinux 安裝光碟檔**  
**/kvm/img：虛擬磁碟的放置目錄，基本上，佔用的檔案系統容量最大！**  







#### Use qemu-img to create a virtual disk file

* **raw: Give a fixed file capacity in advance. As mentioned above, you must create a 100G archive.**

* **qcow2: The virtual disk file format that qemu copies on write. The capacity of a virtual disk can be specified, but the actual disk space occupied by that capacity is calculated based on usage.**






**Creating a virtual disk can be handled using qemu-img! For detailed command usage, please refer to qemu-img --help or related man pages. Below we only use qemu-img for creation and observation!**


```

# 建立一個 30G 的虛擬磁碟，名為 demo1.img
[root@cloud ~]# qemu-img create -f qcow2 -o cluster_size=[512,1K..2M] file.img size

[root@cloud ~]# mkdir /kvm/img
[root@cloud ~]# cd /kvm/img
[root@cloud img]# qemu-img create -f qcow2 -o cluster_size=2M demo1.img 30G
Formatting 'demo1.img', fmt=qcow2 cluster_size=2097152 extended_l2=off compression_type=zlib
  size=32212254720 lazy_refcounts=off refcount_bits=16

```









#### Create a bridge interface templan with NAT function


**Brother Niao doesn't like to use qemu's network very much, because after qemu's network is managed by libvirtd, it is too powerful! It has taken over the ports and firewalls of many physical machines, which makes Brother Bird, who likes to control the system simply, find it annoying ~ However, we have not yet talked about the network basics and firewalls, but currently we need to establish a bridge network card so that the virtual machine can Directly connect to the outside! Alas, there is really no way! Therefore, I had to create a bridge interface again. However, this bridge interface will be deleted directly after we finish talking about the basics of the network! That’s why Brother Bird named him templan!**



```

# 建立一個名為 templan 的網路界面
[root@cloud img]# mkdir /kvm/xml
[root@cloud img]# cd /kvm/xml
[root@cloud xml]# vim templan.xml
<network>
  <name>templan</name>
  <forward mode='nat'/>
  <bridge name='templan' stp='on' delay='0'/>
  <mac address='52:54:00:00:ff:01'/>
  <ip address='192.168.10.254' netmask='255.255.255.0'>
    <dhcp>
      <range start='192.168.10.1' end='192.168.10.100'/>
    </dhcp>
  </ip>
</network>

```



* **name: The name required by libvirtd to manage this network card**  
* **forward mode='nat': The virtual machine connects to the Internet using NAT (will be mentioned in a later chapter)**  
* **bridge name='templan': A network interface named templan will be generated on the physical machine.**  
* **mac address: the card number of the network interface on the physical machine**  
* **ip address: IP address parameter setting of this network card**  
* **dhcp...range..: Proactively provide services for automatically obtaining network parameters (dhcp will be mentioned later)**  
* **After it is properly created, we can activate this bridge card through virsh!**  


```

# virsh 管理網路的基本指令
[root@cloud xml]# virsh net-list
[root@cloud xml]# virsh net-create  file.xml  <==啟動網路設定
[root@cloud xml]# virsh net-destroy file.xml  <==終止網路設定

# 實際建立橋接網路界面，並且觀察建立後的網路參數
[root@cloud xml]# virsh net-create templan.xml
Network templan created from templan.xml

[root@cloud xml]# virsh net-list
 Name      State    Autostart   Persistent
--------------------------------------------
 templan   active   no          no

[root@cloud xml]# ip addr show
.....
4: templan: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc noqueue state DOWN group default qlen 1000
    link/ether 52:54:00:00:ff:01 brd ff:ff:ff:ff:ff:ff
    inet 192.168.10.254/24 brd 192.168.10.255 scope global templan
       valid_lft forever preferred_lft forever

```






## 📋 Virtual machine hardware configuration file (XML format)



* **Confirm virtual machine hardware that supports q35 via virsh capabilities**  
* **Confirm support for efi boot mode via virsh domcapabilities**  
* **Confirm host-passthrough support via virsh domcapabilities**  
* **The disc image file is placed as: /kvm/iso/Rocky-9.0-x86_64-minimal.iso**  
* **The hard disk file is placed in: /kvm/img/demo1.img**  
* **The bridge name is templan**  



**Let’s start using virt-install to create an XML virtual machine hardware configuration file!**



```

# 安裝 virt-install
[root@cloud ~]# yum -y install virt-install

# 開始使用 virt-install 規劃一個用戶端硬體資源
[root@cloud ~]# cd /kvm/xml
[root@cloud xml]# virt-install \
> --name demo1 --cpu host-passthrough,cache.mode=passthrough --vcpu 4 \
> --memory 2048 --memballoon virtio --machine q35 \
> --boot uefi,loader.type=pflash,nvram=/kvm/xml/demo1.uefi.fd,loader_secure=no \
> --controller virtio-scsi \
> --disk /kvm/img/demo1.img,cache=writeback,io=threads,device=disk,bus=virtio \
> --network network=templan,model=virtio \
> --graphics vnc,port=5911,listen=0.0.0.0,password=rocky9 \
> --cdrom /kvm/iso/Rocky-9.0-x86_64-minimal.iso --video virtio \
> --dry-run --print-xml > /kvm/xml/demo1.xml
# 詳細的參數請參考 man virt-install。上述指令執行完畢，產生 demo1.xml 檔案！

```






**After the configuration file is created, it's weird! There are actually two sets of settings in XML! They are all projects starting from <domain...>! Therefore, please edit the file first so that only one copy of the data exists, and confirm whether the setting information is correct.**


```
[root@cloud ~]# vim /kvm/xml/demo1.xml
<domain type="kvm">
  <name>demo1</name>
  <uuid>ad651202-0ff0-4214-88df-aa2fd0ce8611</uuid>
  <metadata>
    <libosinfo:libosinfo xmlns:libosinfo="http://libosinfo.org/xmlns/libvirt/domain/1.0">
      <libosinfo:os id="http://rockylinux.org/rocky/9"/>
    </libosinfo:libosinfo>
  </metadata>
  <memory>2097152</memory>
  <currentMemory>2097152</currentMemory>
  <vcpu>4</vcpu>
  <os>  <== 這一段前三行應該要修改一下喔！否則啟動會失敗！
    <type arch="x86_64" machine="q35">hvm</type>
    <loader readonly="yes" type="pflash" secure="no">/usr/share/edk2/ovmf/OVMF_CODE.secboot.fd</loader>
    <nvram>/kvm/xml/demo1.uefi.fd</nvram>
    <boot dev="cdrom"/>
    <boot dev="hd"/>
  </os>
  <features>
    <acpi/>
    <apic/>
  </features>
  <cpu mode="host-passthrough">
    <cache mode="passthrough"/>
  </cpu>
  <clock offset="utc">
    <timer name="rtc" tickpolicy="catchup"/>
    <timer name="pit" tickpolicy="delay"/>
    <timer name="hpet" present="no"/>
  </clock>
  <on_reboot>restart</on_reboot>  <== 建議增加這三行
  <on_poweroff>destroy</on_poweroff>
  <on_crash>restart</on_crash>
  <pm>
    <suspend-to-mem enabled="no"/>
    <suspend-to-disk enabled="no"/>
  </pm>
  <devices>
    <emulator>/usr/libexec/qemu-kvm</emulator>
    <disk type="file" device="disk"> <== 底下這兩個是硬碟與光碟設計
      <driver name="qemu" type="qcow2" cache="writeback" io="threads"/>
      <source file="/kvm/img/demo1.img"/>
      <target dev="vda" bus="virtio"/>
    </disk>
    <disk type="file" device="cdrom">
      <driver name="qemu" type="raw"/>
      <source file="/kvm/iso/Rocky-9.0-x86_64-minimal.iso"/>
      <target dev="sda" bus="sata"/>
      <readonly/>
    </disk>
    <controller type="virtio-scsi"/> <== 這個可以拿掉
    <controller type="usb" model="qemu-xhci" ports="15"/>
    <controller type="pci" model="pcie-root"/>
    <controller type="pci" model="pcie-root-port"/>
    <controller type="pci" model="pcie-root-port"/>
    <controller type="pci" model="pcie-root-port"/>
    <controller type="pci" model="pcie-root-port"/>
    <controller type="pci" model="pcie-root-port"/>
    <controller type="pci" model="pcie-root-port"/>
    <controller type="pci" model="pcie-root-port"/>
    <controller type="pci" model="pcie-root-port"/>
    <controller type="pci" model="pcie-root-port"/>
    <controller type="pci" model="pcie-root-port"/>
    <controller type="pci" model="pcie-root-port"/>
    <controller type="pci" model="pcie-root-port"/>
    <controller type="pci" model="pcie-root-port"/>
    <controller type="pci" model="pcie-root-port"/>
    <interface type="network">  <== 單純觀察一下網路卡界面
      <source network="templan"/>
      <mac address="52:54:00:ed:63:1f"/>
      <model type="virtio"/>
    </interface>
    <console type="pty"/>
    <channel type="unix">
      <source mode="bind"/>
      <target type="virtio" name="org.qemu.guest_agent.0"/>
    </channel>
    <input type="tablet" bus="usb"/>
    <tpm model="tpm-crb">
      <backend type="emulator"/>
    </tpm>
    <graphics type="vnc" port="5911" listen="0.0.0.0" passwd="rocky9"/> <== 顯示界面與顯示卡
    <video>
      <model type="virtio"/>
    </video>
    <memballoon model="virtio"/>
    <rng model="virtio">
      <backend model="random">/dev/urandom</backend>
    </rng>
  </devices>
  <on_reboot>destroy</on_reboot>
</domain>

```





#### Start, install, connect and observe virtual machines


**The action in the last section is equivalent to the concept of "purchasing a host", and at the same time pretend that you have placed the RockyLinux 9.x CD into the CD drive! In addition, the UEFI BIOS flash memory is pretended to be placed in the /kvm/xml/demo1.uefi.fd record file ~ this UEFI record file will be automatically generated! Of course, now you have to press the "power" of the virtual machine! How to observe it?**


```

# virsh 針對虛擬機器啟動、關閉、觀察的指令
[root@cloud ~]# virsh create file.xml  <==從 xml 檔案啟動虛擬機器
[root@cloud ~]# virsh list [--all]     <==列出目前 host 上面運作的虛擬機器
[root@cloud ~]# virsh shutdown VMname  <==讓某個名為 VMname 的虛擬機器關機
[root@cloud ~]# virsh destroy VMname   <==強迫某個名為 VMname 的虛擬機器斷電

```





#### Observe the operation of virtual machines (on KVM host)

**Using virsh you can have a lot of observation data:**



```

# 看有哪些 VM 在運作
[root@cloud ~]# virsh list
 Id   Name    State
-----------------------
 2    demo1   running

# 看上面 demo1 的狀態
[root@cloud ~]# virsh dominfo demo1
Id:             2
Name:           demo1
UUID:           4e8312e9-ef32-4edc-8dc0-651d7e13a51c
OS Type:        hvm
State:          running
CPU(s):         4
CPU time:       173.7s
Max memory:     2097152 KiB
Used memory:    2097152 KiB
Persistent:     no
Autostart:      disable
Managed save:   no
Security model: selinux
Security DOI:   0
Security label: system_u:system_r:svirt_t:s0:c145,c782 (enforcing)

# 看 demo1 的記憶體使用狀態
[root@cloud ~]# virsh dommemstat demo1
actual 2097152
swap_in 0
swap_out 0
major_fault 226
minor_fault 302780
unused 1521280
available 1854924
usable 1558904
last_update 1658386440
disk_caches 188752
hugetlb_pgalloc 0
hugetlb_pgfail 0
rss 2139836

```








#### During the operation of VM, the optical disc is replaced.


**After installing rockylinux, you naturally need to reboot. At this time, the best way is of course to go to /kvm/xml/demo1.xml and comment out the disk file name of the CD-ROM drive, so that you will not enter the installation screen every time you start the computer. If you don’t want to close this VM, you can use the following method to eject the disc! (Note: Do not remove the disc until the installation is complete and you are ready to start the computer!)**


```

# 看看目前 demo1 所使用到的磁碟方面的裝置有哪些
[root@cloud ~]# virsh domblklist demo1
 Target   Source
-------------------------------------------------
 vda      /kvm/img/demo1.img
 sda      /kvm/iso/Rocky-9.0-x86_64-minimal.iso

# 當 VM 內的 RockyLinux 9.x 安裝完畢後，在開機過程中，將光碟 sda 抽出不用
[root@cloud ~]# virsh change-media demo1 /kvm/iso/Rocky-9.0-x86_64-minimal.iso --eject
Successfully ejected media.

[root@cloud ~]# virsh domblklist demo1
 Target   Source
------------------------------
 vda      /kvm/img/demo1.img
 sda      -

```


**Since there is no CD at this time, after the VM is restarted, you can directly enter the new rockylinux boot process! In addition, you'd better remove the CD in demo1.xml! In this way, the next time you start this VM, this CD will not be used to boot:**


```

[root@cloud ~]# vim /kvm/xml/demo1.xml
    <disk type="file" device="cdrom">
      <driver name="qemu" type="raw"/>
      <source file=""/>
      <target dev="sda" bus="sata"/>
      <readonly/>
    </disk>

```



**Sometimes, after your VM is installed, you may fail to restart it... The probability is not high, but it may happen... At this time, it seems that you can only force the VM to shut down and then reopen it! The method of operation is a bit like this:**

```

[root@cloud ~]# virsh destroy demo1
Domain 'demo1' destroyed

[root@cloud ~]# virsh create /kvm/xml/demo1.xml
Domain 'demo1' created from /kvm/xml/demo1.xml

```








## 📋 Follow-up arrangement of virtual machine guest OS


**The operating system in the virtual machine is called guest OS. The way this guest OK can be operated is not only to directly obtain the terminal screen of the "screen" through remote-viewer, but in fact, it can also be operated through ssh and the like. Remotely connect to the server, so you don’t have to use remote-viewer! You can also directly transfer it to ssh on the KVM host. Although we can use a command similar to arp on the host to find out the correspondence between the network card number and the IP, it is still not very intuitive. Therefore, we should first go to the remote-viewer to query the correct IP address, and then do other subsequent processing. Bar!**





#### Use the remote-viewer interface to start the virtual machine network


**After logging into the virtual machine from the remote-viewer screen, change your identity to root. Next, let's start the network:**

```

# 1. 先查閱一下網路卡的代號
[root@localhost ~]# ip addr show
....
2: enp1s0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 52:54:00:ed:63:1f brd ff:ff:ff:ff:ff:ff
    inet 192.168.10.52/24 brd 192.168.10.255 scope global dynamic noprefixroute enp1s0
       valid_lft 2857sec preferred_lft 2857sec
    inet6 fe80::5054:ff:feed:631f/64 scope link noprefixroute
       valid_lft forever preferred_lft forever
# 網卡的代號為 enp1s0, 且 rockylinux 9 預設會啟動網路卡喔！ 

# 2. 檢查一下網路連線的名稱為何
[root@localhost ~]# nmcli connection show
NAME    UUID                                  TYPE      DEVICE
enp1s0  0b413fa2-5192-3674-a542-323759ec20bc  ethernet  enp1s0
# 果然順利找到正確的網卡代號！

[root@localhost ~]# nmcli connection show enp1s0 | grep IP4
IP4.ADDRESS[1]:     192.168.10.52/24
IP4.GATEWAY:        192.168.10.254
IP4.ROUTE[1]:       dst = 192.168.10.0/24, nh = 0.0.0.0, mt = 100
IP4.ROUTE[2]:       dst = 0.0.0.0/0, nh = 192.168.10.254, mt = 100
IP4.DNS[1]:         192.168.10.254
# 這樣就可以看到我們這部主機的 IP 位址是 192.168.10.52 喔！

```






#### Observe how to find the VM network address on the KVM host

**We know that the network used by the virtual machine is actually attached to the templan interface on the physical machine! If the VM really starts the network, then the network card "should record the network card records sent by the VM"! At this time, we can reverse the situation and find the corresponding IP address from the network card number recording function of the physical machine!**


```

# 這是在『實體機器』也就是在那部 KVM host 上面，實做 arp 探索！
[root@cloud ~]# arp -n
Address             HWtype  HWaddress           Flags Mask      Iface
192.168.201.252     ether   94:57:a5:9c:df:60   C               eno1
192.168.201.254     ether   18:31:bf:45:58:f2   C               eno1
192.168.10.52       ether   52:54:00:ed:63:1f   C               templan

[root@cloud ~]# arp -n -i templan
Address             HWtype  HWaddress           Flags Mask      Iface
192.168.10.52       ether   52:54:00:ed:63:1f   C               templan

```







**Through the corresponding record of the arp network card number and the IP address on the network card, you can find the client's IP address. We will discuss the related ARP functions with you in subsequent chapters. Now, you probably know that in a local network, before each network card can communicate, it must know the card number of the other party's network card.**

**In addition to using arp, nmap learned from the previous chapter can also be used here!**


```

# 在 KVM host 上面，探索 192.168.10.0/24 上面的活動中的主機
[root@cloud ~]# nmap -sP 192.168.10.0/24
Starting Nmap 7.91 ( https://nmap.org ) at 2023-08-04 13:29 CST
Nmap scan report for 192.168.10.52
Host is up (0.00017s latency).
MAC Address: 52:54:00:ED:63:1F (QEMU virtual NIC)
Nmap scan report for 192.168.10.254
Host is up.
Nmap done: 256 IP addresses (2 hosts up) scanned in 2.41 seconds

```




#### How to connect to the VM from the KVM host for manipulation: ssh

**Why do we need to start the VM's network and know the VM's IP address? This is because... Brother Bird doesn't like to start too many graphical interfaces! Of course, when using remote-viewer to connect, the amount of data transmitted over the network is relatively large, which I don’t like either! Therefore, if you want to use plain text to connect to the VM, it will be much simpler! As for the plain text login method, it is probably easiest to use the default ssh!**

**We will also explore the ssh service later. Here, you first know that we can log in or transfer data through ssh and scp. Now that we know the VM’s IP is 192.168.10.52, let’s log in!**


```

# ssh 的基本語法
[root@cloud ~]# ssh username@host.name.or.ip

# 在 KVM host 登入 VM 取得控制權
[root@cloud ~]# ssh vbird@192.168.10.52
The authenticity of host '192.168.10.52 (192.168.10.52)' can't be established.
ED25519 key fingerprint is SHA256:Va9Z79fb2Kv16vT9QP46iRksvxDayLYKcMY64n3txv8.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '192.168.10.52' (ED25519) to the list of known hosts.
vbird@192.168.10.52's password:  <==這裡輸入使用者的密碼
[vbird@localhost ~]$

```







#### Perform management tasks in virtual machines


**In fact, after a system is installed, many tasks need to be performed! Including upgrades, shutting down network services, date and time correction, general performance adjustments, etc. Even for virtual machines, these actions cannot be ignored. Now, let’s take it step by step!**


```

# 先進行全系統更新
[root@localhost ~]# yum -y update

# 常用軟體安裝
[root@localhost ~]# yum -y install bind-utils vim-enhanced \
> bash-completion net-tools yum-utils tuned

# 網路時間校正
[root@localhost ~]# vim /etc/chrony.conf
#pool 2.pool.ntp.org iburst
server ntp.ksu.edu.tw iburst
server tock.stdtime.gov.tw iburst
server time.stdtime.gov.tw iburst
#sourcedir /run/chrony-dhcp

[root@localhost ~]# systemctl start chronyd
[root@localhost ~]# systemctl enable chronyd
[root@localhost ~]# chronyc sources
MS Name/IP address         Stratum Poll Reach LastRx Last sample
^? dns3.ksu.edu.tw               3   6     3     0  +1028us[+1028us] +/-  122ms
^? 211-22-103-157.hinet-ip.>     2   6     3     1   -190us[ -190us] +/-   22ms
^? 118-163-81-61.hinet-ip.h>     2   6     3     1   +308us[ +308us] +/-   33ms

# tuned 效能調整
[root@localhost ~]# tuned-adm profile virtual-guest

# 管理員慣用操作環境處理
[root@localhost ~]# vim ~/.vimrc
"將自動縮排、自動移動畫面的功能關閉！請自行處理！
setl noai nocin nosi inde=

# 一般帳號操作環境處理
[vbird@localhost ~]# vim ~/.vimrc  <==這裡跟管理員相同動作
[vbird@localhost ~]# vim ~/.bashrc
....
alias rm='rm -i'
alias mv='mv -i'
alias cp='cp -i'

```





**Finally, reboot. When you reboot, you will be thrown out of the VM system... At the same time, because we have not designed the VM to automatically enable the network, you can no longer directly connect to the VM system through the network! Please use remote-viewer to connect! In addition, if there are really no other tasks to be performed, you can also use the virsh command on the KVM host to shut down the VM system.**


```

# 在 KVM host 上面關閉 VM 系統
[root@cloud ~]# virsh list
 Id   Name    State
-----------------------
 3    demo1   running

[root@cloud ~]# virsh reboot demo1
Domain 'demo1' is being rebooted

[root@cloud ~]# virsh shutdown demo1
Domain 'demo1' is being shutdown

```







## 📋 Virtual machine reconstruction and performance testing and adjustment


**In the previous chapter, we knew how to install a virtual machine in the KVM host, including the XML hardware resource configuration file of the virtual machine, the qcow2 virtual disk format created by qemu of the virtual machine, and the virtual machine network created by qemu. road bridge, and knew how to start and observe the virtual machine, and then installed a complete RockyLinux system in the virtual machine! great! Then what?**





#### Replication of virtual machines: via backing_file copy


**Think about it, if we copy and slightly modify the XML file in the previous section, and then copy the qemu image file to a different file name, wouldn't we be able to create a new virtual machine? That’s right! What a great idea to have! But...is there a faster way? Because the virtual disk image file of demo1.img is actually over 2G after installation! If there are other data, the file will get bigger and bigger. If you use copying, will it take up too much capacity?**


**That's right! Great, you found the point! If it can be like a snapshot in LVM, it only uses the original LVM (original) data. The snapshot just maps the data of the same block to the original disk! The same thing will never be reproduced! Only different data will be placed in the snapshot area! Does qemu's virtual disk have this function? some! That's the copy (backing_file).**






#### Use the backing_file mechanism to create a new overlay file

**In the qemu environment, there is a mechanism called backing file. This mechanism allows the original virtual disk (called the base image file, base image, that is, the copy, backing file) to be replaced by a new image file that can be superimposed. Utilized (overlay), this result is very similar to LVM's original being used by snapshots! Interested friends can take a look at the reference materials at the end of the article. We can regard demo1.img as the basic copy file of the backing file, and other new overlay image files are all based on the backing file!**

**It’s actually very simple to do! Create a new image file and specify the file name of its copy source!**


```

# 在 KVM host 上面，參考正確的 backing file 建立第一個 overlay
[root@cloud ~]# ll -h /kvm/img/demo1.img
-rw-r--r--. 1 root root 2.3G Aug  4 13:47 /kvm/img/demo1.img

[root@cloud ~]# qemu-img info /kvm/img/demo1.img
image: /kvm/img/demo1.img
file format: qcow2
virtual size: 30 GiB (32212254720 bytes)
disk size: 2.26 GiB
cluster_size: 2097152
....

# 前往 /kvm/img 建立名為 test1.img 的 ovarlay 映像檔
[root@cloud ~]# cd /kvm/img
[root@cloud img]# qemu-img create -f qcow2 test1.img -F qcow2 -o backing_file=/kvm/img/demo1.img
Formatting 'test1.img', fmt=qcow2 cluster_size=65536 extended_l2=off compression_type=zlib size=32212254720
  backing_file=/kvm/img/demo1.img backing_fmt=qcow2 lazy_refcounts=off refcount_bits=16

[root@cloud img]# ll -h *.img
-rw-r--r--. 1 root root 2.3G Aug  4 13:47 demo1.img
-rw-r--r--. 1 root root 193K Aug  4 15:23 test1.img

[root@cloud img]# qemu-img info test1.img
image: test1.img
file format: qcow2
virtual size: 30 GiB (32212254720 bytes)
disk size: 196 KiB
cluster_size: 65536
backing file: /kvm/img/demo1.img
backing file format: qcow2
....

```







Modify xml hardware file and start VM

```

# 建立 test1.xml 檔案，並且修改完畢之後啟動虛擬機器
[root@cloud ~]# cd /kvm/xml
[root@cloud xml]# cp demo1.xml test1.xml
[root@cloud xml]# vim test1.xml
  2   <name>test1</name>
  9     <nvram>/kvm/xml/test1.uefi.fd</nvram>
 36       <source file="/kvm/img/test1.img"/>
 41       <source file=""/>
 63       <mac address="52:54:00:ed:63:aa"/>
 75     <graphics type="vnc" port="5912" listen="0.0.0.0" passwd="rocky9"/>

[root@cloud xml]# virsh create /kvm/xml/test1.xml
Domain 'test1' created from /kvm/xml/test1.xml

[root@cloud xml]# virsh domblklist test1
 Target   Source
------------------------------
 vda      /kvm/img/test1.img
 sda      -

[root@cloud xml]# netstat -tlunp | egrep 'kvm|Proto'
Proto Recv-Q Send-Q Local Address  Foreign Address  State  PID/Program name
tcp        0      0 0.0.0.0:5912   0.0.0.0:*        LISTEN 5601/qemu-kvm

```




#### Simple performance testing software: dd, fio, iperf3


**To judge whether the performance has changed, you always need some test data, not "feeling". The most interesting test that Brother Niao has ever heard is that in the early days, before there were so many testing tools available, some large companies needed data to support their efforts, so the testers used stopwatches to record time! This is really the tear of the times... Currently, most of the available software can be used to test the results~ However, the data may not be real! Because all kinds of data have their applicability!**






#### Use dd to test reading and writing performance


**There is one of the stupidest ways to test write performance, and that is the dd command. However, it should be noted that because we have a disk array, the chunk capacity of the disk array is not fixed! Therefore, different write block (block) capacities may result, resulting in differences in performance. Anyway, let's find the best performance that is close to the theoretical performance.**


**dd has a very interesting parameter called oflag=direct. This parameter allows writing without the assistance of memory! Therefore, all data is written to disk simultaneously and is not written to memory first. Such a test would be more normal!**

```

# 在 VM 環境下，實做一個連續執行的 dd 測試！記得切換成為 root
[root@localhost ~]# vim checkdd.sh
#!/bin/bash
for block in 32 64 128 512 1024
do
   count=$(( 1024 * 500 / ${block} ))
   echo "test: ${block}k"
   dd if=/dev/zero of=./test.img bs=${block}k count=${count} oflag=direct
done

[root@localhost ~]# sh checkdd.sh
test: 32k
16000+0 records in
16000+0 records out
524288000 bytes (524 MB, 500 MiB) copied, 3.91262 s, 134 MB/s
test: 64k
8000+0 records in
8000+0 records out
524288000 bytes (524 MB, 500 MiB) copied, 3.70768 s, 141 MB/s
test: 128k
4000+0 records in
4000+0 records out
524288000 bytes (524 MB, 500 MiB) copied, 3.4443 s, 152 MB/s
test: 512k
1000+0 records in
1000+0 records out
524288000 bytes (524 MB, 500 MiB) copied, 1.00981 s, 519 MB/s
test: 1024k
500+0 records in
500+0 records out
524288000 bytes (524 MB, 500 MiB) copied, 0.109536 s, 4.8 GB/s

```




**Just test a run and the results will look very different! Recall that in the settings of our XML hardware resource file, the design of the virtual disk is as follows:**


```

<disk type="file" device="disk">
      <driver name="qemu" type="qcow2" cache="writeback" io="threads"/>
      <source file="/kvm/img/test1.img"/>
      <target dev="vda" bus="virtio"/>
</disk>

```

