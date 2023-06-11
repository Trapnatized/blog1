---
topic: htb
author: trap
title: HTB Jupiter
---

...


Ok let's start off with a port scan as usual<br/>
<br/>
<img src="/assets/images/htb/jupiter/image 4.png" /><br/>
<br/>
Port 80 is open but it looks like we need to add `jupiter.htb` to our `/etc/hosts` file. You can use any text editor you want to edit `/etc/hosts` (ex: nano, vim, gedit, mousepad). &nbsp;I will just use echo and append to the end of `/etc/hosts`.<br/>
<br/>
<img src="/assets/images/htb/jupiter/image 3.png" /><br/>
<br/>
Now we can view the website on port 80.<br/>
<br/>
<img src="/assets/images/htb/jupiter/image 2.png" /><br/>
<br/>
Scanning for directories using gobuster we dont really see anything useful.<br/>
<br/>
<img src="/assets/images/htb/jupiter/image.png" /><br/>
<br/>
Let's try scanning for subdomains instead.<br/>
<br/>
<img src="/assets/images/htb/jupiter/image 5.png" /><br/>
<br/>
We find a subdomain called `kiosk.jupiter.htb`. Let's add this to our `/etc/hosts/` file also. I will be using sed to search for the word `jupiter.htb` and replace it with `jupiter.htb kiosk.jupiter.htb` inside `/etc/hosts`<br/>
<br/>
<img src="/assets/images/htb/jupiter/image 6.png" /><br/>
<br/>
Now we can view the new subdomain.<br/>
<br/>
<img src="/assets/images/htb/jupiter/image 7.png" /><br/>
<br/>
If we utilize the firefox extension `Wappalyzer` we can see `Grafana` is running along with some other information about the technologies being used in this site.<br/>
<br/>
<img src="/assets/images/htb/jupiter/image 8.png" /><br/>
<br/>
At first i tried to use this exploit `https://www.exploit-db.com/exploits/50581` to read an arbitrary file. I tried to read `/etc/passwd` and also `/etc/grafana/grafana.ini` but this exploit did not work. On to the next...<br/>
<br/>
According to this article `https://community.grafana.com/t/sql-injection-in-api-tsdb-query-in-grafana/29713` Grafana is vulnerable to SQLi by using `SELECT` in the rawSql parameter. Let's fire up burp suite and find out if any requests contain `rawSql:` <br/>
<br/>
First turn on firefox extension `FoxyProxy` and turn `intercept off` in burp suite.<br/>
<br/>
<img src="/assets/images/htb/jupiter/image 9.png" /><br/>
<br/>
<img src="/assets/images/htb/jupiter/image 10.png" /><br/>
<br/>
Now let's refresh the page and head to the `HTTP history` tab to check out the recent requests. We see a bunch of requests come in when we to go the homepage of this site but the request we are interested in is the POST request to `/api/ds/query`.<br/>
<br/>
<img src="/assets/images/htb/jupiter/image 11.png" /><br/>
<br/>
We see this request does have `rawSql` parameter so we can utilize `sqlmap` to test for SQL injection.<br/>
If we right click on the request and `copy to file` we can save this request and name it raw.req.<br/>
Now let's check `sqlmap`<br/>
<br/>
<img src="/assets/images/htb/jupiter/image 12.png" /><br/>
<br/>
<img src="/assets/images/htb/jupiter/image 13.png" /><br/>
<br/>
So `rawSql` is injectable... Lets get a shell using `sqlmap`.<br/>
<br/>
<img src="/assets/images/htb/jupiter/image 14.png" /><br/>
<br/>
<img src="/assets/images/htb/jupiter/image 15.png" /><br/>
<br/>
<br/>
And we are postgres, but this shell isnt to stable. Let's send ourselves a reverse shell.<br/>
<br/>
Start up a netcat listner<br/>
<br/>
<img src="/assets/images/htb/jupiter/image 16.png" /><br/>
<br/>
<img src="/assets/images/htb/jupiter/image 17.png" /><br/>
<br/>
<img src="/assets/images/htb/jupiter/image 18.png" /><br/>
<br/>
Time to escalate our privilges<br/>
<br/>
<br/>
<br/>
</body></html>
