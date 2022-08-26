## UDP
UDP Packets 

Has a host.outboundPort > host.port, with the length and the payload.

Simple protocol

A port is some kind of way a system is giving a process one or more addresses. If you are listening on port 3000, it just means the system that it is running is on, if someone sends something on port 3000, then send it to this process. A process can be listening on multiple ports. Send it to the socket that is listening on port 3000. That will correspond to some listener on some process.

There are outbound ports. They are randomly signed. One never specifies the outbound ports. This round robins in 60000 ports, as connections of payloads are sent. This is src host and port > destination host and port and meta data

Command with `nc` are using a tool of `netcat`.
`-u` means to send a UDP message
Example:
`nc -u 127.0.0.1 123`
Above translates is to: Use the netcat tool, send a UDP message to 127.0.0.1 on port 123. `127.0.0.1` is basically localhost.
Netcat doesn't realize there is a listener since this is a UDP protocol, there is no echo. Once you put in the payload, it is just done. No acks or nacks. UDP does not leave an open connection.

To setup a listener:
`nc -ul 123`
The `l` in the `ul` indicates to setup a listener. And to listen on port 123.
So on the other end you will see the payload/message that you sent (since this is a localhost)

Features:
- Data integrity via checksums
- Fast

Downsides
- Data loss
- Size limits
- does not guarantee delivery order of packets
- 
Uses

## TCP
Looks like a lot like UDP packets but have flags. It is metadata that defines what kind of packets they are. 
Sequence numbers - is a counter used to keep track of every byte sent outward by a host. If a packet contains 1400 bytes of data, then the sequence number will be increased by 1400 after the packet is transmitted.
then length of the packet. So it will start at sequence # 0. Next packet will have a sequence number of 1400.
TCP matches with the sequence number

Command to send a TCP packet:
`nc 127.0.0.1 123`
It won't work because there is nothing listening. TCP tries to make aconnection to the target to make sure it is listening.
`nc -l 123` - turns on the listener now

TCP now sends an initial packet to ask if we can connect
Receiver sends a sure we can connect
Original sends an okay, let's try and connect and sends the message.

Features:
- Connections
- Retransmission
- Deduplication

Downsides
- Latency


Other Protocols - different layers, can use the previous layer to build upon their protocol (in terms of the levels)
- IP - supposed to be responsible from a source IP to a destination IP
	- Lower-level
	- Delivers packets to destination IP address
	- Almost everything uses it
- ICMP (deals with pings - traceroutes, portless, no listeners)
	- Lower-level
	- Diagnostic messaging
	- Usually receivers know how to deal with ICMP message types if it is not blocked by a firewall
	- uses Types and Codes 0 to 30
	- Common Type is 0 (Echo Request) and 8 (Echo Reply) for Ping and Traceroute respectively.
- IPsec (site to site usage - lower level of encryption that TCP)
	- Lower-level
	- Mutually authenticate and encrypt packets
	- It is lower-level of encryption than TCP
	- This is what Redox uses
	- Take a TCP packet and inject it into an IPsec packet to get encryption.
- HTTP/HTTPs
	- Higher-level
	- Builts upon TCP, and the payload for the IP packet may have HTTP headers and body
	- Request/Response/headers
	- higher level encryption for HTTPS, so all the packet looks exactly the same but the body is all garbled as well as the headers.
- MLLP - (Minimal Lower Layer Protocol)
	- Higher-level
	- Relies on TCP
	- Delimited structured payloads
	- Profile that customers send to us. On same level as HTTP, but injected into body of a TCP packet.
	- HL7 format uses this

In general higher level protocols may inject lower levels.

IPv4 addresses
4 octets, eg A.B.C.D
Each octet is a number between 0 to 255
256^total IPs is about 4 million which isn't that many.

Local host - 127.0.0.0/8
/8 are Cider Blocks, how the ranges are demarcated. The smaller the number on slide of that number on the right side of the slash, the bigger the range.
/8 = the first number is mixed, the other three are 0-255
/16 = the first two are fixed
/24 = first three are fixed
/32 = IP is fixed

Private Ranges
10.0.0 = 10.255.255.255 (10.0.0.0/8)
172.16.0.0 - 172.31.255.255
(172.16.0.0/12)
192.168.0.0-192.168.255.255
(192.168.0.0/16)

Public Ranges
Everything not reserved


## Network Components

High Level explanation:

Network Address Translation (NAT) is the mechanism the router uses to map multiple internal addresses to the single external address.