This file explains the fundamental concepts of peer-to-peer (P2P) networking in go-ethereum (geth), exploring its architecture, key components, the discovery protocol, and the challenges faced when designing decentralized network systems. (shortly)

# How the Peer-to-Peer network works in go-ethereum
## 0. Prior knowledge
- list of terms and useful links

## 2. Basic components of the P2P network in Ethereum
### 2.1 P2P protocols
#### 2.1.1 Discovery Protocol (V4 and V5)
- Explain that the Discovery Protocol is used to locate nodes on the Ethereum network.
- Main Purpose:
	+ V4: Based on Kademlia-style DHT (Distributed Hash Table). Nodes find their "nearest neighbors" by their Node ID.
	+ V5: Protocol update to support large networks and new features. ENR (Ethereum Node Records) has been introduced to improve routing and transfer additional metadata.
- Highlight the difference between V4 and V5:
	+ V5 solves the disadvantages of V4: improved security, scalability and modularity (used in Ethereum 2.0 and more modern frameworks).
#### 2.1.2 DevP2P Protocol
- Describe DevP2P as a low-level protocol that runs on top of Discovery and processes:
	+ Connecting and disconnecting nodes.
	+ Message transfer (transactions, blocks, etc.).
	+ Installation of other subprotocols (for example, the Ethereum wire protocol).
- Mention that DevP2P allows you to run multiple protocols on top of a connection. For example:
	+ Subprotocol "eth" (for blockchain data exchange).
	+ Subprotocol "snap" (for faster synchronization).

## 3. Implementation of P2P in go-ethereum
### 3.1 Basic architecture
#### 3.1.1 RLP Encoding
- Explain RLP (Recursive Length Prefix) as a way to serialize data.
	+ Why compact message serialization in P2P is important (nodes transmit millions of messages, and speed is important).
	+ Using RLP in the Discovery Protocol (for example, for ENR serialization).
### 3.2 The main P2P modules in geth
#### 3.2.1 p2p/discover
- The main module of the Discovery Protocol implementation (V4/V5):
	+ Determines the location of nodes.
	+ Operations: search for nearest neighbors, exchange Node ID and ENR.
- Important functions inside the module (findNode, ping, pong).
#### 3.2.2 eth/protocols/eth
- Ethereum wire protocol, which implements data synchronization between nodes:
	+ Broadcasting of new blocks and transactions.
	+ Data synchronization with other nodes
- A brief example: the DevP2P part, where they "talk" only about Ethereum data.
#### 3.2.3 p2p/server
- Services for establishing connections between nodes:
	+ Listener for incoming connections.
	+ Connection monitoring, network overflow prevention.
### 3.3 Example of Peer Discovery operation
- An example of a real Peer Discovery process:
	+ The new node needs to connect to the network. What is he doing?
	+ Uses Discovery (findNode → Neighbors) to find the nearest neighbors.
	+ The list of nodes is updated during the process.
- Mentioning the use of Kademlia Tree as a data structure.
