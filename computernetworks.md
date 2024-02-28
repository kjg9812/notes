# Computer Networks
- Professor: Anirudh Sivaraman
# 1/22/24
### Application Layer
#### How does a computer communicate with another?
1. identify other computer -> IP address
2. tell it you want to talk to it
3. transfer info
^ All of this is encapsulated in sockets library/api
### Transport Layer
- how are sockets implemented?
    - reliability
        - if i have information, ex. "hello", and i want to send it, how do i make sure it gets there in exactly that form... not "elohl" scrambled
    - congestion control
        - part of the system that cannot move info quickly enough, so there's a backup of information
### Routing/net Layer
how to get from point a to point b?
- finding shortest path between a and b
- piece of data is called a packet, groups of bits
- called the medium access layer
    - transmission medium ex. air, cables, etc.
- needs to be able to survive faults
### Physical Layer
- voltages, antennas
- analog world
### Last third of course
- field is in flux because communication shows up in different contexts
- new uses for networking
- new tech in layers

# 1/24/24
### The Internet
Definition: An internet is a network of interconnected networks
- network meaning some way of connecting computers using some technology (cable, radio, etc)
#### early applications of networks
1. first demo of networks is the arpanet
2. also the arpa radio network (wirless based)
3. NPL
4. Cyclades
#### requirements implemented for the internet (goals)
1. low effort interconnection
    - these networks need to be able to attach to each other without adding additional software or hardware to existing networks
2. generality
#### non goals
1. performance
2. security
#### how these goals were achieved
1. low effort interconnection
    - idea is to get sub networks to participate in the global internet
    - say we have two networks, how will we connect them?
        - simple answer, one edge connects to undirected graphs
    - an IP address: is the universal identifier for hosts
        - a host sitting around in one network wants to read info from a host in another network, the IP address i shared
    - routers could do more than just forwarding on destination IP addresses like prioritization, load balancing
        - but want to keep routers as simple as possible! just want everybody to agree on addresses, and it tells to send a packet one way or the other
2. generality
- the idea that you should be able to run anything on the internet
    - each layer does one thing well
    - it has clean interfaces/contracts with neighboring layers
app layer
- many different apps
transport layer
- connect a process on one host to process on another host
- provide the service or reliability (TCP)
Routing layer: global delivery
- ex. how do i get data from my machine to a google server sitting in Mountain view, maybe an ISP in NJ to an ISP in kansas, to an ISP in CA; cooperating to ensure global delivery
MAC layer: local delivery
- medium access control
- way of identifying things within a local network
Physical layer: rubber meets the road 
### Packet vs Circuit switching
- how does data get to its destination?
#### circuit switching
- data goes through switches in the computer network
- telephone native
- switches have configurations: if data comes in one way, then it must go out a certain way
#### packet swtiching
- divide data into chunks called packets
- attach destination address as a header to the packet
- replace telephone exchange switches with packet switches
    - switches dont need to setup information with a circuit from a to b, no need for all those configurations
    - they can look up in a table that tells them the neighbor they need to go to to get to B -> routing tables
#### pkt vs ckt
1. no setup time
2. less stuff on switches
3. fault tolerance
### Where 5 layers reside?
PKT swtiching/internet performance

# 1/29/24
### Where do the 5 layers reside?
as a stack that resides on all hosts
- app
    - typically in user space
- transport
    - typically in the kernel
- routing
- MAC
- physical
routers only implement
- routing
- MAC
- phys
between hosts
![image](images/layers.png)

### internet performance
#### initially
- when it was just phones and wires, the question was more, was the phone call accepted or denied
    - is there enough capacity for the phone call to happen
    - one in one out, phone call using one path and no one else using that path
