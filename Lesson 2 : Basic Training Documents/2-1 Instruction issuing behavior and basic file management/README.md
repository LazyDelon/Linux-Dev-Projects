# Linux Development - Instruction issuing behavior and basic file management


## 📣 Establishment of text interface terminal operation behavior

**In fact, we all communicate with the system through "software" or "programs". The program obtained after logging in in text mode is called a shell. This is because this program is responsible for communicating with the user (us) from the outside, so it is nicknamed the shell program! The default shell program of CentOS 8 is bash. It is best for users to establish good operating behavior from the beginning, which will be of great help to future Linux use.**

**In addition, there are several commonly confused terms regarding the CLI environment. Let’s understand them first:**


* **Physical Console: Hardware that directly provides physical functions such as keyboard, mouse, and screen can be called a physical console. In our class environment, the remote-viewer should be able to pretend to be a simulated physical console. If we take a physical Linux server as an example, the location of the physical computer is the physical console.**


* **Virtual Console: In the previous lesson, we talked about how you can use key combinations to enter tty1~tty6 respectively. These six login environments are all simulated by the physical console to simulate the console environment, so they are called Virtual console. Each virtual console is independent and can be logged in by different users.**


* **Terminal: Each virtual console provides an interface that provides keyboard access and screen output. This interface is called a terminal. After logging in with an account and password on the terminal, the user can obtain a shell to interact with the system. Generally speaking, terminals mostly refer to pure text interfaces, but in a broad sense, graphical user interface (GUI) can also be regarded as a kind of terminal.**


**Shell: Software that allows users to enter a command string and then throws the command string into the system for execution is called a shell program.**






### 🎓 How to issue text mode commands


**In the bash shell environment, there are basically several things that need to be paid attention to when issuing instructions:**


```

[student@localhost ~]$ command  [-options]  [parameter1...]

```

**command (command part):**

* **The first input part of a line of command is the command (command) or executable file (such as script) or the complete executable file name**

* **『command』: is the name of the command, for example, the command to view the history command is history, etc.;**



**[-option] (option part):**

* **The square brackets "[ ]" do not exist in the actual command, but are only used as an explanation prompt. The prompt is that -option may or may not be present in the command line;**

* **『-options』: It is an option, usually there is a minus sign (-) in front of the option, such as -h;**

* **options Sometimes long options are provided, in which case two minus signs are used, such as --help.**

* **Note that the option -help usually means -h -e -l -p, as opposed to the single long option of --help.**

* **Options sometimes take parameters, so you may find syntax like [-option para] or [--option=para]**



**[parameter1...] (parameter part):**

* **『parameter1』: parameter, which is the parameter attached to the option, or the parameter of command;**

* **If there is a decimal point after parameters..., it means that multiple parameters can be added.**


**Notes on instruction execution:**

* **Instructions, options, and parameters are all separated by spaces or [tab]. No matter how many spaces there are, they are regarded as one space, so spaces are special characters.**

* **The [Enter] button represents the start of a line of instructions.**

* **In the Linux world, English uppercase and lowercase characters are different. For example, cd and CD are different commands.**







#### Practice formatting `date` output using date


**In the previous lesson, we used two simple commands, ls and ll, to view file names. If we want to know the current time, or format the output time, we have to use the date command to process it!**



```

[student@localhost ~]$ date
二  3月  3 17:54:38 CST 2020

```






**Because student selects the Chinese language system, what appears on the screen will be Chinese Tuesday, month and day. If formatted output is required, special options or parameters must be added to process it. For example, in Taiwan, we often use a date output format such as 2020/03/03. At this time, you may need to issue an instruction like this:**


```

[student@localhost ~]$ date +%Y/%m/%d
2020/03/03

```





**The above option information (+%Y/%m%d) basically does not need to be memorized, and can be processed by online query. The simplest way to handle it is to query the functions of each option through the --help long option, as shown below:**


