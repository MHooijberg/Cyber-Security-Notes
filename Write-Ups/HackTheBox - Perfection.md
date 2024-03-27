---
tags:
  - SSTI
  - Easy
  - Linux
  - HackTheBox
  - Cracking
  - Ruby
  - Webapp
Name: Perfection
Difficulty: Easy
Operating System: Linux
IP Address: 10.10.11.253
hostname: Unknown
Start date: 2024-03-25
End date: 2024-03-27
"Machine page:": https://app.hackthebox.com/machines/Perfection
Discussion page: https://forum.hackthebox.com/t/official-perfection-discussion/309066
---
# Description
---
This box simulates an environment where a software engineering student who's new to secure programming offers tools for other students through a website. The project is sponsored by Susan Miller who's a sys admin at the university.
# Foothold
---
First step after joining the box and getting the IP address is to do enumeration:
## Enumeration:
To start we need to know the ports that are open on the box. We'll do this by doing an Nmap scan, this will scan all ports on a given IP and based on the response returns if this port is closed, filtered or open. On top of this we'll use the `-sC` flag to enable script scanning, `-sV` for version enumeration and `-oN` to save the output to a file with in the Nmap format.

```sh
nmap -sC -sV -oN nmapscan 10.10.11.253
```

This command gave the following result:

```sh
# Nmap 7.94SVN scan initiated Mon Mar 25 11:36:11 2024 as: nmap -sC -sV -oN nmapscan 10.10.11.253
Nmap scan report for 10.10.11.253
Host is up (0.034s latency).
Not shown: 998 closed tcp ports (conn-refused)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.9p1 Ubuntu 3ubuntu0.6 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 80:e4:79:e8:59:28:df:95:2d:ad:57:4a:46:04:ea:70 (ECDSA)
|_  256 e9:ea:0c:1d:86:13:ed:95:a9:d0:0b:c8:22:e4:cf:e9 (ED25519)
80/tcp open  http    nginx
|_http-title: Weighted Grade Calculator
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Mon Mar 25 11:36:19 2024 -- 1 IP address (1 host up) scanned in 8.04 seconds
```

From the result we can see that there are two ports open on the box: 22 and 80. Port 22 is an OpenSSH server which we might be able to use later, however I do not expect the vulnerability to be in this service, because on easy boxes in HackTheBox it needs to be straight forward.

Port 80 is an Nginx web server with the title: "Weighted Grade Calculator". Visiting the site in the browser greets us with the following page:

![[Pasted image 20240327143733.png]]

From this page we can see what they used to create the website: `Powered by WEBrick 1.7.0`.
Exploring the website further gave us an About Us page:

![[Pasted image 20240327144236.png]]

From this page we can see that there are two people working on this project: Tina Smith and Susan Miller. From the explanation we can see some interesting details from the team:

> She is an absolute whiz at web development, but she hasn't delved into secure coding too much.

>She is also a sysadmin here at Acme University.

This would give us hints about poor secure programming practices and that Susan Miller is a possible system administrator for this project. At the moment this information doesn't seem crucial but might come in handy later.

Other than the about us page we can see an page for calculating weighted grades:

![[Pasted image 20240327144816.png]]

In this page we have a few input fields which can be used to calculate the weighted grade in a class. There's a maximum of five categories which can be filled out.

After playing around with the calculator can see that the only allowed character other than letters and numbers is a slash. All other input will give us the following error message:

> Malicious input blocked



# User
---

# Root
---