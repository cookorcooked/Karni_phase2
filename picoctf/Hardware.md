# 1. IQ Test

let your input x = 30478191278.

wrap your answer with nite{ } for the flag.

As an example, entering x = 34359738368 gives (y0, ..., y11), so the flag would be nite{010000000011}.

## Solution:

Since logic gates take only 0s and 1s as their input, I converted our input to binary, `11100011000101001000100101010101110`, this is only 35 bits we add a zero to the start and get `011100011000101001000100101010101110` without changing our original number.

I now used an online logic gate solver and painstakingly made the entire setup in it, adding each logic gate one by one, I then put the inputs into the logic gates serially.

This finally gave me the outputs which I then wraped as told in the description to get the flag.



## Flag:

```
nite{100010011000}
```

## Concepts learnt:

- Learnt about different kinds of logic gates and how to compute them.
  
## Notes:

- Doing the solution by hand on paper would have probably taken lesser time than making the setup online.

## Resources:

- [Online Logic Gate Solver](https://logic.ly/demo/)

***


# 2. I like Logic

i like logic and i like files, apparently, they have something in common, what should my next step be.

## Solution:

I searched up what a .sal extention stands for and the most suitable answer I found was that it stands for Saleae Logic Analyzer which is a capture file from a logic analyzer.

So then I install logic 2 as it can analyze .sal files.

Going to the Analyzers section on logic 2, we have an option to use different analyzers like Async Serial, I2C ans SPI, going to the user guide of logic 2, I observed that Async Serial is one of the only analyzers that works on a single channel and gives data, so I use that with the default settings on input channel 3.

Doing this gives us the analysis of the given challenge.sal file, switching to terminal mode, we can see a bunch of text.

<img width="1858" height="1053" alt="image" src="https://github.com/user-attachments/assets/0e07f875-856b-4475-8032-c9d85eb35839" />

Going through the text, the flag is found. 


## Flag:

```
FCSC{b1dee4eeadf6c4e60aeb142b0b486344e64b12b40d1046de95c89ba5e23a9925}
```

## Concepts learnt:

- Learnt what logic analyzers, what are UART communications and how to decode them with logic.

## Notes:

- Understanding the different Analyzers took me quite some time.

## Resources:

- [Logic 2 Async Serial Analyzers Guide](https://support.saleae.com/product/user-guide/protocol-analyzers/analyzer-user-guides/using-async-serial)
- [Using Logic to decode UART](https://support.saleae.com/product/user-guide/protocol-analyzers/analyzer-user-guides/using-async-serial/decode-uart)

***


# 3. Bare Metal Alchemist

my friend recommended me this anime but i think i've heard a wrong name.

## Solution:

We are given an executable file firmware.elf, we use ghidra to decomplile this. In the main function we see R11 is set to 0xA5 which is our XOR key, further analyzing we can see that the values to be XORed are taken from 0x68, for some reason I couldn't see the values at 0x68 in ghidra so I used the terminal to get the values. 

`cookorcooked@ubuntu25:~/Desktop/CTF$ objdump -s --start-address=0x68 --stop-address=0x200 firmware.elf > dump.txt`

Using the cat command I could see the values from 0x68, this is what some of it looked like.

    cookorcooked@ubuntu25:~/Desktop/CTF$ cat dump.txt
    
    firmware.elf:     file format elf32-little
    
    Contents of section .text:
     0068 f1e3e6e6 f1e3def1 cd94d6fa 94d6fad6  ................
     0078 cac896fa d694c8d5 c996fa91 d7c1d094  ................
     0088 cbcafac3 94d7c8d2 91d7c0d8 00001124  ...............$
     0098 1fbecfef d8e0debf cdbf11e0 a0e0b1e0  ................
     00a8 e4eef2e0 02c00590 0d92ae34 b107d9f7  ...........4....
     00b8 21e0aee4 b1e001c0 1d92a735 b207e1f7  !..........5....
     00c8 0e94bb00 0c947001 0c940000 8bb18770  ......p........p
     00d8 8bb985b1 8c7f85b9 08951f92 0f920fb6  ................

Now that we have the values from 0x68 we use a simple XOR decoder with the key as A5 to get the flag. Since just the values at 0x68 didn't give the complete flag, I took the values going past it too and finally got the flag.

<img width="1389" height="611" alt="image" src="https://github.com/user-attachments/assets/515f39ea-1e70-41be-8658-d40e82d90dd6" />

I later went through the main function again and realized that the XORing ends when the program encounters 0x00. Although I got the right flag, I also got the random charcters after XORing as I took more values than were needed. 


## Flag:

```
TFCCTF{Th1s_1s_som3_s1mpl3_4rdu1no_f1rmw4re}
```

## Concepts learnt:

- Learnt how to analyze a main function and decrypt data from a program.

## Notes:

- Should've taken more time reading the main function till I realized the end conditions, instead of doing the trial and error to get the flag.

## Resources:

- [Online XOR decoder](https://md5decrypt.net/en/Xor/)

***


