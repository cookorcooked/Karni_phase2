# 1. Trivial Flag Transfer Protocol

Figure out how they moved the flag.

## Solution:

Opening the .pcapng file on wireshark, we see multiple TFTP packets. Exporting the TFTP objects gives us multiple files.
The instruction.txt file looked to be encrypted so I used a Cipher Identifier to know it is a ROT-13 cipher. 

Decoding the text gives the message "TFTPDOESNTENCRYPTOURTRAFFICSOWEMUSTDISGUISEOURFLAGTRANSFER.FIGUREOUTAWAYTOHIDETHEFLAGANDIWILLCHECKBACKFORTHEPLAN"

Doing the same for the file "plan" gives us "IUSEDTHEPROGRAMANDHIDITWITH-DUEDILIGENCE.CHECKOUTTHEPHOTOS"

The program.deb file is an installation file for steghide, so we know we need to use that tool.

Running steghide with the key as "DUEDILIGENCE" on the pictures gives us the flag with picture3.bmp
(Since I was on Windows as I couldn't get the right version of Wireshark on linux. I used an online Steganographic Decoder which does the same thing as steghide.)

<img width="1916" height="899" alt="image" src="https://github.com/user-attachments/assets/48ee6f81-3d93-405d-97ce-263de3810df8" />


<img width="1369" height="182" alt="image" src="https://github.com/user-attachments/assets/bd50ce9b-0d95-43f4-b1ec-9f1908a0d820" />


## Flag:

```
picoCTF{h1dd3n_1n_pLa1n_51GHT_18375919}
```

## Concepts learnt:

- Learnt about using Wireshark to intercept files
- Learnt about ciphers and keys and how steganography works.

## Notes:

- NONE 

## Resources:

- [Cipher Identifier](https://www.dcode.fr/cipher-identifier)
- [ROT-13 Decoder](https://www.dcode.fr/rot-13-cipher)
- [Steganographic Decoder](https://futureboy.us/stegano/decinput.html)

***


# 3. m00nwalk

Decode this message from the moon.

HINT : How did pictures from the moon landing get sent back to Earth?

## Solution:

Googling what the hint says "How did pictures from the moon landing get sent back to Earth?", gives us the answer that SSTV (Slow Scan Television) telecasts were used to send videos back to Earth.
Wikepedia (I realize it's not a super reliable source) confirms this.

Using an online SSTV Decoder gives us an image containing the flag.

<img width="1439" height="913" alt="image" src="https://github.com/user-attachments/assets/0e31b0ab-3567-4694-8cb2-c06d2b590977" />


## Flag:

```
picoCTF{beep_boop_im_in_space}
```

## Concepts learnt:

- Learnt about SSTV and other types of long distance transmissions 

## Notes:

- I'm so glad I looked up the hint cause I never would've gussed SSTV. 

## Resources:

- [How did pictures from the moon landing get sent back to Earth?](https://en.wikipedia.org/wiki/Apollo_11_missing_tapes)
- [SSTV Decoder](https://sstv-decoder.mathieurenaud.fr/)

***
