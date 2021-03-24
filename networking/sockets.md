# Sockets

A **socket** is one endpoint of a two-way communication link between two programs running on the network. A socket is bound to a port number so that the TCP layer can identify the application that data is destined to be sent to. An endpoint is a combination of an IP address and a port number.

A network socket is a software structure within a network node of a computer network that serves as an endpoint for sending and receiving data across the network. Sockets are created only during the lifetime of a process of an application running in the node.

Example of socket commands in Python:

```py
socket.socket()
s.bind(host, port)
s.send()
s.listen()
s.recv()
s.close()
```

At the time of creation with the API, a network socket is bound to the combination of a type of network protocol to be used for transmissions, a network address of the host, and a port number.

## Types of sockets

Several types of Internet socket are available:

### Datagram sockets

Connectionless sockets, which use User Datagram Protocol (UDP). Each packet sent or received on a datagram socket is individually addressed and routed. Order and reliability are not guaranteed with datagram sockets, so multiple packets sent from one machine or process to another may arrive in any order or might not arrive at all.

### Stream sockets

Connection-oriented sockets, which use TCP, SCTP or DCCP. A stream socket provides a sequenced and unique flow of error-free data without record boundaries, with well-defined mechanisms for creating and destroying connections and reporting errors. A stream socket transmits data reliably, in order, and with out-of-band capabilities. On the Internet, stream sockets are typically implemented using TCP so that applications can run across any networks using TCP/IP protocol.

### Raw sockets

Allow direct sending and receiving of IP packets without any protocol-specific transport layer formatting. With other types of sockets, the payload is automatically encapsulated according to the chosen transport layer protocol (e.g. TCP, UDP), and the socket user is unaware of the existence of protocol headers that are broadcast with the payload. When reading from a raw socket, the headers are usually included.
