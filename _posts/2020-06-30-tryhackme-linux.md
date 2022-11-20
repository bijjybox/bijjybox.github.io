---
layout: post
title: TryHackMe
subtitle: Learning Linux
gh-repo: bijjybox/beautiful-jekyll
gh-badge: [star, fork, follow]
tags: [test]
comments: true
---


So, a li’l break from OverTheWire wargames, I tried this platform to learn the basics. This one is way more interactive and interesting…

The first requirement is to deploy the machine.

**[Task 6] [Section 2: Running Commands] — Manual Pages and Flags**

1. How would you output hello without a newline
```sh
    echo -n hello
```
**[Task 7] [Section 3: Basic File Operations] — ls**

1. What flag outputs all entries
```sh
-a
```

2. What flag outputs things in a “long list” format
```sh
-l
```

**[Task 8] [Section 3: Basic File Operations] — cat**

1. What flag numbers all output lines?
```sh
-n
```

**[Task 10] [Section 3: Basic File Operations] — Running A Binary**

1. How would you run a binary called hello using the directory shortcut . ?
```sh
./hello
```

2. How would you run a binary called hello in your home directory using the shortcut ~ ?
```sh
~/hello
```

3. How would you run a binary called hello in the previous directory using the shortcut .. ?
```sh
    ../hello
```

**[Task 11] Binary — Shiba1**

1. What’s the password for shiba2
```sh
pinguftw
```

![](https://miro.medium.com/max/640/1*kqJ_v2gfvLlaKGx_kGJ0nQ.webp)

**[Task 12] su**

1. How do you specify which shell is used when you login?
```sh
su -s
```

![](https://miro.medium.com/max/828/1*okLa5sQe9AndoRs5DeJB_Q.webp)

**[Task 13] [Section 4 — Linux Operators]: Intro**
```sh
su shiba2
```

**[Task 14] [Section 4: Linux Operators]: “>”**
```sh
echo twenty > test
```

![](https://miro.medium.com/max/640/1*YiM9kZGIFeEJ0_2WjY1BNg.webp)

**[Task 15] [Section 4: Linux Operators]: “>>”**

![](https://miro.medium.com/max/640/1*6QLro8_HcAR48QpGpUR2ig.webp)

**[Task 16] [Section 4: Linux Operators]: “&&”**

![](https://miro.medium.com/max/640/1*ETOj9kuShwh0JWMlLvnplA.webp)

**[Task 18] [Section 4: Linux Operators]: “$”**

1. How would you set nootnoot equal to 1111?
```sh
export nootnoot=1111
```

![](https://miro.medium.com/max/640/1*vTJqqqWW337ytN24jGJF_w.webp)

2. What is the value of the home environment variable?
```sh
/home/shiba2
```
![](https://miro.medium.com/max/632/1*_sz7sEA7_mqNE5OGiqX_Kg.webp)

**[Task 21] Binary — shiba2**

1. What is shiba3’s password?
```sh
happynootnoises
```

![](https://miro.medium.com/max/640/1*XqdUbYuRorGWMkYIKmb_yA.webp)

**[Task 24] [Section 5: Advanced File Operations]: chmod**

1. What permissions mean the user can read the file, the group can read and write to the file, and no one else can read, write or execute the file?
```sh
460
```
2. What permissions mean the user can read, write, and execute the file, the group can read, write, and execute the file, and everyone else can read, write, and execute the file?
```sh
777
```

**[Task 25] [Section 5: Advanced File Operations] — chown**

1. How would you change the owner of file to paradox
```sh
chown paradox file
```

2. What about the owner and the group of file to paradox
```sh
chown paradox:paradoox file
```

3. What flag allows you to operate on every file in the directory at once?
```sh
-r
```

**[Task 26] [Section 5: Advanced File Operations] — rm**

1. What flag deletes every file in a directory
```sh
-r
```

2. How do you suppress all warning prompts
```sh
-f
```

**[Task 27] [Section 5: Advanced File Operations] — mv**

1. How would you move file to /tmp
```sh
mv file /tmp
```

**[Task 29] [Section 5: Advanced file Operations] — cd && mkdir**

1. Using relative paths, how would you cd to your home directory.
```sh
cd ~
```

2. Using absolute paths how would you make a directory called test in /tmp
```sh
mkdir /tmp/test/
```

**[Task 30] [Section 5: Advanced File Operations] ln**

1. How would I link /home/test/testfile to /tmp/test
```sh
ln /home/test/testfile /tmp/test
```

**[Task 31] [Section 5 — Advanced File Operations]: find**

1. How do you find files that have specific permissions?
```sh
-perm
```

2. How would you find all the files in /home
```sh
find /home
```

3. How would you find all the files owned by paradox on the whole system
```sh
find / -user paradox
```

**[Task 32] [Section 5: Advanced File Operations] — grep**

1. What flag lists line numbers for every string found?
```sh
-n
```

2. How would I search for the string boop in the file aaaa in the directory /tmp
```sh
grep boop /tmp/aaaa
```

**[Task 33] Binary — Shiba3**

1. What is shiba4’s password
```sh
find / -name shiba4 | grep shiba4 | grep shiba4
cd /opt/secret
./shiba4
test1234
```

![](https://miro.medium.com/max/478/1*9JWgRGVjhKciCHVaVbf2kg.webp)

![](https://miro.medium.com/max/640/1*E9QrJS-JLlf-65mVBzXpVQ.webp)

**[Task 35] [Section 6: Miscellaneous]: sudo**

1. How do you specify which user you want to run a command as.
```sh
-u
```

2. How would I run whoami as user jen?
```sh
sudo -u jen whoami
```

3. How do you list your current sudo privileges(what commands you can run, who you can run them as etc.)
```sh
-l
```

**[Task 36] [Section 6: Miscellaneous]: Adding users and groups**

1. How would I add the user test to the group test
```sh
sudo usermod -a -G test test
```

**[Task 43] Bonus Challenge — The True Ending**

Let’s search through each user’s files.
```sh
find / -user <username> -type f 2>>/dev/null
```

![](https://miro.medium.com/max/786/1*5o6xhPX7RaMQTSh_VaSgMA.webp)
```sh
cat /var/log/test1234
```
*Permission Denied!!!*


![](https://miro.medium.com/max/640/1*kcb_TH78r2pA8WZnyRWiRQ.webp)
```sh
ls -l /var/log/test1234
```

It’s belong to shiba2

![](https://miro.medium.com/max/828/1*Fauum5TzRMUGcAtajONBMQ.webp)
```sh
su shiba2
cat /var/log/test1234
```

![](https://miro.medium.com/max/640/1*VzpiZY7TYyo-lrvBTH6AIw.webp)
```sh
su nootnoot
```

Check if nootnoot can run sudo
```sh
sudo -l
```
nootnoot can run sudo command

![](https://miro.medium.com/max/828/1*iAyKzKQ5sUaoIlZnlpR5jw.webp)
```sh
sudo cat /root/root.txt
```

![](https://miro.medium.com/max/828/1*cU-mGwEVX34__LWoic1Maw.webp)

And yey! 
I got my first badge!

![](https://miro.medium.com/max/360/1*J6m7nx2H7vzFynafm1kg6w.webp)
