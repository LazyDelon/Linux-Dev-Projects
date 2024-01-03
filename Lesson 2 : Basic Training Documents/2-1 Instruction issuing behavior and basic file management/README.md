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
  or:  date [-u|--utc|--universal] [MMDDhhmm[[CC]YY][.ss]]                 <==上面這兩行是語法的部份
Display the current time in the given FORMAT, or set the system date.      <==這一行是指令說明

Mandatory arguments to long options are mandatory for short options too.   <==底下是主要的 option 說明
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

FORMAT controls the output.  Interpreted sequences are:  <==底下則是格式 (FORMAT) 的說明

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



Use the identity switch "su-"







