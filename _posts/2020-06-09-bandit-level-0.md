---
layout: post
title: Bandit Level 0
subtitle: Learning Linux using
gh-repo: bijjybox/beautiful-jekyll
gh-badge: [star, fork, follow]
tags: [test]
comments: true
---


So I started working on my linux skills which I’ve been thinking for a while. And as the quarantine time is increasing, I thought of playing this wargame.

## What is actually OverTheWire Wargames?

The wargames offered by the OverTheWire community can help you to learn and practice security concepts in the form of fun-filled games.
[**Wargames**](https://overthewire.org/wargames/)

## Suggested order to play the games in

* [Bandit](https://overthewire.org/wargames/bandit)
* [Natas](https://overthewire.org/wargames/natas)
* [Leviathan](https://overthewire.org/wargames/leviathan)
* [Krypton](https://overthewire.org/wargames/krypton)
* [Narnia](https://overthewire.org/wargames/narnia)
* [Behemoth](https://overthewire.org/wargames/behemoth)
* [Utumno](https://overthewire.org/wargames/utumno)
* [Maze](https://overthewire.org/wargames/maze)
* [Vortex](https://overthewire.org/wargames/vortex)
* [Semtex](https://overthewire.org/wargames/semtex)
* [Manpage](https://overthewire.org/wargames/manpage)
* [Drifter](https://overthewire.org/wargames/drifter)

I am starting with Bandit today

![OverTheWire](https://miro.medium.com/max/828/0*0aFfTS51R1vUZol9.png)

## Bandit Level 0 → Level 1

{: .box-warning}
**Level Goal:** The goal of this level is for you to log into the game using SSH. The host to which you need to connect is **bandit.labs.overthewire.org**, on port 2220. The username is **bandit0** and the password is **bandit0**. Once logged in, go to the [Level 1](https://overthewire.org/wargames/bandit/bandit1.html) page to find out how to beat Level 1.

```console
apsychogirl@dell~ ssh bandit0@bandit.labs.overthewire.org -p 2220

bandit0@bandit:~$ ls
readme
bandit0@bandit:~$ cat readme
**boJ9jbbUNNfktd78OOpsqOltutMc3MY1**
bandit0@bandit:~$
```

And voilaa!

![](https://cdn-images-1.medium.com/max/2000/1*bB2cXagqoZ6r9pALMaGVuA.png)

## Bandit Level 1 → Level 2

{: .box-warning}
**Level Goal:** The password for the next level is stored in a file called — located in the home directory

```console
apsychogirl@dell~ ssh bandit1@bandit.labs.overthewire.org -p 2220

bandit1@bandit:~$ls
-
bandit1@bandit:~$ cat ./-
**CV1DtqXWVFXTvM2F0k09SHz0YwRINYA9**
bandit1@bandit:~$
```

## Bandit Level 2 → Level 3

{: .box-warning}
**Level Goal:** The password for the next level is stored in a file called **spaces in this filename** located in the home directory

```console
apsychogirl@dell~ ssh bandit2@bandit.labs.overthewire.org -p 2220

bandit2@bandit:~$ls
spaces in this filename
bandit2@bandit:~$ cat spaces\ in\ this\ filename
**UmHadQclWmgdLOKQ3YNgjWxGoRMb5luK**
bandit2@bandit:~$
```

{: .box-note}
Files with spaces in their name can be accessed in this way.

## Bandit Level 3 → Level 4

{: .box-warning}
**Level Goal:** The password for the next level is stored in a hidden file in the **inhere** directory.

```console
apsychogirl@dell~ ssh bandit3@bandit.labs.overthewire.org -p 2220
bandit3@bandit:~$ ls -a
.  ..  .bash_logout  .bashrc  inhere  .profile
bandit3@bandit:~$ cd inhere
bandit3@bandit:~/inhere$ ls -a
.  ..  .hidden
bandit3@bandit:~/inhere$ cat .hidden
**pIwrPrtPN36QITSp3EQaw936yaFoFgAB
**bandit3@bandit:~/inhere$
```

## Bandit Level 4 → Level 5

{: .box-warning}
**Level Goal:** The password for the next level is stored in the only human-readable file in the **inhere** directory. Tip: if your terminal is messed up, try the “reset” command.

```console
apsychogirl@dell~ ssh bandit4@bandit.labs.overthewire.org -p 2220
bandit4@bandit:~$ ls
inhere
bandit4@bandit:~$ cd inhere
bandit4@bandit:~/inhere$ ls
-file00  -file01  -file02  -file03  -file04  -file05  -file06  -file07  -file08  -file09
bandit4@bandit:~/inhere$ cat ./-file07
**koReBOKuIDDepwhWk7jZC0RTdopnAYKh
**bandit4@bandit:~/inhere$
```

## Bandit Level 5 → Level 6

{: .box-warning}
**Level Goal:** The password for the next level is stored in a file somewhere under the **inhere** directory and has all of the following properties:
-human-readable
-1033 bytes in size
-not executable

```console
apsychogirl@dell~ ssh bandit5@bandit.labs.overthewire.org -p 2220
bandit5@bandit:~$ ls -a
.  ..  .bash_logout  .bashrc  inhere  .profile
bandit5@bandit:~$ cd inhere
bandit5@bandit:~/inhere$ ls -la
total 88
drwxr-x--- 22 root bandit5 4096 May  7 20:15 .
drwxr-xr-x  3 root root    4096 May  7 20:15 ..
drwxr-x---  2 root bandit5 4096 May  7 20:15 maybehere00
drwxr-x---  2 root bandit5 4096 May  7 20:15 maybehere01
drwxr-x---  2 root bandit5 4096 May  7 20:15 maybehere02
drwxr-x---  2 root bandit5 4096 May  7 20:15 maybehere03
drwxr-x---  2 root bandit5 4096 May  7 20:15 maybehere04
drwxr-x---  2 root bandit5 4096 May  7 20:15 maybehere05
drwxr-x---  2 root bandit5 4096 May  7 20:15 maybehere06
drwxr-x---  2 root bandit5 4096 May  7 20:15 maybehere07
drwxr-x---  2 root bandit5 4096 May  7 20:15 maybehere08
drwxr-x---  2 root bandit5 4096 May  7 20:15 maybehere09
drwxr-x---  2 root bandit5 4096 May  7 20:15 maybehere10
drwxr-x---  2 root bandit5 4096 May  7 20:15 maybehere11
drwxr-x---  2 root bandit5 4096 May  7 20:15 maybehere12
drwxr-x---  2 root bandit5 4096 May  7 20:15 maybehere13
drwxr-x---  2 root bandit5 4096 May  7 20:15 maybehere14
drwxr-x---  2 root bandit5 4096 May  7 20:15 maybehere15
drwxr-x---  2 root bandit5 4096 May  7 20:15 maybehere16
drwxr-x---  2 root bandit5 4096 May  7 20:15 maybehere17
drwxr-x---  2 root bandit5 4096 May  7 20:15 maybehere18
drwxr-x---  2 root bandit5 4096 May  7 20:15 maybehere19
bandit5@bandit:~/inhere$ **find . -type f -readable -size 1033c ! -executable**
./maybehere07/.file2
bandit5@bandit:~/inhere$ cat ./maybehere07/.file2
**DXjZPULLxYr17uwoI01bNLQbtFemEgo7
**bandit5@bandit:~
```

{: .box-note}
This means find in the current directory which is of type file, readable, of size 1033 bytes and not executable.

## Bandit Level 6 → Level 7

{: .box-warning}
**Level Goal:** The password for the next level is stored **somewhere on the server** and has all of the following properties:
-owned by user bandit7
-owned by group bandit6
-33 bytes in size

```console
apsychogirl@dell~ ssh bandit6@bandit.labs.overthewire.org -p 2220
bandit6@bandit:~$ **find / -user bandit7 -group bandit6 -size 33c 2>/dev/null**
/var/lib/dpkg/info/bandit7.password
bandit6@bandit:~$cat /var/lib/dpkg/info/bandit7.password
**HKBPTKQnIay4Fw76bEy8PVxKEDQRKTzs
**bandit6@bandit:~$
```

{: .box-note}
This means find a file which belongs to user=bandit7, group=bandit6, of file size 33 bytes and **2>/dev/null** means ignore error output from the command, i.e, if there is any error which is generated due to the command it would print messages on stderr. By redirecting stderr to /dev/null, you effectively suppress these messages.
> The N> syntax in Bash means to redirect a file descriptor to somewhere else. 2 is the file descriptor of stderr, and this example redirects it to /dev/null.

## Bandit Level 7 → Level 8

{: .box-warning}
**Level Goal:** The password for the next level is stored in the file **data.txt** next to the word **millionth**

```console
apsychogirl@dell~ ssh bandit7@bandit.labs.overthewire.org -p 2220
bandit7@bandit:~$ ls
data.txt
bandit7@bandit:~$ grep -w millionth data.txt
**millionth cvX2JJa4CFALtqS87jk27qwqGhBM9plV**
```

{: .box-note}
This means find the lines which contain the word “millionth” in it.

## Bandit Level 8 → Level 9

{: .box-warning}
**Level Goal:?** The password for the next level is stored in the file **data.txt** and is the only line of text that occurs only once

```console
apsychogirl@dell~ ssh bandit8@bandit.labs.overthewire.org -p 2220
bandit8@bandit:~$ ls
data.txt
bandit8@bandit:~$ cat data.txt | sort | uniq -u
**UsvVyFSfZZWbi6wgC7dAFyFuR6jQQUhR**
bandit8@bandit:~$
```

{: .box-note}
The sort command sorts text files and stdin. uniq is often used with sort to extract unique lines.

## Bandit Level 9 → Level 10

{: .box-warning}
**Level Goal:** The password for the next level is stored in the file **data.txt** in one of the few human-readable strings, preceded by several ‘=’ characters.

```console
apsychogirl@dell~ ssh bandit9@bandit.labs.overthewire.org -p 2220
bandit9@bandit:~$ ls
data.txt
bandit9@bandit:~$ **strings data.txt | grep =**
========== the*2i"4
=:G e
========== password
<I=zsGi
Z)========== is
A=|t&E
Zdb=
c^ LAh=3G
*SF=s
&========== **truKLdjsbJ5g7yyJ2X2R0o3a5HQJFuLk**
S=A.H&^
bandit9@bandit:~$
```

{: .box-note}
The *strings* [command](http://www.linfo.org/command.html) returns each [string](http://www.linfo.org/string.html)* of *printable characters* in files. Its main uses are to determine the contents of and to extract text from [binary](http://www.linfo.org/binary.html) files (i.e., non-text files).

## Bandit Level 10 → Level 11

{: .box-warning}
**Level Goal:** The password for the next level is stored in the file **data.txt**, which contains base64 encoded data

```console
apsychogirl@dell~ ssh bandit10@bandit.labs.overthewire.org -p 2220
bandit10@bandit:~$ ls
data.txt
bandit10@bandit:~$ cat data.txt 
VGhlIHBhc3N3b3JkIGlzIElGdWt3S0dzRlc4TU9xM0lSRnFyeEUxaHhUTkViVVBSCg==
bandit10@bandit:~$ **cat data.txt | base64 --decode**

**The password is IFukwKGsFW8MOq3IRFqrxE1hxTNEbUPR**
bandit10@bandit:~$
```

## Bandit Level 11 → Level 12

{: .box-warning}
**Level Goal** The password for the next level is stored in the file **data.txt**, where all lowercase (a-z) and uppercase (A-Z) letters have been rotated by 13 positions

```console
apsychogirl@dell~ ssh bandit11@bandit.labs.overthewire.org -p 2220
bandit11@bandit:~$ ls
data.txt
bandit11@bandit:~$ cat data.txt
Gur cnffjbeq vf 5Gr8L4qetPEsPk8htqjhRK8XSP6x2RHh
bandit11@bandit:~$ **cat data.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m'**

**The password is 5Te8Y4drgCRfCx8ugdwuEX8KFC6k2EUu**
bandit11@bandit:~$
```

**ROT13** (“**rotate by 13 places**”, sometimes hyphenated **ROT-13**) is a simple letter [substitution cipher](https://en.wikipedia.org/wiki/Substitution_cipher) that replaces a letter with the 13th letter after it, in the alphabet. ROT13 is a special case of the [Caesar cipher](https://en.wikipedia.org/wiki/Caesar_cipher) which was developed in ancient Rome.

Because there are 26 letters (2×13) in the [basic Latin alphabet](https://en.wikipedia.org/wiki/ISO_basic_Latin_alphabet), ROT13 is its own [inverse](https://en.wikipedia.org/wiki/Inverse_function); that is, to undo ROT13, the same [algorithm](https://en.wikipedia.org/wiki/Algorithm) is applied, so the same action can be used for encoding and decoding. The algorithm provides virtually no [cryptographic](https://en.wikipedia.org/wiki/Cryptography) security, and is often cited as a canonical example of weak encryption.

(Some wikipedia gyaan :P)

TBC in next blog….

![gif](https://giphy.com/gifs/i-give-up-Zsc4dATQgcBmU?utm_source=iframe&utm_medium=embed&utm_campaign=Embeds&utm_term=https%3A%2F%2Fcdn.embedly.com%2F)
