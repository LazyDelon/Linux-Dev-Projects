# Linux Development - Directories & Paths


## 🎓 Relative path vs absolute path


**Before starting to switch directories, you must first understand the so-called "path (PATH)". What is interesting is: what are "relative paths" and "absolute paths"? Although this issue has been briefly mentioned in the previous chapter, I would like to stress it again here!**


* **Absolute path: The path must be written starting from the root directory /, for example: /usr/share/doc directory.**


* **Relative path: The path is written "not starting with /". For example, when going from /usr/share/doc to /usr/share/man, it can be written as: "cd ../man" This is how the relative path is written. ! A relative path means "a path relative to the current working directory!" 』**





#### The use of relative paths

**So what's the big deal between relative paths and absolute paths? drink! That’s really amazing! Suppose you write a software. This software requires a total of three directories, namely etc, bin, and man. However, since different people like to install it in different directories, assume that the directory installed by A is /usr/ local/packages/etc, /usr/local/packages/bin and /usr/local/packages/man, but B likes to install in /home/packages/etc, /home/packages/bin, /home/packages/man Among these three directories, would it be troublesome if absolute paths need to be used? Yes! In this way, it will be difficult to correspond to the things in each directory! At this time, the way of writing relative paths is particularly important!**



#### The use of absolute paths

**But for the accuracy of the file name, "the accuracy of the absolute path is better~". Generally speaking, Brother Niao will advise you that if you are writing programs (shell scripts) to manage the system, you must use absolute paths. how to say? Because although the writing method of absolute path is more troublesome, you can be sure that there will be no problem with this writing method. If you use relative paths in the program, some problems may occur due to the different working environment you are executing.**



## 🎓 Directory related operations

```

.            ← 代表此層目錄
..           ← 代表上一層目錄
-            ← 代表前一個工作目錄
~            ← 代表『目前使用者身份』所在的家目錄
~account     ← 代表 account 這個使用者的家目錄(account是個帳號名稱)

```



**Let’s talk about some common instructions for processing directories:**

* **cd：變換目錄**
* **pwd：顯示目前的目錄**
* **mkdir：建立一個新的目錄**
* **rmdir：刪除一個空的目錄**

  
### 🎓 cd (change directory, change directory)

```

[root@localhost ~]$ su -                     ←  先切換身份成為 root 看看！

[root@localhost ~]# cd                           ←  [相對路徑或絕對路徑]
# 最重要的就是目錄的絕對路徑與相對路徑，還有一些特殊目錄的符號囉！

[root@localhost ~]# cd ~dmtsai
# 代表去到 dmtsai 這個使用者的家目錄，亦即 /home/dmtsai

[root@localhost dmtsai]# cd ~
# 表示回到自己的家目錄，亦即是 /root 這個目錄

[root@localhost ~]# cd
# 沒有加上任何路徑，也還是代表回到自己家目錄的意思喔！

[root@localhost ~]# cd ..
# 表示去到目前的上層目錄，亦即是 /root 的上層目錄的意思；

[root@localhost /]# cd -
# 表示回到剛剛的那個目錄，也就是 /root 囉～

[root@localhost ~]# cd /var/spool/mail
# 這個就是絕對路徑的寫法！直接指定要去的完整路徑名稱！

[root@localhost mail]# cd ../postfix
# 這個是相對路徑的寫法，我們由/var/spool/mail 去到/var/spool/postfix 就這樣寫！

```


### 🎓 pwd (displays the current directory)

```

[root@localhost ~]# pwd [-P]

選項與參數：
-P  ：顯示出確實的路徑，而非使用連結 (link) 路徑。

範例：單純顯示出目前的工作目錄：
[root@localhost ~]# pwd
/root   <== 顯示出目錄啦～

範例：顯示出實際的工作目錄，而非連結檔本身的目錄名而已
[root@localhost ~]# cd /var/mail    ← 注意，/var/mail是一個連結檔

[root@localhost mail]# pwd
/var/mail                       ← 列出目前的工作目錄

[root@localhost mail]# pwd -P
/var/spool/mail                 ← 怎麼回事？有沒有加 -P 差很多～

[root@localhost mail]# ls -ld /var/mail
lrwxrwxrwx. 1 root root 10 May  4 17:51 /var/mail -> spool/mail

# 看到這裡應該知道為啥了吧？因為 /var/mail 是連結檔，連結到 /var/spool/mail 
# 所以，加上 pwd -P 的選項後，會不以連結檔的資料顯示，而是顯示正確的完整路徑啊！

```




