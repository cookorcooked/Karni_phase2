
# 1. JoyDivision

## Solution:

Using Ghidra to get the decompiled code, when we look at the main function it becomes clear to us that the functions flipBits and expand functions are the ones manipulating the flag. 

Analzying flipBits, we can observe that it alternates between flipping the bits and XORing them with a key that changes by the same amount everytime.
This can easily be reversed. The XOR key is first 0x69 and 0x20 is added for the next XOR everytime.

Analyzing expand, we see that, it takes the input and doubles the lenght of it by splitting it's top 4 and bottom 4 bits and it mixes it with a key's bottom and top 4 bits.
This too can be reversed.

Now we just write a python script to reverse the 3 expand functions and the flipBits function to get the flag from the cipher
```
d=open("flag.txt","rb").read()
def f(x):
 r=bytearray(x);k=105;t=0
 for i in range(len(r)):
  if t:r[i]^=k;k=(k+32)&255
  else:r[i]=~r[i]&255
  t^=1
 return bytes(r)
def g(z):
 y=bytearray(len(z)//2);t=0;i=j=0
 while i<len(z):
  a,b=z[i],z[i+1]
  y[j]=((b&240)|(a&15)) if t==0 else ((a&240)|(b&15))
  t^=1;i+=2;j+=1
 return y
for _ in range(3):d=g(d)
print(f(d).decode())

```
I saved this in "reversefuncs.py", using this python script we get out flag.

```
cookorcooked@ubuntu25:~/Desktop/Curated/JoyDivision$ python3 reversefuncs.py
sunshine{C3A5ER_CR055ED_TH3_RUB1C0N}
```


## Flag:

```
sunshine{C3A5ER_CR055ED_TH3_RUB1C0N}
```

## Concepts learnt:

- Learnt how to navigate decompiled code in Ghidra.
- Learnt how to reverse operations on a flag.

## Notes:

- NONE
  
## Resources:

- NONE

***
