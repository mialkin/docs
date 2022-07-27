# Internet Protocol

The **Internet Protocol** or **IP** is a protocol, or set of rules, for routing and addressing packets of data so that they can travel across networks and arrive at the correct destination.

## Table of contents

- [Internet Protocol](#internet-protocol)
  - [Table of contents](#table-of-contents)
  - [IP packet](#ip-packet)
  - [IPv4 address ranges reserved for private networks](#ipv4-address-ranges-reserved-for-private-networks)

## IP packet

An **IP packet** is a piece of data into which entire data traversing the Internet is divided.

Data traversing the Internet is divided into smaller pieces, called **packets**. IP information is attached to each packet, and this information helps routers to send packets to the right place. Every device or domain that connects to the Internet is assigned an IP address, and as packets are directed to the IP address attached to them, data arrives where it is needed.

Each IP packet will contain both the IP address of the device or domain sending the packet and the IP address of the intended recipient, much like how both the destination address and the return address are included on a piece of mail.

Once the packets arrive at their destination, they are handled differently depending on which transport protocol is used in combination with IP. The most common transport protocols are TCP and UDP.

## IPv4 address ranges reserved for private networks

| IP address range              | Number of addresses |
| ----------------------------- | ------------------- |
| 10.0.0.0 – 10.255.255.255     | 16 777 216          |
| 172.16.0.0 – 172.31.255.255   | 1 048 576           |
| 192.168.0.0 – 192.168.255.255 | 65 536              |