### 🎓 mkdir (create new directory)

```

[root@localhost ~]# mkdir [-mp] 目錄名稱

選項與參數：
-m ：設定檔案的權限喔！直接設定，不需要看預設權限 (umask) 的臉色～
-p ：幫助你直接將所需要的目錄(包含上層目錄)遞迴建立起來！

範例：請到/tmp底下嘗試建立數個新目錄看看：
[root@localhost ~]# cd /tmp

[root@localhost tmp]# mkdir test             ← 建立一名為 test 的新目錄

[root@localhost tmp]# mkdir test1/test2/test3/test4
mkdir: cannot create directory ‘test1/test2/test3/test4’: No such file or directory

[root@localhost tmp]# mkdir -p test1/test2/test3/test4
# 原來是要建 test4 上層沒先建 test3 之故！加了這個 -p 的選項，可以自行幫你建立多層目錄！

範例：建立權限為rwx--x--x的目錄
[root@localhost tmp]# mkdir -m 711 test2
[root@localhost tmp]# ls -ld test*
drwxr-xr-x. 2 root   root  6 Jun  4 19:03 test
drwxr-xr-x. 3 root   root 18 Jun  4 19:04 test1
drwx--x--x. 2 root   root  6 Jun  4 19:05 test2
# 仔細看上面的權限部分，如果沒有加上 -m 來強制設定屬性，系統會使用預設屬性。

```


### 🎓 rmdir (delete "empty" directories)


```

[root@localhost ~]# rmdir [-p] 目錄名稱

選項與參數：
-p ：連同『上層』『空的』目錄也一起刪除

範例：將於mkdir範例中建立的目錄(/tmp底下)刪除掉！

[root@localhost tmp]# ls -ld test*           ← 看看有多少目錄存在？
drwxr-xr-x. 2 root   root  6 Jun  4 19:03 test
drwxr-xr-x. 3 root   root 18 Jun  4 19:04 test1
drwx--x--x. 2 root   root  6 Jun  4 19:05 test2

[root@localhost tmp]# rmdir test             ← 可直接刪除掉，沒問題

[root@localhost tmp]# rmdir test1            ← 因為尚有內容，所以無法刪除！
rmdir: failed to remove ‘test1’: Directory not empty

[root@localhost tmp]# rmdir -p test1/test2/test3/test4

[root@localhost tmp]# ls -ld test*            ← 您看看，底下的輸出中test與test1不見了！
drwx--x--x. 2 root   root  6 Jun  4 19:05 test2

# 瞧！利用 -p 這個選項，立刻就可以將 test1/test2/test3/test4 一次刪除～
# 不過要注意的是，這個 rmdir 僅能『刪除空的目錄』喔！

```



## 🎓 Variables about the executable file path: $PATH

**After the explanation of FHS in the previous chapter, we know that the full file name of the ls command to check file attributes is: /bin/ls (this is the absolute path). Then will you find it strange: "Why can I execute it anywhere?" What about the /bin/ls command? Why is it that when I enter ls in any directory, some messages will be displayed instead of saying that the /bin/ls command cannot be found? This is due to the help of the environment variable PATH!**

**When we execute a command, for example "ls", the system will search for the executable file named ls in each directory defined by PATH according to the PATH setting. If it is in the directory defined by PATH Contains multiple executable files named ls, then the command with the same name found first will be executed first!**

**Now, please issue "echo $PATH" to see which directories are defined? Echo means "display, print out", and the $ added in front of PATH means that it is followed by variables, so the current PATH will be displayed!**


```

範例：先用root的身份列出搜尋的路徑為何？
[root@localhost ~]# echo $PATH
/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin:/root/bin

範例：用dmtsai的身份列出搜尋的路徑為何？
[root@localhost ~]# exit         ← 由之前的 su - 離開，變回原本的帳號！或再取得一個終端機皆可！

[dmtsai@study ~]$ echo $PATH
/usr/local/bin:/usr/bin:/usr/local/sbin:/usr/sbin:/home/dmtsai/.local/bin:/home/dmtsai/bin
# 記不記得我們前一章說過，目前 /bin 是連結到 /usr/bin 當中的喔！

```


## 🎓 View files and directories: ls

