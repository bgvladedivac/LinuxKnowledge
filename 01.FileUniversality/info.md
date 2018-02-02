## Everything is a file
An often saying of Unix-like OS is that everything is a file. Imagine a file in the context of a word processor. There are two fundamental operations u could do: <br />
1. Read it <br />
2. Write to it <br />

Consider some of the commont things attached to a computer and how they relate to out fundamental file operations: <br />
1. The screen is read/write. <br />
2. The printer is write only. <br />
3. The CD-ROM is read/write. <br />

The concept of a file is a good abstraction of either a sink for, or source of data. Its a great abstaction of all the devices one might attach to the computer. It is one of the fundamental roles of the OS to provide this abstraction of the hardware to the programmer. The biggest advantage is the fact that you can use common tools(API, command line utilities ...) to operate on different things(text files, block devices...).

## Impelementing abstraction
Abstraction in terms of **everything is a file** is imlemented by **Application Programming Interface**(API). A programmer designs a set of functions and documents their **interface** (with which arguements they are called, the return value that they have, we do not care about the inner implementation of the function, but what it wants as arguments and the result that it brings back). From there on, the API could be provided to any external programmer/side to use it in their application.

## System calls and file descriptors
**System call** is the fundamental interface between an application and the Linux kernel. System calls are how a program enters the kernel to perform some task. Programs use system calls to perform a variety of operations, such as creating processes, doing network and file I/O ... <br /> 

The following are the four key system calls for performing I/O(programming languages and software packages typically employ these calls only indirectly, via I/O libaries). <br />
* **fd = open(pathaneme, flags, mode)** opens the file identified by pathname, returning a file descriptor used to refer to the open file in subsequent calls. The flags argument also specifies whether the file is to be opened for reading, writing, or both. The mode specifies the permissions to be placed on the file if it is created by this call.<br />
* **numread = read(fd, buffer, count)** reads at most count bytes from the open file referred to by fd and stores them in buffer. The read() call returns the number of bytes actually read. <br />
* **numwritten = write(fd, buffer, count)** writes up to count bytes from buffer to the open file referred to by fd. The write() call returns the number of bytes actually written, which may be less than count. <br />
* **status = close(fd)** is called after all I/O has been completed, in order to release the file descriptor fd and its associated kernel resources. <br />

System calls are generally not invoked directly, but rather via wrapper functions in glibc. Each system call returns a **file descriptor**, a small nonnegative integer for use in subsequent system calls. Each process has 3 file descriptors already opened:<br />

0 => standart input. <br />
1 => standart ouput. <br /> 
2 => standart error. <br />

The command **strace** will display the system calls that a process makes as it runs with the **-c** option.

```c
The following code, opens tfile twice, write once and read once via two different file descriptors.

#include < string.h >
#include < unistd.h >
#include < fcntl.h >

int main (void)
{
    int fd[2];
    char buf1[12] = "just a test";
    char buf2[12];

    fd[0] = open("tfile",O_RDWR);
    fd[1] = open("tfile",O_RDWR);
    
    write(fd[0],buf1,strlen(buf1));
    write(1, buf2, read(fd[1],buf2,12));

    close(fd[0]);
    close(fd[1]);

    return 0;
}
The output is then:

UNIX> a.out
just a testUNIX> 

``` 

The same system calls(**open()**, **read()**, **write()**, **close()** ... ) are used to perform I/O on all types of files, including devices. 
