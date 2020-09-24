# tcpsock

tcpsock is a thin abstraction on top of Unix sockets that simplifies the creation 
and use of TCP sockets, both blocking and non-blocking, as well as IPv4 and IPv6. 
It's a single header file, making it easy to include it in your project. 

To get the implementation, define `TCPSOCK_IMPLEMENTATION` before including `tcpsock.h`.

    #define TCPSOCK_IMPLEMENTATION
    #include "tcpsock.h"

##  Interface

### `int tcpsock_create(int ip_type, int block)`

Returns a file descriptor for a TCP socket or -1 on error.
`ip_type` should be either `TCPSOCK_IPV4` or `TCPSOCK_IPV6`.
`block` should be `TCPSOCK_NONBLOCK` or `TCPSOCK_BLOCK`.

### `int tcpsock_connect(int sockfd, int ip_type, const char *host, const char *port)`

Connects the socket `sockfd` to `host` on `port`. `ip_type` should be 
the same as used for the call to `tcpsock_create()`. Returns 0 if the 
connection has been established, otherwise -1.

### `int tcpsock_blocking(int sockfd)`

Checks if the given socket is blocking. Returns 1 for blocking, 0 for 
non-blocking sockets, -1 if the socket's blocking status couldn't be aquired.

### `int tcpsock_status(int sockfd)`

Attempts to figure out the socket's status via `getsockopt()`. 
Returns 0 if the socket seems to be connected, -1 otherwise.

### `int tcpsock_send(int sockfd, const char *msg, size_t len)`

Send the bytes in `msg` using the given socket. `len` is assumed to be 
the size of `msg` in bytes. Returns the number of bytes sent or -1 if 
sending failed.
    
### `int tcpsock_receive(int sockfd, char *buf, size_t len)`

Fetches a maximum of `len` bytes from the socket via `recv()` and places
the data in `buf`. Returns the number of bytes received, 0 if the socket
connection was closed by the peer or -1 on error.

### `int tcpsock_close(int sockfd)`

Closes the given socket using `close()`, which also ends the connection 
to the peer. Returns 0 on success, -1 on error.