```

[root@localhost ~]# ls [-aAdfFhilnrRSt] 檔名或目錄名稱..
[root@localhost ~]# ls [--color={never,auto,always}] 檔名或目錄名稱..
[root@localhost ~]# ls [--full-time] 檔名或目錄名稱..

選項與參數：
-a  ：全部的檔案，連同隱藏檔( 開頭為 . 的檔案) 一起列出來(常用)
-A  ：全部的檔案，連同隱藏檔，但不包括 . 與 .. 這兩個目錄
-d  ：僅列出目錄本身，而不是列出目錄內的檔案資料(常用)
-f  ：直接列出結果，而不進行排序 (ls 預設會以檔名排序！)
-F  ：根據檔案、目錄等資訊，給予附加資料結構，例如：
      *:代表可執行檔； /:代表目錄； =:代表 socket 檔案； |:代表 FIFO 檔案；
-h  ：將檔案容量以人類較易讀的方式(例如 GB, KB 等等)列出來；
-i  ：列出 inode 號碼，inode 的意義下一章將會介紹；
-l  ：長資料串列出，包含檔案的屬性與權限等等資料；(常用)
-n  ：列出 UID 與 GID 而非使用者與群組的名稱 (UID與GID會在帳號管理提到！)
-r  ：將排序結果反向輸出，例如：原本檔名由小到大，反向則為由大到小；
-R  ：連同子目錄內容一起列出來，等於該目錄下的所有檔案都會顯示出來；
-S  ：以檔案容量大小排序，而不是用檔名排序；
-t  ：依時間排序，而不是用檔名。
--color=never  ：不要依據檔案特性給予顏色顯示；
--color=always ：顯示顏色
--color=auto   ：讓系統自行依據設定來判斷是否給予顏色
--full-time    ：以完整時間模式 (包含年、月、日、時、分) 輸出
--time={atime,ctime} ：輸出 access 時間或改變權限屬性時間 (ctime) 
                       而非內容變更時間 (modification time)

```

**In Linux systems, this ls command may be the most commonly executed! Because we need to know the relevant information of files or directories at any time~ However, our Linux files record too much information, and there is no need to list them all in ls~ Therefore, when you only issue ls, pre- It is assumed that only the file names of non-hidden files, sorting by file name, and the color represented by the file name are displayed. For example, after you issue " ls /etc ", only the file names are sorted, directories are displayed in blue and ordinary files are displayed in white, and that's it.**

**Then if I want to add other display information, I can add the useful options mentioned above~ For example, we have been using the -l long string to display the data content, and also hide the hidden files. Listed -a options, etc. Below are some commonly used examples, try them out in practice:**

