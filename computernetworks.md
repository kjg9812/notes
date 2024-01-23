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