#### over time
- when it transitioned into computers and packet switches and routing tables, then what happens when two packets arrive at a switch at the same time
    - two things coming in, only one thing allowed to go out at a time (same as a queue in a grocery store)
    - this is queueing theory at its core
    - need to hold packets on the switch, maybe eventually you have to drop packets since there's not enough space
    - this manifests into **delay/latency** -> the time between the packet being visible at 2 different points (so the time spent travelling to a point basically)
        - queueing delay
        - transmission delay
            - every connection has a finite capacity on the number of bits it can ferry from one point to another
            - $transmission delay = \frac{pktsize}{link capacity}$
        - propagation delay -> L/C
            - length over the speed of light
            - speed of light delay
            - minimum amount of time to get info from one point to another
#### throughput
- if a link coming from a switch is 15 mbit/s but two links are sending 10 mbit/s to that switch, how does the throughput split the data?
    - scheduling problem
    - 10 and 5? -> work conserving algorithm, priority scheduling (as long as A has packets to send, i'll send A's packets preferentially)
    - 10 and 0? -> non work conserving algorithm
    - 7.5 and 7.5? -> work conserving algorithm (fair queueing)

### Identifying hosts
1. IP addresses: numeric (10.23.5)
    - this is for the internal implementation for the host to have a fixed length identifier
2. Host names: english readable (google.com)
    - this is for you and i to remember
#### private and public IP addresses
- initially designed so everybody has a public IP address
    - but realized they were running out of IP addresses
- so now there's Class A, B, C IP addresses that differ in length/size of IP address
- private IP addresses allowed people to re use the same IP addresses
    - what we all see when we connect to residential wifi, ex. 192.68....
    - meaning: my ip address needs to only be visible to my wifi access point
    - analogous to local and global delivery -> so private addresses do local addressing
#### DNS
- need a way to go from host names to IP addresses
    - early way to do it was to put host name and IP address in a file lol, host.txt
- Domain Name System is a global telephone directory for the internet
    - to deal with scale, it exploits hierarchy
    - ex. mail.google.com
        - subdomain is subdomain, google is Domain, com is TLD
        - there is a distributed system to take strings and turn them into ip addresses, where the hierarchy is root servers, PID servers (com,edu,org), authoritative servers

# 1/31/24
root servers-> TLD servers(.com,.org,.net) -> servers (google.com)
Option 1: non authoratiative server: operated by campus nets/ISPs
- dont need to talk to root server hierarchy, saves a lot of latency
Option 2: cache DNS mappings
- a list of hostname and IP address, cache it locally

TCP allows reliable byte string abstraction
- if you send bytes, you will get bytes on the other side

### Socket Demo
#### write something that allows us to send data
```py
from socket import socket
sock_object = socket(AF_INET, SOCK_DGRAM) # two arguments -> (family of sockets, type of socket)
sock_object.sendto(b"hello", ("127.0.0.1",8000)) # two arguments (byte array, (address to send to;currently local host; and port))
sock_object.sendto(b"hello\n\nhi", ("localhost",8000)) # two arguments (byte array, (address to send to;currently local host; and port))
```

#### create something that can receive UDP packets on the same machine
```bash
nc -l 8000 -u # get me a udp socket attached to port 8000, port distinguishes applications on the same machine
# this will receive messages from the socket above
```
#### python equivalent of above
```py
from socket import socket
sock_receiver = socket(AF_INET, SOCK_DGRAM) # two arguments -> (family of sockets, type of socket)
sock_receiver.bind(("localhost",8000)) # bind tells socket to attach to specific port number
sock_receiver.recv(4) # argument tells how many bytes you want to receive
sock_receiver.setblocking(False) # non blocking, does not wait til data is available, it returns right away
```
#### what is the benefit of non blocking -> having data right away or no data
- you can do other work

### polling - checking different sockets to see if there's data wastes CPU
- like a while loop constantly checking sockets
- this reduces latency but takes up all resource

