# Basic Learning for CentOS 8.x

## 📣 Log in to linux through the terminal interface

```
CentOS Linux 8 (Core)

Kernel 4.18.0-147.el8.x86_64 on an x86_64

localhost login: student

Password:                                ← 輸入所設密碼

Last login: Sun Mar 1 16:37:30 on :0     ← 上次登入的情況

[student@localhost ~]$ _                 ← 游標閃爍，等待你的指令輸入
```


## 📋 What is shown above is this:


#### 1. CentOS Linux 8 (Core)
**Display the name of the Linux distribution (CentOS) and version (8);**

#### 2. Kernel 4.18.0-147.el8.x86_64 on an x86_64
**It shows that the version of the Linux kernel is 4.18.0-147.el8.x86_64, and the current hardware level of this host is x86_64.**

#### 3. localhost login:
**localhost is the host name. As for login: it is a program that allows users to log in. You can enter your account after login: and press [enter] to start preparing for the next action.**

#### 4. Password:
**This line will only appear after pressing [enter] in the previous action. When entering the password, nothing will be displayed on the screen! 』 This is because the user is worried that when the user enters the password, the "entered password length" will be peeked into.**

#### 5. Last login: Sun Mar 1 16:37:30 on :0
**When a user logs into the system, the system will list the time and terminal name of the last time this account logged into the system!**

#### 6. [student@localhost ~]$ _：
**This line is a message that is displayed after correct login. The student on the far left displays the "current user account", and the localhost followed by @ is the "host name", and the "~" on the far right refers to It is "the current directory", and the $ is the "prompt character"**


