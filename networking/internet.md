# Internet, IP, TCP, OSI model

## Table of contents

- [Internet, IP, TCP, OSI model](#internet-ip-tcp-osi-model)
  - [Table of contents](#table-of-contents)
  - [Internet](#internet)
    - [Autonomous system](#autonomous-system)
  - [IP](#ip)
    - [IP packet](#ip-packet)
    - [IPv4 address ranges reserved for private networks](#ipv4-address-ranges-reserved-for-private-networks)
  - [TCP](#tcp)
    - [Interfaces](#interfaces)
    - [Handshake](#handshake)
    - [Maximum packet size for a TCP connection](#maximum-packet-size-for-a-tcp-connection)
    - [MSS](#mss)
  - [HTTP](#http)
  - [OSI model](#osi-model)

## Internet

The **Internet** or **internet** is the global system of interconnected computer networks that uses the Internet protocol suite, TCP/IP, to communicate between networks and devices. It is a network of networks that consists of private, public, academic, business, and government networks of local to global scope, linked by a broad array of electronic, wireless, and optical networking technologies.

### Autonomous system

The Internet is a network of networks, and autonomous systems are the big networks that make up the Internet. More specifically, an autonomous system, AS, is a large network or group of networks that has a unified routing policy. Every computer or device that connects to the Internet is connected to an AS.

Imagine an AS as being like a town's post office. Mail goes from post office to post office until it reaches the right town, and that town's post office will then deliver the mail within that town. Similarly, data packets cross the Internet by hopping from AS to AS until they reach the AS that contains their destination Internet Protocol, IP, address. Routers within that AS send the packet to the IP address.

## IP

The **Internet protocol** or **IP** is a protocol, or set of rules, for routing and addressing packets of data so that they can travel across networks and arrive at the correct destination.

### IP packet

Data traversing the Internet is divided into smaller pieces, called *packets*.

An **IP packet** is a piece of data into which entire data traversing the Internet is divided.

IP information is attached to each packet, and this information helps routers to send packets to the right place. Every device or domain that connects to the Internet is assigned an IP address, and as packets are directed to the IP address attached to them, data arrives where it is needed.

Each IP packet will contain both the IP address of the device or domain sending the packet and the IP address of the intended recipient, much like how both the destination address and the return address are included on a piece of mail.

Once the packets arrive at their destination, they are handled differently depending on which transport protocol is used in combination with IP. The most common transport protocols are TCP and UDP.

### IPv4 address ranges reserved for private networks

| IP address range              | Number of addresses |
| ----------------------------- | ------------------- |
| 10.0.0.0 – 10.255.255.255     | 16 777 216          |
| 172.16.0.0 – 172.31.255.255   | 1 048 576           |
| 192.168.0.0 – 192.168.255.255 | 65 536              |

## TCP

RFC 793 of September 1981: [[html]](https://tools.ietf.org/html/rfc793), [[pdf]](https://tools.ietf.org/pdf/rfc793.pdf)

TCP assumes it can obtain a simple, potentially unreliable datagram service from the lower level protocols.  In principle, the TCP should be able to operate above a wide spectrum of communication systems ranging from hard-wired connections to packet-switched or circuit-switched networks.

The TCP fits into a layered protocol architecture just above a basic  Internet Protocol which provides a way for the TCP to send and  receive variable-length segments of information enclosed in internet  datagram "envelopes".  The internet datagram provides a means for  addressing source and destination TCPs in different networks.  The  internet protocol also deals with any fragmentation or reassembly of  the TCP segments required to achieve transport and delivery through  multiple networks and interconnecting gateways.  The internet protocol  also carries information on the precedence, security classification  and compartmentation of the TCP segments, so this information can be  communicated end-to-end across multiple networks.

Protocol Layering:
<pre>
+---------------------+
|     higher-level    |
+---------------------+
|        TCP          |
+---------------------+
|  internet protocol  |
+---------------------+
|communication network|
+---------------------+
</pre>

### Interfaces

The TCP interfaces on one side to user or application processes and on the other side to a lower level protocol such as Internet Protocol.

The interface between TCP and lower level protocol is essentially unspecified except that it is assumed there is a mechanism whereby the  two levels can asynchronously pass information to each other. Typically, one expects the lower level protocol to specify this  interface.  TCP is designed to work in a very general environment of interconnected networks.  The lower level protocol which is assumed  throughout this document is the Internet Protocol.

### Handshake

TCP utilizes a number of flags, or 1-bit boolean fields, in its header to control the state of a connection. The three we're most interested in here are:

- **SYN** - (Synchronize) Initiates a connection
- **FIN** - (Final) Cleanly terminates a connection
- **ACK** - Acknowledges received data

TCP handshake is done via sending 3 packets:

- **SYN**
- **SYN / ACK**
- **ACK**

### Maximum packet size for a TCP connection

The absolute limitation on TCP packet size is 64K (65535 bytes), but in practicality this is far larger than the size of any packet you will see, because the lower layers (e.g. ethernet) have lower packet sizes.

The MTU (Maximum Transmission Unit) for Ethernet, for instance, is 1500 bytes. Some types of networks (like Token Ring) have larger MTUs, and some types have smaller MTUs, but the values are fixed for each physical technology.

### MSS

The maximum segment size (**MSS**) is a parameter of the options field of the TCP header that specifies the largest amount of data, specified in bytes, that a computer or communications device can receive in a single TCP segment. It does not count the TCP header or the IP header (unlike, for example, the MTU for IP datagrams).

The default TCP Maximum Segment Size is 536.

[↑ What are IP & TCP?](https://www.cloudflare.com/learning/ddos/glossary/tcp-ip/).

## HTTP

**HTTP/1** came out in 1996. It is built on top of TCP. Every request to the same server requires a separate TCP connection.

**HTTP/1.1** came out in 1997. It introduced "keep alive" mechanism, so that a connection could be reused for more than a single request. To keep loading performance at the acceptable level browsers normally kept multiple TCP connections open to the same server and sent request to it in parallel.

**HTTP/2** was published in 2015. HTTP/2 introduced HTTP "streams" where multiple stream requests could be sent over single TCP connection. Unlike with HTTP/1.1 each stream is independent of each other and it does not need to be sent or received in order. Also HTTP/2 introduced a push capability to allow servers to send updates to the clients whenever new data is available.

**HTTP/3** began as a draft in 2020 and was published in 2022. It uses a new protocol called QUIC instead of TCP as underlying transport protocol. QUIC is based on UDP and introduces streams as first class citizens at the transport layer. QUIC streams share the same QUIC connection so no additional handshakes are required to create new ones. QUIC is designed for mobile-heavy internet usage. People carrying smartphones constantly switch from network to another as they move about their day, for example from, Wi-Fi to 5G. With TCP the handoff of one connection from one network to another is sluggish. QUIC implements a concept called connection ID, which allows connections to move between IP addresses and network interfaces quickly and reliably.

[↑ HTTP/1 to HTTP/2 to HTTP/3](https://www.youtube.com/watch?v=a-sBfyiXysI).

## OSI model

**Open Systems Interconnection model** or **OSI model** is a conceptual model from the International Organization for Standardization, ISO, that "provides a common basis for the coordination of standards development for the purpose of systems interconnection".

In the OSI reference model, the communications between a computing system are split into seven different abstraction layers: Physical, Data Link, Network, Transport, Session, Presentation, and Application.
