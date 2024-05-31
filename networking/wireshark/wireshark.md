# `ping`, `traceroute`, Wireshark

## Table of contents

- [Tools: `ping`, `traceroute`, Wireshark](#tools-ping-traceroute-wireshark)
  - [Table of contents](#table-of-contents)
  - [`ping`](#ping)
  - [`traceroute`](#traceroute)
  - [Wireshark](#wireshark)
    - [Usage](#usage)
    - [TCP Options](#tcp-options)
    - [Filters](#filters)
    - [IP time to live](#ip-time-to-live)

## `ping`

The `ping` command measures the round-trip time for messages sent from the originating host to a destination computer that are echoed back to the source.

```bash
ping HOST_NAME
```

Ping operates by sending Internet Control Message Protocol, ICMP, echo request packets to the target host and waiting for an ICMP echo reply.

The program reports errors, packet loss, and a statistical summary of the results, typically including the _minimum_, _maximum_, the _mean round-trip_ times, and _standard deviation_ of the mean.

## `traceroute`

The `traceroute` is a network diagnostic tool used to track in real-time the pathway taken by a packet on an IP network from source to destination, reporting the IP addresses of all the routers it pinged in between.

```bash
traceroute HOST_NAME
```

`traceroute` also records the time taken for each hop the packet makes during its route to the destination.

`traceroute` ensures each hop on the way to a destination device drops a packet and sends back an ICMP error message. This means traceroute can measure the duration of time between when the data is sent and when the ICMP message is received back for each hop.

The time-to-live, TTL, value, also known as _hop limit_, is used in determining the intermediate routers being traversed towards the destination. Traceroute sends packets with TTL values that gradually increase from packet to packet, starting with TTL value of one. Routers decrement TTL values of packets by one when routing and discard packets whose TTL value has reached zero, returning the ICMP error message ICMP Time Exceeded. For the first set of packets, the first router receives the packet, decrements the TTL value and drops the packet because it then has TTL value zero. The router sends an ICMP Time Exceeded message back to the source. The next set of packets are given a TTL value of two, so the first router forwards the packets, but the second router drops them and replies with ICMP Time Exceeded.

The sender expects a reply within a specified number of seconds. If a packet is not acknowledged within the expected interval, an asterisk is displayed. The Internet Protocol does not require packets to take the same route towards a particular destination, thus hosts listed might be hosts that other packets have traversed.

## Wireshark

[↑ Wireshark](https://www.wireshark.org) is a free and open-source packet analyzer. It is used for network troubleshooting, analysis, software and communications protocol development, and education.

### Usage

Anytime you see a value inside square brackets — it's a Wireshark's value, that's not part of the data, it's something that Wireshark does to help us interpret what we are seeing.

`[Stream index: 0]` — The number of a stream of any kind. If you see in the trace file a `SYN` and another `SYN` right after it on a different port number — that would be a stream 0 and a stream 1. So you can quickly spot how many different TCP streams you have in trace.

`[TCP Segment Len: 0]` — how much data is in this packet that you are sending.

`Sequence number` — Wireshark will start off sequence number at 0 for you. Sequence number is a number that increments as you send data. If your sequence number goes up by one it's because you've sent 1 byte of data; if it goes up by 100 — you have sent 100 bytes of data. It allows TCP to figure out what actually got there, where am I starting from, what's missing in the package chain, that's because every byte that I've sent has sequence number assigned to it. Actually sequence byte is represented as 4-byte integer. So Wireshark does relative sequence number. Sending sequence number I am telling server where I am starting.

<img src="seq.png">

`Acknowledgement number` that then number from which server starts from.

`Windows` this is an advertisment of what my receive window buffer size is. How much you can send me at once.

### TCP Options

The place where you are laying ground rules for a "relationship".

`Maximum segment size` — maximum size of data sent in TCP payload in one packet.

`No-Operation (NOP)` — pads space to meet you header length. Basically it's a stub.

TCP Header in terms of bytes is always a multiple of 4.

`SACK` — selective acknowledgment TCP option.

SACKs work by appending to a duplicate acknowledgment packet a TCP option containing a range of noncontiguous data received. In other words, it allows the client to say "I only have up to packet #1 in order, but I also have received packets #3 and #4". This allows the server to retransmit only the packet(s) that were not received by the client.

Support for SACK is negotiated at the beginning of a TCP connection; if both hosts support it, it may be used.

### Filters

```text
ip.addr == 35.246.105.145
```

### IP time to live

Every time when packet faces router the TTL number decrements by 1.

If you see that packet has TTL equal 127 and you, as a server, know that client started at 128, that means that client is 1 hop away from you.
