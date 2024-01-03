# Basic Learning for CentOS 7.x

## 📣 Log in to linux through the terminal interface

```
CentOS Linux 7 (Core)

Kernel 3.10.0-229.el7.x86_64 on an x86_64

study login: root

Password:                                       ← 輸入所設密碼

Last login: Fri May 29 11:55:05 on tty1         ← 上次登入的情況

[root@localhost ~]$ _                           ← 游標閃爍，等待你的指令輸入
```

## 📋 What is shown above is this:

#### 1. CentOS Linux 7 (Core):
**Display the name (CentOS) and version (7) of the Linux distribution;**


#### 2. Kernel 3.10.0-229.el7.x86_64 on an x86_64:
**It shows that the Linux kernel version is 3.10.0-229.el7.x86_64, and the current hardware level of this host is x86_64.**


#### 3. localhost login:：
**That localhost is your host name. When we installed in Chapter 3, we filled in the host name as: localhost.centos.vbird. The host name usually displays only the letter before the first decimal point, so it becomes localhost! As for login: it is a program that allows us to log in. You can enter your account number after login:.**


#### 4. Password:：
**This line will appear after the root input on the third line, requiring you to enter your password! Please note that when entering the password, no words will be displayed on the screen! ”, so don’t think your keyboard is broken! Many beginners will ask questions desperately when they first come here! Ah, why doesn't my keyboard work...**

#### 5. Last login: Fri May 29 11:55:05 on tty1：
**When a user logs into the system, the system will list the time and terminal name of the last time this account logged into the system! It is recommended that you still check this information to see if it is really caused by your own login!**

#### 6. [root@localhost ~]$ _:
**This line is a message that is displayed after correct login. The localhost on the far left displays the "current user account", and the root followed by @ is the "host name". As for the ~ on the far right, it refers to " Directory you are currently in", that $ is the "prompt character" we often talk about!**



➤  **資料來源：** [**基礎學習篇 - CentOS 7.x by~鳥哥**](https://linux.vbird.org/linux_basic/centos7/0160startlinux.php) 




## 📣 Start giving orders


```

[root@localhost ~]$ command  [-options]  parameter1  parameter2 ...
                      指令     選項        參數(1)     參數(2)

```


## 📋 The above instructions are detailed as follows:

**1. The first input part in a line of instructions is definitely "command" or "executable file (such as batch script, script)"**

**2. command is the name of the command, for example, the command to change the working directory is cd, etc.;**

**3. The scratch sign [] does not exist in the actual command. When adding option settings, there is usually a - sign before the option, such as -h; sometimes the full name of the option is used, and the option is preceded by -- Symbols, such as --help;**

**4. parameter1 parameter2.. is the parameter attached to the option, or the parameter of command;**

**5. Instructions, options, parameters, etc. are separated by spaces. No matter how many spaces there are, the shell will treat them as one space. So spaces are very important special characters! ;**

**6. After pressing the [Enter] button, the command will be executed immediately. The [Enter] button represents the start of a line of instructions.**

**7. When the instruction is too long, you can use a backslash (\) to escape the [Enter] symbol so that the instruction continues to the next line. Notice! Just follow the special character immediately after the backslash to escape!**

**8. other:
In Linux systems, English uppercase and lowercase letters are different. For example, cd is not the same as CD.**





**Notice in the above description, "The first input data is definitely a command or an executable file"! This is a very important concept! Also, pressing the [Enter] key means to start executing this command. Let's do it in practice: Use the ls "command" to list "all hidden files and related file attributes" under "your home directory (~)". To achieve the above requirements, you need to add the -al option, so:**

```

[root@localhost ~]$ ls -al ~
[root@localhost ~]$ ls           -al   ~
[root@localhost ~]$ ls -a  -l ~

```

**The above three instructions are issued in exactly the same way and the execution results are exactly the same! Why? Please refer to the instructions above! Regarding the more detailed use of text mode, please pay special attention to the fact that in the Linux environment, "uppercase and lowercase letters are different things!" 』In other words, under Linux, the two files VBird and vbird are "completely different" files! Therefore, when you issue an instruction, you must pay attention to whether the instruction is in uppercase or lowercase. For example, when entering the following command, see what happens:**



```

[root@localhost ~]$ date      ← 結果顯示日期與時間
[root@localhost ~]$ Date      ← 結果顯示找不到指令
[root@localhost ~]$ DATE      ← 結果顯示找不到指令

```





## 📋 Basic command operations


* **顯示日期與時間的指令： date**
* **顯示日曆的指令： cal**
* **簡單好用的計算機： bc**



#### 1. Instruction to display date: date

**If you want to know the current time of the Linux system in the text interface, just enter date directly in the command line mode to display:**

```

[root@localhost ~]$ date
Wed Jan 3 13:01:32 CST 2024

```

**What it shows is: Wednesday, January 3rd, 13:01 minutes, 32 seconds, in the 2024 CST time zone! Taiwan is in the CST time zone! Please hurry up and try it out! Okay, so what if I want this program to display a date like "2024/01/03"? Then use the formatted output function of date!**


```

[root@localhost ~]$ date +%Y/%m/%d
2024/01/03

[root@localhost ~]$ date +%H:%M
13:04

```


#### 2. Instruction to display calendar: cal

**So what if I want to list the monthly calendar for the current month? Just give him the cal directly!**


```

[root@localhost ~]$ cal

    January 2024
Su Mo Tu We Th Fr Sa
    1  2  3  4  5  6
 7  8  9 10 11 12 13
14 15 16 17 18 19 20
21 22 23 24 25 26 27
28 29 30 31

```

**In addition to this month's calendar, today's date will also be displayed in highlight! Interesting! The cal (calendar) command can do many things. For example, you can display the monthly calendar for the entire year:**


&nbsp; <img src="./Images/Monthly Calendar For the Whole Year.png" alt="Monthly Calendar For the Whole Year"/>



**Basically, the syntax that can be connected to the cal command is:**

```

[root@localhost ~]$ cal [month] [year]

```



**So, if I want to know the monthly calendar for January 2024, I can directly issue:**

```

[root@localhost ~]$ cal 01 2024

    January 2024
Su Mo Tu We Th Fr Sa
    1  2  3  4  5  6
 7  8  9 10 11 12 13
14 15 16 17 18 19 20
21 22 23 24 25 26 27
28 29 30 31

```

**So, is there 13 months this year? Let’s test the correctness of this command! Issue the following command to see:**

```

[root@localhost ~]$ cal 13 2024

cal: illegal month value: use 1-12

```


#### 3. Simple and easy-to-use computer: bc

**If you are in text mode and suddenly want to do some simple addition, subtraction, multiplication and division, but you don’t have a computer at hand! Do you need to do calculations at this time? No need! Our Linux provides a calculation program, that is bc. After you enter bc in the command line, the version information will be displayed on the screen, and then you will enter the stage of waiting for instructions. As follows:**


```

[root@localhost ~]$ bc
bc 1.06.95
Copyright 1991-1994, 1997, 1998, 2000, 2004, 2006 Free Software Foundation, Inc.
This is free software with ABSOLUTELY NO WARRANTY.
For details type `warranty'.
_                             ← 這個時候，游標會停留在這裡等待你的輸入

```


**In fact, we have "entered the working environment of BC software"! It's like we use "Babacus" in Windows! Therefore, the data we are trying to input below are all calculations being performed in the bc program. So, of course, the information you enter must meet BC’s requirements! Before basic BC computer operations, let’s first tell you a few operators used:**

```

+   addition
-   Subtraction
*   Multiplication
/   division
^   index
%   remainder

```