### select system call - the alternative
- get a bunch of sockets, put in an array
- one system call select, take entire array as input
- wait until any of the sockets have data in them
- sort of like a blocking call on a GROUP of sockets
```py
from select import *
r = select.select([s1.fileno(), s2.fileno()], [],[], 1.0) # returns when one is ready to be written
# wait on array in filedescriptor array
```
#### documentation
select.select(rlist, wlist, xlist[, timeout])
This is a straightforward interface to the Unix select() system call. The first three arguments are iterables of ‘waitable objects’: either integers representing file descriptors or objects with a parameterless method named fileno() returning such an integer:

### TCP sockets

#### send data
```py
from socket import *
tcp_socket = socket(AF_INET,SOCK_STREAM) # family is the same, still talking about internet, but type is sock stream referring to byte stream -> hello on one end will come out as hello on the other end, not hello -> olleh
tcp_client = socket(AF_INET,SOCK_STREAM)
tcp_client.connect(("127.0.0.1", 8000))
tcp_client.send(b"hello")

# new client
tcp_client2 = socket(AF_INET,SOCK_STREAM)
tcp_client2.connect(("127.0.0.1", 8000))
tcp_client2.send(b"helloworld")
```

#### receive data
```py
from socket import *
tcp_server = socket(AF_INET, SOCK_STREAM)
tcp_server.bind(("127.0.0.1", 8000))
tcp_server.listen() # not specifying bytes, because its a 3 way handshake, need to be synced up to provide a reliable byte stream, so puts server in a mode to carry out synchronization
(comm_socket,client_addr) = tcp_server.accept() # for the TCP server, to know how to actually reach the client -> so once tcp server has accepted connection to client, then we can receive data on a different socket, the job of actually receiving and sending data is on a different socket
comm_socket.recv(5) # note that recv is on a different socket than tcp socket

# this is a new client on the same TCP server
(comm_socket2,client_addr2) = tcp_server.accept() # accept returns sockets to allow you to interact with all clients connected to you
comm_socket2.recv(5)
```

#### why the TCP delagates data send/receive to another socket -> it needs to listen to another connection attempt!
- this does not exist in UDP, there is no notion of agreeing
- all TCP guarantees says is that if you send a byte from one end it will get to the other end, does not say anything about the receive
    - stream finishes when you close
- byte stream starts with first byte sent after connection established, ends wtih the close call of the client socket

# 2/5/24
### Reliability problem
- one host and another host
- network in between
    - network can arbitrarily do anything
    - drop, delay, reorder packets
    - but it eventually delivers each packet
- the goal is to send bytes in an order and they should appear in that order on the other end
- ex. send Hello and we get Hello on the other
### solution to reliability
- receiver confirms receipt of packet (acknowledgement or ACK)
- sender attaches a number to every packet (sequence number)
- HELLO
- 01234
- receiver says they got sequence number 0, 1, etc
#### stop & wait protocol
- sender
    1. Tx(transmit) the 1st packet
    2. wait for ACK; re tx (re transmit) if needed
    3. repeat w/ 2nd, 3rd, 4th pkt
- receiver
    1. send an ack for just rx pkt
    2. maintain variable next in order
    3. if rx pkt's seq number = next in order, increment next in order
- this cares about in order reliability
### throughput of stop & wait
- minimum round trip time
    - pkt sent, but also ack needs to be sent back
    - abbreivate as RTT min.
- Tpt (throughput) = packets per unit time
- stop and wait -> 1 packet per RTT -> 1/RTT
- queing delays are more or less 0 because the link capacity exceeds the average throughput going into it
- ex.  tpy = 120 kbits/sec and capacity = 100 mbits/sec
- if the link capacities are much lower, then queues will build up
- but this is only for one conversation going through -> stop and wait doesnt produce enough throughput
- and for extremes -> we want to keep it under 50%, this warrants increasing capacity
#### how can we improve throughput of stop and wait?
- we don't have to wait for ack. -> the line goes idle
- so maybe instead of waiting, dump more packets into the line
- number of packets you keep unacknowledged is called the window (sliding window)
- eventually acknowledge this window of packets
### how long do you wait for an acknowledgement before a packet is "lost"?
- wait for the RTT + a little bit of time (epsilon) ; after the RTT the packet is lost
- this is because the RTT is the time a packet should take and be sent back
- how does the sender know this time? it keeps timestamp of when it sends and when it gets ack -> this gets RTT for future packets
#### stats for RTT
RTT and stats
- model RTT as a normal distribution
- mean and standard deviation
- RTT is the mean + little bit around it is the sd
signal processing to estimate mean and sd
- basically averaging past mean and most recent sample
- timeout (the little time that we add) is set to 4 standard deviations away
# 2/7/24
### From last time
- retransmission timeout
- timeout = $\mu + 4\sigma$
- Pr(RTT > Timeout) should be small
    - also called the probability of spurious retransmission
