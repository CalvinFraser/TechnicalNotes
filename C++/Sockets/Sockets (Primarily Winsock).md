## C0nfiguring a SOCKET
The `SOCKET` type from WinSock is the primary object which is used to identify an underlying Kernel socket used for network traffic.  

## Server-Side
Server side sockets take advantage of passive `listening` sockets which listen on a specific port for connections and then create new sockets from those connections via the `accept` method. The `accept` method is blocking but unblocks once a socket is returned. 

The `SOCKET` returned from the `accept` method is fully configured and can be used to send and receive data. 

## Client side
