```bash
# Nmap 7.91 scan initiated Thu Sep  2 20:59:32 2021 as: nmap --min-rate 1000 -T4 -sS -Pn -n -p- -v -o explore.out 10.10.10.2  
47  
Warning: 10.10.10.247 giving up on port because retransmission cap hit (6).  
Nmap scan report for 10.10.10.247  
Host is up (0.41s latency).  
Not shown: 65530 closed ports  
PORT      STATE    SERVICE  
2222/tcp  open     EtherNetIP-1  
5555/tcp  filtered freeciv  
42135/tcp open     unknown  
44783/tcp open     unknown  
59777/tcp open     unknown  
  
Read data files from: /usr/bin/../share/nmap  
# Nmap done at Thu Sep  2 21:00:52 2021 -- 1 IP address (1 host up) scanned in 79.98 seconds
```

As per google, ports `42135` and `59777` are used by ES File Explorer
This is an Android Box

ES File Explorer has a [known vulnerability](https://www.exploit-db.com/exploits/50070)

Port 2222 is running SSH so we can expect credentials
```bash
$ nc -nv 10.10.10.247 2222  
Connection to 10.10.10.247 2222 port [tcp/*] succeeded!  
SSH-2.0-SSH Server - Banana Studio
```