- how to find $\mu$ and $\sigma$?
    - exponentially weighted moving average
    - formula on slides
#### note
- throughput of stop & wait is quite low
- 1/RTT
- this is because we're waiting for an acknowledgement after every packet send
### Sliding Window Protocol - Sender
- we want reliable and in order delivery
- Sender
    1. send out w packets at once
    2. wait for ack for any of these w
    3. when you get an ack, release 1 more pkt
- sending out w packets at once, theres still a gap between sending the packets
    - this is because we can only send one at a time. Why?
        - voltages, transmission delay
- window is the packets that havent been acknowledged, so you can add a new packet at the end of the window every time you get an ack
- formal def -> window: set of unacknowledged packets
    - window slides from [0,1,2,...,w-1] to [1,2,...,w] to [2,3,...,w+1]
- invariants: Always keep w pkts unacknowledged
- packet conservation principle: put a packet in for every packet out
### sliding window - receiver
- Receiver
    1. ack every packet you receive
    2. receiver might have to buffer out of order packets
        - packets that do not form a contiguous sequence
        - why might this happen?
            - scenario: sender sends w packets
            - S: [0,...,w-1]
            - R: {0,2,5,...,w-1}
                - sender realizes 1 was dropped so it decides to retransmit
                - 0 enters buffer then delivers to the app since its inorder
            - then R: {1,2,5,...,w-1} this is the buffer
                - now 0,1,2 can be delivered to the application
            - then R: {5,...,w-1}
    - just need to look for in order packets, and send those out
        - nextinorder variable
        - what if buffer looks like R: {15,16,17,18,20,21}
            - nextinorder = 14
        - so then R: {14,15,16,17,18,20,21}
            - after sending 14-18 acks
            - nextinorder = 19
    - how does the sender know to retransmit if a packet is dropped?
        - that RTT and timeout time
        - the invariant is satsified because if you realize a packet is dropped then w is decreased then you send it again and w is increased again
### sliding window throughput
- send out w packets, an RTT min later, you get w acks
- 1/c is the time between the w packets sends, but its actually w/c
    - we can assume this is very small w/c << RTT min
- so the throughput is w/RTT min
    - big win against stop and wait
- can you keep increasing w and get more and more throughput?
    - run out of capacity
    - once w > c,then w = c
- tpt = min(w/RTT, c)
    - you cant send more than the capacity of the link
- when w/c becomes comparable to RTT min, arrows sent will start overlapping arrows coming back
    - bandwidth delay product w>c*RTTmin
        - want this to be close to bdp but you dont want it to overlap
- so window needs to be close to the bandwidth delay product (the point when w exceeds cRTTmin)
- when w exceeds BDP -> you get queuing delays, because your throughput to a link exceeds the output
    - Q(in packets) = w-c*RTTmin
    - c is the speed that Q drains -> so speed to get through the queue (queue latency) is Q/c
        - Q (Q such packets that need to be serviced) * 1/c (service time)

queue delay = w/c - Rttmin
- want w to exactly hit the BDP, w=BDP

# 2/12/24
### Congestion Collapse
- w >> w*
    - w is much greater than w* (w* is the BDP)
- figuring out w* is hard:
    - w* = (c x RTTmin)/n
    - c is unknown because capacity can keep varrying over time
    - RTTmin is min RTT
    - n is # of flows
        - flow = sender - receiver pair