```

範例一：將家目錄下的所有檔案列出來(含屬性與隱藏檔)
[root@localhost ~]# ls -al ~
total 56
dr-xr-x---.  5 root root 4096 Jun  4 19:49 .
dr-xr-xr-x. 17 root root 4096 May  4 17:56 ..
-rw-------.  1 root root 1816 May  4 17:57 anaconda-ks.cfg
-rw-------.  1 root root 6798 Jun  4 19:53 .bash_history
-rw-r--r--.  1 root root   18 Dec 29  2013 .bash_logout
-rw-r--r--.  1 root root  176 Dec 29  2013 .bash_profile
-rw-rw-rw-.  1 root root  176 Dec 29  2013 .bashrc
-rw-r--r--.  1 root root  176 Jun  3 00:04 .bashrc_test
drwx------.  4 root root   29 May  6 00:14 .cache
drwxr-xr-x.  3 root root   17 May  6 00:14 .config
# 這個時候你會看到以 . 為開頭的幾個檔案，以及目錄檔 (.) (..) .config 等等，
# 不過，目錄檔檔名都是以深藍色顯示，有點不容易看清楚就是了。

範例二：承上題，不顯示顏色，但在檔名末顯示出該檔名代表的類型(type)
[root@localhost ~]# ls -alF --color=never  ~
total 56
dr-xr-x---.  5 root root 4096 Jun  4 19:49 ./
dr-xr-xr-x. 17 root root 4096 May  4 17:56 ../
-rw-------.  1 root root 1816 May  4 17:57 anaconda-ks.cfg
-rw-------.  1 root root 6798 Jun  4 19:53 .bash_history
-rw-r--r--.  1 root root   18 Dec 29  2013 .bash_logout
-rw-r--r--.  1 root root  176 Dec 29  2013 .bash_profile
-rw-rw-rw-.  1 root root  176 Dec 29  2013 .bashrc
-rw-r--r--.  1 root root  176 Jun  3 00:04 .bashrc_test
drwx------.  4 root root   29 May  6 00:14 .cache/
drwxr-xr-x.  3 root root   17 May  6 00:14 .config/
# 注意看到顯示結果的第一行，嘿嘿～知道為何我們會下達類似 ./command 
# 之類的指令了吧？因為 ./ 代表的是『目前目錄下』的意思啊！至於什麼是 FIFO/Socket ？
# 請參考前一章節的介紹啊！另外，那個.bashrc 時間僅寫2013，能否知道詳細時間？

範例三：完整的呈現檔案的修改時間 (modification time)
[root@localhost ~]# ls -al --full-time  ~
total 56
dr-xr-x---.  5 root root 4096 2015-06-04 19:49:54.520684829 +0800 .
dr-xr-xr-x. 17 root root 4096 2015-05-04 17:56:38.888000000 +0800 ..
-rw-------.  1 root root 1816 2015-05-04 17:57:02.326000000 +0800 anaconda-ks.cfg
-rw-------.  1 root root 6798 2015-06-04 19:53:41.451684829 +0800 .bash_history
-rw-r--r--.  1 root root   18 2013-12-29 10:26:31.000000000 +0800 .bash_logout
-rw-r--r--.  1 root root  176 2013-12-29 10:26:31.000000000 +0800 .bash_profile
-rw-rw-rw-.  1 root root  176 2013-12-29 10:26:31.000000000 +0800 .bashrc
-rw-r--r--.  1 root root  176 2015-06-03 00:04:16.916684829 +0800 .bashrc_test
drwx------.  4 root root   29 2015-05-06 00:14:56.960764950 +0800 .cache
drwxr-xr-x.  3 root root   17 2015-05-06 00:14:56.975764950 +0800 .config
# 請仔細看，上面的『時間』欄位變了喔！變成較為完整的格式。
# 一般來說， ls -al 僅列出目前短格式的時間，有時不會列出年份，
# 藉由 --full-time 可以查閱到比較正確的完整時間格式啊！

```

## 🎓 Copy, Delete and Move: cp, rm, mv


### 🎓 cp (copy file or directory)

```

[root@localhost ~]# cp [-adfilprsu] 來源檔(source) 目標檔(destination)
[root@localhost ~]# cp [options] source1 source2 source3 .... directory
選項與參數：
-a  ：相當於 -dr --preserve=all 的意思，至於 dr 請參考下列說明；(常用)
-d  ：若來源檔為連結檔的屬性(link file)，則複製連結檔屬性而非檔案本身；
-f  ：為強制(force)的意思，若目標檔案已經存在且無法開啟，則移除後再嘗試一次；
-i  ：若目標檔(destination)已經存在時，在覆蓋時會先詢問動作的進行(常用)
-l  ：進行硬式連結(hard link)的連結檔建立，而非複製檔案本身；
-p  ：連同檔案的屬性(權限、用戶、時間)一起複製過去，而非使用預設屬性(備份常用)；
-r  ：遞迴持續複製，用於目錄的複製行為；(常用)
-s  ：複製成為符號連結檔 (symbolic link)，亦即『捷徑』檔案；
-u  ：destination 比 source 舊才更新 destination，或 destination 不存在的情況下才複製。
--preserve=all ：除了 -p 的權限相關參數外，還加入 SELinux 的屬性, links, xattr 等也複製了。
最後需要注意的，如果來源檔有兩個以上，則最後一個目的檔一定要是『目錄』才行！

```

**The copy (cp) command is very important. People with different identities will produce different results when executing this command, especially the -a, -p options. For different identities, the difference is very big! In the following exercises, some identities are root and some are general accounts (I use the account dmtsai here). Please pay special attention to the difference in identities when practicing! good! Let’s start with copying exercises and observations:**


```

範例一：用root身份，將家目錄下的 .bashrc 複製到 /tmp 下，並更名為 bashrc
[root@localhost ~]# cp ~/.bashrc /tmp/bashrc
[root@localhost ~]# cp -i ~/.bashrc /tmp/bashrc
cp: overwrite `/tmp/bashrc'? n           ← n不覆蓋，y為覆蓋

# 重複作兩次動作，由於 /tmp 底下已經存在 bashrc 了，加上 -i 選項後，
# 則在覆蓋前會詢問使用者是否確定！可以按下 n 或者 y 來二次確認呢！

