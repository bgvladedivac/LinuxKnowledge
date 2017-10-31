## Everything is a file
An often saying of Unix-like OS is that everything is a file. Imagine a file in the context of a word processor. There are two fundamental operations u could do: <br />
1. Read it <br />
2. Write to it <br />

Consider some of the commont things attached to a computer and how they relate to out fundamental file operations: <br />
1. The screen is read/write. <br />
2. The printer is write only. <br />
3. The CD-ROM is read/write. <br />

The concept of a file is a good abstraction of either a sink for, or source of data. Its a great abstaction of all the devices one might attach to the computer. It is one of the fundamental roles of the OS to provide this abstraction of the hardware to the programmer.

## Impelementing abstraction
Abstraction in terms of **everything is a file** is imlemented by **Application Programming Interface**(API). A programmer designs a set of functions and documents their **interface** (with which arguements they are called, the return value that they have, we do not care about the inner implementation of the function, but what it wants as arguments and the result that it brings back). From there on, the API could be provided to any external programmer/side to use it in their application.

## System calls and file descriptors
**System call** is the fundamental interface between an application and the Linux kernel. System calls are generally not invoked directly, but rather via wrapper functions in glibc. Each system call returns a **file descriptor**, a small, nonnegative interger for use in subsequent system calls.

```c
#include <unistd.h>
#include <fcntl.h>
 
int main()
{
    int filedesc = open("testfile.txt", O_WRONLY | O_APPEND);
    if(filedesc < 0)
        return 1;
 
    if(write(filedesc,"This will be output to testfile.txt\n", 36) != 36)
    {
        write(2,"There was an error writing to testfile.txt\n");    // strictly not an error, it is allowable for fewer characters than requested to be written.
        return 1;
    }
 
    return 0;
}
``` 
