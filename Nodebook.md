# Fast, Anonymous, Distance-Based Routing

## Table of Contents
- [Fast, Anonymous, Distance-Based Routing](#fast-anonymous-distance-based-routing)
	- [Table of Contents](#table-of-contents)
	- [Problem Statement](#problem-statement)
	- [Background Research](#background-research)
	- [Experiment](#experiment)
		- [Hypothesis](#hypothesis)
		- [Variables](#variables)
		- [Procedure](#procedure)
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
To reproduce, build the simulation program with the rust nightly compiler and run the tests

## Data
######N/A

### Analysis
???

## Discussion

???

## Citations
https://medium.com/@maidsafe/structuring-network-with-xor-431e785b5ee7
https://www.youtube.com/watch?v=w9UObz8o8lY
https://www.torproject.org/
https://geti2p.net/en/about/intro