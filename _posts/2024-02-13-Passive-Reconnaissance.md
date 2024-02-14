---
layout: post
title:  "THM: Passive Reconnaissance"
date:   2024-02-13 18:00:00 +0000
categories: Red
image: /blog/assets/img/pass-reconnaissance-hdr.png
---
Ref: 
- [THM Passive Reconnaissance:](https://tryhackme.com/room/passiverecon?path=undefined)

---

Task1: Indroduction
===
Welcome to the first room of the Network Security Module. This module covers:

1. [Passive Reconnaissance](https://tryhackme.com/room/passiverecon)
2. [Active Reconnaissance](https://tryhackme.com/room/activerecon)
3. [Nmap Live Host Discovery](https://tryhackme.com/room/nmap01)
4. [Nmap Basic Port Scans](https://tryhackme.com/room/nmap02)
5. [Nmap Advanced Port Scans](https://tryhackme.com/room/nmap03)
6. [Nmap Post Port Scans](https://tryhackme.com/room/nmap04)
7. [Protocols and Servers](https://tryhackme.com/room/protocolsandservers)
8. [Protocols and Servers 2](https://tryhackme.com/room/protocolsandservers2)
9. [Network Security Challenge](https://tryhackme.com/room/netsecchallenge)


In this room, after we define passive reconnaissance and active reconnaissance, we focus on essential tools related to passive reconnaissance. We will learn three command-line tools:

- whois to query WHOIS servers
- nslookup to query DNS servers
- dig to query DNS servers

We use whois to query WHOIS records, while we use ```nslookup``` and ```dig``` to query DNS database records. These are all publicly available records and hence do not alert the target.

We will also learn the usage of two online services:

- DNSDumpster
- Shodan.io

These two online services allow us to collect information about our target without directly connecting to it.

Pre-requisites: 

---

This room requires basic networking knowledge along with basic familiarity with the command line. The modules [Network Fundamentals](https://tryhackme.com/module/network-fundamentals) and [Linux Fundamentals](https://tryhackme.com/module/linux-fundamentals) provide the required knowledge if necessary.

## Important Notice: 
Please note that if ```you're not subscribed```, the AttackBox ```won't have Internet access```, so you will need to use the ```VPN to complete the questions that require Internet access```.

Task2: Passive Versus Active Recon
===

This room expects the user to have a working knowledge of computer networks. If you like to brush up on this topic, you are encouraged to study the Network Fundamentals module first.

Before the dawn of computer systems and networks, in the Art of War, Sun Tzu taught, “If you know the enemy and know yourself, your victory will not stand in doubt.” 

If you are playing the role of an ```attacker```, you need to gather information about your target systems. 

If you are playing the role of a ```defender```, you need to know what your adversary will discover about your systems and networks.

```Reconnaissance (recon)``` can be defined as a preliminary survey to ```gather information about a target```. It is the first step in [The Unified Kill Chain](https://www.unifiedkillchain.com/) to gain an initial foothold on a system. We divide reconnaissance into:

1. Passive Reconnaissance
1. Active Reconnaissance

In ```passive reconnaissance```, you rely on ```publicly available knowledge```. It is the knowledge that you can access from publicly available resources without directly engaging with the target. Think of it like you are looking at target territory from afar without stepping foot on that territory.

Passive reconnaissance activities include many activities, for instance:

- Looking up DNS records of a domain from a public DNS server.
- Checking job ads related to the target website.
- Reading news articles about the target company.

Active reconnaissance, on the other hand, cannot be achieved so discreetly. It requires direct engagement with the target. Think of it like you check the locks on the doors and windows, among other potential entry points.
Examples of active reconnaissance activities include:

---

- Connecting to one of the company servers such as HTTP, FTP, and SMTP.
- Calling the company in an attempt to get information (social engineering).
- Entering company premises pretending to be a repairman.
Considering the invasive nature of active reconnaissance, one can quickly get into legal trouble unless one obtains proper legal authorisation.


You visit the Facebook page of the target company, hoping to get some of their employee names. What kind of reconnaissance activity is this? (A for active, P for passive)
```
P
```
You ping the IP address of the company webserver to check if ICMP traffic is blocked. What kind of reconnaissance activity is this? (A for active, P for passive)
```
A
```

You happen to meet the IT administrator of the target company at a party. You try to use social engineering to get more information about their systems and network infrastructure. What kind of reconnaissance activity is this? (A for active, P for passive)
```
A
```

Task3: Whois
===
WHOIS is a request and response protocol that follows the [RFC 3912](https://www.ietf.org/rfc/rfc3912.txt) specification. 

A WHOIS server ```listens on TCP port 43 for incoming requests```. The domain registrar is responsible for maintaining the WHOIS records for the domain names it is leasing. 

The WHOIS server replies with various information related to the domain requested. Of particular interest, we can learn:

- Registrar: Via which registrar was the domain name registered?
- Contact info of registrant: Name, organization, address, phone, among other things. (unless made hidden via a privacy service)
- Creation, update, and expiration dates: When was the domain name first registered? When was it last updated? And when does it need to be renewed?
- Name Server: Which server to ask to resolve the domain name?

To get this information, we need to use a ```whois``` client or an online service. Many online services provide ```whois``` information; however, it is generally faster and more convenient to use your local whois client. 

Using the AttackBox (or your local Linux machine, such as Parrot or Kali), you can easily access your whois client on the terminal. The syntax is ```whois DOMAIN_NAME```, where DOMAIN_NAME is the domain about which you are trying to get more information. 

Consider the following example executing ```whois tryhackme.com```.
```
user@TryHackMe$ whois tryhackme.com
[Querying whois.verisign-grs.com]
[Redirected to whois.namecheap.com]
[Querying whois.namecheap.com]
[whois.namecheap.com]
Domain name: tryhackme.com
Registry Domain ID: 2282723194_DOMAIN_COM-VRSN
Registrar WHOIS Server: whois.namecheap.com
Registrar URL: http://www.namecheap.com
Updated Date: 2021-05-01T19:43:23.31Z
Creation Date: 2018-07-05T19:46:15.00Z
Registrar Registration Expiration Date: 2027-07-05T19:46:15.00Z
Registrar: NAMECHEAP INC
Registrar IANA ID: 1068
Registrar Abuse Contact Email: abuse@namecheap.com
Registrar Abuse Contact Phone: +1.6613102107
Reseller: NAMECHEAP INC
Domain Status: clientTransferProhibited https://icann.org/epp#clientTransferProhibited
Registry Registrant ID: 
Registrant Name: Withheld for Privacy Purposes
Registrant Organization: Privacy service provided by Withheld for Privacy ehf
[...]
URL of the ICANN WHOIS Data Problem Reporting System: http://wdprs.internic.net/
>>> Last update of WHOIS database: 2021-08-25T14:58:29.57Z <<<
For more information on Whois status codes, please visit https://icann.org/epp
```

---

We can see plenty of information; we will inspect them in the order displayed. First, we notice that we were redirected to ```whois.namecheap.com``` to get our information. In this case and at the time being, namecheap.com is maintaining the WHOIS record for this domain name. Furthermore, we can see the creation date along with ```the last-update date and expiration date```.

Next, we obtain ```information about the registrar and the registrant```. We can find the registrant’s name and contact information unless they are using some privacy service. 

Although not displayed above, we get the ```admin and tech contacts for this domain```. 

Finally, we see the domain name servers that we should ```query if we have any DNS records to look up```.

The information collected can be inspected to find new attack surfaces, such as social engineering or technical attacks. 

For instance, depending on the scope of the penetration test, you might consider an ```attack against the email server of the admin user or the DNS servers```, assuming they are owned by your client and fall within the scope of the penetration test.

It is important to note that due to automated tools abusing WHOIS queries to harvest email addresses, many WHOIS services take measures against this. 

They might redact email addresses, for instance. Moreover, many registrants subscribe to privacy services to avoid their email addresses being harvested by spammers and keep their information private.

On the AttackBox, open the terminal and run the ```whois tryhackme.com``` command to get the information you need to answer the following questions.

Who was TryHackMe.com registered?
```

```
