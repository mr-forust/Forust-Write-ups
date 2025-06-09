# [Biohazard TryHackMe room](https://tryhackme.com/room/biohazard) 
## Task 1 "Introduction": Basic Recon
I started with a basic RustScan recon
### RustScan:
```.----. .-. .-. .----..---.  .----. .---.   .--.  .-. .-.
| {}  }| { } |{ {__ {_   _}{ {__  /  ___} / {} \ |  `| |
| .-. \| {_} |.-._} } | |  .-._} }\     }/  /\  \| |\  |
`-' `-'`-----'`----'  `-'  `----'  `---' `-'  `-'`-' `-'
The Modern Day Port Scanner.
________________________________________
: http://discord.skerritt.blog         :
: https://github.com/RustScan/RustScan :
 --------------------------------------
Port scanning: Making networking exciting since... whenever.

[~] The config file is expected to be at "/home/forust/.rustscan.toml"
[~] Automatically increasing ulimit value to 5000.
Open 10.10.164.78:21
Open 10.10.164.78:22
Open 10.10.164.78:80
[~] Starting Script(s)
[~] Starting Nmap 7.95 ( https://nmap.org ) at 2025-06-09 22:14 EEST
Initiating Ping Scan at 22:14
Scanning 10.10.164.78 [2 ports]
Completed Ping Scan at 22:14, 0.05s elapsed (1 total hosts)
Initiating Parallel DNS resolution of 1 host. at 22:14
Completed Parallel DNS resolution of 1 host. at 22:14, 0.00s elapsed
DNS resolution of 1 IPs took 0.00s. Mode: Async [#: 1, OK: 0, NX: 1, DR: 0, SF: 0, TR: 1, CN: 0]
Initiating Connect Scan at 22:14
Scanning 10.10.164.78 [3 ports]
Discovered open port 22/tcp on 10.10.164.78
Discovered open port 80/tcp on 10.10.164.78
Discovered open port 21/tcp on 10.10.164.78
Completed Connect Scan at 22:14, 0.05s elapsed (3 total ports)
Nmap scan report for 10.10.164.78
Host is up, received syn-ack (0.047s latency).
Scanned at 2025-06-09 22:14:19 EEST for 0s

PORT   STATE SERVICE REASON
21/tcp open  ftp     syn-ack
22/tcp open  ssh     syn-ack
80/tcp open  http    syn-ack

Read data files from: /usr/bin/../share/nmap
Nmap done: 1 IP address (1 host up) scanned in 0.11 seconds
```
So, we have a site, ftp and SSH. *No need to try FTP's "anonymous" login-pass pair, as nmap hasn't found it.*
And we have an answer to the first question
> How many open ports?

### FeroxBuster
For FeroxBuster I use --scan-dir-listings and [raft-large-directories wordlist](https://github.com/danielmiessler/SecLists/blob/master/Discovery/Web-Content/raft-large-directories.txt):
``` ___  ___  __   __     __      __         __   ___
|__  |__  |__) |__) | /  `    /  \ \_/ | |  \ |__
|    |___ |  \ |  \ | \__,    \__/ / \ | |__/ |___
by Ben "epi" Risher 🤓                 ver: 2.11.0
───────────────────────────┬──────────────────────
 🎯  Target Url            │ http://10.10.164.78
 🚀  Threads               │ 50
 📖  Wordlist              │ Programs/Pwned/Wordlist/SecLists/Discovery/Web-Content/raft-large-directories.txt
 👌  Status Codes          │ All Status Codes!
 💥  Timeout (secs)        │ 7
 🦡  User-Agent            │ feroxbuster/2.11.0
 🔎  Extract Links         │ true
 📂  Scan Dir Listings     │ true
 🏁  HTTP methods          │ [GET]
 🔃  Recursion Depth       │ 4
───────────────────────────┴──────────────────────
 🏁  Press [ENTER] to use the Scan Management Menu™
──────────────────────────────────────────────────
404      GET        9l       31w      274c Auto-filtering found 404-like response and created new filter; toggle off with --dont-filter
403      GET        9l       28w      277c Auto-filtering found 404-like response and created new filter; toggle off with --dont-filter
301      GET        9l       28w      313c http://10.10.164.78/images => http://10.10.164.78/images/
200      GET      603l     3783w   334746c http://10.10.164.78/images/Mansion_front.jpg
200      GET       16l       86w      692c http://10.10.164.78/
301      GET        9l       28w      310c http://10.10.164.78/css => http://10.10.164.78/css/
301      GET        9l       28w      309c http://10.10.164.78/js => http://10.10.164.78/js/
200      GET       21l       21w      385c http://10.10.164.78/js/login.js
200      GET       61l       74w      870c http://10.10.164.78/css/login.css
301      GET        9l       28w      312c http://10.10.164.78/attic => http://10.10.164.78/attic/
[####################] - 3m    249141/249141  0s      found:8       errors:1302   
[####################] - 3m     62282/62282   375/s   http://10.10.164.78/ 
[####################] - 3m     62282/62282   372/s   http://10.10.164.78/images/ 
[####################] - 3m     62282/62282   373/s   http://10.10.164.78/mansionmain/ 
[####################] - 3m     62282/62282   385/s   http://10.10.164.78/css/ 
[####################] - 3m     62282/62282   389/s   http://10.10.164.78/js/ 
[####################] - 2m     62282/62282   428/s   http://10.10.164.78/attic/
```
I'll filter out unnecessary entries:
```
200      GET       21l       21w      385c http://10.10.164.78/js/login.js
301      GET        9l       28w      312c http://10.10.164.78/attic => http://10.10.164.78/attic/
[####################] - 3m     62282/62282   373/s   http://10.10.164.78/mansionmain/
```
First row tells us, that we will be dealing with some login pages. Will remember the /attic directory and the /mansionmain, but now let's head to the actual webpage
### "/" web directory
On the machine's IP we have some lore, link to the /mansionmain
![](Pasted%20image%2020250609224115.png)and the answer to the next question:
>What is the team name in operation

