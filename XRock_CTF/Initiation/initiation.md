A Write-Up for an Initiation task on XRock CTF

We have an [xrock.chernuha.xyz](https://xrock.chernuha.xyz) site, which is our entrypoint to the main CTF.
![[Pasted image 20250524230412.png]]
Here is some lore info. I'll mainly ignore it here.

After clicking "ENTER", I got a webpage with some text.
Then, I opened web developer tools on F12 and there we can see an 1x1 PNG named 
[initiation.png](https://xrock.chernuha.xyz/initiation/initiation.png)
![[Pasted image 20250524230910.png]]
Opened it in a [new tab](https://xrock.chernuha.xyz/initiation) and saw a link to [/the-noise](https://xrock.chernuha.xyz/the-noise/) , an another image file. Also we've got a clue
> // You're moving blind. The picture looks like garbage. But someone clearly left a mark on it. It's a part of you.

*"a mark"* here maybe means a metadata or an exif tag: let's head to [exif.tools](https://exif.tools/) 

After uploading the-noise.png, in the `comment` section we can see a private telegram link, which leads us to the main CTF.
![[Pasted image 20250525000220.png]]