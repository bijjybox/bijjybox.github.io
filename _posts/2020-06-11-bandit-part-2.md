---
layout: post
title: Bandit (Part 2)
subtitle: Learning Linux using OTW
gh-repo: bijjybox/beautiful-jekyll
gh-badge: [star, fork, follow]
tags: [test]
comments: true
---


This blog is in continuation of my previous blog, check it out here!

![](https://cdn-images-1.medium.com/max/2000/0*0aFfTS51R1vUZol9.png)

## Bandit Level 12 → Level 13

{: .box-warning}
**Level Goal:** The password for the next level is stored in the file **data.txt**, which is a hexdump of a file that has been repeatedly compressed. For this level it may be useful to create a directory under /tmp in which you can work using mkdir. For example: mkdir /tmp/myname123. Then copy the datafile using cp, and rename it using mv

```sh
apsychogirl@dell~ ssh bandit12@bandit.labs.overthewire.org -p 2220
bandit12@bandit:~$ ls
data.txt
bandit12@bandit:~$ mkdir /tmp/megha
bandit12@bandit:~$ cp data.txt /tmp/megha/
bandit12@bandit:~$ cd /tmp/megha
bandit12@bandit:/tmp/megha$ ls
data.txt
```

First of all, we use the xxd command to do a reverse hex dump and store the file with its original name, **data**.

After using the file command to fetch the information of **data**, we know that **data** is a gzip compressed file.

```sh
bandit12@bandit:/tmp/megha$ file data.txt 
data.txt: ASCII text
bandit12@bandit:/tmp/megha$ cat data.txt | xxd -r > data
bandit12@bandit:/tmp/megha$ ls
data data.txt
bandit12@bandit:/tmp/megha$ **file data**
data: **gzip compressed data**, was “data2.bin”, last modified: Thu May 7 18:14:30 2020, max compression, from Unix
```

We change the suffix of **data** back to .gz, which means the file would be renamed to **data2.gz**. Then, we use gzip to decompress the file. Afterward, we use the file command to check the information of **data2** again.

```sh
bandit12@bandit:/tmp/megha$ mv data data2.gz
bandit12@bandit:/tmp/megha$ ls
data2.gz data.txt
bandit12@bandit:/tmp/megha$ **gzip -d data2.gz **
bandit12@bandit:/tmp/megha$ ls
data2 data.txt
bandit12@bandit:/tmp/megha$ file data2 
data2: bzip2 compressed data, block size = 900k
```

This time, the **data2** file is a bzip2 compressed file. We change the suffix of **data2** back to .bz, which means the file would be renamed to **data3.bz**.

```sh
bandit12@bandit:/tmp/megha$ mv data2 data3.bz
bandit12@bandit:/tmp/megha$ ls
data3.bz  data.txt
bandit12@bandit:/tmp/megha$ **bzip2 -d data3.bz **
bandit12@bandit:/tmp/megha$ ls
data3  data.txt
bandit12@bandit:/tmp/megha$ file data3
data3: gzip compressed data, was "data4.bin", last modified: Thu May  7 18:14:30 2020, max compression, from Unix
```

It seems like an execution loop that we have to take the steps of checking the file, modifying the suffix, and then decompressing the file.

```sh
bandit12@bandit:/tmp/megha$ mv data3 data4.gz
bandit12@bandit:/tmp/megha$ ls
data4.gz  data.txt
bandit12@bandit:/tmp/megha$ **gzip -d data4.gz
**bandit12@bandit:/tmp/megha$ ls
data4  data.txt
bandit12@bandit:/tmp/megha$ file data4
data4: POSIX tar archive (GNU)

bandit12@bandit:/tmp/megha$ mv data4 data5.tar
bandit12@bandit:/tmp/megha$ ls
data5.tar  data.txt
bandit12@bandit:/tmp/megha$ **tar xvf data5.tar** 
data5.bin
bandit12@bandit:/tmp/megha$ file data5.bin 
data5.bin: POSIX tar archive (GNU)

bandit12@bandit:/tmp/megha$ **tar xvf data5.bin**
data6.bin
bandit12@bandit:/tmp/megha$ file data6.bin
data6.bin: bzip2 compressed data, block size = 900k

bandit12@bandit:/tmp/megha$ mv data6.bin data7.bz
bandit12@bandit:/tmp/megha$ ls
data5.bin  data5.tar  data7.bz  data.txt
bandit12@bandit:/tmp/megha$ **bzip2 -d data7.bz**
bandit12@bandit:/tmp/megha$ ls
data5.bin  data5.tar  data7  data.txt
bandit12@bandit:/tmp/megha$ file data7
data7: POSIX tar archive (GNU)

bandit12@bandit:/tmp/megha$ mv data7 data8.tar
bandit12@bandit:/tmp/megha$ ls
data5.bin  data5.tar  data8.tar  data.txt
bandit12@bandit:/tmp/megha$ **tar xvf data8.tar**
data8.bin
bandit12@bandit:/tmp/megha$ file data8.bin
data8.bin: gzip compressed data, was "data9.bin", last modified: Thu May  7 18:14:30 2020, max compression, from Unix

bandit12@bandit:/tmp/megha$ mv data8.bin data9.gz
bandit12@bandit:/tmp/megha$ ls
data5.bin  data5.tar  data8.tar  data9.gz  data.txt
bandit12@bandit:/tmp/megha$ **gzip -d data9.gz**
bandit12@bandit:/tmp/megha$ ls
data5.bin  data5.tar  data8.tar  data9  data.txt
bandit12@bandit:/tmp/megha$ file data9
**data9: ASCII text**

bandit12@bandit:/tmp/megha$ cat data9
**The password is 8ZjyCRiBWFYkneahHwxCv3wb2a1ORpYL**
```

Too much repetitive commands, damn!

**Key Takeaways**

reverse hexdump
```sh
xxd -r >output
```

gzip decompress
```sh
$ gzip -d file
```

bzip2 decompress
```sh
$ bzip2 -d file
```

tar decompress
```sh
$ tar xvf file
```

## Bandit Level 13 → Level 14

{: .box-warning}
**Level Goal:** The password for the next level is stored in **/etc/bandit_pass/bandit14 and can only be read by user bandit14**. For this level, you don’t get the next password, but you get a private SSH key that can be used to log into the next level. **Note:** **localhost** is a hostname that refers to the machine you are working on

```sh
apsychogirl@dell~ ssh [bandit13@bandit.labs.overthewire.org](mailto:bandit13@bandit.labs.overthewire.org) -p 2220
bandit13@bandit:~$ ls
sshkey.private
bandit13@bandit:~$ **file sshkey.private**
sshkey.private: PEM RSA private key
bandit13@bandit:~$ cat sshkey.private 
-----BEGIN RSA PRIVATE KEY-----
MIIEpAIBAAKCAQEAxkkOE83W2cOT7IWhFc9aPaaQmQDdgzuXCv+ppZHa++buSkN+
gg0tcr7Fw8NLGa5+Uzec2rEg0WmeevB13AIoYp0MZyETq46t+jk9puNwZwIt9XgB
ZufGtZEwWbFWw/vVLNwOXBe4UWStGRWzgPpEeSv5Tb1VjLZIBdGphTIK22Amz6Zb
ThMsiMnyJafEwJ/T8PQO3myS91vUHEuoOMAzoUID4kN0MEZ3+XahyK0HJVq68KsV
ObefXG1vvA3GAJ29kxJaqvRfgYnqZryWN7w3CHjNU4c/2Jkp+n8L0SnxaNA+WYA7
jiPyTF0is8uzMlYQ4l1Lzh/8/MpvhCQF8r22dwIDAQABAoIBAQC6dWBjhyEOzjeA
J3j/RWmap9M5zfJ/wb2bfidNpwbB8rsJ4sZIDZQ7XuIh4LfygoAQSS+bBw3RXvzE
pvJt3SmU8hIDuLsCjL1VnBY5pY7Bju8g8aR/3FyjyNAqx/TLfzlLYfOu7i9Jet67
xAh0tONG/u8FB5I3LAI2Vp6OviwvdWeC4nOxCthldpuPKNLA8rmMMVRTKQ+7T2VS
nXmwYckKUcUgzoVSpiNZaS0zUDypdpy2+tRH3MQa5kqN1YKjvF8RC47woOYCktsD
o3FFpGNFec9Taa3Msy+DfQQhHKZFKIL3bJDONtmrVvtYK40/yeU4aZ/HA2DQzwhe
ol1AfiEhAoGBAOnVjosBkm7sblK+n4IEwPxs8sOmhPnTDUy5WGrpSCrXOmsVIBUf
laL3ZGLx3xCIwtCnEucB9DvN2HZkupc/h6hTKUYLqXuyLD8njTrbRhLgbC9QrKrS
M1F2fSTxVqPtZDlDMwjNR04xHA/fKh8bXXyTMqOHNJTHHNhbh3McdURjAoGBANkU
1hqfnw7+aXncJ9bjysr1ZWbqOE5Nd8AFgfwaKuGTTVX2NsUQnCMWdOp+wFak40JH
PKWkJNdBG+ex0H9JNQsTK3X5PBMAS8AfX0GrKeuwKWA6erytVTqjOfLYcdp5+z9s
8DtVCxDuVsM+i4X8UqIGOlvGbtKEVokHPFXP1q/dAoGAcHg5YX7WEehCgCYTzpO+
xysX8ScM2qS6xuZ3MqUWAxUWkh7NGZvhe0sGy9iOdANzwKw7mUUFViaCMR/t54W1
GC83sOs3D7n5Mj8x3NdO8xFit7dT9a245TvaoYQ7KgmqpSg/ScKCw4c3eiLava+J
3btnJeSIU+8ZXq9XjPRpKwUCgYA7z6LiOQKxNeXH3qHXcnHok855maUj5fJNpPbY
iDkyZ8ySF8GlcFsky8Yw6fWCqfG3zDrohJ5l9JmEsBh7SadkwsZhvecQcS9t4vby
9/8X4jS0P8ibfcKS4nBP+dT81kkkg5Z5MohXBORA7VWx+ACohcDEkprsQ+w32xeD
qT1EvQKBgQDKm8ws2ByvSUVs9GjTilCajFqLJ0eVYzRPaY6f++Gv/UVfAPV4c+S0
kAWpXbv5tbkkzbS0eaLPTKgLzavXtQoTtKwrjpolHKIHUz6Wu+n4abfAIRFubOdN
/+aLoRQ0yBDRbdXMsZN/jvY44eM+xRLdRVyMmdPtP8belRi2E2aEzA==
-----END RSA PRIVATE KEY-----

bandit13@bandit:~$ ssh -i sshkey.private bandit14@localhost -p 2220
ssh: connect to host localhost port 2220: Connection refused

bandit13@bandit:~$ **ssh bandit14@localhost -i sshkey.private **

bandit14@bandit:~$ whoami
bandit14
bandit14@bandit:~$ cat /etc/bandit_pass/bandit14
**4wcYUJFw0k0XLShlDzztnTBHiqxU3b3e**
bandit14@bandit:~$
```

## Bandit Level 14 → Level 15

{: .box-warning}
**Level Goal:** The password for the next level can be retrieved by submitting the password of the current level to **port 30000 on localhost**.

```sh
bandit14@bandit:~$ cat /etc/bandit_pass/bandit14
**4wcYUJFw0k0XLShlDzztnTBHiqxU3b3e**
bandit14@bandit:~$ nc localhost 30000
4wcYUJFw0k0XLShlDzztnTBHiqxU3b3e
Correct!
**BfMYroe26WYalil77FoDi9qh59eK5xNr**

bandit14@bandit:~$
```

## Bandit Level 15 → Level 16

{: .box-warning}
**Level Goal:** The password for the next level can be retrieved by submitting the password of the current level to **port 30001 on localhost** using SSL encryption.

```sh
bandit14@bandit:~$ **openssl s_client -connect localhost:30001 -ign_eof **
CONNECTED(00000003)
depth=0 CN = localhost
verify error:num=18:self signed certificate
verify return:1
depth=0 CN = localhost
verify return:1
---
Certificate chain
    0 s:/CN=localhost
    i:/CN=localhost
---
Server certificate
-----BEGIN CERTIFICATE-----
MIICBjCCAW+gAwIBAgIEDU18oTANBgkqhkiG9w0BAQUFADAUMRIwEAYDVQQDDAls
b2NhbGhvc3QwHhcNMjAwNTA3MTgxNTQzWhcNMjEwNTA3MTgxNTQzWjAUMRIwEAYD
VQQDDAlsb2NhbGhvc3QwgZ8wDQYJKoZIhvcNAQEBBQADgY0AMIGJAoGBAK3CPNFR
FEypcqUa8NslmIMWl9xq53Cwhs/fvYHAvauyfE3uDVyyX79Z34Tkot6YflAoufnS
+puh2Kgq7aDaF+xhE+FPcz1JE0C2bflGfEtx4l3qy79SRpLiZ7eio8NPasvduG5e
pkuHefwI4c7GS6Y7OTz/6IpxqXBzv3c+x93TAgMBAAGjZTBjMBQGA1UdEQQNMAuC
CWxvY2FsaG9zdDBLBglghkgBhvhCAQ0EPhY8QXV0b21hdGljYWxseSBnZW5lcmF0
ZWQgYnkgTmNhdC4gU2VlIGh0dHBzOi8vbm1hcC5vcmcvbmNhdC8uMA0GCSqGSIb3
DQEBBQUAA4GBAC9uy1rF2U/OSBXbQJYuPuzT5mYwcjEEV0XwyiX1MFZbKUlyFZUw
rq+P1HfFp+BSODtk6tHM9bTz+p2OJRXuELG0ly8+Nf/hO/mYS1i5Ekzv4PL9hO8q
PfmDXTHs23Tc7ctLqPRj4/4qxw6RF4SM+uxkAuHgT/NDW1LphxkJlKGn
-----END CERTIFICATE-----
subject=/CN=localhost
issuer=/CN=localhost
---
No client certificate CA names sent
Peer signing digest: SHA512
Server Temp Key: X25519, 253 bits
---
SSL handshake has read 1019 bytes and written 269 bytes
Verification error: self signed certificate
---
New, TLSv1.2, Cipher is ECDHE-RSA-AES256-GCM-SHA384
Server public key is 1024 bit
Secure Renegotiation IS supported
Compression: NONE
Expansion: NONE
No ALPN negotiated
SSL-Session:
    Protocol  : TLSv1.2
    Cipher    : ECDHE-RSA-AES256-GCM-SHA384
    Session-ID: 758919214D7518C387D26D953EEFD476645F628CB72DD54164CC25EB251F58B3
    Session-ID-ctx: 
    Master-Key: 662A73401DE10EEFA2E6B0E563F8E0228BE40911B51367DF116631DBFE22812C5C0B4712F4D3C712131DE3578A145AF4
    PSK identity: None
    PSK identity hint: None
    SRP username: None
    TLS session ticket lifetime hint: 7200 (seconds)
    TLS session ticket:
    0000 - aa 02 e6 3a 2e 0b c8 5d-6f 54 4a 1b 5a e0 2c 0e   ...:...]oTJ.Z.,.
    0010 - 26 1c b7 09 66 57 08 33-c7 ca b8 6d 04 c0 0a e7   &...fW.3...m....
    0020 - 76 f9 97 f9 7a 7c cc 37-c8 49 92 fb 26 ce f9 b4   v...z|.7.I..&...
    0030 - 9b b4 a6 3e f1 99 7d 8e-e1 62 80 7f aa 08 10 4a   ...>..}..b.....J
    0040 - 08 19 75 93 09 7b 23 79-24 bb eb cb 29 c2 fb 29   ..u..{#y$...)..)
    0050 - 75 f4 36 58 2d b8 15 77-53 fa cd 39 df 30 b9 9e   u.6X-..wS..9.0..
    0060 - ff 50 69 18 2c b7 d3 3f-68 94 d3 e9 bf 9d 17 3b   .Pi.,..?h......;
    0070 - e4 e6 b5 e0 c2 0a 1b 7c-6a eb 26 bc a3 e2 ea 50   .......|j.&....P
    0080 - 0d 8a db ee ed 35 dc 8a-68 09 4f fe 04 6f 1d ca   .....5..h.O..o..
    0090 - 00 3e d2 ed ca 1d 19 51-aa 6c 54 50 97 8d c8 68   .>.....Q.lTP...h

Start Time: 1591972224
    Timeout   : 7200 (sec)
    Verify return code: 18 (self signed certificate)
    Extended master secret: yes
---
**BfMYroe26WYalil77FoDi9qh59eK5xNr**
Correct!
**cluFn7wTiGryunymYOu4RcffSxQluehd**

closed
bandit14@bandit:~$
```

* -ign_eof
```sh
Inhibit shutting down the connection when end of file is reached in the input.
```
## Bandit Level 16 → Level 17

{: .box-warning}
**Level Goal:** The credentials for the next level can be retrieved by submitting the password of the current level to **a port on localhost in the range 31000 to 32000**. First find out which of these ports have a server listening on them. Then find out which of those speak SSL and which don’t. There is only 1 server that will give the next credentials, the others will simply send back to you whatever you send to it.

```sh
bandit14@melinda:~$ nmap -sT -A -p 31000-32000 localhost
Starting Nmap 6.40 ( [http://nmap.org](http://nmap.org) ) at 2017-05-21 02:18 UTC
Nmap scan report for localhost (127.0.0.1)
Host is up (0.00037s latency).
Not shown: 996 closed ports
PORT      STATE SERVICE VERSION
31046/tcp open  echo
31518/tcp open  msdtc   Microsoft Distributed Transaction Coordinator (error)
31691/tcp open  echo
**31790/tcp open  msdtc   Microsoft Distributed Transaction Coordinator (error)**
31960/tcp open  echo
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windowsService detection performed. Please report any incorrect results at [http://nmap.org/submit/](http://nmap.org/submit/) .
Nmap done: 1 IP address (1 host up) scanned in 41.29 seconds
bandit14@melinda:~$ **openssl s_client -connect localhost:31790**
CONNECTED(00000003)
depth=0 CN = li190-250.members.linode.com
verify error:num=18:self signed certificate
verify return:1
depth=0 CN = li190-250.members.linode.com
verify return:1
---
Certificate chain
 0 s:/CN=li190-250.members.linode.com
   i:/CN=li190-250.members.linode.com
---
Server certificate
-----BEGIN CERTIFICATE-----
MIIC3jCCAcagAwIBAgIJAI5QiWZw4YHbMA0GCSqGSIb3DQEBCwUAMCcxJTAjBgNV
BAMTHGxpMTkwLTI1MC5tZW1iZXJzLmxpbm9kZS5jb20wHhcNMTQxMTE0MTAyODA0
WhcNMjQxMTExMTAyODA0WjAnMSUwIwYDVQQDExxsaTE5MC0yNTAubWVtYmVycy5s
aW5vZGUuY29tMIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAsKmy9o5z
WU+1EH7Z3bB5TGQA+16zXDcEJy6tZWZ8CDrRyQXiahendp45BWUc/ZuLDo0+B3Wt
ZXjofmLw/F4fmR+8X1s1fQZX2dFt920qEm7LxqzWd0c7FdHiBwwRrwhkk+3cQpOB
TTGdLWEgpdmwwNZDTUdsDLzjDczPnju6T6p6ArTECztPbmTjfY4QIRtC6capL1Z+
yPJSQVAuAMEX1wTDWTGdm0VV7oW4F5cGZutf6QAP51jdhSyZuGilIPHbnj0l6Qc7
a7+OtEsEGi31aJ8KpRf7LNZ7DXCuoB3Hf75Pd6VjDgoOIagcH0NYqa75gEjBkGzs
ktLWykT7ag7fKwIDAQABow0wCzAJBgNVHRMEAjAAMA0GCSqGSIb3DQEBCwUAA4IB
AQCaZdUNAj8WDEKWdoU3LNXUBJlTJwiWBrh550PbHSQORcCz2K0kiMei1A4ojK2N
dMHFGAqAeUEaxtz92p2BoFpZasAtdSa3u63tBckFhfUolIS1TC7Cj51y19ysTeep
fGPFpuPCVqVPsruei8Z/iqn3bFIhQQdmumeePZQdPMwZSWHNVYC5XODd7PvNDrDu
5MZJjkz4+6LbwwAvyew62meFN2QEsYbK2Brtbhze+IjE27FGWlSw4K3jlwa409MD
MTf4JU41ELaYY8G/LSNDJsBVhhkHzvXR9iCbXxNz3IL0dQDNj7h4LKhBy0q7hvqg
kDzwlmBO4WKSmCAuky44cXmd
-----END CERTIFICATE-----
subject=/CN=li190-250.members.linode.com
issuer=/CN=li190-250.members.linode.com
---
No client certificate CA names sent
---
SSL handshake has read 1714 bytes and written 637 bytes
---
New, TLSv1/SSLv3, Cipher is DHE-RSA-AES256-SHA
Server public key is 2048 bit
Secure Renegotiation IS supported
Compression: NONE
Expansion: NONE
SSL-Session:
    Protocol  : SSLv3
    Cipher    : DHE-RSA-AES256-SHA
    Session-ID: 20CA4FD2722C9FC893DECEE1CA87C16F698563B7116265F39D48D3D8F6853EAF
    Session-ID-ctx:
    Master-Key: FE0F0C093E12801D5CF052F1734410396EF1D35B1C85BA0DA685ED8A990E62A96221321469CA02D4C7374A628EDEECE8
    Key-Arg   : None
    PSK identity: None
    PSK identity hint: None
    SRP username: None
    Start Time: 1495333239
    Timeout   : 300 (sec)
    Verify return code: 18 (self signed certificate)
---
**cluFn7wTiGryunymYOu4RcffSxQluehd**
Correct!
**-----BEGIN RSA PRIVATE KEY-----
MIIEogIBAAKCAQEAvmOkuifmMg6HL2YPIOjon6iWfbp7c3jx34YkYWqUH57SUdyJ
imZzeyGC0gtZPGujUSxiJSWI/oTqexh+cAMTSMlOJf7+BrJObArnxd9Y7YT2bRPQ
Ja6Lzb558YW3FZl87ORiO+rW4LCDCNd2lUvLE/GL2GWyuKN0K5iCd5TbtJzEkQTu
DSt2mcNn4rhAL+JFr56o4T6z8WWAW18BR6yGrMq7Q/kALHYW3OekePQAzL0VUYbW
JGTi65CxbCnzc/w4+mqQyvmzpWtMAzJTzAzQxNbkR2MBGySxDLrjg0LWN6sK7wNX
x0YVztz/zbIkPjfkU1jHS+9EbVNj+D1XFOJuaQIDAQABAoIBABagpxpM1aoLWfvD
KHcj10nqcoBc4oE11aFYQwik7xfW+24pRNuDE6SFthOar69jp5RlLwD1NhPx3iBl
J9nOM8OJ0VToum43UOS8YxF8WwhXriYGnc1sskbwpXOUDc9uX4+UESzH22P29ovd
d8WErY0gPxun8pbJLmxkAtWNhpMvfe0050vk9TL5wqbu9AlbssgTcCXkMQnPw9nC
YNN6DDP2lbcBrvgT9YCNL6C+ZKufD52yOQ9qOkwFTEQpjtF4uNtJom+asvlpmS8A
vLY9r60wYSvmZhNqBUrj7lyCtXMIu1kkd4w7F77k+DjHoAXyxcUp1DGL51sOmama
+TOWWgECgYEA8JtPxP0GRJ+IQkX262jM3dEIkza8ky5moIwUqYdsx0NxHgRRhORT
8c8hAuRBb2G82so8vUHk/fur85OEfc9TncnCY2crpoqsghifKLxrLgtT+qDpfZnx
SatLdt8GfQ85yA7hnWWJ2MxF3NaeSDm75Lsm+tBbAiyc9P2jGRNtMSkCgYEAypHd
HCctNi/FwjulhttFx/rHYKhLidZDFYeiE/v45bN4yFm8x7R/b0iE7KaszX+Exdvt
SghaTdcG0Knyw1bpJVyusavPzpaJMjdJ6tcFhVAbAjm7enCIvGCSx+X3l5SiWg0A
R57hJglezIiVjv3aGwHwvlZvtszK6zV6oXFAu0ECgYAbjo46T4hyP5tJi93V5HDi
Ttiek7xRVxUl+iU7rWkGAXFpMLFteQEsRr7PJ/lemmEY5eTDAFMLy9FL2m9oQWCg
R8VdwSk8r9FGLS+9aKcV5PI/WEKlwgXinB3OhYimtiG2Cg5JCqIZFHxD6MjEGOiu
L8ktHMPvodBwNsSBULpG0QKBgBAplTfC1HOnWiMGOU3KPwYWt0O6CdTkmJOmL8Ni
blh9elyZ9FsGxsgtRBXRsqXuz7wtsQAgLHxbdLq/ZJQ7YfzOKU4ZxEnabvXnvWkU
YOdjHdSOoKvDQNWu6ucyLRAWFuISeXw9a/9p7ftpxm0TSgyvmfLF2MIAEwyzRqaM
77pBAoGAMmjmIJdjp+Ez8duyn3ieo36yrttF5NSsJLAbxFpdlc1gvtGCWW+9Cq0b
dxviW8+TFVEBl1O4f7HVm6EpTscdDxU+bCXWkfjuRb7Dy9GOtt9JPsX8MBTakzh3
vBgsyi/sN3RqRBcGU40fOoZyfAMT8s1m/uYv52O6IgeuZ/ujbjY=
-----END RSA PRIVATE KEY-----
**read:errno=0
bandit14@melinda:~$
```

To scan multiple ports instead of all common ports:
```sh
sudo nmap -p port1-portX remote_host
```

To scan for TCP connections, nmap can perform a 3-way handshake with the targeted port:
```sh
sudo nmap -sT remote_host
```

Enables OS detection, version detection, script scanning, and traceroute:
```sh
sudo nmap -A remote_host
```

## Bandit Level 17 → Level 18

{: .box-warning}
**Level Goal:** There are 2 files in the homedirectory: **passwords.old and passwords.new**. The password for the next level is in **passwords.new** and is the only line that has been changed between **passwords.old and passwords.new**

> **NOTE: if you have solved this level and see ‘Byebye!’ when trying to log into bandit18, this is related to the next level, bandit19**

```sh
apsychogirl@dell~ touch bandit17.private
apsychogirl@dell~ nano bandit17.private
```

Save the key we got from above level locally in bandit17.private and change the permissions for the private file and then ssh like level 13.
```sh
apsychogirl@dell~ chmod 600 bandit17.private                                          
apsychogirl@dell~ ssh [bandit17@bandit.labs.overthewire.org](mailto:bandit17@bandit.labs.overthewire.org) -p 2220 -i bandit17.private

bandit17@bandit:~$ ls
passwords.new  passwords.old
bandit17@bandit:~$ file passwords.new 
passwords.new: ASCII text
bandit17@bandit:~$ file passwords.old 
passwords.old: ASCII text
bandit17@bandit:~$ **diff passwords.new passwords.old **
42c42
< **kfBf3eYk5BPBRzwjqutbbfE887SVc5Yd**
---
> w0Yfolrc5bwjS4qw5mq1nnQi6mF03bii
```

## Bandit Level 18 → Level 19

{: .box-warning}
**Level Goal:** The password for the next level is stored in a file **readme** in the homedirectory. Unfortunately, someone has modified **.bashrc** to log you out when you log in with SSH.

```sh
apsychogirl@dell~ **ssh bandit18@bandit.labs.overthewire.org -p 2220 ls**
This is a OverTheWire game server. More information on [http://www.overthewire.org/wargames](http://www.overthewire.org/wargames)

bandit18@bandit.labs.overthewire.org\'s password: 
readme
apsychogirl@dell~ **ssh bandit18@bandit.labs.overthewire.org -p 2220 cat readme**
This is a OverTheWire game server. More information on [http://www.overthewire.org/wargames](http://www.overthewire.org/wargames)

bandit18@bandit.labs.overthewire.org\'s password: 
**IueksS7Ubh8G3DCwVzrTd8rAVOwq3M5x**
apsychogirl@dell~ 
```
## Bandit Level 19 → Level 20

{: .box-warning}
**Level Goal:** To gain access to the next level, you should use the setuid binary in the homedirectory. Execute it without arguments to find out how to use it. The password for this level can be found in the usual place (/etc/bandit_pass), after you have used the setuid binary.

```sh
bandit19@bandit:~$ ls
**bandit20-do
**bandit19@melinda:~$ **ls -la bandit20-do**
**-rwsr-x--- 1 bandit20 bandit19 7370 Nov 14  2014 bandit20-do**

bandit19@bandit:~$ ./bandit20-do 
Run a command as another user.
  Example: ./bandit20-do id

bandit19@bandit:~$ .**/bandit20-do id**
uid=11019(bandit19) gid=11019(bandit19) euid=11020(bandit20) groups=11019(bandit19)

bandit19@bandit:~$ **file bandit20-do **
bandit20-do: setuid ELF 32-bit LSB executable, Intel 80386, version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux.so.2, for GNU/Linux 2.6.32, BuildID[sha1]=8e941f24b8c5cd0af67b22b724c57e1ab92a92a1, not stripped

bandit19@bandit:~$ **./bandit20-do cat /etc/bandit_pass/bandit20**
**GbKksEFF4yrVs6il55v6gwY5aVje5f0j**
bandit19@bandit:~$
```

## Bandit Level 20 → Level 21

{: .box-warning}
**Level Goal:** There is a setuid binary in the homedirectory that does the following: it makes a connection to localhost on the port you specify as a commandline argument. It then reads a line of text from the connection and compares it to the password in the previous level (bandit20). If the password is correct, it will transmit the password for the next level (bandit21).
{: .box-note}
**NOTE:** Try connecting to your own network daemon to see if it works as you think

```sh
bandit20@bandit:~$ ls
suconnect
bandit20@bandit:~$ ls -al ./suconnect
**-rwsr-x---** 1 bandit21 bandit20 12088 Oct 16 14:00 ./suconnect
```
First, we set up a simple TCP server which listens to port 61337 and we let the process running in the background.

```sh
bandit20@bandit:~$ **echo "GbKksEFF4yrVs6il55v6gwY5aVje5f0j" | nc -l localhost -p 61337 &**
[1] 24550
bandit20@bandit:~$ ps aux
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
bandit20 23100  0.0  0.1  21148  4916 pts/27   Ss   09:19   0:00 -bash
bandit20 24550  0.0  0.0   6300  1656 pts/27   S    09:25   0:00 nc -l localhost -p 61337
bandit20 24727  0.0  0.0  19188  2376 pts/27   R+   09:26   0:00 ps aux
```    

Now, we execute the setuid binary to connect localhost by 61337 port.

```sh
bandit20@bandit:~$ **./suconnect 61337**
Read: GbKksEFF4yrVs6il55v6gwY5aVje5f0j
Password matches, sending next password
**gE269g2h3mw3pwgrj0Ha9Uoqen1c9DGr**
```

## Bandit Level 21 → Level 22

{: .box-warning}
**Level Goal:** A program is running automatically at regular intervals from **cron**, the time-based job scheduler. Look in **/etc/cron.d/** for the configuration and see what command is being executed.

```sh
apsychogirl@dell~ ssh bandit21@bandit.labs.overthewire.org -p 2220
bandit21@bandit:/etc/cron.d$ ls -la
total 36
drwxr-xr-x  2 root root 4096 May 14 14:04 .
drwxr-xr-x 87 root root 4096 May 14 09:41 ..
-rw-r--r--  1 root root   62 May 14 13:40 cronjob_bandit15_root
-rw-r--r--  1 root root   62 May 14 14:03 cronjob_bandit17_root
**-rw-r--r--  1 root root  120 May  7 20:14 cronjob_bandit22**
-rw-r--r--  1 root root  122 May  7 20:14 cronjob_bandit23
-rw-r--r--  1 root root  120 May 14 09:41 cronjob_bandit24
-rw-r--r--  1 root root   62 May 14 14:04 cronjob_bandit25_root
-rw-r--r--  1 root root  102 Oct  7  2017 .placeholder

bandit21@bandit:/etc/cron.d$ **cat cronjob_bandit22**
[@reboot](http://twitter.com/reboot) bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
* * * * * bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null

bandit21@bandit:~$** cat /usr/bin/cronjob_bandit22.sh**
#!/bin/bash
chmod 644 /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
cat /etc/bandit_pass/bandit22 > tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
bandit21@bandit:~$ **file /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv**
/tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv: ASCII text

bandit21@bandit:~$ cat /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
**Yk7owGAcWjwMVRwrTesJEwB7WVOiILLI**
```

## Bandit Level 22 → Level 23

{: .box-warning}
**Level Goal:** A program is running automatically at regular intervals from **cron**, the time-based job scheduler. Look in **/etc/cron.d/** for the configuration and see what command is being executed.

{: .box-note}
**NOTE:** Looking at shell scripts written by other people is a very useful skill. The script for this level is intentionally made easy to read. If you are having problems understanding what it does, try executing it to see the debug information it prints.

```sh
apsychogirl@dell~ ssh bandit22@bandit.labs.overthewire.org -p 2220
bandit22@bandit:~$ **cd /etc/cron.d**
bandit22@bandit:/etc/cron.d$ ls -la
total 36
drwxr-xr-x  2 root root 4096 May 14 14:04 .
drwxr-xr-x 87 root root 4096 May 14 09:41 ..
-rw-r--r--  1 root root   62 May 14 13:40 cronjob_bandit15_root
-rw-r--r--  1 root root   62 May 14 14:03 cronjob_bandit17_root
-rw-r--r--  1 root root  120 May  7 20:14 cronjob_bandit22
-rw-r--r--  1 root root  122 May  7 20:14 cronjob_bandit23
-rw-r--r--  1 root root  120 May 14 09:41 cronjob_bandit24
-rw-r--r--  1 root root   62 May 14 14:04 cronjob_bandit25_root
-rw-r--r--  1 root root  102 Oct  7  2017 .placeholder
bandit22@bandit:/etc/cron.d$ **cat cronjob_bandit23**
[@reboot](http://twitter.com/reboot) bandit23 /usr/bin/cronjob_bandit23.sh  &> /dev/null
* * * * * bandit23 /usr/bin/cronjob_bandit23.sh  &> /dev/null

bandit22@bandit:/etc/cron.d$ cat /usr/bin/cronjob_bandit23.sh
#!/bin/bash

myname=\$(whoami)
mytarget=\$(echo I am user $myname | md5sum | cut -d ' ' -f 1)

echo \"Copying passwordfile /etc/bandit_pass/$myname to /tmp/$mytarget\"

cat /etc/bandit_pass/$myname > /tmp/$mytarget**
```    

I executed the script line by line to go further

```sh
bandit22@bandit:/etc/cron.d$ **whoami**
bandit22
bandit22@bandit:/etc/cron.d$ echo I am user bandit23 | md5sum | cut -d \' \' -f 1
8ca319486bfbbc3663ea0fbe81326349
bandit22@bandit:/etc/cron.d$ cat /tmp/8ca319486bfbbc3663ea0fbe81326349
**jc1udXuA1tiHqjIsL8yaapX5XIAI6i0n**
bandit22@bandit:/etc/cron.d$
```
{: .box-note}
**Super Important** — Run the script using bandit23 and not bandit22 else you will keep getting previous pass. (I missed it in first go🎃)

## Bandit Level 23 → Level 24

{: .box-warning}
**Level Goal:** A program is running automatically at regular intervals from **cron**, the time-based job scheduler. Look in **/etc/cron.d/** for the configuration and see what command is being executed.

{: .box-note}
**NOTE:** This level requires you to create your own first shell-script. This is a very big step and you should be proud of yourself when you beat this level!
{: .box-note}
**NOTE 2:** Keep in mind that your shell script is removed once executed, so you may want to keep a copy around…

```sh
bandit23@bandit:~$ cat /etc/cron.d/cronjob_bandit24
[@reboot](http://twitter.com/reboot) bandit24 /usr/bin/cronjob_bandit24.sh &> /dev/null
* * * * * bandit24 /usr/bin/cronjob_bandit24.sh &> /dev/null
bandit23@bandit:~$ cat /usr/bin/cronjob_bandit24.sh
#!/bin/bashmyname=$(whoami)cd /var/spool/$myname
echo \"Executing and deleting all scripts in /var/spool/$myname:\"
for i in * .*;
do
    if [ "$i" != "." -a "$i" != ".." ];
    then
        echo "Handling $i"
        **timeout -s 9 60 ./$i**
        rm -f ./$i
    fi
done
```

From the script, we know that every file in the path of /var/spool/$myname will be executed once and be deleted after 60 seconds. Let us try to write a script to fetch the password from **/etc/bandit_pass/bandit24** and store it to another temporary file. First, we create a workspace to work around …

```sh
bandit23@bandit:~$ mkdir -p /tmp/sec
bandit23@bandit:~$ cd /tmp/sec
bandit23@bandit:/tmp/sec$ touch sec.sh
bandit23@bandit:/tmp/sec$ chmod 777 sec.sh
bandit23@bandit:/tmp/sec$ ls -al sec.sh
-rwxrwxrwx 1 bandit23 root 68 Mar 26 08:09 sec.sh
bandit23@bandit:/tmp/sec$ nano sec.sh
```

Following is the content of the script, please refer it.

```sh
#!/bin/bash
cat /etc/bandit_pass/bandit24 > /tmp/sec/password
```
Set permission of the password file which allows everyone to write information to the file.

```sh
bandit23@bandit:/tmp/sec$ touch password
bandit23@bandit:/tmp/sec$ chmod 666 password
bandit23@bandit:/tmp/sec$ ls -al password
-rw-rw-rw- 1 bandit23 root 0 Mar 26 08:11 password
```
Finally, we copy the script to **/var/spool/bandit24** and wait for the script running automatically in a minute.
```sh
bandit23@bandit:/tmp/sec$ cp sec.sh /var/spool/bandit24/
bandit23@bandit:/tmp/sec$ ls -al /var/spool/bandit24/sec.sh
-rwxr-xr-x 1 bandit23 bandit23 68 Mar 26 08:16 /var/spool/bandit24/sec.sh
bandit23@bandit:/tmp/sec$ ls -al password
-rw-rw-rw- 1 bandit23 root **0** Mar 26 08:17 password
```
    ...(wait a minute)
```sh
bandit23@bandit:/tmp/sec$ ls -al password
-rw-rw-rw- 1 bandit23 root 33 Mar 26 08:18 password

bandit23@bandit:/tmp/sec$ cat password
UoMYTrfrBFHyQXmg6gzctqAwOmw1IohZ
```

## Bandit Level 24 → Level 25

{: .box-warning}
**Level Goal:** A daemon is listening on port 30002 and will give you the password for bandit25 if given the password for bandit24 and a secret numeric 4-digit pincode. There is no way to retrieve the pincode except by going through all of the 10000 combinations, called brute-forcing.

```sh
bandit24@bandit:~$ mkdir /tmp/imp
bandit24@bandit:~$ cd /tmp/imp
bandit24@bandit:/tmp/imp$ touch file.sh
bandit24@bandit:/tmp/imp$ nano file.sh
```
Type this script
```sh
#!/bin/bash
password24=UoMYTrfrBFHyQXmg6gzctqAwOmw1IohZ

for i in {0000..9999}
  do
    echo $password24 $i >> passlist.txt
  done
```

Give the permissions

```sh
bandit24@bandit:/tmp/imp$ chmod 777 file.sh
```
Execute the file.sh
```sh
bandit24@bandit:/tmp/imp$ ./file.sh
bandit24@bandit:/tmp/imp$ ls -la
total 896
drwxr-sr-x    2 bandit24 root   4096 Jun 14 14:55 .
drwxrws-wt 4493 root     root 524288 Jun 14 14:55 ..
-rwxrwxrwx    1 bandit24 root    131 Jun 14 14:52 file.sh
-rw-r--r--    1 bandit24 root 380000 Jun 14 14:55 passlist.txt
bandit24@bandit:/tmp/imp$ cat passlist.txt | nc localhost 30002
I am the pincode checker for user bandit25. Please enter the password for user bandit24 and the secret pincode on a single line, separated by a space.
Wrong! Please enter the correct pincode. Try again.
Wrong! Please enter the correct pincode. Try again.
Wrong! Please enter the correct pincode. Try again.
Correct!
The password of user bandit25 is uNG9O58gUE7snukf3bvZ0rxhtnjzSGzG

Exiting.
bandit24@bandit:/tmp/imp$
```
### See ya in some days, this made me dizzy 😩


