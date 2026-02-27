# TryHackMe Writeup: GoBuster Introduction

Learning Objectives
- Understanding the basics of enumeration
- How to use Gobuster to enumerate web directories and files
- How to use Gobuster to enumerate subdomains
- How to use Gobuster to enumerate virtual hosts
- How to use a wordlist

## Environment Setup

sudo nano /etc/resolv-dnsmasq

nameserver 10.66.183.122 <TARGET_IP_ADDRESS>


## Task 3

Question: Flag to specify the target url

```bash
-u http://example.com
```

Question: Command to enumerate subdomains

```bash
dns
```

## Task 4

Question: Flag to skip SSL certificate verification

```bash
-k	or --no-tls-validation
```

Enumerate the web directories and files on www.offensivetools.thm using the following command:

```bash
gobuster dir -u www.offensivetools.thm -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -r -t 64
```

Enumeratins js files on www.offensivetools.thm using the following command:

```bash
gobuster dir -u www.offensivetools.thm -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -r -x js -t 64
```

## Task 5

Using GoBuster to enumerate subdomains

```bash
gobuster dns -d example.com -w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt
```

Apart dns command and wordlist specification, the -d flag is also required to specify the domain to enumerate subdomains for.

Using GoBuster command to enumerate offensive tools subdomains

```bash
gobuster dns -d offensivetools.thm -w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-20000.txt
```

## Task 6

Using GoBuster to enumerate virtual hosts

```bash
gobuster vhost -u "http://10.66.183.122" --domain offensivetools.thm  -w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt --append-domain --exclude-length 250-320 

--domain offensivetools.thm sets the top- and second-level domains in the Hostname: part of the request to offensivetools.thm.
--append-domain appends the configured domain to each entry in the wordlist. If this flag is not configured, the set hostname would be www, blog, etc. This will cause the command to work incorrectly and display false positives.

--exclude-length 250-320 excludes any response with a content length between 250 and 320 bytes. This is useful to exclude false positives that may be returned by the server for non-existent virtual hosts.

```

