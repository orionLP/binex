## Type conversions and the effects on their data

In this topic i will cover some type conversions between data of different sizes and types (something that when i didn't know gave me some hard to find buggs)

<details>
<summary>Signed to unsigned</summary>

The resumed case is very simple, any conversion between signed and unsigned preserves the data of the variable in the state is was,
so it simply changes the meaning of the data, for example:

```c
    unsigned int a = 0xffffffff;
    unsigned int * a_pointer = &a;
    int b = (int) a;
    unsigned int * b_pointer = &b;
    printf("%u, %x\n", a, *a_pointer);
    printf("%d, %x\n", b, *b_pointer);
```
produces the result:

>>4294967295, ffffffff
>>-1, ffffffff

which means that the two's complement representation of -1 in 32 bits its the same as the unsinged representation of 4294967295 in 32 bits.

Generaly C considers any number signed unless told otherwise 

>**Note**  
>Any comparation between signed and unsinged numbers implicitly turns all signed numbers to unsigned.

For example:

```c
    int a = -1;
    unsigned b = 2;
    printf("%d\n", a < b);
```

Produces the result 0, since the unsigned representation of a is bigger than b.

<details>


