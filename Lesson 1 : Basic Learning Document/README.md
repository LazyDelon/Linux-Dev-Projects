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
