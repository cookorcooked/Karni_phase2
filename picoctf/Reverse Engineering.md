# 1. GDB baby step 1

Can you figure out what is in the eax register at the end of the main function? Put your answer in the picoCTF flag format: picoCTF{n} where n is the contents of the eax register in the decimal number base. If the answer was 0x11 your flag would be picoCTF{17}. Disassemble this.

## Solution:

Since we are supposed to decompile the given executable file, I decided to learn the basics of ghidra to get the code of the file.
Upon using the CodeBrowser and decompiling the executable file, we can see the different functions used in the file.

<img width="1657" height="949" alt="image" src="https://github.com/user-attachments/assets/a920082e-e6c0-4295-a635-05c6d7083141" />

As the return value of the main function we find the, hexadecimal **0x86342**

Converting the hexadecimal we found to decimal, we get digits **549698**, which gives us the flag.


Since I didn't know what an EAX register is, I decided to search up about it and learnt that EAX is a 32 Bit accumalator and that it holds the values of arithmetic and logical operations. Return values are also stored in the EAX, which is the value we had to find in our case.

## Flag:

```
picoCTF{549698}
```

## Concepts learnt:

- Learnt to use Ghidra to decompile code
- Learnt what is EAX register  

## Notes:

- NONE 

## Resources:

- NONE


***

# 1. ARMssembly 1

For what argument does this program print `win` with variables 81, 0 and 3? File: chall_1.S Flag format: picoCTF{XXXXXXXX} -> (hex, lowercase, no 0x, and 32 bits. ex. 5614267 would be picoCTF{0055aabb})

## Solution:

The file "chall_1.S" given to us contains Assembly code. Since I know nothing about assembly I watch a video about it to understand the way it works and basic commands that were used in the code.

Understanding the function func:
This function has a series of arithmetic operations, let us follow the commands step by step


_func:

	sub	sp, sp, #32      // sp is a special register that acts as the stack pointer, this command allocated space on the stack for computation
	str	w0, [sp, 12]     // Stores the value of w0 register in sp+12, which is a storage location on the stack, this can be x as we don't know it.
	mov	w0, 81           // Stores 81 in w0
	str	w0, [sp, 16]     // sp+16 = 81
	str	wzr, [sp, 20]    // sp+20 = 0     since wzr is the zero register.
	mov	w0, 3            // w0 = 3
	str	w0, [sp, 24]     // sp+24 = 3
	ldr	w0, [sp, 20]     // w0 = 0
	ldr	w1, [sp, 16]     // w1 = 81
	lsl	w0, w1, w0       // Performs logical shift left and stores it in w0, here, w0 = w1 << w0, which gives us w0 = 81 << 0 => w0 = 81
	str	w0, [sp, 28]     // sp+28 = 81
	ldr	w1, [sp, 28]     // w1 = 81
	ldr	w0, [sp, 24]     // w0 = 3
	sdiv	w0, w1, w0     // w0 = w1 / w0   => w0 = 81/3   => w0 = 27
	str	w0, [sp, 28]     // sp+28 = 27
	ldr	w1, [sp, 28]     // w1 = 27
	ldr	w0, [sp, 12]     // w0 = x
	sub	w0, w1, w0       // w0 = 27-x
	str	w0, [sp, 28]     // sp+28 = 27-x
	ldr	w0, [sp, 28]     // w0 = 27-x
	add	sp, sp, 32       // Frees space on the stack
	ret                  // Returns value in w0 ie. 27-x_



## Flag:

```
picoCTF{0000001b}
```

## Concepts learnt:

- Learnt The Basics Of Assembly

## Notes:

- NONE 

## Resources:

- [ Assembly Language Programming with ARM – Full Tutorial for Beginners ](https://youtu.be/gfmRrPjnEw4?si=HBN_2fgWqWfvQdHn)


***


