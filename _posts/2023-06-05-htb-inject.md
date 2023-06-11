---
topic: htb
author: trap
title: HTB Inject
---


...


<br/>
Lets start off with a nmap scan..<br/>
<br/>
<img src="/assets/images/htb/inject.md/image 2.png" /><br/>
<br/>
We will come back to look for exploits on Nagios. Let's check out whats on port 8080<br/>
<br/>
<img src="/assets/images/htb/inject.md/image 3.png" /><br/>
<br/>
We have an /upload page and a /register page. Let's check out the upload page and see if we can upload something without signing up.<br/>
And it looks like we can upload an /assets/images/htb/inject.md/image file, time to view our newly uploaded file.. (maybe we can **inject** a php reverse shell using burp..)<br/>
<br/>
<br/>
<img src="/assets/images/htb/inject.md/image 4.png" /><br/>
<br/>
<br/>
<br/>
<img src="/assets/images/htb/inject.md/image 5.png" /><br/>
<br/>
Hmmm... lets try **LFI** <br/>
<br/>
<img src="/assets/images/htb/inject.md/image 6.png" /><br/>
<br/>
We get a 500 error code.. Maybe we didnt go far enough back to make it to the root directory.. <br/>
Using curl we are able to see that is indeed the problem.<br/>
<br/>
<img src="/assets/images/htb/inject.md/image 7.png" /><br/>
<br/>
Looks like we need to go back 2 more directories.<br/>
<br/>
<img src="/assets/images/htb/inject.md/image 8.png" /><br/>
<br/>
????... Let's check the output using curl...<br/>
<br/>
<img src="/assets/images/htb/inject.md/image 9.png" /><br/>
<br/>
We have **LFI** and also 2 users on this machine, Frank and Phil. Lets check for any id_rsa keys for these users.<br/>
<br/>
<img src="/assets/images/htb/inject.md/image 10.png" /><br/>
<br/>
Looks like no id_rsa key for Frank or Phil but we can further enumerate the machine to look for other interesting files by &nbsp;just searching directories.<br/>
<br/>
<img src="/assets/images/htb/inject.md/image 11.png" /><br/>
<br/>
ahhh a flag.. &nbsp;Tried to curl the user.txt but i dont have the right permissions.. Let's get a shell<br/>
Further enumerating the system using curl we come across<br/>
<br/>
<img src="/assets/images/htb/inject.md/image 12.png" /><br/>
<br/>
<img src="/assets/images/htb/inject.md/image 13.png" /><br/>
<br/>
Let's check searchsploit.. This exploit might work. We can download this python script to our machine and test it out.<br/>
<br/>
<img src="/assets/images/htb/inject.md/image 14.png" /><br/>
<br/>
...<br/>
<br/>
Let's see if we can get a meterpreter shell.<br/>
First we'll start metasploit and search Spring Cloud. There are a few modules we could try but let's go with the exploit.<br/>
<br/>
<img src="/assets/images/htb/inject.md/image 15.png" /><br/>
<br/>
Next we will set the options to be able to run the exploit.<br/>
<br/>
<img src="/assets/images/htb/inject.md/image 16.png" /><br/>
<br/>
So we will need to set RHOSTS &amp; LHOST then we can run exploit.<br/>
<br/>
<img src="/assets/images/htb/inject.md/image 17.png" /><br/>
<br/>
And we get a shell. Type help to get a list of commands we can use while in our meterpreter shell. Lets drop into a system command shell and check our id.<br/>
<br/>
<img src="/assets/images/htb/inject.md/image 18.png" /><br/>
<br/>
We are Frank. Lets look for ways to escalate to root or pivot to phil.<br/>
<br/>
<br/>
<img src="/assets/images/htb/inject.md/image 19.png" /><br/>
<br/>
Well that was too easy.... I dont think that was the intended way but ill take the easy win....<br/>
<br/>
<br/>
<br/>