➤  **資料來源：** [**基礎學習篇 - CentOS 8 by~鳥哥**](https://linux.vbird.org/linux_basic_train/centos8/unit01.php) 






## 📋 ls and ll check the file name data of your own directory


**Please log in to the Linux system as a normal user and start a terminal on the desktop. Now let's execute two instructions to confirm how to operate the system and observe the output data.**

```

[student@localhost ~]$ ls
Desktop Documents Downloads Music Pictures Public Templates text1.txt Videos

```


**Use ls to simply list the file names, which is the information listed above "Desktop Documents Downloads..." and so on. However, various file permission information related to this file name is not displayed, including time, capacity, etc. If you need to check more detailed information, you need to use ll (lower case of LL).**

```

[student@localhost ~]$ ll
drwx------. 2 student student  6  3月  1 15:35 Desktop
drwx------. 2 student student  6  3月  1 15:35 Documents
drwx------. 2 student student  6  3月  1 15:35 Downloads
drwx------. 2 student student  6  3月  1 15:35 Music
drwx------. 2 student student  6  3月  1 15:35 Pictures
drwx------. 2 student student  6  3月  1 15:35 Public
drwx------. 2 student student  6  3月  1 15:35 Templates
-rw-r--r--. 1 student student 22  3月  1 16:12 text1.txt
drwx------. 2 student student  6  3月  1 15:35 Videos

```



#### 🎓 If you want to check the root directory (similar to the "Computer" project of Windows), use the following command:

```

[student@localhost ~]$ ll /
總計 24
lrwxrwxrwx.   1 root root    7  5月 11  2019 bin -> usr/bin
dr-xr-xr-x.   6 root root 4096  2月 26 09:15 boot
drwxr-xr-x.  20 root root 3260  3月  1 16:44 dev
drwxr-xr-x. 135 root root 8192  3月  1 16:49 etc
drwxr-xr-x.   3 root root   21  2月 26 09:10 home
lrwxrwxrwx.   1 root root    7  5月 11  2019 lib -> usr/lib
lrwxrwxrwx.   1 root root    9  5月 11  2019 lib64 -> usr/lib64
......

```


**At this time, what is displayed on the screen is the file name under the root directory, not the student's home directory. This exercise allows the operator to understand that parameters can be added after the command. And if you want to know whether there are "hidden files" under the student's home directory, you can use the following command:**

```

[student@localhost ~]$ ll -a
總計 36
drwx------. 15 student student 4096  3月  1 16:47 .
drwxr-xr-x.  3 root    root      21  2月 26 09:10 ..
-rw-------.  1 student student   44  3月  1 16:44 .bash_history
-rw-r--r--.  1 student student   18 11月  9 00:21 .bash_logout
-rw-r--r--.  1 student student  141 11月  9 00:21 .bash_profile
-rw-r--r--.  1 student student  312 11月  9 00:21 .bashrc
drwx------. 11 student student  244  3月  1 15:56 .cache
drwx------. 14 student student 4096  3月  1 16:36 .config
drwx------.  2 student student    6  3月  1 15:35 Desktop
......

```


**Finally, if you want to know the permissions of the root directory itself, rather than the file names under the root directory, you should use the following command:**

```

[student@localhost ~]$ ll -d /
dr-xr-xr-x. 17root224 February 26 09:02/

```



#### 🎓 History command function

**In the text interface of Linux, you can use several simple methods to check the commands you have issued. The simplest method is to use the direction keys "up and down". Not only can you call out previous commands, but you can also use the direction keys again. The keys "left and right" and the "home/end" keys on the keyboard can be modified directly at the beginning and end of a line of instructions. Familiar with this approach, you can quickly edit a line of instructions.**

**But if the command was made too long ago, it can be called out through the history command "history" at this time.**




**A. Using the identity of student, issue the history command:**

```

[student@station10-101 ~]$ history
......
    6  w
    7  ls
    8  ll
    9  ll /
   10  ll -a
   11  ll -d /
   12  clear
   13  ll /var/spool/mail
   14  ll -d /var/spool/mail
   15  ll /var/spool
   16  history

```



**B. Observing the above instructions, you can find that ll / is on the 9th number, which is the 9th instruction that has been executed. Repeat the above command, you can use "!9"! That is, the exclamation point plus the command number, and there is no space between the exclamation point and 9! as follows:**


```

[student@station10-101 ~]$ !9
ll /          ← 這裡會將指令的完整下達方式顯示一次喔！
總計 24
lrwxrwxrwx.   1 root root    7  5月 11  2019 bin -> usr/bin
dr-xr-xr-x.   6 root root 4096  2月 26 09:15 boot
drwxr-xr-x.  20 root root 3260  3月  1 16:44 dev
drwxr-xr-x. 135 root root 8192  3月  1 16:49 etc
drwxr-xr-x.   3 root root   21  2月 26 09:10 home
lrwxrwxrwx.   1 root root    7  5月 11  2019 lib -> usr/lib
......

```




**C. In addition to "! Number" that can repeatedly execute a command, we can also use the leading character of the command to repeatedly execute a command. Judging from the above output, we have entered the clear command. If I want to execute this command repeatedly, I can directly enter !cl! Because the instructions starting with cl are only clear recently (the result of pushing forward from No. 16).**



```

[student@station10-101 ~]$ !cl

```




## 📋 Leaving the system and shutting down the system


* **exit**
* **logout**
* **[ctrl]+d**


**To close the graphical interface, just click logout. Before logging out, it is best to click "Overview" to take a look at all desktops. Have all software been closed? It’s better to close them all and log out! What if you want to shut down? Better to check first to see if there are any users on the system? If so, it is better to log out the user first, wait until everyone is logged out, and then shut down the computer. Users who observe that the system is online can use the w mentioned before to check:**



```

[student@localhost ~]$ w
 20:48:08 up  4:03,  3 users,  load average: 0.00, 0.00, 0.00
 20:48:08 up  4:03,  3 users,  load average: 0.00, 0.00, 0.00
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
student  :1       :1               16:47   ?xdm?  54.99s  0.02s /usr/libexec/gdm-x-session gnome-session
student  tty3     -                16:46    4:01m  0.03s  0.03s -bash
student  pts/0    172.16.200.254   20:45    0.00s  0.04s  0.01s w

```





**From the above table, it seems that only student is currently online. If this machine is not a server, it should be able to shut down at this time. To shut down, use the following command:**

```

[student@localhost ~]$ poweroff
[student@localhost ~]$ halt
[student@localhost ~]$ shutdown -h now
[student@localhost ~]$ systemctl poweroff

```



