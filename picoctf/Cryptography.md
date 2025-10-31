# 3. RSA Oracle

Can you abuse the oracle? An attacker was able to intercept communications between a bank and a fintech company. They managed to get the message (ciphertext) and the password that was used to encrypt the message.

## Solution:

Since the program decrpyts anything but the password, we find a loophole to do the same, we use 2^e mod n * m^e mod n and then divide it by 2, and since it is not the password directly, it will decrypt it for us.

    from pwn import *
    connection = remote('titan.picoctf.net', 61339)
    response = connection.recvuntil('decrypt.')
    print(response.decode())
    payload = b'E' + b'\n'
    connection.send(payload)
    response = connection.recvuntil('keysize):')
    print(response.decode())
    payload = b'\x02' + b'\n'
    connection.send(payload)
    response = connection.recvuntil('ciphertext (m ^ e mod n)')
    response = connection.recvline()
    num=int(response.decode())*2336150584734702647514724021470643922433811330098144930425575029773908475892259185520495303353109615046654428965662643241365308392679139063000973730368839
    response = connection.recvuntil('decrypt.')
    print(response.decode())
    payload = b'D' + b'\n'
    connection.send(payload)
    response = connection.recvuntil('decrypt:')
    print(response.decode())
    connection.send(str(num)+'\n')
    response = connection.recvuntil('hex (c ^ d mod n):')
    print(response.decode())
    response = connection.recvline()
    print(response.decode())
    num=int(response,16)//2
    print(hex(num))
    hex_string=hex(num)[2:] 
    byte_array=bytes.fromhex(hex_string)
    print(byte_array.decode('ascii'))
    connection.close()

On running the script we get the password 60f50, using the openssl command, you get the flag

    openssl enc -aes-256-cbc -d -in secret.enc -k 60f50
    *** WARNING : deprecated key derivation used.
    Using -iter or -pbkdf2 would be better.
    picoCTF{su((3ss_(r@ck1ng_r3@_60f50766}
    
## Flag:

```
picoCTF{su((3ss_(r@ck1ng_r3@_60f50766}
```

## Concepts learnt:

- Learnt about RSA encyrption

## Notes:

- This challenge cooked me.

## Resources:

- NONE

***