```

[student@localhost ~]$ date --help
Usage: date [OPTION]... [+FORMAT]
  or:  date [-u|--utc|--universal] [MMDDhhmm[[CC]YY][.ss]]                    ← 上面這兩行是語法的部份
Display the current time in the given FORMAT, or set the system date.         ← 這一行是指令說明

Mandatory arguments to long options are mandatory for short options too.      ← 底下是主要的 option 說明
  -d, --date=STRING          display time described by STRING, not 'now'
      --debug                annotate the parsed date,
                              and warn about questionable usage to stderr
  -f, --file=DATEFILE        like --date; once for each line of DATEFILE
  -I[FMT], --iso-8601[=FMT]  output date/time in ISO 8601 format.
                               FMT='date' for date only (the default),
                               'hours', 'minutes', 'seconds', or 'ns'
                               for date and time to the indicated precision.
                               Example: 2006-08-14T02:34:56-06:00
  -R, --rfc-email            output date and time in RFC 5322 format.
                               Example: Mon, 14 Aug 2006 02:34:56 -0600
      --rfc-3339=FMT         output date/time in RFC 3339 format.
                               FMT='date', 'seconds', or 'ns'
                               for date and time to the indicated precision.
                               Example: 2006-08-14 02:34:56-06:00
  -r, --reference=FILE       display the last modification time of FILE
  -s, --set=STRING           set time described by STRING
  -u, --utc, --universal     print or set Coordinated Universal Time (UTC)
      --help     顯示此求助說明並離開
      --version  顯示版本資訊並離開

FORMAT controls the output.  Interpreted sequences are:    ← 底下則是格式 (FORMAT) 的說明

  %%   a literal %
  %a   locale's abbreviated weekday name (e.g., Sun)
  %A   locale's full weekday name (e.g., Sunday)
  %b   locale's abbreviated month name (e.g., Jan)
......

```





**The entire auxiliary file can be roughly divided into several parts:**

**Usage (execution syntax part):**

* **There are two items in the execution syntax, namely:**
* * **date [OPTION]... [+FORMAT]**
* * **date [-u|--utc|--universal] [MMDDhhmm[[CC]YY][.ss]]**

* **Line 3 explains the purpose of this command: (1) Display the current time and format it for output, and (2) Set the system time.**




**Mandatory arguments...(Main option description):**

* **For example, "-d, --date=STRING" means that you can use "-d STRING" or "--date=STRING".**

* **That STRING is the date you specified, not the current time. For example, if you want to know yesterday's date, you can use "date -d yesterday" or "date --date=yesterday" to show the meaning.**




**FORMAT controls...(detailed description of FORMAT format):**

* **For example, %a represents the day of the week in English lowercase, and can be displayed using date +%a**

* **For example, %A represents the day of the week in English capital letters
In this project, you can check the related option functions of %Y, %m, and %d!**





### Use the identity switch "su-"


**Let’s continue to play with the date command! After using date --help from the previous section, you can find that there are two situations in the syntax, as shown below:**


```

[student@localhost ~]$ date --help
Usage: date [OPTION]... [+FORMAT]
  or:  date [-u|--utc|--universal] [MMDDhhmm[[CC]YY][.ss]]
Display the current time in the given FORMAT, or set the system date.

```




**In the command description, it can be "display, display" or "set, set" date. The first line of the syntax (Usage) is just to display the date, and the second line is of course to set the date. What happens if I use the student identity to set the date?**


```

# 先注意，設定日期的格式是： MMDDhhmmYYYY，也就是 月日時分年，除了年，其他都有兩位數
[student@station10-101 ~]$ date
三  3月  4 00:04:53 CST 2020                   ← 當下的日期

[student@station10-101 ~]$ date 030400052020   ← 重設為當下的時間
date: 無法設定時間: 此項操作並不被允許
三  3月  4 00:05:00 CST 2020                    ← 這個動作其實沒有生效！

```





**It can be found that the date has not been changed to the correct date, and the date also clearly tells the operator that the operator does not have permission (Operation not permitted)! Because the date setting can only be set by the system administrator. At this point we have to switch identities to become the system administrator (root). The processing method is as follows:**

```

[student@localhost ~]$ su -
密碼：                  ← 輸入密碼時，不會出現 * 符號！
[root@localhost ~]#

```





**The password for the root of this teaching system is myCentOS8. Please note that the case is different! Therefore, after entering myCentOS8 after "Password:", you can find that the user's identity has changed to root! At this point, use date again to see if the date can be set correctly?**


```

[root@localhost ~]# date 030400052020
Wed Mar  4 00:05:00 CST 2020
[root@localhost ~]# date
Wed Mar  4 00:05:20 CST 2020
[root@localhost ~]# hwclock -w

```





### 🎓 Language function switching


**Since our system environment uses Chinese, the date output may be mainly in Chinese. If you want to display the year and month in English, you have to modify a variable, as shown below:**


```

[student@localhost ~]$ date
三  3月  4 00:12:33 CST 2020                   ← 這個時候是中文
[student@localhost ~]$ LANG=en_US.utf8         ← 這個 LANG 就可以改語系
[student@localhost ~]$ date
Wed Mar  4 00:13:10 CST 2020                   ← 輸出就變成英文了！

```