範例二：變換目錄到/tmp，並將/var/log/wtmp複製到/tmp且觀察屬性：
[root@localhost ~]# cd /tmp
[root@localhost tmp]# cp /var/log/wtmp .      ←  想要複製到目前的目錄，最後的 . 不要忘

[root@localhost tmp]# ls -l /var/log/wtmp wtmp
-rw-rw-r--. 1 root utmp 28416 Jun 11 18:56 /var/log/wtmp
-rw-r--r--. 1 root root 28416 Jun 11 19:01 wtmp

# 注意上面的特殊字體，在不加任何選項的情況下，檔案的某些屬性/權限會改變；
# 這是個很重要的特性！要注意喔！還有，連檔案建立的時間也不一樣了！
# 那如果你想要將檔案的所有特性都一起複製過來該怎辦？可以加上 -a 喔！如下所示：

[root@localhost tmp]# cp -a /var/log/wtmp wtmp_2
[root@localhost tmp]# ls -l /var/log/wtmp wtmp_2
-rw-rw-r--. 1 root utmp 28416 Jun 11 18:56 /var/log/wtmp
-rw-rw-r--. 1 root utmp 28416 Jun 11 18:56 wtmp_2
# 瞭了吧！整個資料特性完全一模一樣ㄟ！真是不賴～這就是 -a 的特性！

```

**Because of this feature, when we perform backup, some special permission files that require special attention, such as password files (/etc/shadow) and some configuration files, cannot be copied directly with cp, but must be Add -a or -p and other options that can completely copy the file permissions! In addition, if you want to copy a file to other users, you must also pay attention to the permissions of the file (including read, write, execute, file owner, etc.), otherwise, others will still be unable to modify the file you gave them. action! Attention please!**

```

範例三：複製 /etc/ 這個目錄下的所有內容到 /tmp 底下
[root@localhost tmp]# cp /etc/ /tmp
cp: omitting directory `/etc'               ←  如果是目錄則不能直接複製，要加上 -r 的選項

[root@localhost tmp]# cp -r /etc/ /tmp
# 還是要再次的強調喔！ -r 是可以複製目錄，但是，檔案與目錄的權限可能會被改變
# 所以，也可以利用『 cp -a /etc /tmp 』來下達指令喔！尤其是在備份的情況下！

範例四：將範例一複製的 bashrc 建立一個連結檔 (symbolic link)
[root@localhost tmp]# ls -l bashrc
-rw-r--r--. 1 root root 176 Jun 11 19:01 bashrc         ← 先觀察一下檔案情況

[root@localhost tmp]# cp -s bashrc bashrc_slink
[root@localhost tmp]# cp -l bashrc bashrc_hlink

[root@localhost tmp]# ls -l bashrc*
-rw-r--r--. 2 root root 176 Jun 11 19:01 bashrc          ← 與原始檔案不太一樣了！
-rw-r--r--. 2 root root 176 Jun 11 19:01 bashrc_hlink
lrwxrwxrwx. 1 root root   6 Jun 11 19:06 bashrc_slink -> bashrc

```


### 🎓 rm (remove files or directories)

```

[root@localhost ~]# rm [-fir] 檔案或目錄
選項與參數：
-f  ：就是 force 的意思，忽略不存在的檔案，不會出現警告訊息；
-i  ：互動模式，在刪除前會詢問使用者是否動作
-r  ：遞迴刪除啊！最常用在目錄的刪除了！這是非常危險的選項！！！

範例一：將剛剛在 cp 的範例中建立的 bashrc 刪除掉！
[root@localhost ~]# cd /tmp
[root@localhost tmp]# rm -i bashrc
rm: remove regular file `bashrc'? y
# 如果加上 -i 的選項就會主動詢問喔，避免你刪除到錯誤的檔名！

範例二：透過萬用字元*的幫忙，將/tmp底下開頭為bashrc的檔名通通刪除：
[root@localhost tmp]# rm -i bashrc*
# 注意那個星號，代表的是 0 到無窮多個任意字元喔！很好用的東西！

範例三：將 cp 範例中所建立的 /tmp/etc/ 這個目錄刪除掉！
[root@localhost tmp]# rmdir /tmp/etc
rmdir: failed to remove '/tmp/etc': Directory not empty     ←  刪不掉啊！因為這不是空的目錄！

