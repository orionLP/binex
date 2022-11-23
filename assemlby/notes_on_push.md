## Some things about the rbp and rsp registers

The common use for the rbp and rsp registers are to handle function calling.

Something needed to understand this first is that rsp will always point to the last thing that you pushed to the stack, and when you push the following thing happens:

first it is decremented by 8 (only 8 byte pushes are allowed), and then the value is placed at whatever rsp points to.

for example the following code:

```asm
    mov     eax,        0x20
    push    rax
    mov     eax,        0x30
    push    rax
```

leads to the following execution 

```bash
5	    mov     eax,        0x20
(gdb) n
6	    push    rax
(gdb) n

rsp            0x7fffffffdd00      0x7fffffffdd00

(gdb) x/8bx 0x7fffffffdd00
0x7fffffffdd00:	0x20	0x00	0x00	0x00	0x00	0x00	0x00	0x00
```