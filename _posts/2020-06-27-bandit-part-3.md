---
layout: post
title:  Bandit (Part 3)
subtitle: Learning Linux using OTW
gh-repo: bijjybox/beautiful-jekyll
gh-badge: [star, fork, follow]
tags: [test]
comments: true
---


This blog covers the last section of Bandit — OverTheWire wargame.

For Bandit (Part 1) -> [Click me!](https://medium.com/@apsychogirl/bandit-level-0-4e87f36a37e6)

For Bandit (Part 2) -> [Click me!](https://medium.com/@apsychogirl/bandit-part-2-c77dc29d77a8)

![](https://cdn-images-1.medium.com/max/2000/0*Fgq5jxdzeM9TuiYa.png)

## Bandit Level 25 → Level 26

{: .box-warning}
**Level Goal:** Logging in to bandit26 from bandit25 should be fairly easy… The shell for user bandit26 is not **/bin/bash**, but something else. Find out what it is, how it works and how to break out of it.
```sh
apsychogirl@dell~ ssh bandit25@bandit.labs.overthewire.org -p 2220
```
First we’ll *ssh* into the server and have a look in the home directory:
```sh
bandit25@bandit:~$ ls
bandit26.sshkey
bandit25@bandit:~$
```
So there’s a key here, this must be what they’re referring to when they say that connecting to bandit26 “should be” easy. Let’s try* ssh*’ing into bandit26 and see what happens.
```sh
bandit25@bandit:~$ ssh bandit26@localhost -i bandit26.sshkey
```
![](https://miro.medium.com/max/828/1*hHwm6EpkIFXLzlQpNYhpWw.png)

Well it kicks us straight back out. The challenge mentions that the shell for bandit26 isn’t *bash*, so let’s see what it has instead.
```sh
bandit25@bandit:~$ cat /etc/passwd | grep bandit26
bandit26:x:11026:11026:bandit level 26:/home/bandit26:/usr/bin/showtext
bandit25@bandit:~$
```
*/usr/bin/showtext*! We’ll have a look inside it (it’s a *bash* script so I’m just going to use *cat*).
```sh
bandit25@bandit:~$ cat /usr/bin/showtext
#!/bin/sh
export TERM=linux

more ~/text.txt
exit 0
bandit25@bandit:~$
```
At this point I did “OK Google! Tell me how to solve level 25 of Bandit plis ”

What we need to do is to trigger *more *to go into its command view so that the program doesn’t just exit. In other words, make your terminal as small as possible then *ssh* in.

![](https://miro.medium.com/max/828/1*ro0jvaSB7vb4HWvrD5c4sw.png)

Yep, that’s the way to break out. Very out of the box to get out of the box.

So now we’ve got *more* working, what can we do with it? Well in *more *you have a few commands, one of them letting us open the file in vim. All we have to do is press *v* on the keyboard. (If you want more information check the *man* page)

![](https://miro.medium.com/max/828/1*fMbHpRixms3s6C0s877hqQ.png)

I now have vim running on the file. I’ve also rescWell in *more *you have a few commands, one of them lettingaled my windows so that I can actually see.

So now we have vim, we can open another file using the *:e* command. We will want the bandit26 password so this is the command we will use:
```sh
:e /etc/bandit_pass/bandit26
```
If you’re unfamiliar with vim, make sure you press escape to enter command mode (it’s in there automatically but if you pressed any other keys you may need to change back to it).

![](https://miro.medium.com/max/828/1*zG7NzzvPdn-44VMVqljQLQ.png)

Ok so the password is ***5czgV9L3Xx8JPOyRbXh6lQbmIOWvPT6Z***, but we haven’t got a shell and *ssh*’ing in with it will still just leave us in that *showtext* script from earlier. So how can we get to a shell from vim? Well from looking up vim in Google, vim has a shell command. So if we type ***:shell*** then it should return us into a shell.

![](https://miro.medium.com/max/828/1*gIfSh14-qnL30C9jPU8v8w.png)

![](https://miro.medium.com/max/828/1*N0XxFFNNMPsQQTPNSuFwRg.png)

What? It just put us back into *more*. Pretty much, vim knows that the shell for bandit26 is the *showtext *file and it stores this in a variable called *shell*. So if we want to break out we need to change this variable first. So we get back into vim and use the following command to set that value
```sh
:set shell=/bin/bash
```
After that we can now tell vim to start a shell with *:shell* and…

![](https://miro.medium.com/max/828/1*kUJu5QCTa7DD6TsBdOKBLQ.png)

## Bandit Level 26 → Level 27

{: .box-warning}
**Level Goal:** Good job getting a shell! Now hurry and grab the password for bandit27!
```sh
bandit26@bandit:~$ ls
bandit27-do  text.txt
bandit26@bandit:~$ ./bandit27-do 
Run a command as another user.
    Example: ./bandit27-do id
bandit26@bandit:~$ ./bandit27-do cat /etc/bandit_pass/bandit27
3ba3118a22e93127a4ed485be72ef5ea
bandit26@bandit:~$
```
## Bandit Level 27 → Level 28

{: .box-warning}
**Level Goal:** There is a git repository at ssh://bandit27-git@localhost/home/bandit27-git/repo. The password for the user bandit27-git is the same as for the user bandit27. Clone the repository and find the password for the next level.
```sh
bandit27@bandit:~$ cd /tmp/megha
bandit27@bandit:/tmp/megha$ git clone ssh://bandit27-git@localhost/home/bandit27-git/repo
Cloning into 'repo'...
Could not create directory '/home/bandit27/.ssh'.
The authenticity of host 'localhost (127.0.0.1)' can't be established.
ECDSA key fingerprint is SHA256:98UL0ZWr85496EtCRkKlo20X3OPnyPSB5tB5RPbhczc.
Are you sure you want to continue connecting (yes/no)? yes
Failed to add the host to the list of known hosts (/home/bandit27/.ssh/known_hosts).
This is a OverTheWire game server. More information on [http://www.overthewire.org/wargames](http://www.overthewire.org/wargames)

bandit27-git@localhost's password: 
remote: Counting objects: 3, done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 0), reused 0 (delta 0)
Receiving objects: 100% (3/3), done.
bandit27@bandit:/tmp/megha$ ls
repo
bandit27@bandit:/tmp/megha$ cd repo/
bandit27@bandit:/tmp/megha/repo$ ls
README
bandit27@bandit:/tmp/megha/repo$ cat README 
The password to the next level is: 0ef186ac70e04ea33b4c1853d2526fa2
bandit27@bandit:/tmp/megha/repo$
```
## Bandit Level 28 → Level 29

{: .box-warning}
**Level Goal:** There is a git repository at ssh://bandit28-git@localhost/home/bandit28-git/repo. The password for the user bandit28-git is the same as for the user bandit28. Clone the repository and find the password for the next level.
```sh
bandit28@bandit:~$ mkdir -p /tmp/megga
bandit28@bandit:~$ cd /tmp/megga
bandit28@bandit:/tmp/megga$ ls
bandit28@bandit:/tmp/megga$ git clone ssh://bandit28-git@localhost/home/bandit28-git/repo
Cloning into 'repo'...
Could not create directory '/home/bandit28/.ssh'.
The authenticity of host 'localhost (127.0.0.1)' can't be established.
ECDSA key fingerprint is SHA256:98UL0ZWr85496EtCRkKlo20X3OPnyPSB5tB5RPbhczc.
Are you sure you want to continue connecting (yes/no)? yes
Failed to add the host to the list of known hosts (/home/bandit28/.ssh/known_hosts).
This is a OverTheWire game server. More information on [http://www.overthewire.org/wargames](http://www.overthewire.org/wargames)

bandit28-git@localhost's password: 
remote: Counting objects: 9, done.
remote: Compressing objects: 100% (6/6), done.
remote: Total 9 (delta 2), reused 0 (delta 0)
Receiving objects: 100% (9/9), done.
Resolving deltas: 100% (2/2), done.
bandit28@bandit:/tmp/megga$ ls
repo
bandit28@bandit:/tmp/megga$ cd repo/
bandit28@bandit:/tmp/megga/repo$ ls
README.md
bandit28@bandit:/tmp/megga/repo$ cat README.md 
# Bandit Notes
Some notes for level29 of bandit.

## credentials

- username: bandit29
- password: xxxxxxxxxx
```

Password is hidden. Let’s check the log history for this git repo
```sh
bandit28@bandit:/tmp/megga/repo$ git log
commit edd935d60906b33f0619605abd1689808ccdd5ee
Author: Morla Porla <[morla@overthewire.org](mailto:morla@overthewire.org)>
Date:   Thu May 7 20:14:49 2020 +0200

fix info leak

commit c086d11a00c0648d095d04c089786efef5e01264
Author: Morla Porla <[morla@overthewire.org](mailto:morla@overthewire.org)>
Date:   Thu May 7 20:14:49 2020 +0200

add missing data

commit de2ebe2d5fd1598cd547f4d56247e053be3fdc38
Author: Ben Dover <[noone@overthewire.org](mailto:noone@overthewire.org)>
Date:   Thu May 7 20:14:49 2020 +0200

initial commit of README.
```

So, it seems like password was changed in the last commit. I am comparing the first two commits to check if I can get the actual pass.

```sh
bandit28@bandit:/tmp/megga/repo$ git diff edd935 c086d11a
diff --git a/README.md b/README.md
index 5c6457b..3f7cee8 100644
--- a/README.md
+++ b/README.md
@@ -4,5 +4,5 @@ Some notes for level29 of bandit.
 ## credentials
 
 - username: bandit29
-- password: xxxxxxxxxx
+- password: bbc96594b4e001778eee9975372716b2
 
bandit28@bandit:/tmp/megga/repo$
```
## Bandit Level 29 → Level 30

{: .box-warning}
**Level Goal:** There is a git repository at ssh://bandit29-git@localhost/home/bandit29-git/repo. The password for the user bandit29-git is the same as for the user bandit29. Clone the repository and find the password for the next level.

```sh
bandit29@bandit:~$ mkdir -p /tmp/dibbs
bandit29@bandit:~$ cd /tmp/dibbs
bandit29@bandit:/tmp/dibbs$ git clone ssh://bandit29-git@localhost/home/bandit29-git/repo
Cloning into 'repo'...
Could not create directory '/home/bandit29/.ssh'.
The authenticity of host 'localhost (127.0.0.1)' can't be established.
ECDSA key fingerprint is SHA256:98UL0ZWr85496EtCRkKlo20X3OPnyPSB5tB5RPbhczc.
Are you sure you want to continue connecting (yes/no)? yes
Failed to add the host to the list of known hosts (/home/bandit29/.ssh/known_hosts).
This is a OverTheWire game server. More information on [http://www.overthewire.org/wargames](http://www.overthewire.org/wargames)

bandit29-git@localhost's password: 
remote: Counting objects: 16, done.
remote: Compressing objects: 100% (11/11), done.
remote: Total 16 (delta 2), reused 0 (delta 0)
Receiving objects: 100% (16/16), done.
Resolving deltas: 100% (2/2), done.
bandit29@bandit:/tmp/dibbs$ ls
repo
bandit29@bandit:/tmp/dibbs$ cd repo
bandit29@bandit:/tmp/dibbs/repo$ ls
README.md
bandit29@bandit:/tmp/dibbs/repo$ cat README.md 
# Bandit Notes
Some notes for bandit30 of bandit.

## credentials

- username: bandit30
- password: <no passwords in production!>
```
Can’t get the pass here. Let’s check using git log

```sh
bandit29@bandit:/tmp/dibbs/repo$ git log
commit 208f463b5b3992906eabf23c562eda3277fea912
Author: Ben Dover <[noone@overthewire.org](mailto:noone@overthewire.org)>
Date:   Thu May 7 20:14:51 2020 +0200

fix username

commit 18a6fd6d5ef7f0874bbdda2fa0d77b3b81fd63f7
Author: Ben Dover <[noone@overthewire.org](mailto:noone@overthewire.org)>
Date:   Thu May 7 20:14:51 2020 +0200

initial commit of README.md
```

The message shows us “no passwords in production”. Therefore we have to see whether there are different branches of this repository.
```sh
bandit29@bandit:/tmp/dibbs/repo$ git branch -a
* master
  remotes/origin/HEAD -> origin/master
  remotes/origin/dev
  remotes/origin/master
  remotes/origin/sploits-dev
bandit29@bandit:/tmp/dibbs/repo$ git checkout dev 
Branch dev set up to track remote branch dev from origin.
Switched to a new branch 'dev'
bandit29@bandit:/tmp/dibbs/repo$ ls
code  README.md
bandit29@bandit:/tmp/dibbs/repo$ cat README.md 
# Bandit Notes
Some notes for bandit30 of bandit.

## credentials

- username: bandit30
- password: 5b90576bedb2cc04c86a9e924ce42faf
```
## Bandit Level 30 → Level 31

{: .box-warning}
**Level Goal:** There is a git repository at ssh://bandit30-git@localhost/home/bandit30-git/repo. The password for the user bandit30-git is the same as for the user bandit30. Clone the repository and find the password for the next level.

```sh
bandit30@bandit:~$ mkdir -p /tmp/meggaa
bandit30@bandit:~$ cd /tmp/meggaa
bandit30@bandit:/tmp/meggaa$ git clone ssh://bandit30-git@localhost/home/bandit30-git/repo
Cloning into 'repo'...
Could not create directory '/home/bandit30/.ssh'.
The authenticity of host 'localhost (127.0.0.1)' can't be established.
ECDSA key fingerprint is SHA256:98UL0ZWr85496EtCRkKlo20X3OPnyPSB5tB5RPbhczc.
Are you sure you want to continue connecting (yes/no)? yes
Failed to add the host to the list of known hosts (/home/bandit30/.ssh/known_hosts).
This is a OverTheWire game server. More information on [http://www.overthewire.org/wargames](http://www.overthewire.org/wargames)

bandit30-git@localhost's password: 
remote: Counting objects: 4, done.
remote: Total 4 (delta 0), reused 0 (delta 0)
Receiving objects: 100% (4/4), done.
bandit30@bandit:/tmp/meggaa$ ls
repo
bandit30@bandit:/tmp/meggaa$ cd repo/
bandit30@bandit:/tmp/meggaa/repo$ ls
README.md
bandit30@bandit:/tmp/meggaa/repo$ cat README.md 
just an epmty file... muahaha
```

Tried looking for git log and branches
```sh
bandit30@bandit:/tmp/meggaa/repo$ git log -p
commit 3aefa229469b7ba1cc08203e5d8fa299354c496b
Author: Ben Dover <[noone@overthewire.org](mailto:noone@overthewire.org)>
Date:   Thu May 7 20:14:54 2020 +0200

initial commit of README.md

diff --git a/README.md b/README.md
new file mode 100644
index 0000000..029ba42
--- /dev/null
+++ b/README.md
@@ -0,0 +1 @@
+just an epmty file... muahaha
bandit30@bandit:/tmp/meggaa/repo$ git branch -a
* master
  remotes/origin/HEAD -> origin/master
  remotes/origin/master
```
How about git tagging?
```sh
bandit30@bandit:/tmp/meggaa/repo$ git tag
secret
bandit30@bandit:/tmp/meggaa/repo$ git show secret 
47e603bb428404d265f59c42920d81e5
bandit30@bandit:/tmp/meggaa/repo$
```
## Bandit Level 31 → Level 32

{: .box-warning}
**Level Goal:** There is a git repository at ssh://bandit31-git@localhost/home/bandit31-git/repo. The password for the user bandit31-git is the same as for the user bandit31. Clone the repository and find the password for the next level.
```sh
bandit31@bandit:/tmp/megha$ mkdir -p /tmp/bits
bandit31@bandit:/tmp/megha$ cd /tmp/bits
bandit31@bandit:/tmp/bits$ ls
bandit31@bandit:/tmp/bits$ git clone ssh://bandit31-git@localhost/home/bandit31-git/repo
Cloning into 'repo'...
Could not create directory '/home/bandit31/.ssh'.
The authenticity of host 'localhost (127.0.0.1)' can't be established.
ECDSA key fingerprint is SHA256:98UL0ZWr85496EtCRkKlo20X3OPnyPSB5tB5RPbhczc.
Are you sure you want to continue connecting (yes/no)? yes
Failed to add the host to the list of known hosts (/home/bandit31/.ssh/known_hosts).
This is a OverTheWire game server. More information on [http://www.overthewire.org/wargames](http://www.overthewire.org/wargames)

bandit31-git@localhost's password: 
remote: Counting objects: 4, done.
remote: Compressing objects: 100% (3/3), done.
remote: Total 4 (delta 0), reused 0 (delta 0)
Receiving objects: 100% (4/4), done.
bandit31@bandit:/tmp/bits$ ls
repo
bandit31@bandit:/tmp/bits$ cd repo/
bandit31@bandit:/tmp/bits/repo$ ls
README.md
bandit31@bandit:/tmp/bits/repo$ cat README.md 
This time your task is to push a file to the remote repository.

Details:
    File name: key.txt
    Content: 'May I come in?'
    Branch: master
```

So, we need to push the details first.
```sh
bandit31@bandit:/tmp/bits/repo$ touch key.txt
bandit31@bandit:/tmp/bits/repo$ nano key.txt 
Unable to create directory /home/bandit31/.nano: Permission denied
It is required for saving/loading search history or cursor positions.

Press Enter to continue

bandit31@bandit:/tmp/bits/repo$ ls
key.txt  README.md
bandit31@bandit:/tmp/bits/repo$ cat key.txt 
May I come in?

bandit31@bandit:/tmp/bits/repo$ git add key.txt 
The following paths are ignored by one of your .gitignore files:
key.txt
Use -f if you really want to add them.
bandit31@bandit:/tmp/bits/repo$ git add -f key.txt 
bandit31@bandit:/tmp/bits/repo$ git commit -m "done"
[master 5137958] done
 1 file changed, 1 insertion(+)
 create mode 100644 key.txt
bandit31@bandit:/tmp/bits/repo$ git push origin master
Could not create directory '/home/bandit31/.ssh'.
The authenticity of host 'localhost (127.0.0.1)' can't be established.
ECDSA key fingerprint is SHA256:98UL0ZWr85496EtCRkKlo20X3OPnyPSB5tB5RPbhczc.
Are you sure you want to continue connecting (yes/no)? yes
Failed to add the host to the list of known hosts (/home/bandit31/.ssh/known_hosts).
This is a OverTheWire game server. More information on [http://www.overthewire.org/wargames](http://www.overthewire.org/wargames)

bandit31-git@localhost's password: 
Counting objects: 3, done.
Delta compression using up to 2 threads.
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 315 bytes | 0 bytes/s, done.
Total 3 (delta 0), reused 0 (delta 0)
remote: ### Attempting to validate files... ####
remote: 
remote: .oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.
remote: 
remote: Well done! Here is the password for the next level:
remote: 56a9bf19c63d650ce78e6ec0354ee45e
remote: 
remote: .oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.
remote: 
To ssh://localhost/home/bandit31-git/repo
 ! [remote rejected] master -> master (pre-receive hook declined)
error: failed to push some refs to 'ssh://bandit31-git@localhost/home/bandit31-git/repo'
```
## Bandit Level 32 → Level 33

After all this git stuff its time for another escape. Good luck!
```sh
WELCOME TO THE UPPERCASE SHELL
>> ls
sh: 1: LS: not found
>> LS
sh: 1: LS: not found
```
The shell converts every command into uppercase. We need to fix it and gain the normal shell again. Since this is an interactive shell, we have the chance to execute it again using the variable $0 .
```sh
>> $0$ pwd
/home/bandit32
$ ls -al *
-rwsr-x--- 1 bandit33 bandit32 7556 Oct 16 14:00 uppershell
$ cat /etc/bandit_pass/bandit33
c9c3199ddf4121b10cf581a98d51caee
```
And hurray! completed this one 😅
