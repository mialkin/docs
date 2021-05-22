# Commands

## ping

```bash
ping HOST_NAME
```

Ping measures the round-trip time for messages sent from the originating host to a destination computer that are echoed back to the source.

Ping operates by sending Internet Control Message Protocol (ICMP) echo request packets to the target host and waiting for an ICMP echo reply. The program reports errors, packet loss, and a statistical summary of the results, typically including the *minimum*, *maximum*, the *mean round-trip* times, and *standard deviation* of the mean.

## traceroute

On macOS run:

```bash
traceroute HOST_NAME
```

**Traceroute** is a network diagnostic tool used to track in real-time the pathway taken by a packet on an IP network from source to destination, reporting the IP addresses of all the routers it pinged in between. Traceroute also records the time taken for each hop the packet makes during its route to the destination.

Traceroute ensures each hop on the way to a destination device drops a packet and sends back an ICMP error message. This means traceroute can measure the duration of time between when the data is sent and when the ICMP message is received back for each hop.

The time-to-live (TTL) value, also known as *hop limit*, is used in determining the intermediate routers being traversed towards the destination. Traceroute sends packets with TTL values that gradually increase from packet to packet, starting with TTL value of one. Routers decrement TTL values of packets by one when routing and discard packets whose TTL value has reached zero, returning the ICMP error message ICMP Time Exceeded. For the first set of packets, the first router receives the packet, decrements the TTL value and drops the packet because it then has TTL value zero. The router sends an ICMP Time Exceeded message back to the source. The next set of packets are given a TTL value of two, so the first router forwards the packets, but the second router drops them and replies with ICMP Time Exceeded.

The sender expects a reply within a specified number of seconds. If a packet is not acknowledged within the expected interval, an asterisk is displayed. The Internet Protocol does not require packets to take the same route towards a particular destination, thus hosts listed might be hosts that other packets have traversed.
