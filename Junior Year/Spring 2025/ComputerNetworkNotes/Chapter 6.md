Computer networks
04-10-2025

(Link layer through physical connections)
skipping MLPS stuff, physical layer stuff will only be in class, nothing in the book

## VLANS 
>VLANS: way to split one physical network into multiple separate networks.


Networks without VLANs are like one big room where everyone can hear each other. With VLANs, you can create walls between different groups of computers.

Computers in the same VLAN can talk to each other easily. Computers in different VLANs need special permission to communicate.

The way it works is there will be two sides to a network, lets say one side is the engineering dept. and the other side is the sales dept., the entire network could split into two sides (using a VLAN) and will be more organized. The way the two can communicate is through a switch (very old way). 

Trunking - carries multiple VLAN's over a single network. Kinda like a highway that can carry multiple cars at once
Without trunking, you would need a separate cable for each VLAN between switches. Trunking saves a lot of cables and ports.


802.1p is a protocol that prioritizes sound quality over transmission time.

CAM - table of MAC addresses 
spanning tree protocol (STP) -  Finds the loops in a layer 2 network and turns off certain links. If a link goes down, then a calculation is made and links will be reinstated to ensure a level of fault tolerance inside of a network. **This avoids loops within a network**.

## Synthesis "a day in the life of a web request"
1. Laptop connects to the network and needs an IP. 
	- Sends a DHCP discover to get an IP address
	- CAM table gets populated as host gets MAC addresses
2.  Before HTTP request can be sent, host needs to find google.com IP address
	- DNS query asking for IP is made and is then encapsulated. MAC address is requested to find frame to router. Then with the MAC address of the router which has the IP address of google, the router sends a DNS request to the DNS server of google. This updates the SRC MAC address to the router of googles DNS server. (MAC address gets rewritten every hop)
3. Now we know the IP address of google. Now TCP connection needs to be made. SYN is sent to begin 3 way handshake. Web server returns with SYN ACK. Host responds with ACK completing 3 way handshake
4. Web server replies and host gets the webpage