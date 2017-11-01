## Device special files/API
A device special file corresponds to a device on the system. Within the kernel, each device type has a corresponding device driver(a unit of kernel code), which handles all I/O requests for the device.  The API provided by device drivers is fixed,  to the system calls **open()**, **close()**, **read()**, **write()**, **mmap()**, and **ioctl()**.

The model that each device driver provides a consistent interface, hiding the differences in operation of individual devices, allows for universality of I/O.

## Main distinguishment Character vs. block devices

## Character devices
Those for which no buffering is performed. They handle data on a character-by-character basis. Terminals and keyboards are exampples of character devices. **Character devices** are read from and written to with two function: foo_read() and foo_write().


**Block devices** must be random access, but character devices are not required to be, though some are. **Filesystems can only be mounted if they are on block devices.**

**Character devices** are read from and written to with two function: foo_read() and foo_write(). The read() and write() calls do not return until the operation is complete. By contrast, block devices do not even implement the read() and write() functions, and instead have a function which has historically been called the ``strategy routine.'' Reads and writes are done through the buffer cache mechanism by the generic functions bread(), breada(), and bwrite(). These functions go through the buffer cache, and so may or may not actually call the strategy routine, depending on whether or not the block requested is in the buffer cache (for reads) or on whether or not the buffer cache is full (for writes). A request may be asyncronous: breada() can request the strategy routine to schedule reads that have not been asked for, and to do it asyncronously, in the background, in the hopes that they will be needed later.

## Impelementing abstraction
Abstraction in terms of **everything is a file** is imlemented by **Application Programming Interface**(API). A programmer designs a set of functions and documents their **interface** (with which arguements they are called, the return value that they have, we do not care about the inner implementation of the function, but what it wants as arguments and the result that it brings back). From there on, the API could be provided to any external programmer/side to use it in their application.

## System calls and file descriptors
**System call** is the fundamental interface between an application and the Linux kernel. System calls are generally not invoked directly, but rather via wrapper functions in glibc. Each system call returns a **file descriptor**, a small, nonnegative interger for use in subsequent system calls. Each process has 3 file descriptors already opened:< br/>
0 => standart input, 1 => standart ouput, 2 => standart error.

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
