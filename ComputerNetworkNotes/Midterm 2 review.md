Computer networks
04-01-2025

**Exam will be ENTIRELY multiple choice**
Topics are covered from 3.5-5.2 in the book...
## *Q&A Questions which may (probably will) be on the exam:*

Why might an application developer choose UDP over TCP.
- An application dev would use UDP because it is faster than a TCP connection. UDP is also computationally cheaper

How does TCP ensure that data is delivered in the correct order.
- By using sequence numbers are rearranging packets once it gets received

What happens if a TCP segment is lost during transmission.
- There is a sender timer and when it runs out then it will retransmit the message. This can also happen if the receiver sends a NACK.

What effect does congestion have on TCP connections.
- TCP congestion can cause queueing delays as well as overwhelming the timing threshold and can cause a second transmission from sending causing overall latency.

What is the purpose of the SYN ACK flags and the in the TCP header list
- SYN is used to establish a connection
- ACK is used to acknowledge that the receiver received the SYN

When would a TCP sender request a retransmission 
- When a timeout occurs or when there are 3 duplicate ACK 

What makes QUIC more suitable for mobile connections than TCP
- Its because QUIC uses UDP which is more suitable for using mobile networks because it is tolerant for changing network addresses like IP

What info is stored in a routers forwarding table?
- Forwarding tables tell network devices where to send data packets. They are like maps that routers use to guide traffic. 

What does trace route use to do what trace route does?
- TraceRoutes trace the path a packet takes from source to destination. They use ICMP packets by using TTL to increase it by every hop 

How does a link state routing use faster convergence than distance vector writing?
- Because they already know all the links on the network thus saving the timing for calculations.

Why is BGP largely policy based?
- Big orgs talking to other big orgs making sure they don't want to route through networks they do not want to deal with.

What is the signification of cost networks in SFPF?
- Allows path selection of bandwidth 

How are TCP sequence numbers and ACK numbers related?
- If A is talking to B, A transmits sequence a sequence number to B (Lets say SEQ = 100 : length = 10 so total is 100-109), B would send an ACK of the next expected number in the sequence (110). Essentially the ACK acknowledges the bytes that A or B ended up receiving up to.

What is the purpose of TCP flow control?
- To send packets over a TCP connection proportional to what the receiver can actually handle/receive.

How does TCP estimate the RTT?
- It sends a packet and waits for ACK and it will compute how long it took for it so get the ACK thus getting the RTT.

Why is UDP used for DNS?
- The DNS mechanism itself has built in redundancies 

What protocol is used to map IP addresses to MAC messages
- ARP protocol 

Whats the purpose of ICMP redirect message
- When a router that is not connected to where a packet needs to go it will send a redirect to the host telling them that there is a better router that the host can use to properly send the data.

How do routers prevent packets looping forever?
- TTL (Time to live). When the countdown hits zero the packet gets dropped. The countdown goes down by reducing its health by one.

There will be a problem on Dijkstrasn's algorithm where you have to partially fill out a table and likely one that will have Bellman-Ford's Equation

Try to understand how TCP evolved.

Know how timeouts are calculated. Specifically TCP.

What is a middle-box and their roles in a network
- Something that rewrites packets or stops packets from doing something. For more about middle-boxes ----> [[Chapter 4 notes cont.]]
- most middle-box functions such as firewall come as one box for the last 25 years. 