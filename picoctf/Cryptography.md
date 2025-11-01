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

Reading the script we can understand how it works to get the flag, first we need to calculate the shared-key, then get the semi-cipher which is basically the plaintext reversed which is then XORed with the key "trudeau". And then get the fincal cipher which is the ASCII values of semicipher which is multiplied by shared-key*311. 
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


***


# 3. Mini RSA

What happens if you have a small exponent? There is a twist though, we padded the plaintext so that (M ** e) is just barely larger than N. Let's decrypt this: ciphertext

## Solution:

We usually have ciphertext c as `c ≡ M^e (mod N)`, the original text was padded such that M^3 is only a bit larger than N, so we use `M^3 = c + k*N` we then brutefore this by running a loop for the values of k, until the value is a perfect cube, upon which we can cube root it and convert it to bytes to read the text.


    N = 1615765684321463054078226051959887884233678317734892901740763321135213636796075462401950274602405095138589898087428337758445013281488966866073355710771864671726991918706558071231266976427184673800225254531695928541272546385146495736420261815693810544589811104967829354461491178200126099661909654163542661541699404839644035177445092988952614918424317082380174383819025585076206641993479326576180793544321194357018916215113009742654408597083724508169216182008449693917227497813165444372201517541788989925461711067825681947947471001390843774746442699739386923285801022685451221261010798837646928092277556198145662924691803032880040492762442561497760689933601781401617086600593482127465655390841361154025890679757514060456103104199255917164678161972735858939464790960448345988941481499050248673128656508055285037090026439683847266536283160142071643015434813473463469733112182328678706702116054036618277506997666534567846763938692335069955755244438415377933440029498378955355877502743215305768814857864433151287
    c = 1220012318588871886132524757898884422174534558055593713309088304910273991073554732659977133980685370899257850121970812405700793710546674062154237544840177616746805668666317481140872605653768484867292138139949076102907399831998827567645230986345455915692863094364797526497302082734955903755050638155202890599808147276605782889813772992918898400408026067642464141885067379614918437023839625205930332182990301333585691581437573708925507991608699550931884734959475780164693178925308303420298715231388421829397209435815583140323329070684583974607064056215836529244330562254568162453025117819569708767522400676415959028292550922595255396203239357606521218664984826377129270592358124859832816717406984802489441913267065210674087743685058164539822623810831754845900660230416950321563523723959232766094292905543274107712867486590646161628402198049221567774173578088527367084843924843266361134842269034459560612360763383547251378793641304151038512392821572406034926965112582374825926358165795831789031647600129008730
    e = 3
    
    def int_to_bytes(n):
        size = (n.bit_length() + 7) // 8
        return n.to_bytes(size, 'big')
    
    def cube_root(n):
        low, high = 0, n
        while low <= high:
            mid = (low + high) // 2
            if mid**3 == n:
                return mid, True
            elif mid**3 < n:
                low = mid + 1
            else:
                high = mid - 1
        return high, False
    
    for k in range(100000):
        value = c + k * N
        m, ok = cube_root(value)
        if ok:
            print("flag:", int_to_bytes(m))
            break

Running this script gives us the flag.

    cookorcooked@ubuntu25:~/Desktop/CTF$ python3 plswork.py
    flag: b'                                                                                                        picoCTF{e_sh0u1d_b3_lArg3r_85d643d5}'


## Flag:

```
picoCTF{e_sh0u1d_b3_lArg3r_85d643d5}
```

## Concepts learnt:

- Learnt about RSA encryption and why e should be a large value.

## Notes:

- NONE

## Resources:

- NONE

***



