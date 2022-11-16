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

To understand this one, you must keep in mind two things: a program can be thought of a large array of bytes, some of them are instructions, data, etc..., and the variables in your code are stored in it are just in this memory lane, and they are stored upwards (in the following example first would come the c then the b and then the a). The second thing is that linux is little endian. In the following example when a = 0x1f2f3f4f5f6f7f8f, it is stored as 8f,7f,6f,5f,4f,3f,2f,1f. 

The following code:

```c
    unsigned long long a = 0x1f2f3f4f5f6f7f8f;
    unsigned long long* b = &a;
    int* c = (int*) b;
    printf("%llx,%x,%x\n",*b,*c,*(c+1));
```
Produces the result:

```bash
1f2f3f4f5f6f7f8f,5f6f7f8f,1f2f3f4f
```

This is because (for *c) in the first 4 bytes of memory there is the sequence 8f,7f,6f,5f which for an int it means 5f6f7f8f (little endian), and the same can be said about *(c+1).

Something similar will happen if we change from a lesser to a bigger pointer, we will just be looking at a bigger portion of the memory lane.
