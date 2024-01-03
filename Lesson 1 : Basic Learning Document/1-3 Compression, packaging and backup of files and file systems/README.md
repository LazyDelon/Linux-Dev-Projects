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

[dmtsai@study ~]$ gzip [-cdtv#] 檔名
[dmtsai@study ~]$ zcat 檔名.gz
選項與參數：
-c  ：將壓縮的資料輸出到螢幕上，可透過資料流重導向來處理；
-d  ：解壓縮的參數；
-t  ：可以用來檢驗一個壓縮檔的一致性～看看檔案有無錯誤；
-v  ：可以顯示出原檔案/壓縮檔案的壓縮比等資訊；
-#  ：# 為數字的意思，代表壓縮等級，-1 最快，但是壓縮比最差、-9 最慢，但是壓縮比最好！預設是 -6

範例一：找出 /etc 底下 (不含子目錄) 容量最大的檔案，並將它複製到 /tmp ，然後以 gzip 壓縮
[dmtsai@study ~]$ ls -ldSr /etc/*   # 忘記選項意義？請自行 man 囉！
.....(前面省略).....
-rw-r--r--.  1 root root    25213 Jun 10  2014 /etc/dnsmasq.conf
-rw-r--r--.  1 root root    69768 May  4 17:55 /etc/ld.so.cache
-rw-r--r--.  1 root root   670293 Jun  7  2013 /etc/services

[dmtsai@study ~]$ cd /tmp 
[dmtsai@study tmp]$ cp /etc/services .
[dmtsai@study tmp]$ gzip -v services
services:        79.7% -- replaced with services.gz
[dmtsai@study tmp]$ ll /etc/services /tmp/services*
-rw-r--r--. 1 root   root   670293 Jun  7  2013 /etc/services
-rw-r--r--. 1 dmtsai dmtsai 136088 Jun 30 18:40 /tmp/services.gz

```
