
# 1. ImmaDev

## Solution:

When the program runs, we can see that there are 3 options to choose from, but we cannot use option 2 as it says we need root privileges.

Opening the program in Ghidra, we can start to analyze the code, looking into the function handleOption, the vulnerability becomes clear.

Here is the pseudo code that has the vulnerability that we can exploit. 

```
  if ((local_428[0] == 2) && (_Var3 = geteuid(), _Var3 != 0)) {
    bVar2 = true;
  }
  else {
    bVar2 = false;
  }
  if (bVar2) {
    poVar5 = std::operator<<((ostream *)std::cout,"Error: Option  2 requires root privileges HAHA");
    std::ostream::operator<<(poVar5,std::endl<>);
  }
  else {
    for (local_5cc = 0; local_5cc < local_5d0; local_5cc = local_ 5cc + 1) {
      iVar1 = local_428[local_5cc];
      if (iVar1 == 3) {
        login();
      }
      else if (iVar1 < 4) {
        if (iVar1 == 1) {
          sayHello();
        }
        else if (iVar1 == 2) {
          printFlag();
```
The program only blocks us if the first number is 2 and we are not in root, but if the number is any other option we can bypass this.
The program loops through every number we input, so if we enter the input `3 2`, the root check only checks 3, then the loop runs, takes 2 as input and gives us the flag regardless of the username and password we type.

```
cookorcooked@ubuntu25:~/Desktop/Curated/Imma Dev/src$ nc immadeveloper.nitephase.live 61234
Hi I'm sudonymouse!
I'm learning development, checkout this binary!
Option 1: Hello <USER>
Option 2: Flag(maybe?)
Option 3: Log into my binary!
3 2
Input Username: 
hello
Enter Password: 
hi
lmeow i forgot to make the db
nite{n0t_4ll_b1n3x_15_st4ck_b4s3d!}
```


## Flag:

```
nite{n0t_4ll_b1n3x_15_st4ck_b4s3d!}
```

## Concepts learnt:

- Learnt how to navigate decompiled code in Ghidra.
- Learnt to find flaws in logic of a program and how it can be exploited.

## Notes:

- NONE
  
## Resources:

- NONE

***
