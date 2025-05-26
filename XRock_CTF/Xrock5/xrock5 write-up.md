*chernuha, bro, where are the other 4??*

From the previous morse task, we have webpage with an SSH connect link
`ssh -p 2233 xrock5@qfxazzhumg.chernuha.xyz`
I run a rustscan on `qfxazzhumg.chernuha.xyz`, even if it's not necessary. I will not try to hack into the XRrock's infrasctructure, but u can try. 

*If you found a bug, please, contact with a developer via method, that provided in the XRock's telegram channel, or via `@MrForust` and `@chernuha_dev` telegram accounts (we are friendly. If you are speaking English - MrForust is the preferable option, but, anyway, message them all), or via email: `bobrovod@national.shitposting.agency`, but the response may take up a long time*

`rustscan -a qfxazzhumg.chernuha.xyz --ulimit 5000` didn't gave us much:
![[Pasted image 20250526020025.png]]
A whole bunch of ports. The most reasonable explanation is that they set up decoy-services, so I'll dismiss this scan

Let's try to log into the SSH session:
`ssh -p 2233 xrock5@qfxazzhumg.chernuha.xyz`
`xrock5@qfxazzhumg.chernuha.xyz password:`

We've should expect, that SSH will be protected with a password, which we don't have. Let's dive into the HTML of [qfxazzhumg.chernuha.xyz](qfxazzhumg.chernuha.xyz):
![[Pasted image 20250526020649.png]]
We have a \<P>aragraph, that is hidden because of `display: none;` css atribute:
> old habits die hard — try /data.txt

Let's head there:

![[Pasted image 20250526020930.png]]
> dyhfdhv1hu
// Our mom used to make Caesar salad for us.

`Ceasar` certainly means a [Ceasar cipher](https://en.wikipedia.org/wiki/Caesar_cipher)
I searched for an `Ceasar decod` and used [cryptii](https://cryptii.com/pipes/caesar-cipher) to play around with a decipher key (letters offset)
![[Pasted image 20250526022741.png]]
Some time later, i got a human-readable phrase, so let's try it as an SSH pass:
And it worked
![[Pasted image 20250526022851.png]]