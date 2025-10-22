# 1. GDB baby step 1

Can you figure out what is in the eax register at the end of the main function? Put your answer in the picoCTF flag format: picoCTF{n} where n is the contents of the eax register in the decimal number base. If the answer was 0x11 your flag would be picoCTF{17}. Disassemble this.

## Solution:

Since we are supposed to decompile the given executable file, I decided to learn the basics of ghidra to get the code of the file.
Upon using the CodeBrowser and decompiling the executable file, we can see the different functions used in the file.

<img width="1657" height="949" alt="image" src="https://github.com/user-attachments/assets/a920082e-e6c0-4295-a635-05c6d7083141" />

Converting the hexadecimal we found to decimal, we get digits 549698, which gives us the flag.

## Flag:

```
picoCTF{549698}
```

## Concepts learnt:

- Include the new topics you've come across and explain them in brief
- 

## Notes:

- Include any alternate tangents you went on while solving the challenge, including mistakes & other solutions you found.
- 

## Resources:

- Include the resources you've referred to with links. [example hyperlink](https://google.com)


***
