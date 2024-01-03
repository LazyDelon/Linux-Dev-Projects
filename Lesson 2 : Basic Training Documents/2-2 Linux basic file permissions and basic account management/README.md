# Linux Development - Linux basic file permissions and basic account management




## 📣 Linux traditional permissions


**The purpose of Linux permissions is to "protect certain people's file data." Therefore, from the perspective of understanding "permissions", readers should think about "which person will be affected by the permission setting of this file?" or the opening or protection of literacy for a group of people. Therefore, these permissions are ultimately "applied to a certain/group of accounts"! Moreover, the permissions are "set on the file/directory", not on the account. This needs to be clarified first.**



### 🎓 Users, groups, and others

**user / owner /： the person who owns the file**

**Group： which group team this file belongs to**

**Others：Accounts that are either user and have not joined the group, or other people.**




#### Observation on file permissions


**For simple file permission observation, you can use ls -l or ll to check. The following is the query method for the permissions of the system /var/spool/mail directory:**


```

[student@localhost ~]$ ls -ld /var/spool/mail
drwxrwxr-x.   2   root   mail   32      2月 26 09:10   /var/spool/mail
[    A    ]  [B]  [C ]   [D ]   [E ]    [    F     ]   [    G        ]

```

**A. File type and permissions, the first character is the file type, and the subsequent 9 characters are in groups of 3, divided into 3 groups in total, which are the permissions of the three identities;**

**B. The number of file links, which is related to the file system, readers can temporarily ignore it;**

**C. The owner of the file. In this case, the owner is root.**

**D. The group to which the file belongs. In this example, the file belongs to the mail group.**

**E. The capacity of this file**

**F. The date and time the file was last modified/revised**

**G. The file name of this file.**





**Readers can first analyze the "type" of this "file". Readers should have seen the representation of the first character - and d before. In fact, there are many common file types. The following only introduces the common types:**

**-: Indicates that the following file name is a normal file**

**d: means that the following file name is a directory file**

**l: represents the following file name as the link file (somewhat similar to the windows shortcut concept)**

**b: means that the following file name is a device file. The device is mainly a block device, that is, there are many devices that store media.**

**c: represents that the following file name is a peripheral device file, such as a mouse, keyboard, etc.**




**So readers can know that /var/spool/mail is a directory file (starting with d, which is the abbreviation of directory). After determining the file type, the next 9 characters are just rwx and minus signs. Judging from these 9 characters, readers can probably guess that the meaning of rwx is:**

**r: read, readable meaning**

**w: write, meaning it can be written/edited/modified**

**x: eXecutable, meaning executable**





#### How to modify file attributes and permissions



**For modification of file permissions and attributes, based on the output of ls -l, the instructions that can be modified for each part are roughly as follows:**

```

[student@localhost ~]$ cd /dev/shm/
[student@localhost shm]$ touch checking
[student@localhost shm]$ ls -l checking
-rw-rw-r--. 1 student student 0  3月 14 21:45 checking
 [ chmod ]    [chown] [chgrp]    [   touch  ] [  mv  ]

```




**Since a general account can only modify the file name, time and permissions of its own files, it cannot switch user and group settings at will. Therefore, in the examples below, readers should use the identity of root to process them smoothly. First, switch to root and change the working directory to /dev/shm.**

```

[student@localhost shm]$ su -
password:
[root@localhost ~]# cd /dev/shm
[root@localhost shm]# ll checking
-rw-rw-r--. 1 student student 0 Mar 14 21:45 checking
# 因為不同的帳號使用的語系不同，root 使用英文語系，因此可以發現日期格式不同。

```






#### Use `chown` to change file owner

Check whether there is an account named daemon in the system. If the account exists, please change the user of checking to be owned by daemon instead of student.

```

[root@localhost shm]# id daemon
uid=2(daemon) gid=2(daemon) groups=2(daemon)

[root@localhost shm]# chown daemon checking
[root@localhost shm]# ll checking
-rw-rw-r--. 1 daemon student 0 Mar 14 21:45 checking

```

In fact, chown has many functions. chown can also be used to modify groups, and it can also modify the file owner and group at the same time. It is recommended that readers should man chown to query the relevant syntax.







