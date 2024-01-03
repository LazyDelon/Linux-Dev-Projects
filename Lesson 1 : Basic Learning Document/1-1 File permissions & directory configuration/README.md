# Linux Development - File permissions & directory configuration



## 🎓 Users & Groups

#### 1. File Owner

**Friends who come into contact with Linux for the first time may find it strange. Why are there so many users of Linux? What groups are they divided into? What is the use? 』. This "Users and Groups" function is a very complete and easy-to-use security protection! how to say? Since Linux is a multi-person, multi-tasking system, it may often happen that multiple people use this host to work at the same time. In order to consider everyone's privacy and everyone's preferred working environment, therefore, this file has The role of the author is quite important!**

**For example, when you transfer your e-mail love letters to a file and put them in your own home directory, you don't want others to see your love letters, right? At this time, you set the file to "Only the file owner, who is me, can view and modify the contents of this file." Then even if other people know that you have this quite "interesting" file, because you have set Set appropriate permissions, so others will naturally not be able to know the contents of the file!**



#### 2. Group Concept

**What about groups? Why do we need to set the file and the group it belongs to? In fact, one of the most useful functions of groups is when you develop resources in a team! For example, suppose there are two groups of topics created in my host. The first topic group is projecta, and its members are class1, class2, and class3; the second topic group is projectb, and its members are class4, class5, class6. The two topics are competitive but require the submission of the same report. Members of each group must be able to modify each other's information, but members of other groups cannot see the contents of their own files. What should I do now?**

**Such restrictions are very simple under Linux! I can restrict others who are not my team (that is, a group) from reading the content through simple file permission settings! And I can also allow my team members to modify the files I created! At the same time, if I still have private and confidential files, I can still set them so that my team members cannot see my file information. Very convenient!**



* **User meaning: Since the three members of the Wang family each have their own room, although Wang Ermao can enter Wang Sanmao's room, Ermao cannot go through Sanmao's drawers! That will get you fucked by Sanmao! Because there may be Sanmao's personal things in the drawer, such as love letters, diaries, etc. This is a "private space", so of course you can't let Ermao take it!**

* **The concept of a group: Since they share the living room, the three Wang brothers can turn on the TV, read newspapers, sit on the sofa, and so on! Anyway, as long as it is in the living room, the three brothers can use it! Because we are all a family!**



**In this way, you should know a little bit! That "Wang Damao Family" is the so-called "group", and the three brothers are three "users" respectively, and these three users are in the same group! Although the three users are in the same group, we can set "permissions" so that certain users' personal information will not be queried by the owner of the group, so as to maintain their personal "private space"! And setting up group sharing allows everyone to share it together!**




## 🎓 Linux File Permission Concepts

#### 1. Linux File Properties

```

[root@localhost ~]$ su -                               ← 先來切換一下身份看看
Password:
Last login: Web Jan 3 11:55:05 on tty1
[root@localhost ~]# ls -al
total 24
dr-xr-x---.        2       root        root        114          Jan  3 11:27       .
dr-xr-xr-x.       17       root        root        224          Jan  3 11:27       ..
-rw-------.        1       root        root       1256          Jan  3 11:27       anaconda-ks.cfg
-rw-r--r--.        1       root        root         18          Dec 29  2013       .bash_logout
-rw-r--r--.        1       root        root        176          Dec 29  2013       .bash_profile
-rw-r--r--.        1       root        root        176          Dec 29  2013       .bashrc
-rw-r--r--.        1       root        root        100          Dec 29  2013       .cshrc
-rw-r--r--.        1       root        root        129          Dec 29  2013       .tcshrc
[     1     ]   [ 2 ]    [   3   ]   [  4   ]    [   5   ]      [    6    ]     [     7        ]
[檔案類型權限]   [連結]   [ 擁有者 ]   [ 群組 ]    [檔案容量]      [ 修改日期 ]     [    檔名      ]

```


**The first character indicates whether the file is a "directory, file, link file, etc.":**

* **When it is [ d ], it is a directory, such as the line with the file name ".config" in the table above;**

* **When it is [-], it means a file, such as the line with the file name "initial-setup-ks.cfg" in the table above;**

* **If it is [l], it means a link file;**

* **If it is [ b ], it means a peripheral device that can be stored in the device file (a random access device);**

* **If it is [c], it means the serial port device in the device file, such as keyboard and mouse (one-time reading device).**


**The following characters are grouped into groups of three, and they are all combinations of the three parameters of "rwx". Among them, [ r ] represents readable (read), [ w ] represents writable (write), and [ x ] represents executable (execute). It should be noted that the positions of these three permissions will not change. If there is no permission, a minus sign [-] will appear.**

* **The first group is "Permissions that the file owner can have". Taking the file "initial-setup-ks.cfg" as an example,
  the owner of the file can read and write, but cannot execute;**

* **The second group is "Permissions for accounts that join this group";**

* **The third group is "Other accounts that are not my own and do not have permission to join this group".**


```

例題：
若有一個檔案的類型與權限資料為『-rwxr-xr--』，請說明其意義為何？

答：
先將整個類型與權限資料分開查閱，並將十個字元整理成為如下所示：
[-]  [rwx]  [r-x]  [r--]
 1    234    567    890

 1  為：代表這個檔名為目錄或檔案，本例中為檔案(-)；
234 為：擁有者的權限，本例中為可讀、可寫、可執行(rwx)；
567 為：同群組使用者權限，本例中為可讀可執行(rx)；
890 為：其他使用者權限，本例中為可讀(r)，就是唯讀之意

同時注意到，rwx所在的位置是不會改變的，有該權限就會顯示字元，沒有該權限就變成減號(-)就是了。

```

