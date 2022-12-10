---
layout: post
title: "Raytheon CTF Business Card"
---

# Raytheon CTF

I was at a trade show recently and I stopped by a Raytheon booth. I was looking for a business contact but what I came away with was even better -- a business card with CTF problems on it. Over the next month or two, I casually worked on the problems presented. You can see an image of the card below.

![Raytheon Card Front](/assets/img/raytheon_card_front.jpg)
![Raytheon Card Back](/assets/img/raytheon_card_back.jpg)

Names and emails have been redacted to protect the innocent. Also, sorry for the potato quality photos. 

# Email

There's a bit of strange encoding on the front of the card: 

```
J7YE0X3QNCD091YZPRITBB13BCLPYIGUWC2G
```

This looked similar to base64 but with a smaller character set. I ran it through [Featherduster](https://github.com/nccgroup/featherduster) and it popped out the following base36 encoded info:


`Send to ctf@raytheon.com`

Thank god for featherduster. I've been burned on this kind of problem before, and it's a great tool to have in your back pocket for strange encodings.

# Brainfuck

The first problem I decided to tackle was a strange set of characters below.

```brainfuck
++++3++++2++[>b+>++i+>+++t++++ >+++R++++C+++<4<<<-]:>>>. >+++2++++A+.--E-----2----5.+++4+++.2<---8----4-----3.+++3++++D++++F+++++C++++E++.>6----1--.+3+++++C++++5++++2+++.B----0-----C----9-.++4++++F++++A+.-.
```

If you're not familiar, this strange syntax is an esoteric programming language called [Brainfuck](https://en.wikipedia.org/wiki/Brainfuck). The language isn't intended to be a useful language. It only has a data pointer with a few operations such as `>` to move the data pointer to the right `<` to move the data pointer to the left, `+` which increases the value at the data pointer, `-` which decreases the value at the data pointer, and a few other operators to enable loops, print to the screen or read from `stdin`. 

All I did to solve this problem was to run it. You can run it locally, or find an online Brainfuck interpreter such as [this one](https://www.tutorialspoint.com/execute_brainfk_online.php). 

The only difficult problem was to figure out exactly how the BF program was constructed as it's split between two parts. 

If you run the program you get the following printed to `stdout`. 

`Flag:Pardon`

# NFC

The card contains a [NFC](https://en.wikipedia.org/wiki/Near-field_communication) chip. This chip contains the following info: 

```
QlpoOTFBWSZTWWXD59sAAJrfgEAQUW+9UrlH3Iu/79/wQAIdacbqduGhKn5qaTE2mpqfqjMoaNNDTRkGTZQBolP0xCmp5Bo01DQAAyMTQNAaZCpkTyJ5QAbTUBoMQNADBKCBTU9Q2KbU wmmhoAADQbUGSr62ReEGvrP2sv6rqgosFRakdRIwQZPCRkiW7PLzVBqpKjIPo/x63O6yMBgx4HK3g8YNqOYapxkNcIxYuIq3ZVB2cno9QQ4itD3ur314WdgmWQQs6aGxqoxHjY6DRW0PI2yrY QbPFbICnsneL1DhsvnAtiRGTBWIP8WFcZjY7AwwtdTg83RwVIZqOh9WmOn0vF3fm7THnrQbRlVYTpaIinT9+HNNt2nGKwN4iq5MmbUEAQWCBBPcwzDcBe+rHshhRd2gF357wWdlN6xROdR1UZ9UjvhMw/KFXKIBCR8TUwIIiLWkPiLTyj9/7FZJqtmuBEzMYBpoQswoNMjKC5DdTUPJzdlFSEnzvE7JQUzsRtaiihjVICnKZHHutOsZ1A2+m2QRI4IXJLBwoub63C68yvkAxTyJMMEBmQjJmiE9f3 mkLorMKhmxMSEhRiDUIjqOTMyJJ5ipMlg4zskRXog8o1omq8ilu6ajcCOvHSfFM9y833MMqBlKrLbP7FvQCGJILQ4UnCAkP4UTHbJkubHaljRGTHQy2EqIYmYk7kuiM1YYUd1tVSITGimJCJhbbnpQd7QAlAQ4hVNwkoYswoLieSLKTXSHnaLuSKcKEgy4fPtg
```

A hint is given that its [base64](https://en.wikipedia.org/wiki/Base64) encoded and [bzip2](https://en.wikipedia.org/wiki/Bzip2) compressed. Once we extract the base64 encoded data, and [bunzip2](https://linux.die.net/man/1/bunzip2) it, we get the following data in a file:

```
Flag:b64_bz_unwrap

Challenge 1:
Let f(n) be the number of 1s in the binary representation of n^13 + 12 where ^ denote exponent

Let g(n,1) = f(n),
    g(n,2) = f(f(n)),
    g(n,3) = f(f(f(n))), etc.

Find g(3,2^48).

Provide the solution and your code used to find it.


Challenge 2:
There is a long line of bowls with a machine next to each. When bowl[i] has more than 14 M&Ms, the machines take 3 seconds to divide them as follows:
1. N = bowl[i]
2. Simultaneously
   A. bowl[i-1] += N/2 (round down)
   B. bowl[i+1] += N/2 (round down)
   C. bowl[i]    = N mod 2
3. Rest

All bowls start empty. One bowl labeled "B", gets a new M&M every hour. A bowl may receive M&Ms from up to three machines at a time. 

How many days will pass before the machines take a full hour to settle from the chain reaction caused by adding an M&M to B?

Provide the solution and your code used to find it.
```

This contains a flag in and of itself, and some programming challenges. I haven't solved these yet, maybe that'll be another article. As it stands I'm already working on Advent of Code, so time is limited.

`Flag:b64_bz_unwrap`


# QR Code Challenge

On the card is a [QR code](https://en.wikipedia.org/wiki/QR_code). This QR code contains the following binary bytes: 

```
\xeb\x3f\x41\x58\x6a\x00\x48\x89\xe6\x48\xc7\xc7\x01\x00\x00\x00\x48\xc7\xc2\x01\x00\x00\x00\x43\x8a\x04\x08\x3c\x00\x74\x12\x30\xd8\x88\x06\x48\xc7\xc0\x01\x00\x00\x00\x0f\x05\x49\xff\xc1\xeb\xe6\x48\xc7\xc0\x3c\x00\x00\x00\x48\xc7\xc7\x00\x00\x00\x00\x0f\x05\xe8\xbc\xff\xff\xff\xcc\xee\xf3\xe4\    xa1\xc8\xef\xf2\xf5\xf3\xf4\xe2\xf5\xe8\xee\xef\xf2\xa1\xd1\xed\xe4\xe0\xf2\xe4\xad\xa1\xd2\xe8\xf3\xbb\x89\x81\x81\x92\x81\x81\x81\x81\x82\x61\xc1\xa4\xa2\x3c\x7e\x7d\xa2\x24\x81\x82\xa5\x85\x81\x80\xa5\x87\x81\x80\x10\x88\x81\x81\x90\xa1\x81\x86\x81\x81\x81\x81\x80\xb1\xc9\xa7\x2e\x28\x81\x81\xa5\x83\x8e\x25\    x81\x81\x81\x8d\x89\x81\x81\x86\xa0\x89\x81\x80\xa5\x83\x8e\x20\xa5\x85\x81\x81\x81\x81\x81\x8d\x8d\x81\x81\x83\x81\x81\x81\x81\xdf\xf5\xf8\xfe\xa3\xf8\xeb\xfa\xf1\xf0\xed\xfc\xfa\xed\x81\x81\x81\x81\x81\x81\x81\x81\x81\x81\x81\x81\x81\x81\x00
```

If we disassemble these bytes we get the following

```nasm
0x0:    jmp     0x41                                        
0x2:    pop     r8                                          
0x4:    push    0                                             
0x6:    mov     rsi, rsp                                         
0x9:    mov     rdi, 1                                      
0x10:   mov     rdx, 1                                                                                                                                        
0x17:   mov     al, byte ptr [r8 + r9]                                                                                                                        
0x1b:   cmp     al, 0                                                                                                                                         
0x1d:   je      0x31                                                                                                                                          
0x1f:   xor     al, bl                                                                                                                                        
0x21:   mov     byte ptr [rsi], al                                
0x23:   mov     rax, 1                                      
0x2a:   syscall                                                                                                                                               
0x2c:   inc     r9                                                                                                                                                                                                                                                                                                          0x2f:   jmp     0x17                                        
0x31:   mov     rax, 0x3c                                   
0x38:   mov     rdi, 0                                                                                                                                        
0x3f:   syscall                                                                                                                                                                                                                                                                                                             
0x41:   call    2                                                                                                                                             
0x46:   int3                                                                                                                                                                                                                                                                                                                
0x47:   out     dx, al                                                                                                                                        
0x48:   in      al, 0xa1                                    
0x4b:   enter   -0xd11, -0xb                                                                                                                                  
0x4f:   hlt                                                                                                                                                   
0x51:   loop    0x48                                        
0x53:   call    0xffffffffa1f2f046                          
0x58:   shr     ebp, 1                                                                                                                                        
0x5a:   in      al, 0xe0                                         
0x5c:   in      al, 0xad                                          
0x5f:   movabs  eax, dword ptr [0x92818189bbf3e8d2]                                                                                                           
0x68:   add     dword ptr [rcx + 0x61828181], 0x3ca2a4c1                                                                                                      
0x72:   jle     0xf1                                        
0x74:   movabs  byte ptr [0xa5808185a5828124], al           
0x7d:   xchg    dword ptr [rcx - 0x7e77ef80], eax                                                                                                             
0x83:   adc     dword ptr [rax - 0x7e797e5f], 0x80818181                
0x8d:   mov     cl, 0xc9                                          
0x8f:   cmpsd   dword ptr [rsi], dword ptr [rdi]        
0x90:   sub     byte ptr cs:[rcx - 0x717c5a7f], al                                                                                                                                                                                                                                                                          
0x97:   and     eax, 0x8d818181                                                                                                                                                                                                                                                                                             
0x9c:   mov     dword ptr [rcx - 0x765f797f], eax                                                                                                                                                                                                                                                                           
0xa2:   add     dword ptr [rax + 0x208e83a5], 0x818185a5                                                                                                      
0xac:   add     dword ptr [rcx - 0x7e72727f], 0x81818381      
0xb6:   add     dword ptr [rcx - 0x1070a21], 0xfaebf8a3                
0xc0:   int1

```

The code actually ends at `0x46`, and the rest is just data. The data looks like this:

```
CC EE F3 E4 A1 C8 EF F2 F5 F3 F4 E2 F5 E8 EE EF F2 A1 D1 ED E4 E0 F2 E4 AD A1 D2 E8 F3 BB 89 81 81 92 81 81 81 81 82 61 C1 A4 A2 3C 7E 7D A2 24 81 82 A5 85 81 80 A5 87 81 80 10 88 81 81 90 A1 81 86 81 81 81 81 80 B1 C9 A7 2E 28 81 81 A5 83 8E 25 81 81 81 8D 89 81 81 86 A0 89 81 80 A5 83 8E 20 A5 85 81 81 81 81 81 8
D 8D 81 81 83 81 81 81 81 DF F5 F8 FE A3 F8 EB FA F1 F0 ED FC FA ED 81 81 81 81 81 81 81 81 81 81 81 81 81 81 00
```

If you read the code carefully, you can see that the data section is being XOR'd by a single (unknown) byte. There are several ways to solve this problem, but one give away for a single-byte XOR is that the data is almost certainly going to contain several `0x00` values. When it does, that XOR operation is going to reveal the key. For example if `key = 0x41` then `0x41 XOR 0x00` is going to be `0x41`. In this case, we see the value `0x81` repeating over and over, so it's a good possibility that `0x81` is our key. 

Another way you could have solved it would be to brute force the XOR key until you got something to spit out that seemed reasonable. This is is just as effective, as you only have to search through 255 values (it's almost certainly not `0x00`). 

With the un-xor'd data, we get the following new data:

```More Instructions Please, Sir:```

Followed by more instructions:

```
08 00 00 13 00 00 00 00 03 e0 40 25 23 bd ff fc 23 a5 00 03 24 04 00 01 24 06 00 01 91 09 00 00 11 20 00 07 00 00 00 00 01 30 48 26 af a9 00 00 24 02 0f a4 00 00 00 0c 08 00 00 07 21 08 00 01 24 02 0f a1 24 04 00 00 00 00 00 0c 0c 00 00 02 00 00 00 00 5e 74 79 7f 22 79 6a 7b 70 71 6c 7d 7b 6c 00 00 00 00 00 00 00 00 00 00 00 00 00 00 81
```

These disassemble to the following:
 
 ```nasm
 0:    08 00                    or     BYTE PTR [eax],  al
   2:    00 13                    add    BYTE PTR [ebx],  dl
   4:    00 00                    add    BYTE PTR [eax],  al
   6:    00 00                    add    BYTE PTR [eax],  al
   8:    03 e0                    add    esp,  eax
   a:    40                       inc    eax
   b:    25 23 bd ff fc           and    eax,  0xfcffbd23
  10:    23 a5 00 03 24 04        and    esp,  DWORD PTR [ebp+0x4240300]
  16:    00 01                    add    BYTE PTR [ecx],  al
  18:    24 06                    and    al,  0x6
  1a:    00 01                    add    BYTE PTR [ecx],  al
  1c:    91                       xchg   ecx,  eax
  1d:    09 00                    or     DWORD PTR [eax],  eax
  1f:    00 11                    add    BYTE PTR [ecx],  dl
  21:    20 00                    and    BYTE PTR [eax],  al
  23:    07                       pop    es
  24:    00 00                    add    BYTE PTR [eax],  al
  26:    00 00                    add    BYTE PTR [eax],  al
  28:    01 30                    add    DWORD PTR [eax],  esi
  2a:    48                       dec    eax
  2b:    26 af                    es scas eax,  DWORD PTR es:[edi]
  2d:    a9 00 00 24 02           test   eax,  0x2240000
  32:    0f a4 00 00              shld   DWORD PTR [eax],  eax,  0x0
  36:    00 0c 08                 add    BYTE PTR [eax+ecx*1],  cl
  39:    00 00                    add    BYTE PTR [eax],  al
  3b:    07                       pop    es
  3c:    21 08                    and    DWORD PTR [eax],  ecx
  3e:    00 01                    add    BYTE PTR [ecx],  al
  40:    24 02                    and    al,  0x2
  42:    0f a1                    pop    fs
  44:    24 04                    and    al,  0x4
  46:    00 00                    add    BYTE PTR [eax],  al
  48:    00 00                    add    BYTE PTR [eax],  al
  4a:    00 0c 0c                 add    BYTE PTR [esp+ecx*1],  cl
  4d:    00 00                    add    BYTE PTR [eax],  al
  4f:    02 00                    add    al,  BYTE PTR [eax]
  51:    00 00                    add    BYTE PTR [eax],  al
  53:    00 5e 74                 add    BYTE PTR [esi+0x74],  bl
  56:    79 7f                    jns    0xd7
  58:    22 79 6a                 and    bh,  BYTE PTR [ecx+0x6a]
  5b:    7b 70                    jnp    0xcd
  5d:    71 6c                    jno    0xcb
  5f:    7d 7b                    jge    0xdc
  61:    6c                       ins    BYTE PTR es:[edi],  dx
        ...         ... ...

 ```


I didn't bother looking at the code at this point, as the last bytes of the data look like this: 

```
5e 74 79 7f 22 79 6a 7b 70 71 6c 7d 7b 6c
```

Those values are way to close to the ASCII range. So I threw those bytes into Cyber Chef, and ran a single byte XOR brute force across them to reveal the flag:


`Flag:architect`


How did I know to do that? Well, one part of it was laziness. The other part is just doing problems like this a bunch and recognizing that the values at the end of the data could easily be single-byte XOR'd and this code above just un-xor'd them. Maybe there's something additional I missed. Some other flags that I skipped past. I'm not sure, maybe I'll re-visit this and see if there's more to this problem than meets the eye in the future.


# BCD Problem
I actually needed help on this problem, along with a bit of research. The biggest issue with this problem was to figure out where to start. The provided bytes are the following: 

```
df 28 
df 35 0a 00 00 00 
df 68 08 
df 35 0a 00 00 00 
cc 
46 38 88 15 78 56 54 85 02 54 40 10 00 11 25 14 92 01
```

This disassembles to the following:

```nasm
0x0:    fild    qword ptr [rax]
0x2:    fbstp    tbyte ptr [rip + 0xa]
0x8:    fild    qword ptr [rax + 8]
0xb:    fbstp    tbyte ptr [rip + 0xa]
0x11:   int3    
0x12:   46 38 88 15 78 56 54 85 02 54 40 10 00 11 25 14 92 01
```

This code is a bit strange as many of the instructions aren't very common so let's go through it in detail.

1. `fild` will read an 8-byte signed integer pointed to by `rax` and place it onto the FPU stack.
2. `fbstp` Will write the value of the FPU stack to a pointer (in our case `rip + 0xa`). This data will be 10 bytes long and will be the [Binary Coded Decimal (BCD)](https://en.wikipedia.org/wiki/Binary-coded_decimal) value of the value on the FPU stack.
3. Breakpoint

The big question for this problem is what `rax` is supposed to be. I tried pointing `rax` to the data at the end of the disassembly, but got pretty frustrated that I couldn't turn the bytes of data into anything interesting, a colleague pointed out that this problem might actually be the reverse. That is to say, the ending bytes are the result of the above code's transformations and we need to figure out the starting data instead. 

This was the key insight. Another way to notice this is that all the data in the data section after the disassembly is in BCD format already. This should have been a dead give-away. 

A little more research provided the following assembly code that reverses the above process. 

```nasm
extern puts
    SECTION .text

    global main 
main:
    push rbp 
    mov rbp, rsp 
    lea rax, [buffer + 9]
    lea rbx, [buffer2]
    fbld [rax]
    fistp qword [rel buffer2]

    lea rax, [buffer]
    lea rbx, [buffer+8]
    fbld [rax]
    fistp qword [rel buffer2+8]

    int3
    buffer: db 0x46, 0x38, 0x88, 0x15, 0x78, 0x56, 0x54, 0x85, 0x02, 0x54, 0x40, 0x10, 0x00, 0x11, 0x25, 0x14, 0x92, 0x01
    msg: db "Hello world!", 10, 0

    ;SECTION .data
    buffer2: db 0x46, 0x38, 0x88, 0x15, 0x78, 0x56, 0x54, 0x85, 0x02, 0x54, 0x40, 0x10, 0x00, 0x11, 0x25, 0x14, 0x92, 0x01
```

`Flag:reversBCD`


# Modular Inverse

This was a really fun problem similar to the previous BCD problem where we needed to work backwards. Here's the instructions: 

```nasm
00: xor ecx, ecx
02: inc ecx
04: jmp 0x24
06: pop rsi
07: mov ebx, dword ptr [rsi]
09: mov eax, dword ptr [rdi+rcx*4+4]
0D: mov edx, dword ptr [rsi+rcx*4]
10: mul rdx
13: div rbx
16: dec rdx
19: jne 0x23
1b: inc ecx
1d: cmp ecx, 5
20: jl 9
22: ret
23: hlt
24: call 6
29: 6716bc79
2d: 7e9edd77 f598e85c 798e2172 221b523c
```

This problems take some unknown array of numbers referenced by `rdi` (line `0x09`) and multiplies it by the same index number at line `0x2d`. This is then divided by the value at line `0x29`. The remainder of the division is then checked to be equal to `0x1` by performing a decrement which will set the zero register (kinda clever). This means we have a modular congruency problem:

`a * b =~ 1 (mod c)`

Where a is our unknown number, `b` is one of the numbers given at `0x2d` (`0x7e9edd77 0xf598e85c, 0x798e2172, or 0x221b523c`) and `c` is the value given at `0x29` (`6716bc79`).

If we look at the numbers at `0x2d` we notice that they're all coprime with the value (0x6716bc79). This means we can calculate a multiplicitive inverse to solve this modular congruency. The following code does the proper calculations:

```python3
def gcdExtended(a, b): 

    if a == 0:
        return b, 0, 1

    gcd, x1, y1 = gcdExtended(b % a, a)

    x = y1 - (b//a) *x1 
    y = x1

    return gcd, x, y


a = 0x79bc1667
bs = [0x77dd9e7e, 0x5ce898f5, 0x72218e79, 0x3c521b22]
for b in bs: 
    g, x, y = gcdExtended(b, a)
    mi = x % a 
    for i in range(0, 4): 
        print(chr((mi >> i*8) & 0xff))

```

The above code uses euclid's extended greatest common denominator formula to calculate
the multiplicative inverse. The answer to the problem is that inverse. The final `for` loop at the bottom, simply takes the binary integer and prints out the characters in a nice format to give the final flag:

`Flag:remainder`

# Conclusion

Overall this card was a lot of fun. There was a good problem gradient especially considering it all fits on a business card. I wonder if there are things I missed. Maybe that's something to revisit in the future.