[root@localhost tmp]# rm -r /tmp/etc
rm: descend into directory `/tmp/etc'? y
rm: remove regular file `/tmp/etc/fstab'? y
rm: remove regular empty file `/tmp/etc/crypttab'? ^C       ←  按下 [ctrl]+c 中斷
.....(中間省略).....
# 因為身份是 root ，預設已經加入了 -i 的選項，所以你要一直按 y 才會刪除！
# 如果不想要繼續按 y ，可以按下『 [ctrl]-c 』來結束 rm 的工作。
# 這是一種保護的動作，如果確定要刪除掉此目錄而不要詢問，可以這樣做：

[root@localhost tmp]# \rm -r /tmp/etc
# 在指令前加上反斜線，可以忽略掉 alias 的指定選項喔！至於 alias 我們在bash再談！
# 拜託！這個範例很可怕！你不要刪錯了！刪除 /etc 系統是會掛掉的！

範例四：刪除一個帶有 - 開頭的檔案
[root@localhost tmp]# touch ./-aaa-                        ← touch這個指令可以建立空檔案！
[root@localhost tmp]# ls -l 
-rw-r--r--. 1 root   root       0 Jun 11 19:22 -aaa-   ← 檔案大小為0，所以是空檔案

[root@localhost tmp]# rm -aaa-
rm: invalid option -- 'a'                              ←  因為 "-" 是選項嘛！所以系統誤判了！
Try 'rm ./-aaa-' to remove the file `-aaa-'.           ←  新的 bash 有給建議的
Try 'rm --help' for more information.
[root@localhost tmp]# rm ./-aaa-

```

**This is the removal command (remove). It should be noted that usually under Linux systems, in order to prevent files from being accidentally killed by root, many distributions have already added the -i option by default! And if you want to kill all the things in the directory together, for example, if there are subdirectories in the subdirectory, then you need to use the -r option! However, please be careful before using the "rm -r" command, because the directory or file will "definitely" be killed by root! Because the system will not ask you again whether you want to cut it off! So that is a very serious order! Pay special attention! However, if you are sure that the directory is no longer needed, then using rm -r to kill it in a loop is a good way!**


**In addition, Example 4 is also a very interesting example. We have talked before that it is best not to start the file name with a "-" because "-" is followed by options. Therefore, simply use "rm -aaa-" The system's instructions will be misjudged! If you use the formal representation that will be discussed later, there will still be problems! Therefore, the only way is to avoid the first character being "-"! Just add this directory "./"! If you use man rm, there is actually another way, that is "rm -- -aaa-" can also be used!**


### 🎓 mv (move files and directories, or rename)

```

[root@localhost ~]# mv [-fiu] source destination

[root@localhost ~]# mv [options] source1 source2 source3 .... directory
選項與參數：
-f  ：force 強制的意思，如果目標檔案已經存在，不會詢問而直接覆蓋；
-i  ：若目標檔案 (destination) 已經存在時，就會詢問是否覆蓋！
-u  ：若目標檔案已經存在，且 source 比較新，才會更新 (update)

範例一：複製一檔案，建立一目錄，將檔案移動到目錄中
[root@localhost ~]# cd /tmp
[root@localhost tmp]# cp ~/.bashrc bashrc
[root@localhost tmp]# mkdir mvtest
[root@localhost tmp]# mv bashrc mvtest
# 將某個檔案移動到某個目錄去，就是這樣做！

範例二：將剛剛的目錄名稱更名為 mvtest2
[root@localhost tmp]# mv mvtest mvtest2      ← 這樣就更名了！簡單～
# 其實在 Linux 底下還有個有趣的指令，名稱為 rename ，
# 該指令專職進行多個檔名的同時更名，並非針對單一檔名變更，與mv不同。請man rename。

範例三：再建立兩個檔案，再全部移動到 /tmp/mvtest2 當中
[root@localhost tmp]# cp ~/.bashrc bashrc1
[root@localhost tmp]# cp ~/.bashrc bashrc2
[root@localhost tmp]# mv bashrc1 bashrc2 mvtest2
# 注意到這邊，如果有多個來源檔案或目錄，則最後一個目標檔一定是『目錄！』
# 意思是說，將所有的資料移動到該目錄的意思！

