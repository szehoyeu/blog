---
layout: post
title:  "THM: Linux Fundamentals 02"
date:   2024-02-09 14:00:00 +0000
categories: Linux
image: /blog/assets/img/linux.png
---
Ref: 
- [DisroWatch](https://distrowatch.com/dwres.php?resource=major "Link")
- [THM Linux Fundamentals 02:](https://tryhackme.com/room/linuxfundamentalspart2)

---

Accessing Your Linux Machince Using SSH (Deploy)
---
In fact, the in-browser functionality uses the exact same protocol that we are going to be using today. This protocol is called Secure Shell or SSH for short and is the common means of connecting to and interacting with the command line of a remote Linux machine.

We will be deploying two machines in this room:

- Your Linux machine
- The TryHackMe AttackBox


What is SSH & how Does it Work?
---

Secure Shell or SSH simply is a protocol between devices in an encrypted form. Using cryptography, any input we send in a human-readable format is encrypted for travelling over a network -- where it is then unencrypted once it reaches the remote machine, such as in the diagram below.

![SSH]({{site.baseurl}}/assets/img/ssh.png)

- SSH allows us to remotely execute commands on another device remotely.
- Any data sent between the devices is encrypted when it is sent over a network such as the Internet
Using SSH to Login to Your Linux Machine

---
The syntax to use SSH is very simple. We only need to provide two things:

1. The IP address of the remote machine
2. Correct credentials to a valid account to login with on the remote machine

```
For this room, we will be logging in as "tryhackme", whose password is "tryhackme" without the quotation ("") marks. Let's use the IP address of the machine displayed in the card at the top of the room as the IP address and this user, to construct a command to log in to the remote machine using SSH. The command to do so is ssh and then the username of the account, @ the IP address of the machine.

But first, we need to open a terminal on the TryHackMe AttackBox. There is an icon placed on the desktop named "Terminal". And now, we can proceed to input commands.
```
For example: ```ssh tryhackme@MACHINE_IP``` . Replacing the IP address with the IP address for your Linux target machine. Once executed, we will then be asked to trust the host and then ```provide a password for the "tryhackme" account, which is also "tryhackme"```.

Introduction to Flags and Switches
---
Using our ls example, ls informs us that there is only one folder named "folder1" as highlighted in the screenshot below. Note that the contents in the screenshots below are only examples.
```
 ls
=>folder1
```

However, after using the```-a``` argument (short for ```--all```), we now suddenly have an output with a few more files and folders such as ".hiddenfolder". 
```Files and folders with "." are hidden files.```
```
ls -a 
.hiddenfolder folder1
```
--help option for ls
```
ls --help
---
Usage: ls [OPTION]... [FILE]...
List information about the FILEs (the current directory by default).
Sort entries alphabetically if none of -cftuvSUX nor --sort is specified.

Mandatory arguments to long options are mandatory for short options too.
  -a, --all                  do not ignore entries starting with .
  -A, --almost-all           do not list implied . and ..
      --author               with -l, print the author of each file
  -b, --escape               print C-style escapes for nongraphic characters
      --block-size=SIZE      with -l, scale sizes by SIZE when printing them;
                               e.g., '--block-size=M'; see SIZE format below
  -B, --ignore-backups       do not list implied entries ending with ~
  -c                         with -lt: sort by, and show, ctime (time of last
                               modification of file status information);
                               with -l: show ctime and sort by name;
                               otherwise: sort by ctime, newest first
  -C                         list entries by columns
      --color[=WHEN]         colorize the output; WHEN can be 'always' (default
                               if omitted), 'auto', or 'never'; more info below
  -d, --directory            list directories themselves, not their contents
  -D, --dired                generate output designed for Emacs' dired mode
  -f                         do not sort, enable -aU, disable -ls --color
  -F, --classify             append indicator (one of */=>@|) to entries
      --file-type            likewise, except do not append '*'
      --format=WORD          across -x, commas -m, horizontal -x, long -l,
                               single-column -1, verbose -l, vertical -C
      --full-time            like -l --time-style=full-iso
  -g                         like -l, but do not list owner
      --group-directories-first

```
The Man(ual) Page
```
man ls
LS(1)                                               User Commands                                               LS(1)

NAME
       ls - list directory contents

SYNOPSIS
       ls [OPTION]... [FILE]...

DESCRIPTION
       List  information  about the FILEs (the current directory by default).  Sort entries alphabetically if none of
       -cftuvSUX nor --sort is specified.

       Mandatory arguments to long options are mandatory for short options too.

       -a, --all
              do not ignore entries starting with .

       -A, --almost-all
              do not list implied . and ..

       --author
              with -l, print the author of each file

       -b, --escape
              print C-style escapes for nongraphic characters

       --block-size=SIZE
              with -l, scale sizes by SIZE when printing them; e.g., '--block-size=M'; see SIZE format below

 Manual page ls(1) line 1 (press h for help or q to quit)
 ```
 
What directional arrow key would we use to navigate down the manual page?
```
down
```

What flag would we use to display the output in a "human-readable" way?
```
-h
```


Filesystem Interation Continued
---
We covered some of the most fundamental commands when interacting with the filesystem on the Linux machine. For example, we covered how to list and find the contents of folders using ```ls``` and ```find``` and navigating the filesystem using ```cd```. 

In this task, we're going to learn some more commands for interacting with the filesystem to allow us to:-

- create files and folders
- move files and folders
- delete files and folders

![Command ]({{site.baseurl}}/assets/img/linux-cmd03.png)

