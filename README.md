# IPFS PubSub Messaging Platform

This repository provides a messaging platform between two IPFS nodes using the Publish-Subscribe pattern, also known as 'pubsub'. IPFS (InterPlanetary File System) is a protocol and network designed to create a distributed and decentralized file system. The pubsub feature in IPFS enables greater network scalability and flexibility by allowing publishers to send messages classified by topic or content, and subscribers to receive only the messages they are interested in.

## Prerequisites

Make sure you have the following prerequisites installed on your development machine:

- Git: [Download & Install Git](https://git-scm.com/downloads). Git is usually pre-installed on macOS and Linux machines.
- Node.js: [Download & Install Node.js](https://nodejs.org/en/download/) and the npm package manager.
- IPFS Daemon: Install [js-ipfs](https://github.com/ipfs/js-ipfs) and [IPFS Desktop](https://docs.ipfs.io/install/ipfs-desktop/) (which runs the go version of IPFS) or download [go-ipfs](https://dist.ipfs.io/#go-ipfs) and follow the installation instructions.

## Installation and Running the Example

With Node.js and git installed, install the project dependencies:

```console
$ npm install
```

Start the example application:

```console
$ node ./node1-api/serv.js
```
```console
$ node ./node2-api/serv.js
```
In another terminal
```console
cd node1-react-app

npm run start
```

In another terminal
```console
cd node2-react-app

npm run start
```

### 1. Start two IPFS nodes

To demonstrate pubsub we need two nodes running so pubsub messages can be passed between them.

Right now the easiest way to do this is to install and start a `js-ipfs` and `go-ipfs` node. 

### 2. Start the IPFS nodes

#### JS IPFS node

```sh
npm install -g ipfs
jsipfs init
# Configure CORS to allow ipfs-http-client to access this IPFS node
jsipfs config --json API.HTTPHeaders.Access-Control-Allow-Origin '["http://127.0.0.1:8888"]'
# Start the IPFS node, enabling pubsub
jsipfs daemon
```

#### GO IPFS node

```sh
ipfs init
# Configure CORS to allow ipfs-http-client to access this IPFS node
ipfs config --json API.HTTPHeaders.Access-Control-Allow-Origin '["http://127.0.0.1:8888"]'
# Start the IPFS node, enabling pubsub
ipfs daemon --enable-pubsub-experiment
```

In the "Node to connect to" field enter `/ip4/127.0.0.1/tcp/5001` in one browser and `/ip4/127.0.0.1/tcp/5002` in the other.

This connects each browser to an IPFS node and now from the comfort of our browser we can instruct each node to listen to a pubsub topic and send/receive pubsub messages to each other.

> N.B. Since our two IPFS nodes are running on the same network they should have already found each other by MDNS. So the "CONNECT TO PEER" field is not required. If you find your pubsub messages aren't getting through, check the output from your `jsipfs daemon` command and find the first address listed in "Swarm listening on" - it'll look like `/ip4/127.0.0.1/tcp/4002/ipfs/Qm...`. Paste this address into the "Peer to connect to" field for the browser that is connected to your go-ipfs node and hit connect.

Finally, use the "Subscribe To" and "Message to send" fields to do some pubsub-ing, you should see messages sent from one browser appear in the log of the other (provided they're both subscribed to the same topic) by clicking on Call API button.


## Code Execution
#### Clip:
![alt text](https://github.com/Aryamanraj/IPFS-pubsub-React-App/blob/master/files/tutorial.gif)

## Code Breakdown
The node1-api has serv.js file that contains functions:
- `function nodeConnect` to create IPFS node and connect it to the browser
- `function peerConnect`for peer discovery and connection
- `function subscribe` takes a topic as an argument and uses pubsub.subscribe to recieve messages relate to the topic.
- `function send` takes message as and uses pubsub.publish to send messages related to the topic.


The node2-api has similar code breakdown.


## Conclusion
The exchange of messages between Node1 and Node2 on topic of common interest is successfully implemented by the use of PubSub.
