From the previous task we have a Telegram Channel, which, provides updates for CTF state and this is a place, where developer notes are placed.

But we are interested in only two messages right now: an voice-message and a description for it
> Cipher.
Like someone was trying to tell you something.

![[Pasted image 20250526000256.png]]
`Cipher` means, that we maybe will be working with a [Steganography](https://en.wikipedia.org/wiki/Steganography):
> Steganography is the practice of concealing information within another message or physical object to avoid detection

After a few second of listening, I realize, that this is the morse code *(because of longer and shorter beeps, which represents "dashes" and "dots")*

I looked up `morse decode` in google and went to the https://morsecodetranslator.com/. There, I manually retyped the morse code (later I discovered more convenient method, but it's up to you. My hint for that tool is: `download the audio`)
`..- -- --. .-.-.- -.-. .... . .-. -. ..- .... .- .-.-.- -..- -.-- --..` *(this morse code was cut to match the output, which is on a screenshot below)*

*Fun fact: at the moment of first walkthrough I dropped retyping on half of the recording, as I saw first letters of Dev's username and thought, that it was somekind of autograph. Because of that, i played around a simple morse code task for an hour. **Do not dismiss any "non-critical" info, don't be me pls** *

And got an link with a `chernuha` domain and a `xyz` suffix, which allows us to know, that we are on a right path
![[Pasted image 20250526003817.png]]
After going to the link, we have a message, that this was only beginning and an SSH connect link
![[Pasted image 20250526004017.png]]
