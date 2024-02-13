---
layout: post
title:  "THM: Linux Fundamentals 03"
date:   2024-02-12 14:00:00 +0000
categories: Linux
image: /blog/assets/img/linux.png
---
Ref: 
- [DisroWatch](https://distrowatch.com/dwres.php?resource=major "Link")
- [THM Linux Fundamentals 03:](https://tryhackme.com/room/linuxfundamentalspart3)

---

Terminal Text Editors
---
Throughout the series so far, we have only stored text in files using a combination of the ```echo``` command and the pipe operators ```(> and >>)```. This isn't an efficient way to handle data when you're working with files with multiple lines and the sorts!



Introducing terminal text editors
---

There are a few options that you can use, all with a variety of friendliness and utility. This task is going to introduce you to ```nano``` but also show you an alternative named ```VIM``` (which TryHackMe has a room dedicated to!)



Nano
---

It is easy to get started with Nano! To create or edit a file using nano, we simply use ```nano filename``` -- replacing "filename" with the name of the file you wish to edit.
```
tryhackme@linux3:/tmp# nano myfile
  GNU nano 4.8                                             myfile                                                       

^G Get Help    ^O Write Out   ^W Where Is    ^K Cut Text    ^J Justify     ^C Cur Pos     M-U Undo       M-A Mark Text
^X Exit        ^R Read File   ^\ Replace     ^U Paste Text  ^T To Spell    ^_ Go To Line  M-E Redo       M-6 Copy Text
```

Once we press enter to execute the command, nano will launch! Where we can just begin to start entering or modifying our text. You can navigate each line using the "up" and "down" arrow keys or start a new line using the "Enter" key on your keyboard.

```
tryhackme@linux3:/tmp# nano myfile
  GNU nano 4.8                                             myfile                                             Modified  

Hello TryHackMe
I can write things into "myfile"


^G Get Help    ^O Write Out   ^W Where Is    ^K Cut Text    ^J Justify     ^C Cur Pos     M-U Undo       M-A Mark Text
^X Exit        ^R Read File   ^\ Replace     ^U Paste Text  ^T To Spell    ^_ Go To Line  M-E Redo       M-6 Copy Text
```
Nano has a few features that are easy to remember & covers the most general things you would want out of a text editor, including:

- Searching for text
- Copying and Pasting
- Jumping to a line number
- Finding out what line number you are on

You can use these features of nano by pressing the ```"Ctrl" key``` (which is represented as an ^ on Linux)  and ```a corresponding letter```. 

For example, to exit, we would want to press ```"Ctrl" and "X" ```to exit Nano.

VIM
---

VIM is a much more advanced text editor. Whilst you're not expected to know all advanced features, it's helpful to mention it for powering up your Linux skills.

Some of VIM's benefits, albeit taking a much longer time to become familiar with, includes:

- Customisable - you can modify the keyboard shortcuts to be of your choosing
- Syntax Highlighting - this is useful if you are writing or maintaining code, making it a popular choice for software developers
- VIM works on all terminals where nano may not be installed
- There are a lot of resources such as cheatsheets, tutorials, and the sorts available to you use.

TryHackMe has a room showcasing VIM if you wish to learn more about this editor!

General/Useful Utilities
===
Downloading Files (Wget)
---

A pretty fundamental feature of computing is the ```ability to transfer files```. For example, you may want to download a program, a script, or even a picture. Thankfully for us, there are multiple ways in which we can retrieve these files.

We're going to cover the use of ```wget```.  This command allows us ```to download files from the web via HTTP``` -- as if you were accessing the file in your browser. We simply need to provide the address of the resource that we wish to download. 

For example, if I wanted to download a file named "myfile.txt" onto my machine, assuming I knew the web address it -- it would look something like this:
```
wget https://assets.tryhackme.com/additional/linux-fundamentals/part3/myfile.txt
```
Transferring Files From Your Host - SCP (SSH)
---

Secure copy, or ```SCP```, is just that -- a means of securely copying files. Unlike the regular cp command, this command ```allows you to transfer files between two computers using the SSH protocol to provide both authentication and encryption.```

Working on a model of SOURCE and DESTINATION, SCP allows you to:

- Copy files & directories from your current system to a remote system
- Copy files & directories from a remote system to your current system

Provided that we know usernames and passwords for a user on your current system and a user on the remote system. 

For example, let's copy an example file from our machine to a remote machine, which I have neatly laid out in the table below:

![Linux] ({{site.baseurl}}/assets/img/linux-cmd04.png)

With this information, let's craft our ```scp ```command (remembering that the format of SCP is just SOURCE and DESTINATION)
```
scp important.txt ubuntu@192.168.1.30:/home/ubuntu/transferred.txt
```

And now let's reverse this and layout the syntax for using ```scp to copy a file from a remote computer``` that we're not logged into 

![Linux] ({{site.baseurl}}/assets/img/linux-cmd05.png)

```
scp ubuntu@192.168.1.30:/home/ubuntu/documents.txt notes.txt 
```

Serving Files From Your Host - WEB
---

Ubuntu machines come pre-packaged with python3. Python helpfully provides a lightweight and easy-to-use module called ```"HTTPServer"```. 

This module turns your computer into a quick and easy web server that you can use to serve your own files, where they can then be downloaded by another computing using commands such as ```curl``` and ```wget```. 

Python3's "HTTPServer" will serve the files in the directory where you run the command, but this can be changed by providing options that can be found within the manual pages. Simply, all we need to do is run ```python3 -m  http.server``` in the terminal to start the module! 

In the snippet below, we are serving from a directory called ```"webserver"```, which has a single named "file".
```
tryhackme@linux3:/webserver# python3 -m http.server
Serving HTTP on 0.0.0.0 port 8000 (http://0.0.0.0:8000/) ...
```
Now, let's use ```wget``` to download the file using the 10.10.84.165 address and the name of the file. 

Remember, because the python3 server is running port 8000, you will need to specify this within your wget command. For example:

```
wget http://10.10.84.165:8000/myfile
```

Note, you will need to open a new terminal to use wget and leave the one that you have started the Python3 web server in. This is because, once you start the Python3 web server, it will run in that terminal until you cancel it.