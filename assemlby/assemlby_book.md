## EVERYTHING ABOUT ASSEMLBY

>**Note**
>I currently use ubuntu with an intel x86_64 processor and use the gcc assembler for the examples

First of all we need to know that binaries in linux are elf files. These files are separated into sections, which you can see with the info file command in the gdb, some of which are the following:


|   Name   |                                            description                                                          |
|---------:|-----------------------------------------------------------------------------------------------------------------|
|.interp   |In this section there is a path to a dynamic loader (it is a helper program that helps load shared libraries with the program, prepares the progam, and loads it)                                                                              |
|.rodata   |In this section there is data that cannot be modified (such as const variables)                                  |
|.data     |In this section there is data that has been initialized and is modifiable (such as global variables)             |
|.bss      |In this section there is data that has not been initialized for global variables, at the start you will only see empty space                                                                                                                  |
|.text     |In this section there is only code to be executed                                                                |

You may ask where are the other variables in your code, well they do not "exist" until your program runs. Any variable inside of the code is simply a number or empty space in the .text area awaiting to be executed.

Now we will look into an example of this through the gcc assemlber. But before that i would like to talk about assembler directives.They are parts of the assemlby code that start with a dot and each one has different effects but for now lets look at these:

```asm
.data
/ Transforms the current section into a data section.
.text
/ Transforms the current section into a text section.
.string "example"
/ Allocates directly into the code a string "example" as a 0 ended sequence of bytes.
.globl name
/ Tells the linker that an object "name" is global (it can be seen by the rest of the code even if it is not in the same file) (it also creates the object)
.type name, @type
/ Tells the assemlber what type the object "name" is (@object,@function)
.size name,bytes
/ Tells the assemlber the size in bytes of the object "name"
.section .rodata
/ Transforms the current section into a read only data section.
.bss
/ Transforms the current section into a text section.
```

Here the example, a simple hello world with some extra unnecessary variables:

```c
const int a = 0x20202020;
int b = 0x10101010;
long long c;
int main(){
    int d;
    printf("hellothere");
    return 0;
}
```