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

