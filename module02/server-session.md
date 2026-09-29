# Server Session: My Working Directory

**Course:** CIT 386 · Module 2 · Assignment 2.2
**Author:** Nabin Chaulagain
**Date:** September 24, 2026

## Notes

- I connected using Terminal on macOS instead of PuTTY, because PuTTY is a Windows-only program and my laptop is a Mac. The commands and the result are the same.
- My first connection dropped ("broken pipe"), so I reconnected. The working session is below.
- The folder structure from the session handout was not available to me, so my directory contains `hello.txt` only.

## What I did

1. Connected to the shared server with my key.
2. Ran `pwd` **before creating anything**, to confirm where I had landed.
3. Ran `ls -l` to see what was already there.
4. Created my working directory `nabin-chaulagain`.
5. Created an empty file `hello.txt` inside it.
6. Listed the contents in long form to show ownership and dates.

## Transcript

nabinchaulagain@MacBook-Air ~ ssh -i ~/.ssh/VM01_key.pem azureuser@48.211.168.52
Welcome to Ubuntu 24.04.4 LTS (GNU/Linux 6.17.0-1022-azure x86_64)

System information as of Thu Sep 24 15:43:26 UTC 2026

System load: 0.11 Processes: 125
Usage of /: 8.7% of 28.02GB Users logged in: 0
Memory usage: 6% IPv4 address for eth0: 172.16.0.4
Swap usage: 0%

Last login: Thu Sep 24 15:15:15 2026
azureuser@VM01:~$ pwd
/home/azureuser
azureuser@VM01:~$ ls -l
total 8
-rw-rw-r-- 1 azureuser azureuser 0 Sep 20 14:11 VM-status.json
-rw-rw-r-- 1 azureuser azureuser 0 Sep 20 14:08 VM-technical-configuration.json
-rw-rw-r-- 1 azureuser azureuser 0 Sep 20 14:13 VM01-status.json
-rw-rw-r-- 1 azureuser azureuser 0 Sep 20 14:08 VM01-technical-configuration.json
drwxrwxr-x 2 azureuser azureuser 4096 Sep 15 17:18 argin-lilagan
drwxrwxr-x 2 azureuser azureuser 4096 Sep 13 19:43 matthew-espinel
azureuser@VM01:~$ mkdir nabin-chaulagain
azureuser@VM01:~$ cd nabin-chaulagain
azureuser@VM01:~/nabin-chaulagain$ touch hello.txt
azureuser@VM01:~/nabin-chaulagain$ ls -l
total 0
-rw-rw-r-- 1 azureuser azureuser 0 Sep 24 15:47 hello.txt
azureuser@VM01:~/nabin-chaulagain$ cd ..
azureuser@VM01:~$ ls -l
total 12
-rw-rw-r-- 1 azureuser azureuser 0 Sep 20 14:11 VM-status.json
-rw-rw-r-- 1 azureuser azureuser 0 Sep 20 14:08 VM-technical-configuration.json
-rw-rw-r-- 1 azureuser azureuser 0 Sep 20 14:13 VM01-status.json
-rw-rw-r-- 1 azureuser azureuser 0 Sep 20 14:08 VM01-technical-configuration.json
drwxrwxr-x 2 azureuser azureuser 4096 Sep 15 17:18 argin-lilagan
drwxrwxr-x 2 azureuser azureuser 4096 Sep 13 19:43 matthew-espinel
drwxrwxr-x 2 azureuser azureuser 4096 Sep 24 15:47 nabin-chaulagain
azureuser@VM01:~$


## Confirmation

- My working directory is `/home/azureuser/nabin-chaulagain`
- `ls -l` shows the owner as `azureuser`, so everything I created belongs to my account.
- I stayed inside my own directory and did not open anyone else's.