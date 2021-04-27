### Nmap
```bash
$ sudo nmap -sC -sV -oA nmap/initial 10.10.10.216
Starting Nmap 7.80 ( https://nmap.org ) at 2021-04-25 15:47 +07
Nmap scan report for 10.10.10.216
Host is up (0.036s latency).
Not shown: 997 filtered ports
PORT    STATE SERVICE  VERSION
22/tcp  open  ssh      OpenSSH 8.2p1 Ubuntu 4ubuntu0.1 (Ubuntu Linux; protocol 2.0)
80/tcp  open  http     Apache httpd 2.4.41
|_http-server-header: Apache/2.4.41 (Ubuntu)
|_http-title: Did not follow redirect to https://laboratory.htb/
443/tcp open  ssl/http Apache httpd 2.4.41 ((Ubuntu))
|_http-server-header: Apache/2.4.41 (Ubuntu)
|_http-title: The Laboratory
| ssl-cert: Subject: commonName=laboratory.htb
| Subject Alternative Name: DNS:git.laboratory.htb
| Not valid before: 2020-07-05T10:39:28
|_Not valid after:  2024-03-03T10:39:28
| tls-alpn: 
|_  http/1.1
Service Info: Host: laboratory.htb; OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 20.29 seconds

```

So we should add `laboratory.htb` to our host file

***Important***
```
OpenSSH 8.2p1 Ubuntu 4ubuntu0.1
ssl-cert: Subject: commonName=laboratory.htb
Subject Alternative Name: DNS:git.laboratory.htb
```
### HTTP
this is probably a static website
#### Laboratory.htb
![](Screen-shot/Pasted%20image%2020210425164440.png)
#### User
- Dexter, the CEO

### Gitlab
Get the gitlab subdomain `gitlab.laboratory.htb` from the certificate, add it to the host file
![](Screen-shot/certificate%20http.png)

![](Screen-shot/Gitlab%20login%20page.png)
 
Have to register with the `laboratory.htb` domain
![](Screen-shot/Not%20authorized%20domain%20when%20register.png)

#### Version 12.8.1
release date: Feb 24, 2020
![](Screen-shot/Gitlab%20version.png)

This is a vulnerable version

