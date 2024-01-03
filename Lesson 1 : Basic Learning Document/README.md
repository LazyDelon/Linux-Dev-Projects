# Basic Learning for CentOS 7.x

## 📣 在終端介面登入linux

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