```



## 🎓 Get the file name and directory name of the path


**The complete file name of each file includes the previous directory and the final file name, and the length of each file name can reach 255 characters! So how do you know that is the file name? Is that the directory name? hey-hey! Just use the slash (/) to tell the difference! In fact, the general purpose of obtaining file names or directory names is for judgment when writing programs~ Therefore, this part of the instructions can be used in the shell scripts in the third article! Let's briefly talk about the uses of basename and dirname with a few examples!**


```

[root@localhost ~]# basename /etc/sysconfig/network
network         ←  很簡單！就取得最後的檔名～

[root@localhost ~]# dirname /etc/sysconfig/network
/etc/sysconfig  ←  取得的變成目錄名了！

```


## 🎓 Document content review

**What if we want to view the contents of a file? There are quite a few interesting commands to share here: Arguably the most commonly used commands for displaying file contents are cat, more and less! Furthermore, what if we want to view a very large archive (hundreds of MB) but we only need a few lines from the backend? hehe! Use the tail. In addition, the tac command can also achieve this purpose! Okay, let’s talk about the purpose of each command!**

* **"cat" displays the file contents starting from the first line**

* **"tac" is displayed starting from the last line. It can be seen that tac is cat written backwards!**

* **"nl"  When nl is displayed, the line number is output!**

* **"more" displays the file content page by page**

* **"less is similar to more", but better than more, it can turn pages forward!**

* **"head" only looks at the first few lines**

* **"tail" only looks at the tail lines**

* **"od" reads the file contents in binary format!**




## 🎓 Default and hidden permissions for files and directories



```

例題：
你的系統有個一般身份使用者 dmtsai，他的群組屬於 dmtsai，他的家目錄在 /home/dmtsai， 你是root，你想將你的 ~/.bashrc 複製給他，可以怎麼作？
答：
由上一章的權限概念我們可以知道 root 雖然可以將這個檔案複製給 dmtsai，不過這個檔案在 dmtsai 的家目錄中卻可能讓 dmtsai 沒有辦法讀寫(因為該檔案屬於 root 的嘛！而 dmtsai 又不能使用 chown 之故)。 此外，我們又擔心覆蓋掉 dmtsai 自己的 .bashrc 設定檔，因此，我們可以進行如下的動作喔：
複製檔案： cp ~/.bashrc ~dmtsai/bashrc
修改屬性： chown dmtsai:dmtsai ~dmtsai/bashrc

```

```

例題：
我想在 /tmp 底下建立一個目錄，這個目錄名稱為 chapter6_1 ，並且這個目錄擁有者為 dmtsai， 群組為 dmtsai，此外，任何人都可以進入該目錄瀏覽檔案，不過除了 dmtsai 之外，其他人都不能修改該目錄下的檔案。
答：
因為除了 dmtsai 之外，其他人不能修改該目錄下的檔案，所以整個目錄的權限應該是 drwxr-xr-x 才對！ 因此你應該這樣做：
建立目錄： mkdir /tmp/chapter6_1
修改屬性： chown -R dmtsai:dmtsai /tmp/chapter6_1
修改權限： chmod -R 755 /tmp/chapter6_1

```


### 🎓 File default permission: umask

**OK! So now we know how to create or change the attributes of a directory or file, but do you know when you create a new file or directory, what are its default permissions? hehe! That has something to do with the umask thing! So what is umask doing? Basically, umask specifies "the current user's default permissions when creating files or directories." So how to know or set umask? Its specification conditions are specified in the following way:**

```

[root@localhost ~]# umask
0022               ← 與一般權限有關的是後面三個數字！

[root@localhost ~]# umask -S
u=rwx,g=rx,o=rx

```

* **If the user creates a "file", the default is "no executable (x) permission", that is, there are only two items rw, which means the maximum is 666 points. The default permissions are as follows:
-rw-rw-rw-**

* **If the user is created as a "directory", since x is related to whether the directory can be entered, the default is that all permissions are open, which is 777 points. The default permissions are as follows: drwxrwxrwx**

* **建立檔案時：(-rw-rw-rw-) - (-----w--w-) ==> -rw-r--r--**
* **建立目錄時：(drwxrwxrwx) - (d----w--w-) ==> drwxr-xr-x**


```

[root@localhost ~]# umask
0022

[root@localhost ~]# touch test1
[root@localhost ~]# mkdir test2
[root@localhost ~]# ll -d test*
-rw-r--r--. 1 root root 0  6月 16 01:11 test1
drwxr-xr-x. 2 root root 6  6月 16 01:11 test2

```