## Task 2 "The Mansion"

## /mainmansion
After entering /mansionmain we have a message:
>The team reach the mansion safe and sound. However, it appear that Chris is missing
Jill try to open the door but stopped by Weasker
Suddenly, a gunshot can be heard in the nearby room. Weaker order Jill to make an investigate on the gunshot. Where is the room?

On the page itself there are no any clue about that room, let's dive into HTML code:
![](Pasted%20image%2020250609224512.png)
And we can see a commented line:
`<!-- It is in the /diningRoom/ -->`
Let's head to the `MACHINE_IP/diningRoom`
## /diningRoom
There is a link to an `/diningRoom/emblem.php`
And because I had my WebDevTools opened, I spotted an another commented line: `<!-- SG93IGFib3V0IHRoZSAvdGVhUm9vbS8= -->` and it looks like a base64 codding, heading to the [cyberchef](https://gchq.github.io/CyberChef/)
![](Pasted%20image%2020250609225131.png)
After using autodetect feature, we have a line that was decoded from Base64: `How about the /teaRoom/`
![](Pasted%20image%2020250609225222.png)
Okay, will note that. 
Let's return to the `/diningRoom/emblem.php`, and here we have first flag of the second task:
>What is the emblem flag

Also, the message says:
>Look like you can put something on the emblem slot, refresh /diningRoom/

Let's head back
After refreshing, we can see a new input field 
![](Pasted%20image%2020250609225641.png)
I tried to use emblem's flag, but it only gave me `Nothing happen`
Okay for now let's leave it hanging. We have a /teaRoom - going there
## /teaRoom
There is one link and one new directory:
`/teaRoom/master_of_unlock.html`
and
`/artRoom`
After going to the `/teaRoom/master_of_unlock.html` I've got next flag:
>What is the lock pick flag

Nothing can do here anymore, let's go to /artRoom
## /artRoom 
Here is only one link: `/artRoom/MansionMap.html` and we've got a sitemap:
```
Look like a map

Location:  
/diningRoom/  
/teaRoom/  
/artRoom/  
/barRoom/  
/diningRoom2F/  
/tigerStatusRoom/  
/galleryRoom/  
/studyRoom/  
/armorRoom/  
/attic/
```
I ran katana (crawler) with this sitemap, but it gave nothing useful.
Let's cross-out directories where we've already been
``` 
/barRoom/  
/diningRoom2F/  
/tigerStatusRoom/  
/galleryRoom/  
/studyRoom/  
/armorRoom/  
/attic/
```
Let's continue with a `/barRoom`
![](Pasted%20image%2020250609231159.png)
It asked for a lockpick: I entered lock_pick flag, and it let me in:
![](Pasted%20image%2020250609231256.png)
We have not piano flag, so let's head to the `/barRoom357162e3db904857963e6e0b64b96ba7/musicNote.html`
```
Look like a music note

NV2XG2LDL5ZWQZLFOR5TGNRSMQ3TEZDFMFTDMNLGGVRGIYZWGNSGCZLDMU3GCMLGGY3TMZL5
```
And again some kind of crypto suff. Going to the CyberChef
![](Pasted%20image%2020250609231449.png)
After decoding from Base32 it gave me an answer to the 
>What is the music sheet flag?

question
`music_sheet{362d72deaf65f5bdc63daece6a1f676e}`
Let's enter this flag to the piano input box, and it brought me to the secret room with only one link:
`/barRoom357162e3db904857963e6e0b64b96ba7/gold_emblem.php`
There is another flag:
>What is the gold emblem flag

And another "refreshprevpage" request:
>Look like you can put something on the emblem slot, refresh the previous page

on the `/barRoom357162e3db904857963e6e0b64b96ba7/barRoomHidden.php` a new input box appeared:
>There is an emblem slot on the wall, put the emblem?

Let's put here an emblem flag
It worked, and gave me a single line: `rebecca`
I tried putting `rebecca` into `What is the FTP username` answer, but it didn't work.
Lets proceed with our map
## /diningRoom2F
Nothing useful on the page itself, but there is a commented line in the HTML:
`<!-- Lbh trg gur oyhr trz ol chfuvat gur fgnghf gb gur ybjre sybbe. Gur trz vf ba gur qvavatEbbz svefg sybbe. Ivfvg fnccuver.ugzy -->`
It looks like a Ceasar Cipher. I'll head to the [criptii](https://cryptii.com/pipes/caesar-cipher) and try to brute-force this cipher.
When +13 shift was choosen (a replaces with n), I got a text:
`You get the blue gem by pushing the status to the lower floor. The gem is on the diningRoom first floor. Visit sapphire.html`
okayy, going to `/diningRoom/sapphire.html`
And I got another flag
>What is the blue gem flag

Continuing with a sitemap
## /tigerStatusRoom
On this page, we are being promted to enter gem flag
![](Pasted%20image%2020250609233513.png)
Let's put blue [gem flag](BioHazard%20Flags%20and%20Notes.md) here:
And we've got a bunch of text:
```
crest 1:  
S0pXRkVVS0pKQkxIVVdTWUpFM0VTUlk9  
Hint 1: Crest 1 has been encoded twice  
Hint 2: Crest 1 contanis 14 letters  
Note: You need to collect all 4 crests, combine and decode to reavel another path  
The combination should be crest 1 + crest 2 + crest 3 + crest 4. Also, the combination is a type of encoded base and you need to decode it
```
idk what is this, but let's go to the cyberchef:
![](Pasted%20image%2020250610000855.png)
Auto-Recipe gives me `from base64 -- from base32` chain.
I think we are done here, let's proceed with a sitemap
``` 
/galleryRoom/  
/studyRoom/  
/armorRoom/  
/attic/
```
## /galleryRoom
Here we have a link to a `/galleryRoom/note.txt` with a following text:
```
crest 2:
GVFWK5KHK5WTGTCILE4DKY3DNN4GQQRTM5AVCTKE
Hint 1: Crest 2 has been encoded twice
Hint 2: Crest 2 contanis 18 letters
Note: You need to collect all 4 crests, combine and decode to reavel another path
The combination should be crest 1 + crest 2 + crest 3 + crest 4. Also, the combination is a type of encoded base and you need to decode it
```
CyberChef chain `from Base32 -- from Base58`
![](Pasted%20image%2020250610002019.png)
And we are done here
```   
/studyRoom/  
/armorRoom/  
/attic/
```
## /armorRoom
/*studyRoom will be in the next paragraph. it is skipped due to blockage by the /armorRoom*


## /studyRoom
On the page text tells us, that the door is locked (access is blocked) with a text:
>Look like the door has been locked
A **helmet symbol** is embedded on the door

Let's head to the armor room
