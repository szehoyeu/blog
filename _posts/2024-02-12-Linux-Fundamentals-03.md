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

Note, you will need to open a new terminal to use ```wget``` and leave the one that you have started the Python3 web server in. This is because, once you start the Python3 web server, it will run in that terminal until you cancel it.

```
tryhackme@linux3:/tmp# wget http://10.10.84.165:8000/file

2021-05-04 14:26:16  http://127.0.0.1:8000/file
Connecting to http://127.0.0.1:8000... connected.
HTTP request sent, awaiting response... 200 OK
Length: 51095 (50K) [text]
Saving to: ‘file’

file                    100%[=================================================>]  49.90K  --.-KB/s    in 0.04s

2021-05-04 14:26:16 (1.31 MB/s) - ‘file’ saved [51095/51095]
```
![Linux] ({{site.baseurl}}/assets/img/linux-cmd06.png)


One flaw with this module is that you have no way of indexing, so you must know the exact name and location of the file that you wish to use. This is why I prefer to use ```Updog.```


What's Updog? A more advanced yet lightweight webserver. But for now, let's stick to using Python's "HTTP Server".


Now, use Python 3's "HTTPServer" module to start a web server in the home directory of the "tryhackme" user on the deployed instance.
```
python3 -m http.server
```



Download the file http://10.10.84.165:8000/.flag.txt onto the TryHackMe AttackBox. Remember, you will need to do this in a new terminal.
```
wget  http://10.10.84.165:8000/.flag.txt
--2024-02-13 03:29:44--  http://10.10.84.165:8000/.flag.txt
Connecting to 10.10.84.165:8000... connected.
HTTP request sent, awaiting response... 200 OK
Length: 20 [text/plain]
Saving to: \u2018.flag.txt\u2019

.flag.txt           100%[===================>]      20  --.-KB/s    in 0s      

2024-02-13 03:29:44 (3.14 MB/s) - \u2018.flag.txt\u2019 saved [20/20]

```
```
root@ip-10-10-64-22:~# ls -a
.              .flag.txt      .local            .subversion
..             .flag.txt.1    .mozilla          .terraform.d
.aspnet        .flag.txt.2    .msf4             .themes
.bash_aliases  .flag.txt.3    .nuget            thinclient_drives
.bash_history  .gem           Pictures          Tools
.bashrc        .ghidra        .pki              .viminfo
.bundle        .gnupg         Postman           .vnc
.BurpSuite     .gradle        .profile          .wfuzz
.cache         .gvfs          .python_history   .wget-hsts
.config        .hashcat       .recon-ng         .wpscan
CTFBuilder     .ICEauthority  Rooms             .Xauthority
.dbus          .icons         .rpmdb            .xorgxrdp.10.log
Desktop        .install4j     Scripts           .xorgxrdp.10.log.old
.dmrc          Instructions   .selected_editor  .xsession-errors
.dotnet        .java          .set              .xsession-errors.old
Downloads      .john          .ssh              .ZAP

root@ip-10-10-64-22:~# cat .flag.txt
THM{WGET_WEBSERVER}
```


Create and download files to further apply your learning -- see how you can read the documentation on Python3's "HTTPServer" module. 

Use ```Ctrl + C``` to stop the Python3 HTTPServer module once you are finished.


Permission 101
===
Processes are the programs that are running on your machine. They are managed by the kernel, where each process will have an ID associated with it, also known as its PID. The ```PID ```increments for the order In which the process starts. I.e. the 60th process will have a PID of 60.

Viewing Processes
---

We can use the friendly ```ps``` command to provide a ```list of the running processes``` as our user's session and some additional information such as its status code, the session that is running it, how much usage time of the CPU it is using, and the name of the actual program or command that is being executed:

![Linux] ({{site.baseurl}}/assets/img/linux-cmd07.png)

Note how in the screenshot above, the second process ps has a PID of 204, and then in the command below it, this is then incremented to 205.

To see the processes run by other users and those that don't run from a session (i.e. system processes), we need to provide aux to the ```ps``` command like so: ```ps aux```

ps aux
---
![Linux] ({{site.baseurl}}/assets/img/linux-cmd08.png)

Note we can see a total of 5 processes -- note how we now have "root"  and "cmnatic"

top
---
Another very useful command is the ```top ```command; top gives you ```real-time statistics about the processes running on your system instead of a one-time view```. 

These statistics will refresh every 10 seconds, but will also refresh when you use the arrow keys to browse the various rows. Another great command to gain insight into your system is via the ```top``` command

![Linux] ({{site.baseurl}}/assets/img/linux-cmd09.png)


Managing Processes
---
You can send signals that terminate processes; there are a variety of types of signals that correlate to exactly how "cleanly" the process is dealt with by the kernel. To kill a command, we can use the appropriately named kill command and the associated PID that we wish to kill. 

i.e., to kill PID 1337, we'd use ```kill 1337```.

Below are some of the signals that we can send to a process when it is killed:

- SIGTERM - Kill the process, but allow it to do some cleanup tasks beforehand
- SIGKILL - Kill the process - doesn't do any cleanup after the fact
- SIGSTOP - Stop/suspend a process