# System Design: Basic Concepts

#### Basic Concepts
- **[Client Server Architecture](#Client-Server-Architecture)**
- **[Horizontal VS Vertical Scaling](#Horizontal-VS-Vertical-Scaling)**
- **[Distributed System](#Distributed-System)**
- Consistent Hashing
- **[CAP Theorem](#CAP-Theorem)**
- **[HTTP Long Polling](#HTTP-Long-Polling)**
- **[Domain Name Service](#domain-name-service)**

---
## Client-Server Architecture
- The client-server architecture is a distributed application framework consisting of clients and servers in which server hosts, manages, and delivers client services. 
- Clients are connected to a central server, and they communicate over internet connection through computer network. 
- Whenever client needs any service, it sends a request to servers, which process the request and return response to the client.

#### Components of Client-Server Architecture
- **Server:**
  - A software that receives and processes requests from clients. 
  - It usually operates on a remote machine and can be accessed by a user’s local computer or workstation.
  - A client can use a server to share resources and distribute tasks.
- **Load Balancer:**
  - Responsible for distributing incoming requests across a group of servers to manage traffic and optimize resource usage.
- **Client:**
  -  A computer application that takes the input and sends requests to the servers. 
  -  It runs on a user’s local computer/remote machines and connects to a server. 
  -  They are software application that requests resources and services made available by a server.
- **Network protocols:**
  -  The client-server model follows a request-response messaging pattern.
  -  It communicates using the typical TCP/IP protocol suite, which distributes the application data into packets that networks can deliver and manages flow control.
  -  Once connection is established in TCP protocol, it is maintained until client and server have completed the message exchange.
  -  While IP is a connectionless protocol in which each independent unit of data is unrelated to any other data units and travels through the internet.
- ![image](https://user-images.githubusercontent.com/57194114/201549140-931827aa-634f-4f2e-8bc2-d475ee46e5f9.png)

#### 
---

#### Horizontal VS Vertical Scaling

#### Distributed System
- A distributed system is a collection of computers that work together to form a single computer for end users. 
- All of the distributed machines have one shared state and operate concurrently. 
- With distributed systems, users must be able to communicate with any of the distributed machines without knowing it’s only one machine.
- The distributed system network stores its data on more than just a single node, using multiple physical or virtual machines at the same time.

#### CAP Theorem



### Domain Name Service

#### Links
- https://www.educative.io/blog/what-is-cap-theorem
- 
