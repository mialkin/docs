# Transmission Control Protocol

RFC 793 of September 1981: [[html]](https://tools.ietf.org/html/rfc793), [[pdf]](https://tools.ietf.org/pdf/rfc793.pdf)

TCP assumes it can obtain a simple, potentially unreliable datagram service from the lower level protocols.  In principle, the TCP should be able to operate above a wide spectrum of communication systems ranging from hard-wired connections to packet-switched or circuit-switched networks. [1]

The TCP fits into a layered protocol architecture just above a basic  Internet Protocol which provides a way for the TCP to send and  receive variable-length segments of information enclosed in internet  datagram "envelopes".  The internet datagram provides a means for  addressing source and destination TCPs in different networks.  The  internet protocol also deals with any fragmentation or reassembly of  the TCP segments required to achieve transport and delivery through  multiple networks and interconnecting gateways.  The internet protocol  also carries information on the precedence, security classification  and compartmentation of the TCP segments, so this information can be  communicated end-to-end across multiple networks. [2]

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

## Interfaces

The TCP interfaces on one side to user or application processes and on the other side to a lower level protocol such as Internet Protocol.

The interface between TCP and lower level protocol is essentially unspecified except that it is assumed there is a mechanism whereby the  two levels can asynchronously pass information to each other. Typically, one expects the lower level protocol to specify this  interface.  TCP is designed to work in a very general environment of interconnected networks.  The lower level protocol which is assumed  throughout this document is the Internet Protocol. [3]

## Handshake

TCP utilizes a number of flags, or 1-bit boolean fields, in its header to control the state of a connection. The three we're most interested in here are:

- **SYN** - (Synchronize) Initiates a connection
- **FIN** - (Final) Cleanly terminates a connection
- **ACK** - Acknowledges received data

TCP handshake is done via sending 3 packets:

- **SYN**
- **SYN / ACK**
- **ACK**

## Maximum packet size for a TCP connection

The absolute limitation on TCP packet size is 64K (65535 bytes), but in practicality this is far larger than the size of any packet you will see, because the lower layers (e.g. ethernet) have lower packet sizes.

The MTU (Maximum Transmission Unit) for Ethernet, for instance, is 1500 bytes. Some types of networks (like Token Ring) have larger MTUs, and some types have smaller MTUs, but the values are fixed for each physical technology.

## MSS

The maximum segment size (**MSS**) is a parameter of the options field of the TCP header that specifies the largest amount of data, specified in bytes, that a computer or communications device can receive in a single TCP segment. It does not count the TCP header or the IP header (unlike, for example, the MTU for IP datagrams).

The default TCP Maximum Segment Size is 536.