➤  **資料來源：** [**基礎學習篇 - CentOS 7.x by~鳥哥**](https://linux.vbird.org/linux_basic/centos7/0210filepermission.php) 





## 📋 How to change file attributes and permissions

**chgrp ：改變檔案所屬群組**
**chown ：改變檔案擁有者**
**chmod ：改變檔案的權限, SUID, SGID, SBIT等等的特性**


#### Change the groups you belong to, chgrp


**Changing the group of a file is really simple. Just use chgrp to change it. Hey! This command is the abbreviation of change group! This will be easy to remember! ^_^. However, please remember that the group name to be changed must exist in the /etc/group file, otherwise an error will be displayed!**

**Assume that you are already root, then there is a file named initial-setup-ks.cfg in your home directory. How to change the group of this file? Suppose you already know that there is already a group named users in /etc/group, but the group name testing does not exist in /etc/group. What will happen if you change the group to users and testing respectively? happened?**

```

[root@study ~]# chgrp [-R] dirname/filename ...
選項與參數：
-R : 進行遞迴(recursive)的持續變更，亦即連同次目錄下的所有檔案、目錄
     都更新成為這個群組之意。常常用在變更某一目錄內所有的檔案之情況。
範例：
[root@study ~]# chgrp users initial-setup-ks.cfg
[root@study ~]# ls -l
-rw-r--r--. 1 root users 1864 May  4 18:01 initial-setup-ks.cfg
[root@study ~]# chgrp testing initial-setup-ks.cfg
chgrp: invalid group:  `testing' <== 發生錯誤訊息囉～找不到這個群組名～

```

#### Change file owner, chown

**How to change the owner of a file? It's very simple! Since the change group is change group, then the change owner is change owner! BINGO! That is the purpose of the chown command. It should be noted that the user must have an account that already exists in the system, that is, the user name recorded in the /etc/passwd file can be changed.**

**Chown has many uses, and he can also directly modify the name of the group! In addition, if you want to change the file owners of all subdirectories or files in the directory at the same time, just add the -R option! Let’s take a look at the syntax and examples:**

```

[root@study ~]# chown [-R] 帳號名稱 檔案或目錄
[root@study ~]# chown [-R] 帳號名稱:群組名稱 檔案或目錄
選項與參數：
-R : 進行遞迴(recursive)的持續變更，亦即連同次目錄下的所有檔案都變更

範例：將 initial-setup-ks.cfg 的擁有者改為bin這個帳號：
[root@study ~]# chown bin initial-setup-ks.cfg
[root@study ~]# ls -l
-rw-r--r--. 1 bin  users 1864 May  4 18:01 initial-setup-ks.cfg

範例：將 initial-setup-ks.cfg 的擁有者與群組改回為root：
[root@study ~]# chown root:root initial-setup-ks.cfg
[root@study ~]# ls -l
-rw-r--r--. 1 root root 1864 May  4 18:01 initial-setup-ks.cfg

```


**Now that you know how to change the group and owner of a file, when should you use chown or chgrp? Maybe you find it strange? Yes, sometimes it is necessary to change the owner of a file. The most common example is when copying a file to someone other than you. We use the simplest cp command to illustrate:**


```

[root@study ~]# cp 來源檔案 目的檔案

```



**Suppose you want to copy the .bashrc file to the .bashrc_test file name today and give it to bin, you can do this:**

```

[root@study ~]# cp .bashrc .bashrc_test
[root@study ~]# ls -al .bashrc*
-rw-r--r--. 1 root root 176 Dec 29  2013 .bashrc
-rw-r--r--. 1 root root 176 Jun  3 00:04 .bashrc_test   <==新檔案的屬性沒變


```


#### Change permissions, chmod


**To change file permissions, use the chmod command. However, there are two ways to set permissions. You can use numbers or symbols to change permissions. Let’s talk about it:**

```

每種身份(owner/group/others)各自的三個權限(r/w/x)分數是需要累加的，例如當權限為： [-rwxrwx---] 分數則是：
owner = rwx = 4+2+1 = 7
group = rwx = 4+2+1 = 7
others= --- = 0+0+0 = 0

```

**So when we set the permission changes later, the permission number of the file will be 770! The syntax of the chmod command to change permissions is as follows:**

```

[root@study ~]# chmod [-R] xyz 檔案或目錄
選項與參數：
xyz : 就是剛剛提到的數字類型的權限屬性，為 rwx 屬性數值的相加。
-R : 進行遞迴(recursive)的持續變更，亦即連同次目錄下的所有檔案都會變更

```

**For example, if you want to enable all permissions on the .bashrc file, issue:**

```

[root@study ~]# ls -al .bashrc
-rw-r--r--. 1 root root 176 Dec 29  2013 .bashrc
[root@study ~]# chmod 777 .bashrc
[root@study ~]# ls -al .bashrc
-rwxrwxrwx. 1 root root 176 Dec 29  2013 .bashrc

```

```

例題：
將剛剛你的.bashrc這個檔案的權限修改回-rw-r--r--的情況吧！
答：
-rw-r--r--的分數是644，所以指令為：
chmod 644 .bashrc

```

