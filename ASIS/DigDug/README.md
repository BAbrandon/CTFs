# Dig Dug

## Problem

The pot calling the kettle black.

https://digx.asisctf.com


## Flag 

ASIS{_just_Go_Offline_When_you_want_to_be_creative_!}

Dig stands for Domain information grapper, Dig will get all the information it can about a website. Dig without any arguments queries the DNS root zone. 

```
dig digx.asisctf.com

; <<>> DiG 9.9.5-11ubuntu1.3-Ubuntu <<>> digx.asisctf.com
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 61187
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4000
;; QUESTION SECTION:
;digx.asisctf.com.		IN	A

;; ANSWER SECTION:
digx.asisctf.com.	300	IN	A	192.81.223.250

;; Query time: 46 msec
;; SERVER: 127.0.1.1#53(127.0.1.1)
;; WHEN: Fri Sep 15 13:20:08 CDT 2017
;; MSG SIZE  rcvd: 61
```

As you can see we now have an ip adress for the domain 

```
;; ANSWER SECTION:
digx.asisctf.com.       300     IN      A       192.81.223.250

```
We can use dig to do an nslookup with the -x flag

```
dig -x 192.81.223.250

; <<>> DiG 9.9.5-11ubuntu1.3-Ubuntu <<>> -x 192.81.223.250
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NXDOMAIN, id: 3532
;; flags: qr rd ra; QUERY: 1, ANSWER: 0, AUTHORITY: 1, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4000
;; QUESTION SECTION:
;250.223.81.192.in-addr.arpa.	IN	PTR

;; ANSWER SECTION:
250.223.81.192.in-addr.arpa.	IN	PTR	airplane.asisctf.com.

;; Query time: 91 msec
;; SERVER: 127.0.1.1#53(127.0.1.1)
;; WHEN: Fri Sep 15 13:24:07 CDT 2017
;; MSG SIZE  rcvd: 123

```

If we do to airplane.asisctf.com, we see it says to do into offline mode. I switched off my wifi and the webpage changed so show text talking about saying offline so that you can do work. At the bottom is the flag


ASIS{_just_Go_Offline_When_you_want_to_be_creative_!}





