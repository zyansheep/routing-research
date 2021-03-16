# Distance-Based Routing

## Table of Contents
- [Distance-Based Routing](#distance-based-routing)
	- [Table of Contents](#table-of-contents)
	- [Problem Statement](#problem-statement)
	- [Background Research](#background-research)
	- [Experiment](#experiment)
		- [Hypothesis](#hypothesis)
		- [Variables](#variables)
		- [Procedure](#procedure)
			- [Node Message Sending](#node-message-sending)
			- [Distance Based Routing](#distance-based-routing-1)
	- [Data](#data)
	- [Analysis](#analysis)
	- [Discussion](#discussion)
	- [Citations](#citations)

## Problem Statement
Conventional internet anonymization networks (i.e. [Tor](https://www.torproject.org/) and [I2P](https://geti2p.net/en/about/intro)) are slow to use making them unappealing for use in real-time applications. This is because data is routed through random nodes in the network from source to destination. Packets could be routed from Europe to Asia and around the world before reaching their destination. This random-routing protocol is necessary, however, for anonymizing a connection and keeping participants's real IPs private which is the main goal of these networks.

This research project aims to test an alternative method of packet routing that is more efficient than random-routing while also keeping anonymity. Instead of randomly routing data through internal nodes in the network, the network self-organizes itself so that each node only knows about nodes closest to it. Nodes have a concept of *Virtual Distance* which is derived from a combination of the average latency between 2 nodes and the bandwidth each node is willing to contribute. Each node assigns itself a *Routing Vector* based on surrounding node's coordinates and their *Virtual Distance* which it then publishes to a distributed hash table (DHT) known by the whole network.

This self-organization allows any node to route to any other node and all that intermediate nodes need to do is route packets in such a way where the next node that a packet is passed to decrease the euclidian distance between a intermediate node's *Routing Vector* and the destination's vector.


## Background Research

There are many tools and mathematical concepts needed to create a decentralized system. Chief among 
Talk about Tor project, Kademlia routing etc.
XOR based distance routing: 
https://medium.com/@maidsafe/structuring-network-with-xor-431e785b5ee7
https://www.youtube.com/watch?v=w9UObz8o8lY

## Experiment
### Hypothesis
Routing speed and connection establishment can be made faster by a distance-based routing protocol compared to a random-routing protocol.

### Variables
 - Independent
   - Routing system type
     - Random-routing vs Distance-based routing
 - Dependent
   - Time for packets to reach their destination
   - Time to establish connection
 - Control
   - Max number of constant direct connections
   - Number of nodes testing with
   - Area of simulation
 - Constants
   - Network routing protocol


### Procedure
This experiment was run by creating a model of the internet that can route packets between *Nodes*. These *Nodes* keep track of their view of the network and pass packets around to replicate how real computers would implement this protocol.
To reproduce, build the simulation program with the rust nightly compiler and run the tests.

#### Node Message Sending

Connect Stage: - Establishes encrypted tunnel to another node
`Handshake` - Out of a simulation, this would be the point where symmetric encryption is initiated from an exchange of public keys. This is followed by an `Acknowledgement` to establish an encrypted connection

Exchange Stage: - Nodes query other nodes for relevent information like Route Coordinates, number of remote peers and reciprocal latency.

Locate Stage: - Request interaction with other nodes on the network
`RequestPings(number_of_pings)` - This requests another node to instruct a certain number of their peers to Ping this node. It is used to find close nodes on the network
Ping requests will be sent to the closest nodes in hope to find even closer nodes. This will occur recursively until the closest nodes are found.
`WantPing` - This will be sent by the other node to it's peers when this node is trying to find nodes to connect to.
`AcceptPing` - Sent from other node's peers to this node establishing connection

Establish Stage: - Notify closest nodes that they are close
`PeerNotify` - This notifies another node that this node thinks of it as a closer node (a peer) and that they should also test the connection. It can also be used to signal that a node no longer thinks of another node as close.

Coordinate Stage: - Calculate own Route coordinates based on closest node's coordinates adjusting for any discrepecies.

Typical message exchange for new node (1) bootstrapping to established node (2) who is also connected to node (3) and organizing itself onto the network.

1 -> 2: `Handshake`
2 -> 1: `Acknowledgement`
1 -> 2: `ExchangeInfo`
2 -> 1: `ExchangeInfoResponse` x5
1 -> 2: `RequestPings(5)`
2 -> 3: `WantPing(1)`
3 -> 1: `Handshake`
1 -> 3: `Acknowledgement`
3 -> 1: `AcceptPing`
1 -> 3: `Ping` x5
3 -> 1: `PingResponse` x5
1 -> 3: `RequestPings(5)`
Repeat until there are no closer nodes known
1 -> 15,67,23,...: `PeerNotify`

TODO: Once enough close peers are found, Routing Coordinates can be calculated 

#### Distance Based Routing
Once a Node calculates it's own Routing Coordinates, it publishes them to a Distributed Hash Table (DHT) which is a way to store key-value pairs across a network in a decentralized way.
Once a node's Routing Coordinates are public, anyone that knows a node's public Node ID can figure out where they are relative to 

## Data
Two sets of data: Ticks/Packet for Targeted Routing and Random Routing respectively 
######N/A

## Analysis
Hypothesis - The new distance-based routing method is faster than a random routing method

2 Samples - Sample of old routing times, Sample of new routing times
Need samples > 30 for approx normal distribution


2-Sample T-Test
Null Hypothesis: Mnew = Mold (try to disprove)
H_a: Mnew < Mold (proved by rejecting null hypothesis)

t = ((xbar_new - xbar_old) - 0 ) / stddev
stdev = sqrt(s1^2/n1  +s2^2/n2)

## Discussion

Since the early days of the internet, the main way to browse while hiding your own IP address is to use a proxy. This is what VPNs are essentially, routing your traffic to another computer on earth. However, while the desination server may not be able to correlate your request with a specific IP, the proxy can do that since all your traffic is running through it. 
This is what the TOR project aims to solve. TOR (The Onion Router) creates a network of proxies with which packets are routed through in such a way where no proxy can know both the origin of the packet and the destiation. It accomplishes this with onion routing: encrypting packets in layers such that each intermediate proxy can decrypt the outermost layer and figure out where to forward the packet next. 
This multi-staged routing approach is excellent at obscuring connections through the network of proxies. However, it has no way to determine which series of proxies packets should be sent through. For TOR, proxies are chosen at random from a database. This means that packets routed through TOR could travel around the globe and back before reaching their destination. This inefficent router selection process is what this paper aims to solve.
By programming each proxy in the network so that it locates other nearby proxies, DBR (Distance-based routing) can resolve the problem with TOR of randomly locating proxies.

## Citations
MaidSafe. (2018, December 11). Structuring network with XOR. https://medium.com/@maidsafe/structuring-network-with-xor-431e785b5ee7. 
safepodmtl. (2014). 1.2. Xor distance and basic routing. https://www.youtube.com/watch?v=w9UObz8o8lY. 
Computerphile. (2017). Onion Routing - Computerphile. YouTube. https://www.youtube.com/watch?v=QRYzre4bf7I. 
I2P Developers. (2021, February 17). Intro to I2P. I2P. https://geti2p.net/en/about/intro. 