**You can find that the date has been changed to be displayed in English! This is the setting function of LANG language variables. The two commonly used language families in Taiwan are Chinese (zh_TW) and English (en_US) Unicode. Of course, older data may need to use big5 encoding, so the common language families in Taiwan are:**

* **zh_TW.utf8**
* **zh_TW.big5**
* **en_US.utf8**



#### A. First adjust the language to the default zh_TW.utf8

```

[student@station10-101 ~]$ LANG=zh_TW.utf8
[student@station10-101 ~]$ echo ${LANG}
zh_TW.utf8        ← 這樣就變更了語系

```



#### B. There are many language data, including language characters, currency symbols, numerical modes, time formats, address formats, etc. Each item can be set independently. If you want to know all the above output messages in the current language, use the locale command to query, as follows:

```

[student@station10-101 ~]$ locale
LANG=zh_TW.utf8                 ← 整體語系，底下則是個別訊息的語系
LC_CTYPE="zh_TW.utf8"
LC_NUMERIC="zh_TW.utf8"
LC_TIME="zh_TW.utf8"
LC_COLLATE="zh_TW.utf8"
LC_MONETARY="zh_TW.utf8"
LC_MESSAGES="zh_TW.utf8"
LC_PAPER="zh_TW.utf8"
LC_NAME="zh_TW.utf8"
LC_ADDRESS="zh_TW.utf8"
LC_TELEPHONE="zh_TW.utf8"
LC_MEASUREMENT="zh_TW.utf8"
LC_IDENTIFICATION="zh_TW.utf8"
LC_ALL=

[student@station10-101 ~]$ locale --help
使用方式: locale [參數…] 名稱
  或者:  locale [參數…] [-a|-m]
取得語區資料特定的資訊

 系統相關資訊:
  -a, --all-locales          寫出存在的語區資料名稱
  -m, --charmaps             寫出存在的字集對照表名稱
....

```





#### C. List all the languages ​​currently supported on this Linux host? You can find the -a option from the final locale --help of the above question, so:


```

[student@station10-101 ~]$ locale -a
C              ← 標準 C 程式語言語系
C.utf8
en_AG
en_AU
en_AU.utf8
......
zh_TW
zh_TW.euctw
zh_TW.utf8

```



#### D. If you want to change the language display of the entire system, you can use the localectl command. For example, although our student account uses Chinese language, the default language of the entire Linux system is still English, as follows:


```

[student@station10-101 ~]$ echo ${LANG}
zh_TW.utf8         ← 是中文
[student@station10-101 ~]$ localectl
   System Locale: LANG=en_US.UTF-8   ← 是英文
       VC Keymap: us
      X11 Layout: us

[student@station10-101 ~]$ localectl --help
localectl [OPTIONS...] COMMAND ...
......

Commands:
  status                   Show current locale settings
  set-locale LOCALE...     Set system locale    ← 看起來是可以設定成為不同的語系
  list-locales             Show known locales
  set-keymap MAP [MAP]     Set console and X11 keyboard mappings
......

[student@station10-101 ~]$ localectl set-locale zh_TW.utf8
# 這個指令要經過 root 驗證，因為會更動到系統設定值！所以這裡先知道即可。不建議修改啦！

```



### 🎓 Common hotkeys and key combinations

**[tab]: It can be command completion, file name completion, or variable name completion.**

**[ctrl]+c: Interrupt a running command**

**[shift]+[PageUp], [shift]+[PageDown]: Move the screen up and down**






## 📣 A preliminary study on Linux file management


**Under Linux systems, there will always be situations where file management is required, including creating directories and files, copying and moving files, deleting files and directories, etc. In addition, readers should also know that under Linux systems, which directories exist in regular systems, and what information should be placed in this directory, etc.**





### 🎓 Introduction to Linux Directory Tree System

**static (fixed content): includes tool programs, function libraries and documentation that will not change**

**dynamic / variable (changed content): may be modified by the running program**

**Persistent (persistent content): data that will persist after booting and mostly will not change, such as some environment setting information**

**runtime (running content): Contains data in the operation of user programs or system programs. These data will be deleted after restarting.**







**In addition, in the Linux environment, all directories are derived from the root directory (/), and file names written starting from the root directory are also called "absolute paths". In terms of disk planning, if you need to know the combination of disks and directory trees, you can use the df (display filesystem) software to check:**


```

[student@localhost ~]$ df
檔案系統                 1K-區段    已用    可用 已用% 掛載點
devtmpfs                  918108       0  918108    0% /dev
tmpfs                     936140       0  936140    0% /dev/shm
tmpfs                     936140    9388  926752    2% /run
tmpfs                     936140       0  936140    0% /sys/fs/cgroup
/dev/mapper/centos-root 10475520 4359596 6115924   42% /
/dev/vda2                1998672  149448 1727984    8% /boot
/dev/mapper/centos-home  3135488   70240 3065248    3% /home
tmpfs                     187228      16  187212    1% /run/user/42
tmpfs                     187228       4  187224    1% /run/user/0
tmpfs                     187228       4  187224    1% /run/user/1000

```






