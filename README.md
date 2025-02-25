Overview of Chat Applications Using Sockets in C
Sockets are endpoints for communication between processes. In this overview, we discuss two types of socket-based chat applications in C:

Unix Domain Sockets (AF_UNIX) → For communication between processes on the same machine.
Internet Domain Sockets (AF_INET) → For communication between machines over a network (TCP/IP).
1. Unix Domain Sockets (AF_UNIX)
Uses file-based communication instead of networking.
Works only on the same system.
More efficient than TCP/IP since it avoids network overhead.
How it Works:
Server:

Creates a socket (socket()).
Binds it to a special file (/tmp/unix_socket).
Listens for a client connection (listen()).
Accepts a client and starts communication (accept()).
Client:

Creates a socket and connects to the server (connect()).
Sends and receives messages using send() and recv().
Use Case: Inter-process communication (IPC) within the same machine.

2. Internet Domain Sockets (AF_INET) - TCP
Uses IPv4 and TCP to enable communication over a network.
Allows communication between different machines.
Data is transmitted in a reliable, ordered, and error-checked way using TCP.
How it Works:
Server:

Creates a socket (socket()).
Binds it to an IP and port (bind()).
Listens for incoming connections (listen()).
Accepts a client and starts communication (accept()).
Client:

Creates a socket.
Connects to the server using IP and port (connect()).
Sends and receives messages (send() and recv()).