#### Use `chgrp` to modify the group owned by a file


The groups of the system are recorded in the /etc/group file. If you want to know whether a certain group exists in the system, you can use the grep keyword retrieval command to query. For example, when there is a group bin in the system, the checking group will be changed to belong to bin, otherwise it will not be modified.

```

[root@localhost shm]# grep myname /etc/group
# 不會出現任何資訊，因為沒有這個群組存在的意思。

[root@localhost shm]# grep bin /etc/group
bin:x:1:        <==代表確實有這個群組存在！

[root@localhost shm]# chgrp bin checking
[root@localhost shm]# ll checking
-rw-rw-r--. 1 daemon bin 0 Mar 14 21:45 checking

```




#### Use `chmod` with numerical methods to modify permissions



**Since the file records three identities, each identity has the maximum permissions of rwx and --- no permissions. For the convenience of matching, the 2-bit method is used to remember! That is, the case of 2 carry:**



**r ==> read    ==> 22 ==> 4**  
**w ==> write   ==> 21 ==> 2**  
**x ==> eXecute ==> 20 ==> 1**  







## 📣 Basic account management


### 🎓 Simple account management



**Readers should still remember that when logging into the system, you need to enter two pieces of information. One is to click on the account name, and the other is to enter the password of the account. Therefore, the simplest account management is the task of creating an account and giving a password.**

**Readers are asked to try to create an account named myuser1 and give the password Mypassword as follows: (Remember, account management is the task of the system administrator, so the identity must be root!)**


```

[root@localhost ~]# useradd  myuser1
[root@localhost ~]# passwd  myuser1
Changing password for user myuser1.
New password:          <==此處輸入密碼
BAD PASSWORD: The password fails the dictionary check - it is based on a dictionary word
Retype new password:   <==再輸入密碼一次
passwd: all authentication tokens updated successfully.

[root@localhost ~]# id myuser1
uid=1014(myuser1) gid=1015(myuser1) groups=1015(myuser1)

```








**When operating the passwd command that gives a password, there are several key points that require special attention:**



* **Simply entering passwd is "changing your own password", so you need to confirm whose password you change to. So as shown in the table above, pay special attention to which account appears in the first line (some students often accidentally change the password to root...)**

* **Since the system administrator can give an account any password, even though MypasswordD is not a good password, the system still accepts it. However, don’t set a password that is too simple, as the system can easily be breached!**

* **The New keyword appears, which means entering a new password. Entering Retype shows that the password just now is acceptable.**







**If the account setting is wrong, you can use userdel to delete the account, for example:**


```

[root@localhost ~]# userdel -r myuser1
# 有趣的是， myuser1 的出現也能用 [tab] 按鈕喔！ userdel -r my[tab] 看看！

```





### 🎓 Account and group association management


**If you need to create an account and give the account a secondary group support, you need to create the group first. For example, taking school theme production as an example, how to create three accounts prouser1, prouser2, and prouser3 when they join the shared group progroup? First, you should first create a group and process it through groupadd. Then, use useradd --help to find the option -G supported by the secondary group. Then you can create the group, account and password. At the same time, administrators can use passwd --help to find the --stdin option to operate password giving. The overall process is as follows:**


```

[root@localhost ~]# groupadd progroup
[root@localhost ~]# grep progroup /etc/group
progroup:x:1001:    <==確定有 progroup 在設定檔當中了

[root@localhost ~]# useradd -G progroup prouser1
[root@localhost ~]# useradd -G progroup prouser2
[root@localhost ~]# useradd -G progroup prouser3
[root@localhost ~]# id prouser1
uid=1001(prouser1) gid=1002(prouser1) groups=1002(prouser1),1001(progroup)

[root@localhost ~]# echo mypassword
mypassword    <== echo 會將訊息從螢幕上輸出

[root@localhost ~]# echo mypassword | passwd --stdin prouser1
Changing password for user prouser1.
passwd: all authentication tokens updated successfully.
[root@localhost ~]# echo mypassword | passwd --stdin prouser2
[root@localhost ~]# echo mypassword | passwd --stdin prouser3

```






### 🎓 Single user ownership