### 🎓 Working directory switching and relative/absolute paths


**By default, when users obtain the shell environment, it is usually in their own "home directory". For example, after opening Windows File Explorer, what appears on the screen is usually an environment such as "My Folder". If you want to change the "working directory", for example, change the working directory to /var/spool/mail, you can do this:**


```

[student@localhost ~]$ ls
Desktop    Downloads  Music     Public     text1.txt
Documents  group      Pictures  Templates  Videos

[student@localhost ~]$ cd /var/spool/mail
[student@localhost mail]$ ls
rpc  student

```







**As shown above, the reader will be in the student home directory at first, so when simply using ls, the information under the working directory (home directory) will be listed, that is, a bunch of directory names where the home directory should exist. When the reader operates " cd /var/spool/mail ", the working directory will become this directory, so the prompt character will also change ~ to mail. Therefore, if you use the data in the working directory listed by ls, different file names will appear. Readers should pay special attention to the "working directory" when operating instructions. The way to list the current working directory is to use pwd:**


```

[student@localhost mail]$ pwd
/var/spool/mail
[student@localhost mail]$ 

```





**When readers operate the operating system, don't just look at the file name under the prompt character. It is best to check the actual directory. The following cases:**


```

[student@localhost mail]$ cd /etc
[student@localhost etc]$ pwd
/etc
[student@localhost etc]$ cd /usr/local/etc
[student@localhost etc]$ pwd
/usr/local/etc
[student@localhost etc]$ 

```





## 📣 Simple file management exercises



**From the description in this chapter, readers can understand that /etc and /boot are two very important directories, and /etc is the one that needs to be backed up. If a reader uses the identity of student to temporarily perform file management activities, such as a full backup of /etc, what can be done?**



1. First go to the memory simulation directory /dev/shm to operate the subsequent instructions:

```

[student@localhost ~]$ cd /dev/shm
[student@localhost shm]$ pwd
/dev/shm

```




2. Create a directory named backup and wait for backup data:

```

[student@localhost shm]$ mkdir backup
[student@localhost shm]$ ll
總計 0
drwxrwxr-x. 2 student student 40  3月  4 13:14 backup

```




3. Enter the backup directory:


```

[student@localhost shm]$ cd backup
[student@localhost backup]$ pwd
/dev/shm/backup

```





4. Copy /etc completely: you should use cp --help to check the instructions for copying! You can also use man cp to check it!



```

[student@localhost backup]$ cp --help
用法：cp [選項]... [-T] 來源 目的地
  或：cp [選項]... 來源... 目錄     ← 這個是最常用的指令串！
  或：cp [選項]... -t 目錄 來源...
Copy SOURCE to DEST, or multiple SOURCE(s) to DIRECTORY.

Mandatory arguments to long options are mandatory for short options too.
  -a, --archive                same as -dR --preserve=all     ← 嘗試完整保留權限
      --attributes-only        don't copy the file data, just the attributes
      --backup[=CONTROL]       make a backup of each existing destination file
......
  -R, -r, --recursive          copy directories recursively   ← 複製目錄
......

# 開始嘗試將 /etc 檔名複製到本目錄 (.) 底下
[student@localhost backup]$ cp /etc .
cp: -r not specified; omitting directory '/etc'

```





5. Start copying the directory (-r):



```

# 剛剛 cp --help 有看到 -r 可以複製目錄，因此加上這個選項功能來處理目錄複製：
[student@localhost backup]$ cp -r /etc .
cp: 無法開啟 ‘/etc/crypttab’ 來讀取資料: 拒絕不符權限的操作
cp: 無法存取 ‘/etc/pki/CA/private’: 拒絕不符權限的操作
cp: 無法存取 ‘/etc/pki/rsyslog’: 拒絕不符權限的操作
.......
[student@localhost backup]$ ll
總計 0
drwxr-xr-x. 135 student student 5020  3月  4 13:21 etc

```


6. Copy the file again and send the error message to the Trash instead of showing it on the screen:


```

[student@localhost backup]$ cp -r /etc . 2> /dev/null
[student@localhost backup]$ ll -d /etc ./etc
drwxr-xr-x. 135 student student 5020  3月  4 13:22 ./etc
drwxr-xr-x. 135 root    root    8192  3月  3 17:54 /etc

```
