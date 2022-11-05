## EVERYTHING ABOUT ASSEMLBY

>**Note**
>I currently use ubuntu with an intel x86_64 processor and use the gcc assembler

First of all we need to know that binaries in linux are elf files. These files are separated into sections, which you can see with the info file command in the gdb, some of which are the following:


|   Name   |    description     |
|---------:|--------------------|
|.interp   |In this section there is a path to a dynamic loader (it is a helper program that helps load shared libraries with the program, prepares the progam, and loads it)|

All programs in assembly are divided in different parts called sections, which are all specified with something called directives, which all start with ".", the three most common are:

```asm
.data
-- These are 
.text
--In this section we can put asselmby code
.bss
--In here we allocate the global variables and also any with the static keyword
```