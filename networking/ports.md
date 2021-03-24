# Ports

In computer networking, a **port** is a communication endpoint. At the software level, within an operating system, a port is a logical construct that identifies a specific process or a type of network service. A port is identified for each transport protocol and address combination by a 16-bit unsigned number, known as the _port number_. The most common transport protocols that use port numbers are the Transmission Control Protocol (TCP) and the User Datagram Protocol (UDP).

A port number is always associated with an IP address of a host and the type of transport protocol used for communication. It completes the destination or origination network address of a message. Specific port numbers are reserved to identify specific services so that an arriving packet can be easily forwarded to a running application. For this purpose, port numbers lower than 1024 identify the historically most commonly used services and are called the _well-known port numbers_. Higher-numbered ports are available for general use by applications and are known as _ephemeral ports_.

Ports provide a multiplexing service for multiple services or multiple communication sessions at one network address.

## Port number

A port number is a 16-bit unsigned integer, thus ranging from 0 to 65535. For TCP, port number 0 is reserved and cannot be used, while for UDP, the source port is optional and a value of zero means no port. A process associates its input or output channels via an Internet socket, which is a type of file descriptor, associated with a transport protocol, an IP address, and a port number. This is known as _binding_. A socket is used by a process to send and receive data via the network. The operating system's networking software has the task of transmitting outgoing data from all application ports onto the network, and forwarding arriving network packets to processes by matching the packet's IP address and port number to a socket. For TCP, only one process may bind to a specific IP address and port combination. Common application failures, sometimes called _port conflicts_, occur when multiple programs attempt to use the same port number on the same IP address with the same protocol.

The port numbers are divided into three ranges:

-   [0 – 1023] well-known ports
-   [1024 – 49151] registered ports
-   [49152 – 65535] dynamic or private ports

## Notable well-known port numbers

| Number | Assignment    |
| ------ | ------------- |
| 22     | SSH           |
| 25     | SMTP          |
| 53     | DNS           |
| 67, 68 | DHCP          |
| 80     | HTTP          |
| 143    | IMAP          |
| 143    | HTTP over TLS |

## List open ports

```bash
sudo lsof -PiTCP -sTCP:LISTEN
```

`lsof` lists open files. Network sockets count as files, so each open network socket (either listening or actively in use) will be listed in `lsof`:

```bash
lsof -Pn -i4
```

Any "ESTABLISHED" socket means that there is a connection currently made there.

Any "LISTEN" means that the socket is waiting for a connection.

Both are opened ports but one is waiting for a connection to be made while the other has a connection already made.

You can imagine this like the following:

The HTTP protocol (typically port 80) is on LISTEN mode until somebody actually goes to the server. The moment somebody visits the page, then it will be in ESTABLISHED mode.

The same applies for the MySQL 3306. When nobody is using the service, it is on LISTEN mode. When somebody actually uses it, at that moment it will be in ESTABLISHED mode.

Basically with this you will see how the the ports work, how they are handled and probably more information regarding sockets and their states. And yes, as stated ESTABLISHED & LISTEN both are Open Ports but ESTABLISHED means it is connected while LISTEN means that is waiting to be connected.
