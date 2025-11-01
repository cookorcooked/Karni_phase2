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

- Understanding of the different Analyzers took me quite some time.

## Resources:

- [Logic 2 Async Serial Analyzers Guide](https://support.saleae.com/product/user-guide/protocol-analyzers/analyzer-user-guides/using-async-serial)
- [Using Logic to decode UART](https://support.saleae.com/product/user-guide/protocol-analyzers/analyzer-user-guides/using-async-serial/decode-uart)

***

