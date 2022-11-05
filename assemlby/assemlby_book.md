## EVERYTHING ABOUT ASSEMLBY

>**Note**
>I currently use ubuntu with an intel x86_64 processor and use the gcc assembler

First of all we need to know that binaries in linux are elf files. These files are separated into sections, some of which are the following:


|   Name   |    description     | 
|---------:|--------------------|

All programs in assembly are divided in different parts called sections, which are all specified with something called directives, which all start with ".", the three most common are:

```asm
.data
-- These are 
.text
--In this section we can put asselmby code
.bss
--In here we allocate the global variables and also any with the static keyword
```