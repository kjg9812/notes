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
