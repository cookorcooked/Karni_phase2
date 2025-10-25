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

# 2. ARMssembly 1

For what argument does this program print `win` with variables 81, 0 and 3? File: chall_1.S Flag format: picoCTF{XXXXXXXX} -> (hex, lowercase, no 0x, and 32 bits. ex. 5614267 would be picoCTF{0055aabb})

## Solution:

The file "chall_1.S" given to us contains Assembly code. Since I know nothing about assembly I watch a video about it to understand the way it works and basic commands that were used in the code.

Understanding the function func:
This function has a series of arithmetic operations, let us follow the commands step by step


	main:
		
		stp	x29, x30, [sp, -48]!
		add	x29, sp, 0
		str	w0, [x29, 28]
		str	x1, [x29, 16]
		ldr	x0, [x29, 16]
		add	x0, x0, 8
		ldr	x0, [x0]
		bl	atoi
		str	w0, [x29, 44]
		ldr	w0, [x29, 44]
		// The code till here reads the input from the user as string, then uses the atoi fuunction to convert ascii to integer, then stores that value in w0, let this value be x.
		
		bl	func
		//func is called
	
	_func:
	
		sub	sp, sp, #32      // sp is a special register that acts as the stack pointer, this command allocated space on the stack for computation
		str	w0, [sp, 12]     // Stores the value of w0 register in sp+12, which gives us sp+12 = x
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
		ret                  // Returns value in w0 ie. 27-x

	//The flow is returned to the main function
		
		cmp	w0, 0  			//compares w0 with 0 
		bne	.L4				//bne is Branch if Not Equal, so the program displays "You Lose :(" if w0 is not equal to 0.

		adrp	x0, .LC0	 
		add	x0, x0, :lo12:.LC0
		bl	puts
		b	.L6
	
	//This block of code displays "You Win!" and skips the "You Lose :(" case and goes to the end

After interpretting the code we can clearly see that we get the win condition if we input the number 27

Converting the answer to hexadecimal and using the flagformat we get the flag.


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

# 3. vault-door-3

This vault uses for-loops and byte arrays. The source code for this vault is here: VaultDoor3.java

## Solution:

We have been given a file "VaultDoor3.java", which is a java file, let us take a look at the code
	
	import java.util.*;
	
	class VaultDoor3 {
	    public static void main(String args[]) {
	        VaultDoor3 vaultDoor = new VaultDoor3();
	        Scanner scanner = new Scanner(System.in);
	        System.out.print("Enter vault password: ");
	        String userInput = scanner.next();
		String input = userInput.substring("picoCTF{".length(),userInput.length()-1);
		if (vaultDoor.checkPassword(input)) {
		    System.out.println("Access granted.");
		} else {
		    System.out.println("Access denied!");
	        }
	    }

This block of code takes the input flag and from the string for example "picoCTF{example_password}" it takes the substring "example_password", the password is then checked using the checkPassword function.

	    public boolean checkPassword(String password) {
	        if (password.length() != 32) {
	            return false;
	        }
	        char[] buffer = new char[32];
	        int i;
	        for (i=0; i<8; i++) {
	            buffer[i] = password.charAt(i);
	        }
	        for (; i<16; i++) {
	            buffer[i] = password.charAt(23-i);
	        }
	        for (; i<32; i+=2) {
	            buffer[i] = password.charAt(46-i);
	        }
	        for (i=31; i>=17; i-=2) {
	            buffer[i] = password.charAt(i);
	        }
	        String s = new String(buffer);
	        return s.equals("jU5t_a_sna_3lpm18gb41_u_4_mfr340");
	    }
	}

This block of code first checks if the string lenght is 32, if yes then it continues to use 4 loops to store a jumbled version of the password in buffer[]. This buffer is then checked with "jU5t_a_sna_3lpm18gb41_u_4_mfr340" if it matches it gives True and the program would return "Access granted."

LOOP 1 : For indices i=0 to 1=7, buffer[i] = password.charAt(i), so those indices remain the same
LOOP 2 : For indices i=8 to 1=15, buffer[i] = password.charAt(23-i), so those indices remain the same
LOOP 3 : For indices i=16 to 1=30, for every other index, buffer[i] = password.charAt(46-i), so those indices remain the same
LOOP 4 : For indices i=17 to 1=31, for every other index, buffer[i] = password.charAt(i), so those indices remain the same

Reversing the string "jU5t_a_sna_3lpm18gb41_u_4_mfr340" using the same computation would give us the password to enter.

![Final password](https://github.com/user-attachments/assets/e7d132bc-43a2-44b3-956a-b8a4c4e3763b)

Wrapping the string "jU5t_a_s1mpl3_an4gr4m_4_u_1fb380" with the flag format gives us the flag.  


## Flag:

```
picoCTF{jU5t_a_s1mpl3_an4gr4m_4_u_1fb380}
```

## Concepts learnt:

- Learnt to use Ghidra to decompile code
- Learnt what is EAX register  

## Notes:

- NONE 

## Resources:

- NONE


***


