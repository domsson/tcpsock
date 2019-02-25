# tcpsnob

A thin abstraction on top of Unix sockets that simplifies the creation 
and use of non-blocking TCP sockets. Should work for IPv4 and IPv6.

##  Interface

### `tcpsnob_create()`

Returns a file descriptor for a non-blocking TCP socket or -1 on error.
`ip_type` should be either `AF_INET` for IPv4 or `AF_INET6` for IPv6.

    int tcpsnob_create(int ip_type);
    
---
    
### `tcpsnob_connect()`

Connects the socket `sockfd` to `host` on `port`. `ip_type` should be 
the same as used for the call to `tcpsnob_create()`. Returns 0 if the 
connection has been established, otherwise -1.

    int tcpsnob_connect(int sockfd, int ip_type, const char *host, const char *port);
    
---

### `tcpsnob_status()`

Attempts to figure out the socket's status via `getsockopt()`. 
Returns 0 if the socket seems to be connected, -1 otherwise.

    int tcpsnob_status(int sockfd);
    
---

### `tcpsnob_send()`

Send the bytes in `msg` using the given socket. `len` is assumed to be 
the size of `msg` in bytes. Returns the number of bytes sent or -1 if 
sending failed.

    int tcpsnob_send(int sockfd, const char *msg, size_t len);
    
---
    
### `tcpsnob_receive()`

Fetches a maximum of `len` bytes from the socket via `recv()` and places
the data in `buf`. Returns the number of bytes received, 0 if the socket
connection was closed by the peer or -1 on error.

    int tcpsnob_receive(int sockfd, char *buf, size_t len);
    
---

### `tcpsnob_close()`

Closes the given socket using `close()`, which also ends the connection 
to the peer. Returns 0 on success, -1 on error.

    int tcpsnob_close(int sockfd);
    
    