**General users can only modify the rwx permissions of their own files. Therefore, if root wants to assist in copying data to general users, special attention needs to be paid to the permissions of the data. For example, in the example below, when the administrator wants to copy /etc/tcsd.conf to student, the relevant matters need to be noted as follows:**

```

[root@localhost ~]# ls -l /etc/tcsd.conf
-rw-------. 1 tss tss 7046 Dec 14 00:44 /etc/tcsd.conf  <==一般用戶根本沒權限

[root@localhost ~]# cp /etc/tcsd.conf ~student/
[root@localhost ~]# ls -l ~student/tcsd.conf
-rw-------. 1 root root 7046 Mar 15 23:13 /home/student/tcsd.conf

[root@localhost ~]# chown student.student ~student/tcsd.conf
[root@localhost ~]# ls -l ~student/tcsd.conf
-rw-------. 1 student student 7046 Mar 15 23:13 /home/student/tcsd.conf

```






**Originally, when root copied data to student, if permissions were not considered, student would still be unable to read the contents of the file. This requires special attention when copying data.**


**In addition, if the user wants to copy the command himself or perform additional work tasks, he can move the command to his own home directory for processing. For example, if a student wants to copy ls into myls and directly execute myls to run the system, he can do this deal with:**


```

[student@localhost ~]$ cp /bin/ls myls
[student@localhost ~]$ ls -l myls
-rwxr-xr-x. 1 student student 166448  3月 15 23:15 myls

[student@localhost ~]$ chmod 700 myls
[student@localhost ~]$ ls -l myls
-rwx------. 1 student student 166448  3月 15 23:15 myls
# 未來這個檔案，就只有 student 自己能玩！

[student@localhost ~]$ myls
bash: myls: 找不到指令...

[student@localhost ~]$ ./myls
Desktop    Downloads  history.log  myls      Public     Templates  Videos
Documents  group      Music        Pictures  tcsd.conf  text1.txt
# 但是，要執行就得要這樣做！當然，也能自己寫入 PATH 變數內！如下：

[student@localhost ~]$ mkdir bin
[student@localhost ~]$ mv myls bin
[student@localhost ~]$ myls
bin      Documents  group        Music     Public     Templates  Videos
Desktop  Downloads  history.log  Pictures  tcsd.conf  text1.txt

```







### 🎓 Group sharing function


**In some cases, groups may need to share certain profile data. For example, when doing a project in school, members of the same group may need individual accounts, but they also need a shared directory so that everyone can share each other's project results. For example, progroup members prouser1, prouser2, and prouser3 (account information created in the previous section) need to share the directory /srv/project1/, then the creation and sharing of the directory can be achieved in the following ways:**

```

[root@localhost ~]# mkdir /srv/project1
[root@localhost ~]# chgrp progroup /srv/project1
[root@localhost ~]# chmod 770 /srv/project1
[root@localhost ~]# ls -ld /srv/project1
drwxrwx---. 2 root progroup 6 Mar 16 00:38 /srv/project1/

```





**At this time, members of the progroup can perform any actions in the project1 directory. But 770 is not the best way to deal with it. In the next lesson, readers will learn the function of SGID, and then they will learn more correct permission settings.**

**In addition to shared directories, the design of executable permissions for executable files can also grant executable rights to groups, so that others cannot execute shared instructions at will. For example, to make mycat execute the same result as cat, but only users in the progroup can execute it, you can execute it like this:**


```

[root@localhost ~]# which cat
/usr/bin/cat
[root@localhost ~]# echo $PATH
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/root/bin
# 基本上，我們執行指令時，該指令名稱就會到 PATH 設定的目錄去找，
# 找到的第一個同名的指令，就會被執行喔！

[root@localhost ~]# cp /usr/bin/cat /usr/local/bin/mycat
[root@localhost ~]# ll /usr/local/bin/mycat
-rwxr-xr-x. 1 root root 51856 Mar 16 00:42 /usr/local/bin/mycat

[root@localhost ~]# chgrp progroup /usr/local/bin/mycat
[root@localhost ~]# chmod 750 /usr/local/bin/mycat
[root@localhost ~]# ll /usr/local/bin/mycat
-rwxr-x---. 1 root progroup 51856 Mar 16 00:42 /usr/local/bin/mycat

```
