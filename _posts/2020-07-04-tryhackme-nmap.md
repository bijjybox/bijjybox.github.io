---
layout: post
title:  TryHackMe
subtitle: Nmap
gh-repo: bijjybox/beautiful-jekyll
gh-badge: [star, fork, follow]
tags: [test]
comments: true
---
![](https://miro.medium.com/max/550/0*3DNtm2rGgLAazMsa)
![](https://miro.medium.com/max/786/1*lW3zJ2JsG3YHoXRgTfbxag.png)

## [Task 1]

This section goes over a quick explanation of nmap in the world of pentesting.
{: .box-warning}
*ANS 1:* There is nothing needed to be done. Press complete and away we go!

## [Task 2] Nmap Quiz

A short quiz on the more useful switches that we can use with Nmap.
{: .box-note}
*Question 1:* First, how do you access the help menu?

Mostly help flag is represented by -h.
{: .box-warning}
*ANS 1:* -h
{: .box-note}
*Question 2:*Often referred to as a stealth scan, what is the first switch listed for a ‘Syn Scan’?

![](https://miro.medium.com/max/828/1*9Ul0lgfAU9PTH_PqT0WMqQ.png)
{: .box-warning}
*ANS 2:* -sS
{: .box-note}
*Question 3:* Not quite as useful but how about a ‘UDP Scan’?

![](https://miro.medium.com/max/828/1*B-1XVj3Iv0Yin85panrCtA.png)
{: .box-warning}
*ANS 3:* -sU
{: .box-note}
*Question 4:* What about operating system detection?

With the help of hints we can analyse it’s a one word flag.

![](https://miro.medium.com/max/828/1*IZkcX4Epsyd4l4HhaIO0qA.png)
{: .box-warning}
*ANS 4:* -O
{: .box-note}
*Question 5:* How about service version detection?

![](https://miro.medium.com/max/828/1*mTmOYEAhqIM5SwjW1wJfVQ.png)
{: .box-warning}
*ANS 5:* -sV
{: .box-note}
*Question 6:* Most people like to see some output to know that their scan is actually doing things, what is the verbosity flag?

![](https://miro.medium.com/max/828/1*_uIxD2zDllsQsy7m7mypCA.png)
{: .box-warning}
*ANS 6:* -v
{: .box-note}
*Question 7:* What about ‘very verbose’? (A personal favorite)

![](https://miro.medium.com/max/640/1*kHl4U-40jFmA1tBul9Gibg.png)
{: .box-warning}
*ANS 7:* -vv
{: .box-note}
*Question 8:* Sometimes saving output in a common document format can be really handy for reporting, how do we save output in xml format?

XML, the *extensible markup language*, has its share of critics as well as plenty of zealous proponents.
{: .box-warning}
*ANS 8:* -oX
{: .box-note}
*Question 9:* Aggressive scans can be nice when other scans just aren’t getting the output that you want and you really don’t care how ‘loud’ you are, what is the switch for enabling this?

![](https://miro.medium.com/max/828/1*3QGCB-TVpvNEnTY63_L59w.png)
{: .box-warning}
*ANS 9:* -A
{: .box-note}
*Question 10:* How do I set the timing to the max level, sometimes called 'Insane'?

```sh
-T paranoid|sneaky|polite|normal|aggressive|insane
```

While the fine-grained timing controls discussed in the previous section are powerful and effective, some people find them confusing. Moreover, choosing the appropriate values can sometimes take more time than the scan you are trying to optimize.

![](https://miro.medium.com/max/828/1*STQOB_lR2hx7JqIZ6ii7UA.png)
{: .box-warning}
*ANS 10:* -T5
{: .box-note}
*Question 11:* What about if I want to scan a specific port?

    **Scan a single Port** :**  **nmap -p 22 192.168.1.1
{: .box-warning}
*ANS 11:* -p
{: .box-note}
*Question 12:* How about if I want to scan every port?

    **Scan all 65535 ports : ** nmap -p- 192.168.1.1
{: .box-warning}
*ANS 12:* -p-
{: .box-note}
*Question 13:* What if I want to enable using a script from the nmap scripting engine? For this, just include the first part of the switch without the specification of what script to run.

![](https://miro.medium.com/max/828/1*kyFg3FQho4Ellv8L73_KrA.png)
{: .box-warning}
*ANS 13:* --script
{: .box-note}
*Question 14:* What if I want to run all scripts out of the vulnerability category?

![](https://miro.medium.com/max/828/1*GAa1j1dnKXV8V-JOY-lQ3g.png)
{: .box-warning}
*ANS 14:*--script-vuln
{: .box-note}
*Question 15:* What switch should I include if I don’t want to ping the host?

![](https://miro.medium.com/max/828/1*y916xqBdoSs1BWx4Xcu33A.png)
{: .box-warning}
*ANS 15:* -Pn

## [Task 3] Nmap Scanning
{: .box-note}
*Question 1:* Let’s go ahead and start with the basics and perform a syn scan on the box provided. What will this command be **without** the host IP address?
{: .box-warning}
*ANS 1:* nmap -sS
{: .box-note}
*Question 2:* After scanning this, how many ports do we find open under 1000?

![](https://miro.medium.com/max/640/1*z97QhsNqI3hfD984PHFsAg.png)
{: .box-warning}
*ANS 2:* 2
{: .box-note}
*Question 3:* What communication protocol is given for these ports following the port number?

From the above screenshot, tcp protocol is given
{: .box-warning}
*ANS 3:* tcp
{: .box-note}
*Question 4:* Perform a service version detection scan, what is the version of the software running on port 22?

![](https://miro.medium.com/max/640/1*S3Oq8sqtDnPErLJP07uMgA.png)
{: .box-warning}
*ANS 4:* 6.6.1p1
{: .box-note}
*Question 5:* Perform an aggressive scan, what flag isn’t set under the results for port 80?
{: .box-warning}
*ANS 5:* httponly
{: .box-note}
*Question 6:* Perform a script scan of vulnerabilities associated with this box, what denial of service (DOS) attack is this box susceptible to?

Tests a web server for vulnerability to the Slowloris DoS attack without actually launching a DoS attack.

This script opens two connections to the server, each without the final CRLF. After 10 seconds, second connection sends additional header. Both connections then wait for server timeout. If second connection gets a timeout 10 or more seconds after the first one, we can conclude that sending additional header prolonged its timeout and that the server is vulnerable to slowloris DoS attack.
{: .box-warning}
*ANS 6:* http-slowloris-check

## **RESOURCES:**
[**Nmap: the Network Mapper — Free Security Scanner**](https://nmap.org/)
[**How to Use Nmap Script Engine (NSE) Scripts in Linux**](https://www.tecmint.com/use-nmap-script-engine-nse-scripts-in-linux/)


Completed this one.. See ya

:wq
