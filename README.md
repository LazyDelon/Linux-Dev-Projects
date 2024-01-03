# Linux-Dev-Projects

## 📣 What is Linux?

**Linux is an open source operating system (OS). The so-called operating system refers to the software that directly manages the system hardware and resources (such as CPU, memory and storage space). The operating system sits between applications and hardware and is responsible for establishing connections between all software and related physical resources.**


**Tips: When Mr. Torvalds wrote the Linux kernel in 1991, in fact, the kernel could only "drive all 386 hardware". The so-called "let the 386 computer start running and wait for user command input". In fact, At that time, there were very few softwares that could run on Linux!**


➤  **資料來源：** [**Linux 私房菜 by~鳥哥**](https://linux.vbird.org/)   

## 📋 Setting up your environment

### VirtualBox installs CentOS 7



**Build a virtual practice environment. Today's computers have too much power and space. Although the computer doesn't work, it's enough for practice. No need to worry about computer errors while practicing. This is the benefit of VM OS.**

**When you join a new company, you will be assigned a computer. There are ownership issues with the software on your computer. Especially the larger the company, the stricter the MIS management. Be careful of piracy and do not download bad software. Taking it to the company has become a habit. This is for you and the company. Penalties for private use of pirated software are severe.**

**Preparation software: Oracle VM VirtualBox, any version of Windows ISO**

**Now I want to introduce to you a virtual environment software-Oracle VM VirtualBox. The main point of this software is that it is free. Installation is simple and setup is not too complicated. It can install different operating system software and even simulate a MAC. The key is that you need to have the installation file. Remember to use the WINDOWS operating system as the ISO file first to facilitate software installation.**


#### 1. Virtual Machine Creation。

**Run VirtualBox and click the "New" button, as shown below:**

&nbsp; <img src="./Images/Virtual Machine Creation.png" alt="Virtual Machine Creation"/>



#### 2. Set Up Operating System。

&nbsp; <img src="./Images/Set Up Operating System.png" alt="Set Up Operating System"/>



#### 3. Hardware Settings。

**Configure virtual operating system memory, it is recommended to configure no less than 3GB**

&nbsp; <img src="./Images/Hardware Settings.png" alt="Hardware Settings"/>



#### 4. Virtual Hard Drive。

**Click "Continue". This step requires you to set the type of virtual hard disk. To save space, select "Create virtual hard disk now" and click "Create".**

**Then allocate the disk size, set it according to your actual needs, and then click "Create" to complete the settings of the VirtualBox virtual OS.**

&nbsp; <img src="./Images/Virtual Hard Drive.png" alt="Virtual Hard Drive"/>



#### 5. Overall Content。

**The virtual machine is created, as shown below:**

&nbsp; <img src="./Images/Overall Content.png" alt="Overall Content"/>


#### 6. Virtual Machine Settings。

**Click "Settings" as shown below:**

&nbsp; <img src="./Images/Virtual Machine Settings.png" alt="Virtual Machine Settings"/>


#### 7. Select Disk File。

**Please select the "Storage Device" menu above, then select "Empty", click the disc icon on the right, and select "Select Virtual Disc File", then select the CentOS ISO file you just downloaded and click " Done".**

&nbsp; <img src="./Images/Select Disk File.png" alt="Select Disk File"/>



#### 8. Import Disk Files。

&nbsp; <img src="./Images/Import Disk Files.png" alt="Import Disk Files"/>


#### 9. Start Virtual Machine。

**Click "Start", as shown below:**

&nbsp; <img src="./Images/Start Virtual Machine.png" alt="Start Virtual Machine"/>


#### 10. Install CentOS 7。

**Then please press the "Start" button above, and a black screen will pop up, which means that the installation of Linux has finally begun.**
**Please select "Install CentOS 7" on the first screen.**

&nbsp; <img src="./Images/Install CentOS 7.png" alt="Install CentOS 7"/>


#### 11. Language Selection Interface。

**Please wait for a while, and this screen will appear. Here is the screen to start installing CentOS. Please select the language for the installation process and then click "Continue".**

&nbsp; <img src="./Images/Language Selection Interface.png" alt="Language Selection Interface"/>



#### 12. System Installation Destination。

**After a moment, you will see the installation information summary interface, which prompts that the content with a yellow exclamation mark must be completed before proceeding to the next step, as shown below:**

&nbsp; <img src="./Images/System Installation Destination.png" alt="System Installation Destination"/>


#### 13. Device Selection。

**Click "Installation Location" and you will see the configuration interface for the installation target location, as shown below:**

&nbsp; <img src="./Images/Device Selection.png" alt="Device Selection"/>




#### 14. Network & Hostname。

**Then click "Installation Destination", perform preliminary settings and click "Begin Installation".**

&nbsp; <img src="./Images/Network & Hostname.png" alt="Network & Hostname"/>




#### 15. Set Up Network 。

**Keep the default and click the "Done button" directly. Click "Network and Host Name" again, you will see the network and host name configuration interface, open the network connection, and set the host name, as shown below:**

&nbsp; <img src="./Images/Set Up Network.png" alt="Set Up Network"/>




#### 16. Start Installation。

**Click the "Complete Button" to return to the installation information summary interface and click "Start Installation", as shown below:**

&nbsp; <img src="./Images/Start Installation.png" alt="Start Installation"/>





#### 17. Set ROOT Password。

**Enter the installation process. At this point you should see a similar screen, please click above and set your account and password.**

&nbsp; <img src="./Images/Set ROOT Password.png" alt="Set ROOT Password"/>




#### 18. Configure Users。

**After completing the installation, please click "Reboot" on the lower right.**

&nbsp; <img src="./Images/Configure Users.png" alt="Configure Users"/>



#### 19. Successfully Entered CentOS 7。

**If everything goes well, you will come to the Linux login screen, welcome to the world of CentOS 7.**

&nbsp; <img src="./Images/Successfully Entered CentOS 7.png" alt="Successfully Entered CentOS 7"/>

