Nmap
```bash
# Nmap 7.92 scan initiated Mon Oct 18 12:58:04 2021 as: nmap --min-rate 1000 -sC -sV -Pn -p- -v -T4 -oN nmap.out 10.10.86.147
Nmap scan report for 10.10.86.147
Host is up (0.66s latency).
Not shown: 62601 filtered tcp ports (no-response), 2932 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 37:96:85:98:d1:00:9c:14:63:d9:b0:34:75:b1:f9:57 (RSA)
|   256 53:75:fa:c0:65:da:dd:b1:e8:dd:40:b8:f6:82:39:24 (ECDSA)
|_  256 1c:4a:da:1f:36:54:6d:a6:c6:17:00:27:2e:67:75:9c (ED25519)
80/tcp open  http    Golang net/http server (Go-IPFS json-rpc or InfluxDB API)
|_http-title: Overpass
|_http-favicon: Unknown favicon MD5: 0D4315E5A0B066CEFD5B216C8362564B
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Read data files from: /usr/bin/../share/nmap
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Mon Oct 18 13:11:57 2021 -- 1 IP address (1 host up) scanned in 832.23 seconds
```

Feroxbuser
```bash
301        2l        3w       42c http://10.10.86.147/admin
301        0l        0w        0c http://10.10.86.147/css
301        0l        0w        0c http://10.10.86.147/img
301        0l        0w        0c http://10.10.86.147/downloads
301        0l        0w        0c http://10.10.86.147/downloads/src
301        0l        0w        0c http://10.10.86.147/aboutus
```

