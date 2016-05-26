##Homework

###Eastern Digital

###Jump Outdated Elephants
>Subcategory: Reversing

>Can you extract the flag from the binary?


Maybe... This challenge handles debugging stripped binaries

We're disassembling a binary executable to get the assembly code (extract the flag)

Downloading the file and placing it on the desktop gives us a Bin file named

<code>154817c0b652025ff4590759439810ebd4a658bd190508ac22aaf8f414766468</code>

A bit sketchy...but okay.
We can get some more information about this file first, click on it for Properties
>Type: executable (application/x-executable)

>Size: 5.6 kB (5,564 bytes)

We can find out even more by going to the Terminal and running the <code>file</code> command on it
<code>file 154817c0b652025ff4590759439810ebd4a658bd190508ac22aaf8f
154817c0b652025ff4590759439810ebd4a658bd190508ac22aaf8f414766468: ELF 32-bit LSB executable, Intel 80386, version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux.so.2, for GNU/Linux 2.6.24, BuildID[sha1]=c448b30706799fa1c26e6badce98f5504f9b654a, stripped
</code>

So the file is a stripped executable- stripped binaries do not include symbols and debugging information

There's more though, its a stripped ELF 32-bit LSB (Linux Standard Base) executable to be exact

We're going to have to decompile it somehow to explore its contents.

This can be done using <code>readelf</code> and <code>objdump</code>.

Before we try to do that, we can pull the Strings out of the file in the terminal using the <code>strings</code> command
>/lib/ld-linux.so.2

>libc.so.6

>[...]

>I've tricked you once again!

>Gratz, the flag is '%s'

>Hehe, you're on the wrong track...

>Hmmm, I don't think that's correct...

>[...]

Apart from the sass, the file contains these strings too:

>wub4uiaoiq9uhywibWb9y

>Tei5kechei4ahVoy9thie

>Boemeit0ohCh9opohriet

>li0Li6chahPha0sagohto

>aethee1thaehaiCheifeH

>zeechair8Eigh0iegughi

>[...]

Nothing helpful, we cannot run it either, we're going to have to debug it.
This can be done using GDB (GNU Project Debugger) in the terminal. Our goal is to get the main() function.

><code>gdb 154817c0b652025ff4590759439810ebd4a658bd190508ac22aaf8f414766468</code>

><code>(gbd) info files</code>

What we want is the Entry Point address
><code>Entry point: 0x80485f6</code>

Set a break point
><code>(gdb) break *0x080485f6</code>

>Breakpoint 1 at 0x80485f6

To find the main() function- it should be the last argument passed into libc

Now that we have the main() function, we need to disassemble it

![Using GDB to debug stripped binaries](http://felix.abecassis.me/2012/08/gdb-debugging-stripped-binaries/)