### congestion collapse informally
- when w > w*
    - large queuing delays
    - trigger retransmissions
    - queue has many duplicate of the same packet
        - because packets stuck in queue, but sender still times out
        - c is packets coming out per second
        - time between 1 packet and the next coming out of queue -> 1/c
        - number of unique sequence numbers per unit time (if every packet has one duplicate) -> c/2
            - the "goodput", useful work that the link is doing
- no/little dropped packets
- crowded room example
- tragedy of the commons
- if a lot of people are using the same band, no one can hear

### cong collapse, formally
- offered load (input to the system)
- utility function & agg utility function
- offered load is increasing but agg/overall utility is decreasing

### setup for congestion collapse
1. n flows
2. each flow has a window W
    - effective window = nW
3. each flow has same min RTT -> RTT
4. share the same bottleneck link
    - bottleneck -> exactly where the queues build up that causes congestion
5. bottleneck link capacity is C
6. link has infinite buffers (never drops a packet)
7. no ReTx at all
8. utility function for each flow
    - Throughput/Latency
9. Agg utlity function = Sum_f Tf/Lf

# 2/14/24
### Congestion Collapse Leads to Intuition About Window Size
- instead of a set window size, realized that the window size should be adapted
- adaptively find a sliding window size equal to the BDP of their network

### AIMD Algorithm
- the algorithm to adjust window size
- two aspects:
1. slow start
    - how you get to the right value of window size
2. congestion avoidance
    - maintain right value of window size when you get there even when senders are coming and going -> compensate for packets not being sent anymore

### slow start
- goal: $W_{target} = w^*/n = c*Rtt_{min}/n$
- get to this W target (correct window size)
- binary search alg

### congestion avoidance
- additive increase or multiplicative decrease to maintain the window size
- so slow start gets you to a target value, and congestion avoidance lets you dance around that target value

### So the alg
1. start with w=1
2. mult. increase w til packet loss (2*W)
3. mult decrease w (w = w/2)
4. additive increase w (w = w+1)
5. mult decrease w on loss (w = w/2)
- note: theres no concept of time
- time is increased as acks are sent back
- how to increase w for slow start?
    - increase w by 1 for every ack (every ack) is the same as 2*w_t(every RTT)
- if you have a timeout: w = w/2
    - a sender gets impatient and retransmits
- congestion avoidance
    - w_{t+1} = w_{t} + 1 -> additive increase for every RTT (t is RTT second -> time)
    - so for every ack -> w = w + 1/w

### why the combination of additive increase and multiplicative decrease (AIMD)
- you want to be fair, window size to be fair
- you also want window size to be w*
- so for two windows for two people, theres an intersection between fair and efficient
- MIMID for two windows would make them stay on the same line goig down to 0, AIAD as well but perpinducular to the efficiency line
- MIAD is unstable
    - you don't come back nearly enough, and the next MI takes you farther away from the initial starting point

# 2/21/24
### routing layer
    - global delivery of packets
    - gets packets from one end host of a network to another through a sequence of routers -> a path
    - find a path: routing
    - send packets along path: forwarding
- control plane: determining route/path between A & B
- data plane actually sends packets along these pre-computed paths when a new packet shows up at a router
    - at every node, which packet to go to

### how often does control plane kick in and how often data plane kicks in?
- control plane kicks in when topology changes (considering a graph)
    - ex. a router dissapears
    - ex. or when it starts, only need the path to others once
    - about every 1 ms (milli sec)
        - due to this, typically on a general purpose CPU with a few cores
- data plane kicks in when a packet arrives on a link
    - about every 1 ns (nano sec)
    - implemented on specialized hardware where people make special circuits to forward

### simplest instantiation -> routing table
- map from destination address to output port
- at each router, say which router you need to go to next to get to a destination
    - ex. to get to C -> go to 3
    - so basically a table at each router
