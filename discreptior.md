### "everything is a file theory" unix physcology

treats diverse system resources—regular files, directories, devices, network sockets, and inter-process communication channels—using a unified file interface which is a file descriptor.

When you open a file, device, socket, or pipe in Unix, the kernel returns a file descriptor 

**file descriptor** :- A file descriptor is a non-negative integer that the operating system assigns to a process as a unique identifier(or "handle").

example: 

`open("/etc/passwd", ...)` → kernel sees a path, looks it up in the filesystem, returns fd (file descriptor integer like 3,4,5) pointing to a regular file on disk

`socket(AF_INET, SOCK_STREAM, 0)` → kernel allocates a network socket (no disk involved), returns fd pointing to that socket

now we can give file descriptor the integer 4 and ask for the data bytes it will check which process is linked to fd and gives the data back ez.



### if network buffer is limited how we download big files?

- Server starts sending the 100 MB file
- Your kernel's TCP receive buffer (let's say 87 KB default) fills up with the first ~87 KB
- TCP flow control tells the server "buffer full, slow down"

-io.ReadAll internally runs a loop calling read(fd, buffer, size) repeatedly
- Each read() call copies some bytes (typically 4-32 KB chunks) from the kernel buffer into your application's memory

- As space frees up in the kernel buffer, TCP tells the server "send more data"
