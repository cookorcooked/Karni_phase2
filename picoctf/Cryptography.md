# 1. RSA Oracle

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

# 2. Custom Encryption

Can you get sense of this code file and write the function that will decode the given encrypted file content. Find the encrypted file here flag_info and code file might be good to analyze and get the flag.

## Solution:

Reading the script we can understand how it works to get the flag, first we need to calculate the shared-key, then get the semi-cipher which is basically the plaintext reversed which is then XORed with the key "trudeau". And then get the fincal cipher which is the ASCII values of semicipher which is multiplied by shared-key *311. 
Making a script for this, we get the flag.

    p = 97
    q = 31
    a = 89
    b = 27
    key = "trudeau"
    
    cipher_list = [33588, 276168, 261240, 302292, 343344, 328416, 242580, 85836, 82104, 156744, 0, 309756, 78372, 18660, 253776, 0, 82104, 320952, 3732, 231384, 89568, 100764, 22392, 22392, 63444, 22392, 97032, 190332, 119424, 182868, 97032, 26124, 44784, 63444]
    
    shared_key = pow(q, a * b, p)
    
    divisor = shared_key * 311
    semi_cipher = ""
    for c_val in cipher_list:
        ascii_val = c_val // divisor
        semi_cipher += chr(ascii_val)
    
    key_lenqth = len(key)
    p_rev = ""
    for i, char in enumerate(semi_cipher):
        key_char = key[i % key_lenqth]
        decrypted_char = chr(ord(char) ^ ord(key_char))
        p_rev += decrypted_char
    
    flag = p_rev[::-1]
    print(f"final flag: {flag}")

Running this script we get, 

    cookorcooked@ubuntu25:~/Desktop/CTF$ python3 custom.py
    final flag: picoCTF{custom_d2cr0pt6d_dc499538}


    
## Flag:

```
picoCTF{custom_d2cr0pt6d_dc499538}
```

## Concepts learnt:

- Learnt how to analyze an encryption script and reverse it.

## Notes:

- NONE

## Resources:

- NONE

