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
6. Moving towards an innovative debugging API based on userland exec and userland ptrace
7. Extremely easy to use.

## Examples

Note: All programs are using the ELF_LOAD_F_FORENSICS flag so that all of these
programs will work on ELF executables with corrupted section headers.

### A version of checksec to check binary security

$ ./checksec test_stripped
RELRO: Partial RELRO enabled
Stack canaries: Disabled
Full ASLR: Disabled
DEP: Enabled-- by default when x86_64 NX bit is set
PaX: None
$ ./checksec /usr/sbin/sshd
RELRO: Full RELRO enabled
Stack canaries: Enabled
Full ASLR: Enabled
DEP: Enabled-- by default when x86_64 NX bit is set
PaX: None
$ ./checksec pax_test
RELRO: Partial RELRO enabled
Stack canaries: Enabled
Full ASLR: Disabled
DEP: Enabled-- with PaX mprotect restrictions
PaX: |MPROTECT|RANDMMAP

### A program to get the PLT entries of an executable

 ./plt_dump test32_stripped
0x8048358: __libc_start_main
0x8048348: puts
0x8048338: __stack_chk_fail
0x8048328: pause
0x8048318: PLT-0

### A version of ldd written in just a few lines of code

$ ./ldd /usr/sbin/sshd
libc.so.6                      -->	/lib/x86_64-linux-gnu/libc.so.6
ld-linux-x86-64.so.2           -->	/lib/x86_64-linux-gnu/ld-linux-x86-64.so.2
libcom_err.so.2                -->	/lib/x86_64-linux-gnu/libcom_err.so.2
libpthread.so.0                -->	/lib/x86_64-linux-gnu/libpthread.so.0
libkrb5.so.3                   -->	/usr/lib/x86_64-linux-gnu/libkrb5.so.3
libk5crypto.so.3               -->	/usr/lib/x86_64-linux-gnu/libk5crypto.so.3
libdl.so.2                     -->	/lib/x86_64-linux-gnu/libdl.so.2
libkrb5support.so.0            -->	/usr/lib/x86_64-linux-gnu/libkrb5support.so.0
libkeyutils.so.1               -->	/lib/x86_64-linux-gnu/libkeyutils.so.1
libresolv.so.2                 -->	/lib/x86_64-linux-gnu/libresolv.so.2
libgssapi_krb5.so.2            -->	/usr/lib/x86_64-linux-gnu/libgssapi_krb5.so.2
libcrypt.so.1                  -->	/lib/x86_64-linux-gnu/libcrypt.so.1
libz.so.1                      -->	/lib/x86_64-linux-gnu/libz.so.1
libutil.so.1                   -->	/lib/x86_64-linux-gnu/libutil.so.1
libcrypto.so.1.0.0             -->	/usr/lib/x86_64-linux-gnu/libcrypto.so.1.0.0
libsystemd.so.0                -->	/lib/x86_64-linux-gnu/libsystemd.so.0
librt.so.1                     -->	/lib/x86_64-linux-gnu/librt.so.1
liblzma.so.5                   -->	/lib/x86_64-linux-gnu/liblzma.so.5
liblz4.so.1                    -->	/usr/lib/x86_64-linux-gnu/liblz4.so.1
libgpg-error.so.0              -->	/lib/x86_64-linux-gnu/libgpg-error.so.0
libgcrypt.so.20                -->	/lib/x86_64-linux-gnu/libgcrypt.so.20
libselinux.so.1                -->	/lib/x86_64-linux-gnu/libselinux.so.1
libpcre.so.3                   -->	/lib/x86_64-linux-gnu/libpcre.so.3
libpam.so.0                    -->	/lib/x86_64-linux-gnu/libpam.so.0
libcap-ng.so.0                 -->	/lib/x86_64-linux-gnu/libcap-ng.so.0
libaudit.so.1                  -->	/lib/x86_64-linux-gnu/libaudit.so.1
libaudit.so.1                  -->	/lib/x86_64-linux-gnu/libaudit.so.1
libwrap.so.0                   -->	/lib/x86_64-linux-gnu/libwrap.so.0
libnsl.so.1                    -->	/lib/x86_64-linux-gnu/libnsl.so.1

### A program to print each .got.plt entry of an executable

$ ./pltgot ./test
GOT (0x601000): 0x600e20 DYNAMIC SEGMENT
GOT (0x601008): 00000000 LINKMAP POINTER
GOT (0x601010): 00000000 __DL_RESOLVE POINTER
GOT (0x601018): 0x400436 PLT STUB
GOT (0x601020): 0x400446 PLT STUB
GOT (0x601028): 00000000 
GOT (0x601030): 00000000 
GOT (0x601038): 0x625528203a434347 
GOT (0x601040): 0x332e372075746e75 
$ ./pltgot ./test_stripped
GOT (0x601000): 0x600e20 DYNAMIC SEGMENT
GOT (0x601008): 00000000 LINKMAP POINTER
GOT (0x601010): 00000000 __DL_RESOLVE POINTER
GOT (0x601018): 0x400436 PLT STUB
GOT (0x601020): 0x400446 PLT STUB
GOT (0x601028): 00000000 
GOT (0x601030): 00000000 
GOT (0x601038): 0x625528203a434347 
GOT (0x601040): 0x332e372075746e75 
$ ./pltgot ./test32_stripped
GOT (0x804a000): 0x8049f14 DYNAMIC SEGMENT
GOT (0x804a004): 00000000 LINKMAP POINTER
GOT (0x804a008): 00000000 __DL_RESOLVE POINTER
GOT (0x804a00c): 0x8048356 PLT STUB
GOT (0x804a010): 0x8048366 PLT STUB
GOT (0x804a014): 0x8048376 
GOT (0x804a018): 0x8048386 
GOT (0x804a01c): 00000000 
GOT (0x804a020): 00000000 
GOT (0x804a024): 0x3a434347 
GOT (0x804a028): 0x62552820 


