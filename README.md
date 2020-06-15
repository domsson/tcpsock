# tcpsnob

tcpsnob is a thin abstraction on top of Unix sockets that simplifies the creation 
and use of non-blocking TCP sockets, both IPv4 and IPv6. It's a single header file,
making it easy to include it in your project. 

To get the implementation, define `TCPSNOB_IMPLEMENTATION` before including `tcpsnob.h`.

    #define TCPSNOB_IMPLEMENTATION
    #include "tcpsnob.h"

##  Interface

### `int tcpsnob_create(int ip_type)`

Returns a file descriptor for a non-blocking TCP socket or -1 on error.
`ip_type` should be either `TCPSNOB_IPV4` or `TCPSNOB_IPV6`.

### `int tcpsnob_connect(int sockfd, int ip_type, const char *host, const char *port)`

Connects the socket `sockfd` to `host` on `port`. `ip_type` should be 
the same as used for the call to `tcpsnob_create()`. Returns 0 if the 
connection has been established, otherwise -1.

### `int tcpsnob_status(int sockfd)`

Attempts to figure out the socket's status via `getsockopt()`. 
Returns 0 if the socket seems to be connected, -1 otherwise.

### `int tcpsnob_send(int sockfd, const char *msg, size_t len)`

Send the bytes in `msg` using the given socket. `len` is assumed to be 
the size of `msg` in bytes. Returns the number of bytes sent or -1 if 
sending failed.
    
### `int tcpsnob_receive(int sockfd, char *buf, size_t len)`

Fetches a maximum of `len` bytes from the socket via `recv()` and places
the data in `buf`. Returns the number of bytes received, 0 if the socket
connection was closed by the peer or -1 on error.

### `int tcpsnob_close(int sockfd)`

Closes the given socket using `close()`, which also ends the connection 
to the peer. Returns 0 on success, -1 on error.

