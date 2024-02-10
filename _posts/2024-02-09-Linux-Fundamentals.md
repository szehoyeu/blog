---
layout: post
title:  "Linux Fundamentals 01"
date:   2024-02-09 14:00:00 +0000
categories: Linux
image: /blog/assets/img/linux.png
---
Ref: 
- [DisroWatch](https://distrowatch.com/dwres.php?resource=major "Link")
- [THM Linux Fundamentals 01:](https://tryhackme.com/room/linuxfundamentalspart1)

---

![Table 01]({{site.baseurl}}/assets/img/linux-cmd01.png)
```
echo "Hello Friend!"
---
```
```
whoami
---
```
```
ls
---
Finding out the contents of any files or folders, we need to know what exists in the first place. This can be done using the "ls" command (short for listing)
```
```
cd 
---
"cd" command (short for change directory) to change to that directory.
```
```
cat
---
Outputting the Contents of a File (cat)
"Cat" is short for concatenating & is a fantastic way for us to output the contents of files (not just text files!).

I.e. 
cat todo.txt
Here's something important for me to do later!

You can use cat to output the contents of a file within directories without having to navigate to it by using cat and the name of the directory. 
I.e. 
cat /home/ubuntu/Documents/todo.txt

```
```
pwd
---
Finding out the full Path to our Current Working Directory (pwd)
(print working directory) command 

i.e.
pwd
/home/ubuntu/Documents


```

On the Linux machine that you deploy, how many folders are there?
```
4
```

Which directory contains a file? 
```
folder4
```
What is the contents of this file?
```
cd folder4
cat mote.txt
=> Hello World!
```
Use the cd command to navigate to this file and find out the new current working directory. What is the path?
```
pwd
/home/tryhackme/folder4
```

Search for Files
---

Using Find
---
A list of directories available
```
ls
Desktop Documents Pictures folder1
```
If we remember the filename, we can simply use ```find -name passwords.txt```
 where the command ```will look through every folder in our current directory``` for that specific file like so:
```
find -name passwords.txt
./folder1/passwords.txt
```
wildcard (*) to search for anything that has .txt at the end
```
find -name *.txt
./folder1/passwords.txt
./Documents/todo.txt
```

Using Grep
---
The grep command allows us to search the contents of files for specific values that we are looking for.
Take for example, the access log of a web server. 
In this case, the access.log of a web server has 244 entries.The grep command allows us to ```search the contents of files for specific values``` that we are looking for.

Take for example, the access log of a web server. 
In this case, the access.log of a web server has 244 entries.

Using a command like cat isn't going to cut it too well here. 
Let's say for example if we wanted to search this log file to see the things that a certain user/IP address visited? 
Looking through 244 entries isn't all that efficient considering we want to find a specific value.

Use grep to search the entire contents of this file for any entries of the value that we are searching for.


```
wc -l access.log

244 access.log
```
```
grep "81.143.211.90" access.log
81.143.211.90 - - [25/Mar/2021:11:17 + 0000] "GET / HTTP/1.1" 200 417 "-" "Mozilla/5.0 (Linux; Android 7.0; Moto G(4))"
```

Use grep on "access.log" to find the flag that has a prefix of "THM". What is the flag?
```
=>THM{ACCESS}

grep "THM" access.log

13.127.130.212 - - [04/May/2021:08:35:26 +0000] "GET THM{ACC
ESS} lang=en HTTP/1.1" 404 360 "-" "Mozilla/5.0 (Windows NT 
10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chr
ome/77.0.3865.120 Safari/537.36"

```

Linux operators
---

![Linux Operators]({{site.baseurl}}/assets/img/linux-cmd02.png)



