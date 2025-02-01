For this challenge we can starts off with nmap scan. 
```
┌──(dr4kali㉿kali)-[~/CTF/Codefest/fullpwn/webminds]
└─$ sudo nmap -sC -sV 192.168.1.7 | tee -a nmap.txt
Starting Nmap 7.95 ( https://nmap.org ) at 2025-02-01 22:45 +0530
Nmap scan report for 192.168.1.7
Host is up (0.000065s latency).
Not shown: 997 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.9p1 Ubuntu 3ubuntu0.10 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 cf:e5:6a:28:d8:49:55:25:18:4a:a1:9a:ac:49:e2:fc (ECDSA)
|_  256 32:db:f1:8f:56:fd:66:88:fb:df:40:40:4f:42:84:b7 (ED25519)
53/tcp open  domain  ISC BIND 9.18.30-0ubuntu0.22.04.1 (Ubuntu Linux)
| dns-nsid: 
|_  bind.version: 9.18.30-0ubuntu0.22.04.1-Ubuntu
80/tcp open  http    Apache httpd 2.4.52
|_http-server-header: Apache/2.4.52 (Ubuntu)
|_http-title: Did not follow redirect to http://webminds.local/
MAC Address: 00:0C:29:0D:38:AB (VMware)
<SNIP>
```

We see that domain is `webminds.local`  and port 53 is opened as well. Which means this machine host a  DNS server. Lets try to gather details from it.

```
┌──(dr4kali㉿kali)-[~/CTF/Codefest/fullpwn/webminds]
└─$ dig axfr webminds.local @192.168.1.7                         

; <<>> DiG 9.20.4-3-Debian <<>> axfr webminds.local @192.168.1.7
;; global options: +cmd
webminds.local.         604800  IN      SOA     ns1.webminds.local. admin.webminds.local. 2023010801 604800 86400 2419200 604800
webminds.local.         604800  IN      NS      ns1.webminds.local.
webminds.local.         604800  IN      A       127.0.0.1
internal-invoiceapp-dev.webminds.local. 604800 IN A 127.0.0.1
ns1.webminds.local.     604800  IN      A       127.0.0.1
webminds.local.         604800  IN      SOA     ns1.webminds.local. admin.webminds.local. 2023010801 604800 86400 2419200 604800
;; Query time: 4 msec
;; SERVER: 192.168.1.7#53(192.168.1.7) (TCP)
;; WHEN: Sat Feb 01 22:49:30 +0530 2025
;; XFR size: 6 records (messages 1, bytes 239)

```

Since the DNS server wasn't secured properly we can perform a DNS zone transfer get details from it.

As you can see there are 2 interesting subdomains `internal-invoiceapp-dev` and `admin`. We can add these to our /etc/hosts file and move on with other services now.

As port 80 is opened we can try to visit it on the browser. When we visit the initial web page it says that the website is under maintenance. We can also try visiting other subdomains. And interestingly `internal-invoiceapp-dev.webminds.local` lead us to invoice generating web page. 
Now lets open burpsuite and generate invoice for ourselves to see how this works.

![[webminds-1.png]]

It generates the invoice and redirect us to `download.php` to download it. This is interesting because we can probably try to perform LFI to download system files.

![[webminds-2.png]]

Well as we can read files now, we can try to read any available database file or ssh keys. Since we don't know exact files names to read, we can starts with the ones we know such as `download.php`, `index.php`.  As there was nothing usefull on them we can try to bruteforce to find out available files on the server.  With a FUZZ scan for directory we can see that there is an `admin.php` file. Lets try reading it.

![[webminds-3.png]]


We have some creds now. We know that there is a user named marcus(read /etc/passwd). Lets try logging in as marcus with available creds.

And it works. Now we can try to enumerate the machine for further escalation. I will use linpeas to make it easier.

linpeas tells us that /etc/passwd file writable. We can open up the passwd file with editor and remove x in here.
```
root:x:0:0:root:/root:/bin/bash
```

when x letter is present it means that password is stored in shadow file but when we remove that  it assumes that there is no password set.

Now we can login as root without a password and get the flag.