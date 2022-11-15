# System Design: Basic Concepts

### List of Basic Concepts
- **[Client Server Architecture](#Client-Server-Architecture)**
- **[Peer-to-Peer Networks]** - TODO
- **[Latency](#Latency)**
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

#### How does the Client-Server Model work?
- The data flow is unidirectional, which forms a cycle. It is initiated when a client requests some data, and server processes the request and sends some sort of data back to the client via a protocol. 
- Clients cannot directly talk to each other.
- Data flow in a client-server architecture will look like this:
  - Client requests data from server
  - Load balancer routes the request to an appropriate server
  - Server processes the client request and queries an appropriate database for some data
  - Database returns the queried data back to server
  - Server processes the data and sends data back to client
- To better understand the data flow in a client-server architecture, let’s explore how browser (Client) interacts with servers?
  - User enters the website URL
  - Browser sends a request to the DNS server to look up the IP address of webserver.
  - DNS sends the IP address of webserver to the browser
  - Now browser sends an HTTP/HTTPS request to the IP address of webserver.
  - Web server sends back necessary files for the website.
  - Now browser renders the files and displays the website.

#### Client-Server vs Peer-to-Peer Architecture
- Here are some major differences between peer-to-peer and client-server architecture
- ![image](https://user-images.githubusercontent.com/57194114/201550121-0d85d5d8-cc06-4642-b685-e1074b7ea06e.png)

#### Advantages of client server architecture
- **Centralised Management:**
  - Client server architecture is a centralised network of systems with all data in a single place, with complete leverage to control processes and activities. 
  - One can easily share resources and data across various platforms, and users have the authority to access any file residing in the central storage at any time. 
- **Flexibility:**
  -  Since data being passed between the client and server and server services are entirely up to the programmer.
  -  So there could be several ways to use client-server architecture to solve problems that may arise in the future.
  -  It can also be easy to combine with other types of architecture on the client or server sides.
- **Extensibility:**
  -  The system can be updated based on changes in functional and non-functional requirements without altering the client-server architecture or disrupting service.   
- **Transparency:**
  - Clients only make requests to the server with their input data, so they don’t see how servers will handle the request.
  - It may seem like a single, central server for a user.
- **Availability:**
  - Most of the time, servers do not need to shut down or restart for a long duration.
  - So server uptime is possible during maintenance with server duplication.
  - On another side, there is a clear separation between clients and servers because clients are consumers, and servers are service providers. 
  - If several servers offer the same services, system can still function if one or more servers fail.  
- **Scalability:**
  - Capable of adding or removing servers in the network (Horizontal scaling) or migrating to larger and faster server machines (Vertical scaling). 

#### Disadvantages of client server architecture
- There could be more chances of failure due to centralised control.
  - Many clients can send simultaneous requests to the server (traffic congestion), which might overload the server and drastically slow down performance. 
  - This could also lead to a server failure, in which case the whole system goes down, and clients may not get any responses back.
- Servers are much more powerful than client computers, which means they are more expensive. It also needs some human resources with networking and infrastructure knowledge to manage the system.
- Vulnerable to Denial of Service (DOS) attacks because the number of servers is considerably smaller than the number of clients.
- Data packets may be modified during transmission, so the loss of helpful information can be also common.

#### Links
- https://www.enjoyalgorithms.com/blog/client-server-architecture
---


## Latency
### What is latency?
- The browser (client) sends a signal to the server whenever a request is made. 
- Servers process the requests and retrieve the information to bring it back to the client. 
- So, latency is the time interval between the start of a request from the client end to delivering the result back to the client by the server, i.e., the round trip time between the client and the server.
- ![image](https://user-images.githubusercontent.com/57194114/201819583-dd886d82-f48d-4b80-91a5-0fbc59f565f4.png)
### How does Latency work?
- **Latency is nothing other than the estimated time the client has to wait for after starting the request to receive the result.**

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
