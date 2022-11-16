## Type conversions and the effects on their data

In this topic i will cover some type conversions between data of different sizes and types (something that when i didn't know gave me some hard to find buggs)


<summary>Signed to unsigned</summary>

The typical case is very simple, any conversion between signed and unsigned preserves the data of the variable in the state is was,
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

```bash
4294967295, ffffffff
-1, ffffffff
```

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

<summary>Cast to a bigger size</summary>

This one is straight forward, it will simply "add" zeros to the variable that was being used and will keep the signedness. For example the following:

```c
    int a = 'a';
    long long b = (long long) a;
    printf("%d,%lld\n", a, b);

    unsigned int c = 0xffffffff;
    unsigned long long d = (unsigned long long) c;
    printf("%u,%llu\n", c, d);
```

Produces the result:

```bash
97,97
4294967295,4294967295
```

>**Note**
>When doing any type of computation the type matters, for example if unsigned i = 0xffffffff and in some operation we do 2*i will overflow, however ((unsigned long long)i) * 2 will not.

```c
    int a = 2147483647; // __INT_MAX__
    long long b = ((long long)a )*2;
    printf("%d,%lld\n",a*2,b);
```

Produces the output

```bash
-2,4294967294
```

<summary>Cast to a smaller size</summary>

In this case this is pretty straigth, it will simply take the lowest number of bytes equivalent to the size of the new type and copy them for example:

```c
    unsigned long long a = 0xffffffffafbfcfdf;
    int b = (int) a;
    unsigned c = (unsigned) a;
    printf("%llx,%x,%x\n",a,b,c);
```

Produces the following output:

```bash
ffffffffafbfcfdf,afbfcfdf,afbfcfdf
```

<summary>Cast one pointer type to another</summary>