- the contÍol plane writes these tables
- data plane reads ffrom these tables
- NOTE: we're talking about intradomain routing (one network)
    - domain, collection of networks, owned by same entity

### question
- how do you run djikstras in a distributed setting where each network doesn't know what the entire global network looks like?
    - get info about neighbors

### link state routing protocols
- the link is the edge in networking terms
- link state is a property of the edge like propagation delay or queuing delay
- every node collects link state information from all the links its connected to
- sends a link state advertisement to all its neighbors
    - link state advertisement is the information about the neighborhood of each node
- then run djikstra's

### distance vector protocol
- state: distance vector
    - map from destination address to distance
1. send DV to neighbors
2. upon receiving DV,
$\forall v, d_R(v) = min(d_R(v),d_N(v) + link\_metric_{R,N})$
    - destination is v
    - where you are is R
3. if R's DV changes, R tells its neighbors

# 2/26/24
### internet
- maybe you have google network, comcast network, nyu network
    - routers sit inside these domains
- how do you get global connectivity? communication between the domains
- domains have routers at the edge of the domain, border routers
    - border routers talk to other border routers for info, serve as representatives for the domains

### inter domain routing
- from source to destination, what domains do we go through
    - AS will represent the domain/network
- say D1 owns a block of IP addresses like 10.2.3.* (prefix)
    - D1 tells D2: to get to 10.2.3.*, send pkts to D1
    - D2 tells D3: to get to 10.2.3.*, send pkts to D2,D1 (as a path)
    - D3 tells D4: to get to _____, send pks through D3,D2,D1

### BGP: Border Gateway Protocol
- selective import and export of advertisements (local preference)
- how do border routers talk to other border routers?
    - eBGP: interdomain routing, establishes what sequence of domains you have to go through
    - runs over TCP for reliable delivery of packets
- how do you get information from the border router into the domain itself?
    - iBGP: propagate information learned from eBGP into a domain, takes sequence of domains and within each demain exports the info
        - R3 tells R1 R2 R3 R4 that to get to a please send pkts to R3
            - once in a domain then intradomain routing takes over

### relationships between domains
- two domains from NYC to Philadelphia
- peering -> each ISP carries traffic on behalf of the ISP for free
    - if theres an imbalance maybe its not economically equal for both ISPs
- customer provider relationship
    - only carry traffic if one of the sender or destination is a customer
    - carrying out routing decisions to advance economic incentives

### internet
- hierarchy of tiers of ISPs
- want to avoid valleys

### flattening
- want to bring content closer to users
- local ISPs have relationship to regional ISPs
    - but might also have a peer relationship to content provider like google

# 2/28/24
- hosts and routers: goal is to get from host A to host B
    - as we've seen before we use routing tables

### exact matching
- you have a destination address and look at each entry in the lookup table for the same dst address
- what is better?
    - hashmap
    - but there are so many addresses say for example 32 bit addresses then there could be 2^32 entries

### longest matching
- Destination address -> lookup module -> lookup table
    - but in the lookup table, group of addresses can represent single entries
    - represent IP addresses as a group of bits + a "*"
        - star meaning it can differ
        - so 001001 and 001000 would be 00100*
        - 00100 is the prefix, "*" is unspecified
    - so columns of lookup table are prefix and NH
- 1****** represents 32 addresses with one entry
- what if you have multiple matches?
    - find the most specific, the longest prefix (hence longest prefix matching)
    - take the one with less wildcards(*) in the match
    - in practice , *s are only at the end of the address

### implementation
- in software:
    - write code, give to compiler, produce executable, run executable
- in hardware:
    - take functionality, devise digital circuit, realize this circuit through boolean gates

### LPM Module
- trie
    - go down this tree like data structure to find the IP address
        - when you stop, that is the longest prefix match

#### exact match module
- in software, hash table

### Hardware Implementation of Forwarding
#### exact match in hardware
- take destination address -> put into hash function which gives an index in a table
    - to deal with collisions, can either have a linked list or do linear probing (put in next available slot)
