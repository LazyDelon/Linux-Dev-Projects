# Linux Development - Compression, packaging and backup of files and file systems

## 🎓 Compressed file uses and techniques

**Have you ever had a document file that is too large to be sent via normal email (many emails have a capacity limit of about 25MB per letter!)? Or schools and manufacturers require the use of CDs or DVDs to deliver archival data, but your single files are larger than these traditional disposable storage media! So how to divide it into multiple pieces for burning? Also, have you ever wanted to back up some important data, but the amount of data was too large and consumed a lot of your disk space? At this time, the useful "file compression" technology comes in handy!**

**Because these relatively large files can reduce their disk usage through so-called file compression technology, which can achieve the effect of reducing file capacity. In addition, some compression programs can also limit the capacity, so that a large file can be divided into several small files to facilitate the portability of floppy disks!**

**So what is "file compression"? Let's talk a little bit about his principles. The computer systems we currently use use the so-called bytes unit to measure! However, in fact, the smallest unit of measurement for computers should be bits. In addition, we also know that 1 byte = 8 bits. But what if today we just memorize a number, namely the number 1? How would he record it? Suppose a byte can be viewed as follows:**

➤  **資料來源：** [**基礎學習篇 - CentOS 7.x by~鳥哥**](https://linux.vbird.org/linux_basic/centos7/0240tarcompress.php) 

**Tips：Since 1 byte = 8 bits, there will be 8 spaces in each byte, and each space can be 0, 1!**

**What are the benefits of this "compression" and "decompression" action? The biggest benefit is that the compressed file size becomes smaller, so your hard drive capacity can virtually hold more data. In addition, in the transmission of some network data, the amount of data will also be reduced, so that the network bandwidth can be used to do more work! Instead of always getting stuck on some large file transfers! At present, many WWW websites also use file compression technology to transmit data, so as to increase the availability of website bandwidth!**





## 🎓 Common compression instructions in Linux systems

**In the Linux environment, most of the extensions of compressed files are: "`*.tar`, `*.tar.gz`, `*.tgz`, `*.gz`, `*.Z`, `*.bz2`, `*.xz`". Why? What about such an extension? Doesn’t it mean that Linux file extensions have no effect?**

**This is because Linux supports a lot of compression commands, and different commands use different compression technologies. Of course, they may not be able to compress/decompress files between them. Therefore, when you download a compressed file, you naturally need to know which compression command the file was created by, so that you can use it to decompress it! In other words, although the attributes of Linux files basically have no absolute relationship with the file name, in order to help our little human brains, appropriate file extensions are still necessary! Below we list a few common compressed file extensions:**


```

*.Z           compress 程式壓縮的檔案；
*.zip         zip 程式壓縮的檔案；
*.gz          gzip 程式壓縮的檔案；
*.bz2         bzip2 程式壓縮的檔案；
*.xz          xz 程式壓縮的檔案；
*.tar         tar 程式打包的資料，並沒有壓縮過；
*.tar.gz      tar 程式打包的檔案，其中並且經過 gzip 的壓縮
*.tar.bz2     tar 程式打包的檔案，其中並且經過 bzip2 的壓縮
*.tar.xz      tar 程式打包的檔案，其中並且經過 xz 的壓縮

```

**Common compression commands on Linux are gzip, bzip2 and the latest xz. As for compress, it has become out of fashion. In order to support the common zip in Windows, Linux has already had the zip command! gzip is a compression command developed by the GNU Project, which has replaced compress. Later, GNU developed bzip2 and xz, several compression instructions with better compression ratios! However, these commands can usually only compress and decompress one file. Isn't it annoying that a large number of files are required for each compression and decompression? At this time, the so-called "packaging software, tar" becomes very important!**

**This tar can "package" many files into one file! Even catalogs can be used this way. However, the pure tar function is only "packaging", that is, gathering many files into one file. In fact, it does not provide compression function. Later, in the GNU project, the entire tar was combined with the compression function. Together, this provides users with more convenient and powerful compression and packaging functions! Let’s talk about these basic compression instructions under Linux!**







## 🎓 gzip, zcat/zmore/zless/zgrep

**gzip can be said to be the most widely used compression command! Currently, gzip can decompress files compressed by software such as compress, zip and gzip. As for the compressed file created by gzip, the file name is `*.gz`! Let's take a look at the syntax of this directive:**


```

[root@localhost ~]$ gzip [-cdtv#] 檔名
[root@localhost ~]$ zcat 檔名.gz

選項與參數：
-c  ：將壓縮的資料輸出到螢幕上，可透過資料流重導向來處理；
-d  ：解壓縮的參數；
-t  ：可以用來檢驗一個壓縮檔的一致性～看看檔案有無錯誤；
-v  ：可以顯示出原檔案/壓縮檔案的壓縮比等資訊；
-#  ：# 為數字的意思，代表壓縮等級，-1 最快，但是壓縮比最差、-9 最慢，但是壓縮比最好！預設是 -6

範例一：找出 /etc 底下 (不含子目錄) 容量最大的檔案，並將它複製到 /tmp ，然後以 gzip 壓縮
[root@localhost ~]$ ls -ldSr /etc/*   # 忘記選項意義？請自行 man 囉！
.....(前面省略).....
-rw-r--r--.  1 root root    25213 Jun 10  2014 /etc/dnsmasq.conf
-rw-r--r--.  1 root root    69768 May  4 17:55 /etc/ld.so.cache
-rw-r--r--.  1 root root   670293 Jun  7  2013 /etc/services

[root@localhost ~]$ cd /tmp 
[root@localhost tmp]$ cp /etc/services .
[root@localhost tmp]$ gzip -v services
services:        79.7% -- replaced with services.gz

[root@localhost tmp]$ ll /etc/services /tmp/services*
-rw-r--r--. 1 root   root   670293 Jun  7  2013 /etc/services
-rw-r--r--. 1 dmtsai dmtsai 136088 Jun 30 18:40 /tmp/services.gz

```


**When you use gzip for compression, the original file will be compressed into a .gz file name by default, and the original file no longer exists. This is different from what friends who are accustomed to using windows for compression are familiar with! pay attention! pay attention! In addition, files compressed by gzip can be decompressed by the WinRAR/7zip software in Windows systems! Very useful! As for other usages:**

```

範例二：由於 services 是文字檔，請將範例一的壓縮檔的內容讀出來！
[root@localhost tmp]$ zcat services.gz

# 由於 services 這個原本的檔案是是文字檔，因此我們可以嘗試使用 zcat/zmore/zless 去讀取！
# 此時螢幕上會顯示 servcies.gz 解壓縮之後的原始檔案內容！

範例三：將範例一的檔案解壓縮
[root@localhost tmp]$ gzip -d services.gz
# 與 gzip 相反， gzip -d 會將原本的 .gz 刪除，回復到原本的 services 檔案。

範例四：將範例三解開的 services 用最佳的壓縮比壓縮，並保留原本的檔案
[root@localhost tmp]$ gzip -9 -c services > services.gz

範例五：由範例四再次建立的 services.gz 中，找出 http 這個關鍵字在哪幾行？
[root@localhost tmp]$ zgrep -n 'http' services.gz
14:#       http://www.iana.org/assignments/port-numbers
89:http            80/tcp          www www-http    # WorldWideWeb HTTP
90:http            80/udp          www www-http    # HyperText Transfer Protocol
.....(底下省略).....

```



## 🎓 bzip2, bzcat/bzmore/bzless/bzgrep

**If gzip was established to replace compress and provide a better compression ratio, then bzip2 was established to replace gzip and provide a better compression ratio. bzip2 is really useful. The compression ratio of this thing is even better than gzip. As for the usage of bzip2, it is almost the same as gzip! Take a look at the usage below!**

```

[root@localhost ~]$ bzip2 [-cdkzv#] 檔名
[root@localhost ~]$ bzcat 檔名.bz2

選項與參數：
-c  ：將壓縮的過程產生的資料輸出到螢幕上！
-d  ：解壓縮的參數
-k  ：保留原始檔案，而不會刪除原始的檔案喔！
-z  ：壓縮的參數 (預設值，可以不加)
-v  ：可以顯示出原檔案/壓縮檔案的壓縮比等資訊；
-#  ：與 gzip 同樣的，都是在計算壓縮比的參數， -9 最佳， -1 最快！

範例一：將剛剛 gzip 範例留下來的 /tmp/services 以 bzip2 壓縮
[root@localhost tmp]$ bzip2 -v services
  services:  5.409:1,  1.479 bits/byte, 81.51% saved, 670293 in, 123932 out.

[root@localhost tmp]$ ls -l services*
-rw-r--r--. 1 dmtsai dmtsai 123932 Jun 30 18:40 services.bz2
-rw-rw-r--. 1 dmtsai dmtsai 135489 Jun 30 18:46 services.gz
# 此時 services 會變成 services.bz2 之外，你也可以發現 bzip2 的壓縮比要較 gzip 好喔！！
# 壓縮率由 gzip 的 79% 提升到 bzip2 的 81% 哩！

範例二：將範例一的檔案內容讀出來！
[root@localhost tmp]$ bzcat services.bz2

範例三：將範例一的檔案解壓縮
[root@localhost tmp]$ bzip2 -d services.bz2

範例四：將範例三解開的 services 用最佳的壓縮比壓縮，並保留原本的檔案
[root@localhost tmp]$ bzip2 -9 -c services > services.bz2

```



## 🎓 xz, xzcat/xzmore/xzless/xzgrep

**Although bzip2 already has a great compression ratio, apparently some free software developers were not satisfied, so they later launched xz, a software with a higher compression ratio! The usage of this software is almost exactly the same as gzip/bzip2! Then let’s take a look!**

```

[root@localhost ~]$ xz [-dtlkc#] 檔名
[root@localhost ~]$ xcat 檔名.xz
選項與參數：
-d  ：就是解壓縮啊！
-t  ：測試壓縮檔的完整性，看有沒有錯誤
-l  ：列出壓縮檔的相關資訊
-k  ：保留原本的檔案不刪除～
-c  ：同樣的，就是將資料由螢幕上輸出的意思！
-#  ：同樣的，也有較佳的壓縮比的意思！

範例一：將剛剛由 bzip2 所遺留下來的 /tmp/services 透過 xz 來壓縮！
[root@localhost tmp]$ xz -v services
services (1/1)
  100 %        97.3 KiB / 654.6 KiB = 0.149

[root@localhost tmp]$ ls -l services*
-rw-rw-r--. 1 dmtsai dmtsai 123932 Jun 30 19:09 services.bz2
-rw-rw-r--. 1 dmtsai dmtsai 135489 Jun 30 18:46 services.gz
-rw-r--r--. 1 dmtsai dmtsai  99608 Jun 30 18:40 services.xz
# 各位觀眾！看到沒有啊！！容量又進一步下降的更多耶！好棒的壓縮比！

範例二：列出這個壓縮檔的資訊，然後讀出這個壓縮檔的內容
[root@localhost tmp]$ xz -l services.xz
Strms  Blocks   Compressed Uncompressed  Ratio  Check   Filename
    1       1     97.3 KiB    654.6 KiB  0.149  CRC64   services.xz
# 竟然可以列出這個檔案的壓縮前後的容量，真是太人性化了！這樣觀察就方便多了！

[root@localhost tmp]$ xzcat services.xz

範例三：將他解壓縮吧！
[root@localhost tmp]$ xz -d services.xz

範例四：保留原檔案的檔名，並且建立壓縮檔！
[root@localhost tmp]$ xz -k services

```





## 🎓 Packaging：`tar`


**Most of the commands mentioned in the previous section can only compress a single file. Although gzip, bzip2, and xz can also compress directories, the compression of directories by these two commands refers to "compressing all files in the directory." Files are "separately" compressed! Unlike Windows systems, you can use compression software like WinRAR to "package a lot of data into one file".**

**This command function that packages multiple files or directories into one large file can be called a "packaging command"! Does Linux have such packaging instructions? There is! That is the famous tar thing! tar can package multiple directories or files into one large file, and can also compress the file at the same time through the support of gzip/bzip2/xz! What’s even more interesting is that because tar is so widely used, WinRAR for Windows currently also supports decompression of .tar.gz file names! Very good! So let’s play with this dong dong!**


```

[root@localhost ~]$ tar [-z|-j|-J] [cv] [-f 待建立的新檔名] filename... <==打包與壓縮
[root@localhost ~]$ tar [-z|-j|-J] [tv] [-f 既有的 tar檔名]             <==察看檔名
[root@localhost ~]$ tar [-z|-j|-J] [xv] [-f 既有的 tar檔名] [-C 目錄]   <==解壓縮

選項與參數：
-c  ：建立打包檔案，可搭配 -v 來察看過程中被打包的檔名(filename)
-t  ：察看打包檔案的內容含有哪些檔名，重點在察看『檔名』就是了；
-x  ：解打包或解壓縮的功能，可以搭配 -C (大寫) 在特定目錄解開
      特別留意的是， -c, -t, -x 不可同時出現在一串指令列中。
-z  ：透過 gzip  的支援進行壓縮/解壓縮：此時檔名最好為 *.tar.gz
-j  ：透過 bzip2 的支援進行壓縮/解壓縮：此時檔名最好為 *.tar.bz2
-J  ：透過 xz    的支援進行壓縮/解壓縮：此時檔名最好為 *.tar.xz
      特別留意， -z, -j, -J 不可以同時出現在一串指令列中
-v  ：在壓縮/解壓縮的過程中，將正在處理的檔名顯示出來！
-f filename：-f 後面要立刻接要被處理的檔名！建議 -f 單獨寫一個選項囉！(比較不會忘記)
-C 目錄    ：這個選項用在解壓縮，若要在特定目錄解壓縮，可以使用這個選項。

其他後續練習會使用到的選項介紹：
-p(小寫) ：保留備份資料的原本權限與屬性，常用於備份(-c)重要的設定檔
-P(大寫) ：保留絕對路徑，亦即允許備份資料中含有根目錄存在之意；
--exclude=FILE：在壓縮的過程中，不要將 FILE 打包！ 

```

* **壓　縮：tar -jcv -f filename.tar.bz2      要被壓縮的檔案或目錄名稱**
* **查　詢：tar -jtv -f filename.tar.bz2**
* **解壓縮：tar -jxv -f filename.tar.bz2 -C   欲解壓縮的目錄**



#### Use tar to add the -z, -j or -J parameters to back up the /etc/ directory

**It’s a good thing to back up the /etc directory when necessary! The easiest way to back up /etc is to use tar! Let’s have some fun first:**

```

[root@localhost ~]$ su -  # 因為備份 /etc 需要 root 的權限，否則會出現一堆錯誤

[root@localhost ~]# time tar -zpcv -f /root/etc.tar.gz /etc
tar: Removing leading `/' from member names  <==注意這個警告訊息
/etc/
....(中間省略)....
/etc/hostname
/etc/aliases.db

real    0m0.799s   # 多了 time 會顯示程式運作的時間！看 real 就好了！花去了 0.799s
user    0m0.767s
sys     0m0.046s

# 由於加上 -v 這個選項，因此正在作用中的檔名就會顯示在螢幕上。
# 如果你可以翻到第一頁，會發現出現上面的錯誤訊息！底下會講解。
# 至於 -p 的選項，重點在於『保留原本檔案的權限與屬性』之意。

[root@localhost ~]# time tar -jpcv -f /root/etc.tar.bz2 /etc
....(前面省略)....
real    0m1.913s
user    0m1.881s
sys     0m0.038s

[root@localhost ~]# time tar -Jpcv -f /root/etc.tar.xz  /etc
....(前面省略)....
real    0m9.023s
user    0m8.984s
sys     0m0.086s
# 顯示的訊息會跟上面一模一樣囉！不過時間會花比較多！使用了 -J 時，會花更多時間

[root@localhost ~]# ll /root/etc*
-rw-r--r--. 1 root root 6721809 Jul  1 00:16 /root/etc.tar.bz2
-rw-r--r--. 1 root root 7758826 Jul  1 00:14 /root/etc.tar.gz
-rw-r--r--. 1 root root 5511500 Jul  1 00:16 /root/etc.tar.xz
[root@localhost ~]# du -sm /etc
28     /etc  # 實際目錄約佔有 28MB 的意思！

```




#### Check the data content of the tar file (you can check the file name) and whether the backup file name has the meaning of the root directory.

**It is very easy to check the file names inside the packaged files created by tar! You can do this:**

```

[root@localhost ~]# tar -jtv -f /root/etc.tar.bz2
....(前面省略)....
-rw-r--r-- root/root       131 2015-05-25 17:48 etc/locale.conf
-rw-r--r-- root/root        19 2015-05-04 17:56 etc/hostname
-rw-r--r-- root/root     12288 2015-05-04 17:59 etc/aliases.db

```




#### Decompress the backup data and consider the decompression action of the specific directory (the application of -C option)

**What if you want to unpack it? A very simple action is to unpack it directly!**

```

[root@localhost ~]# tar -jxv -f /root/etc.tar.bz2
[root@localhost ~]# ll
....(前面省略)....
drwxr-xr-x. 131 root root    8192 Jun 26 22:14 etc
....(後面省略)....

```






**At this time, the packaged file will be decompressed in this directory! So, you will find a directory named etc under your home directory in a moment! So, if you want to unzip the file under /tmp, you can cd /tmp and then issue the above command. However, this seems very troublesome~ Is there an easier way to "specify the directory to be unlocked"? Yes, you can use the -C option! for example:**


```

[root@localhost ~]# tar -jxv -f /root/etc.tar.bz2 -C /tmp
[root@localhost ~]# ll /tmp
....(前面省略)....
drwxr-xr-x. 131 root   root     8192 Jun 26 22:14 etc
....(後面省略)....

```





#### How to unlock only a single file

**When we decompressed it just now, we decompressed all the contents of the entire packaged file! Imagine a situation, if I only want to unpack one of the files in the packaged file, how should I do it? It's very simple, you just use -jtv to find the file name you want, and then unzip the file name. Let’s use the following example to illustrate:**


```

# 1. 先找到我們要的檔名，假設解開 shadow 檔案好了：
[root@localhost ~]# tar -jtv -f /root/etc.tar.bz2 | grep 'shadow'
---------- root/root       721 2015-06-17 00:20 etc/gshadow
---------- root/root      1183 2015-06-17 00:20 etc/shadow-
---------- root/root      1210 2015-06-17 00:20 etc/shadow  <==這是我們要的！
---------- root/root       707 2015-06-17 00:20 etc/gshadow-

# 先搜尋重要的檔名！其中那個 grep 是『擷取』關鍵字的功能！我們會在第三篇說明！
# 這裡您先有個概念即可！那個管線 | 配合 grep 可以擷取關鍵字的意思！

# 2. 將該檔案解開！語法與實際作法如下：
[root@localhost ~]# tar -jxv -f 打包檔.tar.bz2 待解開檔名
[root@localhost ~]# tar -jxv -f /root/etc.tar.bz2 etc/shadow
etc/shadow

[root@localhost ~]# ll etc
total 4
----------. 1 root root 1210 Jun 17 00:20 shadow
# 很有趣！此時只會解開一個檔案而已！不過，重點是那個檔名！你要找到正確的檔名。
# 在本例中，你不能寫成 /etc/shadow ！因為記錄在 etc.tar.bz2 內的並沒有 / 之故！

```




#### Only backup files that are newer than a certain time


**In some cases you will want to back up new files only, not old files! At this time, the --newer-mtime option is very important! In fact, there are two options, one is "--newer" and the other is "--newer-mtime". What is the difference between these two options? We talked about three different time parameters in the introduction of touch in Chapter 6. When using --newer, it means that the subsequent date includes "mtime and ctime", while --newer-mtime is just mtime! So you know it! ^_^. Then let us try to deal with it!**

```

# 1. 先由 find 找出比 /etc/passwd 還要新的檔案
[root@localhost ~]# find /etc -newer /etc/passwd
....(過程省略)....
# 此時會顯示出比 /etc/passwd 這個檔案的 mtime 還要新的檔名，
# 這個結果在每部主機都不相同！您先自行查閱自己的主機即可，不會跟鳥哥一樣！

[root@localhost ~]# ll /etc/passwd
-rw-r--r--. 1 root root 2092  Jun 17 00:20 /etc/passwd

# 2. 好了，那麼使用 tar 來進行打包吧！日期為上面看到的 2015/06/17
[root@localhost ~]# tar -jcv -f /root/etc.newer.then.passwd.tar.bz2 \
> --newer-mtime="2015/06/17" /etc/*
tar: Option --newer-mtime: Treating date `2015/06/17' as 2015-06-17 00:00:00
tar: Removing leading `/' from member names
/etc/abrt/
....(中間省略)....
/etc/alsa/
/etc/yum.repos.d/
....(中間省略)....
tar: /etc/yum.repos.d/CentOS-fasttrack.repo: file is unchanged; not dumped
# 最後行顯示的是『沒有被備份的』，亦即 not dumped 的意思！

# 3. 顯示出檔案即可
[root@localhost ~]# tar -jtv -f /root/etc.newer.then.passwd.tar.bz2 | grep -v '/$' 
# 透過這個指令可以呼叫出 tar.bz2 內的結尾非 / 的檔名！就是我們要的啦！

```


#### Base name: tarfile, tarball?

**It is also worth mentioning that the files packaged by tar have different names depending on whether they are compressed or not! If it is just for packaging, it is just "tar -cv -f file.tar". We call this file tarfile. If there is support for compression, such as "tar -jcv -f file.tar.bz2", we call it a tarball (tar ball?)! This is just a basic title, but many books and the Internet will use the name of this tarball! So I have to introduce it to you.**

**In addition, tar can not only package data into files, but also package files into certain special devices. For example, tape is a common example. Since the tape drive is a one-time read/write device, we cannot use instructions such as cp to copy! Then if you want to back up /home, /root, /etc to a tape drive (/dev/st0), you can use: "tar -cv -f /dev/st0 /home /root /etc", it is very simple and easy Bar! It is very common for tape drives to be used in backups (especially enterprise applications)!**







#### Special Application: Utilizing Pipeline Commands and Data Flows


**Among the uses of tar, there is one of the most special ways, which is to package and unpack the files to be processed through standard input/output data flow redirection (standard input/standard output) and pipeline commands (pipe). Compress to the target directory. We will introduce more detailed information about data flow redirection and pipeline commands in Chapter 10 bash. Let’s take a look at an example first!**


```

# 1. 將 /etc 整個目錄一邊打包一邊在 /tmp 解開
[root@localhost ~]# cd /tmp

[root@localhost tmp]# tar -cvf - /etc | tar -xvf -
# 這個動作有點像是 cp -r /etc /tmp 啦～依舊是有其有用途的！
# 要注意的地方在於輸出檔變成 - 而輸入檔也變成 - ，又有一個 | 存在～
# 這分別代表 standard output, standard input 與管線命令啦！
# 簡單的想法中，你可以將 - 想成是在記憶體中的一個裝置(緩衝區)。
# 更詳細的資料流與管線命令，請翻到 bash 章節囉！

```






#### Example: System backup example

**There are many important directories on the system that need to be backed up, and in fact we do not recommend that you place backup data in the /root directory! Assume that you already know that the important directories are as follows:**

* **/etc/ (configuration file)**
* **/home/ (user's home directory)**
* **/var/spool/mail/ (the mailboxes of all accounts in the system)**
* **/var/spool/cron/ (job scheduling configuration file for all accounts)**
* **/root (system administrator’s home directory)**




