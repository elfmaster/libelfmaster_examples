# libelfmaster_examples

Simple ELF tools written to demonstrate libelfmaster capabilities. The libelfmaster
code base will be released in the very near future as a continuous work in progress.
It is still in very early development phase, but could prove useful to people so I
will be releasing it.

## Features

1. libelfmaster aims to be secure, robust, and performant
2. seamlessly parse 32bit/64bit executables with high-level of encapsulation
3. designed to forensically reconstruct malformed executables (i.e. works with bad section headers)
4. Innovative API calls to perform complex tasks such as transitive shared object dependency resolution
5. Various modes or flags to direct how libelfmaster should behave i.e. forensics, strict, or smart.
6. Moving towards a unique debugging API based on a custom userland exec and userland ptrace
7. Extremely easy to use.

## Examples

Note: All programs are using the ELF_LOAD_F_FORENSICS flag so that all of these
programs will work on ELF executables with corrupted section headers, missing
section headers, etc.

### checksec.c

A version of checksec to check binary security that works on stripped or malformed
executables.

### plt_dump.c/plt_dump2.c

two programs to get the PLT entries of an executable

### ldd.c

A version of ldd written in just a few lines of code

### elfparse.c

a simplified readelf like utility demonstrating many features

### pltgot.c

A program to print each .got.plt entry of an executable

### symbols.c

displays symbols from .symtab and .dynsym sections Even on prog

### sections.c

displays section header data, even on programs with no section headers

### eh_frame.c

Displays the function addresses and size of each function referred to by .eh_frame


