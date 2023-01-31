# System Design: Basic Concepts

### List of Basic Concepts
- **[Latency](#Latency)**
- **[Throughput](#Throughput)**
- **[Availability](#Availability)**
- **[Process Management in Operating System](#Process-Management-in-Operating-System)**
  - **[Process Memory](#Process-Memory)**
  - **[Process Control Block](#Process-Control-Block)**
  - **[States of a Process in Operating System](#States-of-a-Process-in-Operating-System)**
- **[Client Server Architecture](#Client-Server-Architecture)**
- **[Peer-to-Peer Networks]** - TODO
- **[Network Protocols](#Network-Protocols)**
  - **[OSI Model](#OSI-Model)**
  - **[Internet Protocol](#Internet-Protocol)**
  - **[IPv4 VS IPv6](#ipv4-vs-ipv6)**
  - **[Transmission Control Protocols](#Transmission-Control-Protocols)**
  - **[HTTP](#HTTP)**
  - **[World Wide Web Consortium](#World-Wide-Web-Consortium)**
  - **[Remote Procedure Call](#Remote-Procedure-Call)**
- **[Long Polling](#Long-Polling)**
- **[WebSockets](#WebSockets)**
- **[Domain Name Service](#domain-name-service)**
- **[IaaS-PaaS-SaaS]**
  - **[IaaS](#IaaS)**
  - **[PaaS](#PaaS)**
  - **[SaaS](#SaaS)**
- **[Caching](#Caching)**
  - **[Types of Caching](#Types-of-Caching)**
  - **[Cache Eviction Policy](#Cache-Eviction-Policy)**
  - **[Cache Invalidation](#Cache-Invalidation)**
- **[Load Balancing](#Load-Balancing)**
  - **[Load Balancing Algorithms](#Load-Balancing-Algorithms)**

---
---

## ```Throughput```
- Throughput: the number of records we can process per second, or the total time it takes to run a job on a dataset of a certain size.
  - // In an ideal world, the running time of a batch job is the size of the dataset divided by the throughput. In practice, the running time is often longer, due to skew (data not being spread evenly across worker processes) and needing to wait for the slowest task to complete.
- Throughput is defined as the total number of items processed per unit of time, or we can say Throughput is the rate at which something is produced.
  - It is generally represented as the number of bits transmitted per second or the number of HTTP operations per day.
  - The system’s Throughput is generally obtained by summing up all the items and dividing the sum by the sample interval.
  - It is a standard way of obtaining the Throughput, but it suffers from ignoring the processing speed variations.
- Misconceptions with Latency
  - Latency is defined as the time interval between making a request and beginning to see a result. It is measured in the unit of time. Latency is always misunderstood with Throughput, and it is taken for granted that High throughput systems should have low latency.
  - **However, this may not always be true**. Consider the data processing in association with disks, which tend to have large Throughput but fail to provide low latency.
  - Similarly, in networked connections, the latency increases with Throughput. With the increase in Throughput, more and more packets will be there on a wire and contribute to increased latency.
  - It is also possible to have systems with Low Throughput and Low Latency also. Hence the combination of Latency and Throughput is best chosen by considering the system and business requirements.

### ```Factors Affecting Throughput```
- The Throughput of the system depends on various factors. It depends on underlying analog limitations, the system’s processing power, accessibility of the service, and various hardware components. It also gets affected by the network’s traffic, interference changes, and transmission errors. Throughput also depends upon the protocol overheads as these overhead affects the data transfer rate and limits the system from achieving the maximum desirable Throughput.
- **Analog limitations**
  - Analog physical medium has a profound influence on the maximum attainable Throughput of the system.
  - In networked communication, the medium’s analog limitations affect the Throughput by sticking to an upper bound limit on the amount of shareable information. 
- **Hardware limitations**
  - There is an upper bound to every computing and processing system.
  - This limits the Throughput of the system as computational systems have some finite processing power only.
  - When a large and complicated query requires massive computations, it causes a sound effect on the processing speed and, hence, the system’s Throughput. 
- **Accessibility**
  - Accessibility to the service also affects the Throughput of the system. When multiple users share a single communication system simultaneously, it may involve sharing the resources and affecting the system’s Throughput.
  - Moreover, accessibility to many customers also increases the traffic in the network, which is also an important factor in decreasing the Throughput. 

#### Links
- https://www.enjoyalgorithms.com/blog/throughput-in-system-design/

---
---

## ```Availability```
- Availability is the percentage of time in a given period that a system is available to perform its task and function under normal conditions.
- One way to look at is how resistant a system is to failures.
- A system’s availability is measured as the percentage of a system’s uptime in a given time period or by dividing the total uptime by the total uptime and downtime in a given period of time.
- High Availability comes with its own tradeoffs, such as higher latency or lower throughput, and achieving high availability is very difficult. To make highly available systems, we need to make sure that the system does not have any 

#### ```Achieving High Availability```
- High Availability comes with its own tradeoffs, such as higher latency or lower throughput, and achieving high availability is very difficult. To make highly available systems, we need to make sure that the system does not have any single point of failure.
- To eliminate any single point of failure, we need to make our system more redundant. 
- **Redundancy is the act of duplicating or adding certain parts of our system.** 
  - Let's take an example; imagine you have a system consisting of two identical web servers that are installed behind a load balancer. The traffic coming from clients will be distributed between the web servers, but if one of the servers goes down, the load balancer will redirect all traffic to the remaining server, which is working.
  - Now that we have made our servers redundant and prone to failure as the load balancer can detect the failure and respond accordingly. But, in this scenario, the load balancing layer itself remains the single point of failure. To avoid this, a simple way out is to make the load balancing layer redundant.
- An essential thing to note here is that redundancy alone cannot ensure high availability.
  - A device also needs mechanisms for detecting failures.
  - It is also important to be able to perform high-availability testing and to be able to take corrective action any time one of the stack’s components becomes unavailable. 
  - Top-to-bottom or distributed high-availability approaches may include both work and hardware, or software-based downtime reduction techniques are also successful.   - Redundancy is a hardware-based approach. The implementation of high availability techniques, on the other hand, almost always requires software.
- **Difference Between High Availability and Fault Tolerance**
  - Fault-tolerant computing requires full hardware redundancy.\
  - To achieve fault tolerance, several systems run in parallel, mirroring programs identically and executing instructions together.
  - If the main system fails, with no loss in uptime, another system can take charge. 
  - It would be best if you had advanced hardware to achieve fault-tolerant computing.
  - It must be able to detect component faults immediately and allow the various systems to operate in conjunction.(???)
  - This form of machine preserves the programs’ memory and records, a major advantage. However, it may take longer for networks and devices that are more complicated to respond to malfunctions. In comparison, technical issues that cause crashing systems may also cause a similar breakdown of redundant systems running parallel, creating a system-wide failure.(???)
  - A high-value strategy instead uses a software-based approach to minimize server downtime rather than a hardware-based approach. A high-display cluster(???) finds a collection of servers together instead of using physical hardware to achieve maximum redundancy.(???)
  - A high-value strategy instead uses a software-based approach to minimize server downtime rather than a hardware-based approach. A high-display cluster finds a collection of servers together instead of using physical hardware to achieve maximum redundancy.

#### Links
- https://www.enjoyalgorithms.com/blog/throughput-in-system-design/
- https://www.youtube.com/watch?v=-BOysyYErLY&list=PLMCXHnjXnTnvo6alSjVkgxV-VH6EPyvoX&index=20&ab_channel=GauravSen

---
---

## ```Process Management in Operating System```

#### ```Process```
- In simple words, a program in execution is called a process
- It is an instance of a program that actually runs, i.e., an entity that can be assigned and executed on a processor
- Two essential process elements are **program code** and a **set of data** associated with that code.

#### ```Process Memory```
- Process memory is divided into four sections:
  - The **program memory** stores the compiled program code, read in form of non-volitile storage when the program is launched
  - The **data section** stores global and static variables, allocated and initialized before executing the main program.
  - The **heap section** is used to manage dynamic memory allocation inside our program. In other words, it is the portion of memory where dynamically allocated memory resides i.e., memory allocation via new or malloc and memory deallocation via delete or free, etc.
  - The **stack section** stores local variables defined inside our program or function. Stack space is created for local variables when declared, and the space is freed up when they go out of scope.
- ![image](https://user-images.githubusercontent.com/57194114/202915150-ee07a87b-5b6b-4e58-8ee6-a5349925bd0a.png)
- Note: The stack and heap sections start at opposite ends of the process's free space and grow towards each other. When they meet, a stack overflow error will occur, or a call to new memory allocation will fail due to insufficient memory available in a heap section.

#### ```Process Control Block```
- https://en.wikipedia.org/wiki/Process_control_block
  - A process control block (PCB) is a data structure used by computer operating systems to store all the information about a process. 
  - It is also known as a process descriptor. 
  - When a process is created (initialized or installed), the operating system creates a corresponding process control block.
  - This specifies the process state i.e. new, ready, running, waiting or terminated.
- A program executing as a process is uniquely determined by various parameters. These parameters are stored in a Process Control Block (PCB). It is a data structure which holds the following information:
  - **Process Id**: A unique id associated with every process to distinguish it from other processes.
  - **Process state**: A process can be in a start, ready, running, wait and terminated state.
  - **CPU-Scheduling information:** This is related to priority level relative to the other processes and pointers to scheduling queues.
  - **CPU registers and Program counter(???):** Need to be saved and restored when swapping processes in and out of the CPU.
  - **I/O status information:** Includes I/O requests, I/O devices (e.g., disk drives) assigned to the process, and a list of files used by the process.
  - **Memory-Management information(???):** Page tables or segment tables(???).
  - **Accounting information(???):** User and kernel CPU time consumed, account numbers, limits, etc.
- ![image](https://user-images.githubusercontent.com/57194114/202916501-6d172c78-b0ea-4cc2-a3f8-8bc73a607d39.png)

---
---

## ```Client-Server Architecture```
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
---
## ```Network Protocols```
- A network can be defined as a group of computers and other devices connected in some ways to exchange data. It is a group of computers that use standard communication protocols over digital interconnections to share resources.
- A **network protocol** is a defined set of rules that specify how data is transmitted on the same network between different devices. In general, it allows connected devices to interact with each other, irrespective of any variations in their internal processes, structure, or configuration.

#### ```OSI Model```
- **Open Systems Interconnection (OSI)** model describes seven layers that computer systems use to communicate over a network. Each layer represents a different category of networking functions.
- These networking functions are made possible by protocols. For example, by indicating where data packets originate and their destination, the Internet Protocol (IP) is responsible for routing data.
  - Internet Protocol allows for network-to-network communications. IP is, thus, considered a protocol of the network layer (layer 3).
  - The Transmission Control Protocol (TCP) ensures, as another example, that the transport of data packets through networks goes smoothly. Thus, TCP is considered to be a transport layer (layer 4)
- **A packet is a tiny segment of a large message.** The data transmitted over networks of machines, such as the Internet, is split into packets. The computer or system that collects them then recombines these packets.

Open Systems Interconnection (OSI)             |  Types Network Protocols
:-------------------------:|:-------------------------:
![image](https://user-images.githubusercontent.com/57194114/204032537-2e6f9cbb-09e6-4629-b777-0f2484a14473.png) | ![image](https://user-images.githubusercontent.com/57194114/204032641-f95d2319-81b3-4819-86a9-4451269caf16.png)

- *```OSI Model Explained | Real World Example```:* https://www.youtube.com/watch?v=LANW3m7UgWs

#### ```Internet Protocol```
- The **Internet Protocol (IP)**  is a network layer protocol or collection of rules that allow data packets to be routed and addressed to pass through networks and reach the correct destination.
  - Modern Internet effectively runs on IP, or it follows the IP protocols. 
- When a machine or a client tries to interact with another machine or a server and sends data to other machines, it is sent in as an IP packet. 
- IP packets are a fundamental unit of data sent from one machine to another, or we can say IP packets are the building blocks of communication between machines. 
  - It is usually made of up bytes. 
  - IP packets have two main sections, IP header, and data.
- **IP address:** An IP address is a unique identifier assigned to a device or domain that connects to the Internet.
- ![image](https://user-images.githubusercontent.com/57194114/204037100-1631756d-d74b-4dd9-9736-4eef8bdc7a1d.png)
  - The image above shows a simple IP Packet. The IP Header contains essential information about the packet. Some of the information is:
    - It contains the source IP address of the packet.
    - It contains the destination IP address of the packet.
    - It also contains other information such as the packet's total size and the version of the IP that the IP packet is operating by (IPv4, IPv6, …).
- *```TCP/IP Model Explained```:* https://www.youtube.com/watch?v=OTwp3xtd4dg

#### ```IPv4 VS IPv6```
- IPv4 was the first version of IP. It was deployed for production in the ARPANET in 1983. Today it is the most widely used IP version. It is used to identify devices on a network using an addressing system.
- The IPv4 uses a 32-bit address scheme to store 2³² addresses which is more than 4 billion addresses. To date, it is considered the primary Internet Protocol and carries 94% of Internet traffic.
- IPv6 is the most recent version of the Internet Protocol. Internet Engineer Taskforce initiated it in early 1994. The design and development of that suite are now called IPv6.
- This new IP address version is being deployed to fulfill the need for more Internet addresses. It was aimed to resolve issues that are associated with IPv4. With 128-bit address space, it allows 340 unique address spaces.
- This new IP address version is being deployed to fulfill the need for more Internet addresses. It was aimed to resolve issues that are associated with IPv4. With 128-bit address space, it allows 340 unique address spaces. IPv6 is also called IPng (Internet Protocol next generation).
- *```IP Address - IPv4 vs IPv6```:* https://www.youtube.com/watch?v=ThdO9beHhpA
- *```IPv6 Addresses Explained```:* https://www.youtube.com/watch?v=irhS0ASkvy8

#### ```Transmission Control Protocols```
- One IP Packet's size is significantly less, which is not enough to send large files such as images, emails, and other heavy information. 
  - So, it will not fit in one IP packet. 
  - So, we have to use multiple packets to send such data. 
  - In this case, there is a chance that all the packets may not reach the destination and get lost in the route. 
  - There may also be a problem in the ordering of packets.
- We need to address the issue mentioned above and establish a protocol to transmit large files with correct ordering.
  - This is where **Transmission Control Protocol (TCP)** comes into play.
- **TCP** is a transport layer protocol built on top of the IP to solve the issues that we can face, as already mentioned.
- The main idea behind the TCP is that when a machine wants to communicate with another machine over TCP, it will create a TCP connection with the destination server when a browser wants to communicate with a website’s server
  - The way this connection is established is what is called a **handshake**.
  - A handshake is a special TCP interaction where one machine contacts the other by sending a packet or few packets. 
  - A request message that says it wants to establish a connection to the other machine to which the other machine responds by sending a message if it accepts the connection request. 
  - Then the client responds again by sending a message saying that the connection has been established (or cannot establish).
  - Now that the connection is established, they can freely communicate with each other.
- **Transmission Control Protocol (TCP) is a more powerful and more functional wrapper around the Internet Protocol (IP)**. It still lacks a robust framework that developers can use to define meaningful and easy-to-use communication channels for clients and servers in a system.
  - ![image](https://user-images.githubusercontent.com/57194114/204059747-b39410d8-cd75-481f-8d47-754e43e6236c.png)
 
- *```TCP: Transmission control protocol```:* https://www.youtube.com/watch?v=4IMc3CaMhyY
- *```TCP vs UDP Comparison```:* https://www.youtube.com/watch?v=cA9ZJdqzOoU

#### ```HTTP```
- **Hypertext transfer protocol:** is built on top of TCP and provides a higher-level abstraction above TCP and IP.
  - Its main characteristic is the request-response sequence.
  - A request is sent by one machine, and a response needs to be returned by the other machine.
  - For communication, most modern systems rely on the HTTP protocol.
- While using HTTP, we don’t need to know about TCP and IP packets.
  - It takes care of the developers of this complicated stuff connected to low-level machines.
  - Some fundamental HTTP methods include Put, Get, Post, Delete.
  - HTTP gives us the ability, rather than just data transfer, to incorporate business logic.

#### ```World Wide Web Consortium```
- World Wide Web Consortium, or W3C, is a vendor-neutral organization created in 1994 that develops common, interoperable protocols for the World Wide Web (WWW)
  - Web for All: The social value of the Web is that it enables human communication, commerce, and opportunities to share knowledge. One of W3C’s primary goals is to make these benefits available to all people, whatever their hardware, software, network infrastructure, native language, culture, geographical location, or physical or mental ability.
  - Web on Everything: The number of different kinds of devices that can access the Web has grown immensely. Mobile phones, smartphones, personal digital assistants, interactive television systems, voice response systems, kiosks, and even certain domestic appliances can all access the Web.
- RFC (stands for Request For Comments) is a document that describes the standards, protocols, and technologies of the Internet and TCP/IP.
  - It is the result of committee drafting and subsequent review by interested parties.
  - Some RFCs are informational in nature.
  - Of those intended to become Internet standards, the final version of the RFC becomes the standard, and no further comments or changes are permitted.

#### ```Remote Procedure Call```
- **Remote Procedure Call (RPC)(???)** is a protocol that can be used by one program to request a service from a program on a network located on another device without having to understand the details of the network.
  - It is an interprocess communication technique that is used for client-server-based applications.
  - It is also known as a subroutine call or a function call.

#### Links
- https://www.enjoyalgorithms.com/blog/network-protocols-concept/

---
---

## ```Long Polling```
- Polling is a technique that allows the servers to push information to a client. On another side, long polling is a version of traditional polling that allows the server to send data to a client whenever available. 
  - It involves the client requesting information from the server in the same way standard polling does, but with the caveat that the server may not respond immediately.
  - A complete response is delivered to the client once the data is accessible.
- Long polling reduces the number of HTTP requests required to send the same amount of information to the client.
  - However, the server must “hold” unfulfilled client requests and deal with the situation in which it receives new information to deliver, but the client has not yet issued a new request.
  - Long polling has the advantage of being part of the HTTP protocol, which means it’s widely accepted and generates less bandwidth than short polling because it requires fewer queries.
  - Overall, it is reliable for continuously updating clients with new information.
- The following is the basic life cycle of an application that uses HTTP Long Polling:
  - The client sends an HTTP request and then waits for a response.
  - The server provides the client with a complete response when an update is available.
  - After getting a response, the client sends a new long-poll request immediately or after a pause to allow for an appropriate latency duration.
  - A timeout is set for each Long-Poll request. After a connection is lost owing to timeouts, the client must rejoin regularly.
  - ![image](https://user-images.githubusercontent.com/57194114/204110558-9f320755-e341-4769-a5aa-f426c815381b.png)
- Long polling is a more efficient version of the basic polling method. 
  - Repeated requests to the server waste resources since each new incoming connection requires establishing a new connection, parsing HTTP headers, a new data query, and generating and delivering a response. 
  - After that, the connection must be ended, and any resources must be cleaned up.
- Long polling is a strategy in which the server chooses to keep a client’s connection open for as long as feasible, only responding when data becomes available, or a timeout threshold is reached, rather than having to repeat the procedure for each client until new data becomes available.

#### Links
- https://www.enjoyalgorithms.com/blog/long-polling-in-system-design/
- *```Short Polling vs Long Polling vs WebSockets:```* https://www.youtube.com/watch?v=ZBM28ZPlin8

---
---

## ```WebSockets```
- WebSocket is a bidirectional lightweight communication protocol that can send the client's data to the server or from the server to the client. It aimed to provide a full-duplex communication channel compared to a single TCP connection.
  - Once the connection is established, then kept alive until it is terminated by either the server or the client.
- Web Sockets are highly used in almost every real-time application like trading, multiplayer, and online games to receive data on a single communication channel in a bidirectional manner.
- **Web sockets and HTTP differ significantly**, but both protocols depend on TCP at layer 4 (transport layer) in the OSI model and are located at layer 7 (application layer).
- Web socket protocol mitigates the overhead associated with HTTP by enabling communication between a client and a server with low-weight overheads, aiming to provide real-time data transfer across the channel.
- It maintains a long-held single TCP socket connection between the client and the server to enable the bi-directional messages to be distributed efficiently, providing a latency-free connection.
- ```How does Web Socket work?```
  - Web Socket is a full-duplex protocol. Once the connection is established in a client-server architecture, the message exchange starts in a bidirectional mode and it will remain alive between client and server until either party terminates it.
  - However, for opening a web socket connection, one needs to call the web socket constructor. For web socket "ws:" and "wss:" URL schema is used; while for HTTP, "https:" is used.
  - After the establishment of a connection with the server, the data is sent to the server using the "send()" method on the connection object.(???)
    - Previously it used to support only strings, but now in the latest spec, it can send even the binary messages using Blob or ArrayBuffer object.
  - Similarly, the server might send us messages by firing the "onmessage" callback. The callback receives an event object while the data property is used to access the actual message.(???)
  - Web sockets also have an additional feature named Extension, which helps in sending compressed or multiplexed frames.(???)
- ```Key Features```
  - Bi-directional data exchange: Web sockets play a significant role in reducing network traffic by transmitting data in both directions simultaneously using a single connection.
  - HTTP compatibility: Web sockets are highly compatible with earlier versions of HTTP connection, which allow us to switch between HTTP and web socket. 
  - Publish/Subscribe event pattern: Web sockets allows a highly efficient data transfer model. Communication channels are established, allowing sending messages to and from a server and receiving event-driven responses without continuously polling the server.
- ```Examples```
  - Instant chat: Web Sockets play a significant role in real-time messaging. They are quite useful in building real-time and complex features like encrypted messages, typing indicators, etc., for chat services.
  - Multi-Player gaming: Web Sockets are quite helpful in synchronizing the game states between players and allow the low-latency network to work with. They allow seamless experiences across devices and allow many real-time interactive features for online and connected gaming.
  - Online maps: Live geo-location data is used to build real-time online maps. Web Sockets play a significant role in routing and navigation of any moving asset on an alive map.
  - Live results: Web Sockets help enhance the platform and keep updating the user with the latest information and lie status like election polls and their results, live score updates, etc. 

#### Links
- *```What are WebSockets? How is it different from HTTP?:```* https://www.youtube.com/watch?v=xTR5OflgwgU

---
---


## ```Latency```

### What is latency?
- The browser (client) sends a signal to the server whenever a request is made. 
- Servers process the requests and retrieve the information to bring it back to the client. 
- So, latency is the time interval between the start of a request from the client end to delivering the result back to the client by the server, i.e., the round trip time between the client and the server.
- ![image](https://user-images.githubusercontent.com/57194114/201819583-dd886d82-f48d-4b80-91a5-0fbc59f565f4.png)

### How does latency work?
**Latency is nothing other than the estimated time the client has to wait for after starting the request to receive the result.**
- Suppose you interact with an e-commerce website, you liked something, and added it to the cart. 
- Now when you press the “Add to Cart” button, the following events will happen:
  - The instant the “Add to Cart” button is pressed, the clock for latency starts, and the browser initiate a request to the servers.
  - The server acknowledges the request and processes it.
  - The server replies to the request, the request reaches your browser, and the product gets added to your Cart.
  - You can start the stopwatch in the first step and stop the stopwatch in the last step, and the diff would be the latency. 

### What causes Latency?
- The latency in a network depends on various parameters, and they have a sound effect in determining its value. 
- One of the major factors contributing to latency is outbound calls. 
- In the previous example of adding cart exercise, when you click the button on the browser, the request goes to some server in the backend, which again calls multiple services internally for computation (in parallel or sequentially) and then waits for a response or aggregates them. 
- All this adds to the latency of the call. However, It is mainly caused by the following factors:
  - **Transmission mediums:** The transmission medium is the physical path between the start and endpoints. The system's latency depends on the type of medium used to transmit requests. Transmission mediums like WAN and Fiber Optic Cables are widely used, but each medium has limitations, affecting the latency.
  - **Propagation:** It refers to the amount of time required for a packet to travel from one source to another. The system's latency highly depends on the distance between the communicating nodes. The farther the nodes are located, the more the latency.
  - **Routers:** Routers are an essential component in communication and take some time to analyze the header information of a packet. The latency depends on how efficiently the router processes the request. Router to router hop increases the system latency.
  - **Storage delays:** The system’s latency also depends on the type of storage system used, as it may take some time to process and return data. Hence accessing stored data can increase the latency of the system.

### How to measure Latency?
- **Ping(???):**
  - Ping is the most common utility used to measure latency.
  - It sends packets to an address and sees how fast the response is coming.
  - It measures how long it takes for the data to travel from source to destination and back to the source.
  - A faster ping corresponds to a more responsive connection.
- **Traceroute(???):**
  - Traceroute is another utility used to test latency. 
  - It also uses packets to calculate the time taken for each hop when routed to the destination.
- **MTR(???):**
  - MTR is a combination of both ping and Traceroute.
  - MTR gives a list of reports on how each hop in a network is required for a packet to travel from one end to the other.
  - The report generally includes details such as percentage Loss, Average Latency, etc.

### Latency optimization

Latency restricts the performance of the system; hence it is necessary to optimize it. We can reduce it by adopting the following measures:
- **HTTP/2:** 
  - We can reduce it by the use of HTTP/2.
  - It allows parallelized transfers and minimizes the round trips from the sender to the receiver, which are highly effective in reducing the latency.
- **Less external HTTP requests:**
  - Latency increases because of third-party services.
  - By reducing the number of external HTTP requests, the system’s latency gets optimized as third-party services affect both speed and quality of the application.
- **CDN:**
  - CDNs proved to be a boon in reducing latency.
  - CDN caches the resources in multiple locations worldwide and reduces the request and response travel time.
  - So instead of going back to the origin server, the request can be fetched using the cached resources closer to the clients.
- **Browser Caching:**
  - Browser Caching can also help reduce the latency by caching specific resources locally to decrease the number of requests made to the server. 
- **Disk I/O:**
  - The goal is to optimize algorithms to minimize the impact of disk I/O.
  - Hence, instead of often writing to disk, use write-through caches or in-memory databases or combine writes where possible or use fast storage systems, such as SSDs.
- As a developer, latency can also be optimized by making smarter choices regarding storage layer, data modelling, outbound call fanout, etc. Here are some ways to optimize it at an **application level:**
  - Inefficient algorithms are the most apparent sources of latency in code. It is necessary to avoid unnecessary loops or nested expensive operations.
  - Use design patterns that avoid locking as multithreaded locks introduce latency.
  - Use an asynchronous programming model to utilize better hardware resources as blocking operations cause long wait times.
  - Limiting the unbounded queue depths and providing back pressure typically lead to less wait time in the code resulting in more predictable latencies.

---
---
## ```IaaS```
- https://aws.amazon.com/what-is/iaas/
- https://azure.microsoft.com/en-us/resources/cloud-computing-dictionary/what-is-iaas/
- https://www.redhat.com/en/topics/cloud-computing/what-is-iaas
- https://www.ibm.com/topics/iaas
- https://www.youtube.com/watch?v=XRdmfo4M_YA&ab_channel=IBMTechnology
---
---

## ```Caching```
- Caching is the process of storing the results of a request in a cache or a temporary storage location so they can be accessed more quickly.
- So critical question is: What is cache? In system design, cache is a high-speed data storage that stores a subset of data so that future requests for that data are served up faster.
- In other words, caching allows us to reuse previously retrieved data efficiently.
- ![image](https://user-images.githubusercontent.com/57194114/202938085-da3a5b93-eac3-4aa9-85b9-b9d057b93998.png)
- Caching Examples:
  - Web Browsers cache the HTML, CSS, JS, and images for faster access to the website when requested again.
  - CDN store static files and help us to reduce latency.
  - DNS is used to get the IP address of a query. So, rather than requesting the IP address multiple times, it can be stored in a cache storage, allowing us not to re-perform a DNS query again, and the web pages can be accessed more quickly.

#### ```Types of Caching```
- **Application server cache**
  - We can cache the data directly in the Application Layer.
  - Every time a request is made to the service, it will return local, cached data quickly if it exists. 
  - If it is not in the cache, it will query the data from the database.
- **Global cache**
  - In Global Caches, the same single cache space is used for all the nodes.
  - Each of the application nodes queries the cache in the same way as a local one would be. 
- **Distributed cache**
  - The cache is usually broken up using a consistent hashing algorithm, and each of its nodes owns part of the cached data.
  - If a requesting node is searching for a certain piece of data, it can easily use the hashing function to locate information from the distributed cache to decide if the data is available. 
- **Content Distribution Network (CDN) (???)**
  - When our pages serve huge amounts of static media, this is the best option.
  - Suppose that the framework we are developing is not yet big enough to have a CDN of its own!
  - “Using a lightweight HTTP server like apache, we can serve static media from a different subdomain such as “blog.foobar.com” and cut the DNS from your servers to a CDN layer.(???)
  - ![image](https://user-images.githubusercontent.com/57194114/202943964-659be543-0d1b-4e59-b18b-390340f9ebfe.png)
- **Client-Side Caches**
  - Client-side caching duplicates the data of previously requested files directly within browser applications or other clients (such as intermediate network caches). 
- **ISP layer cache**
  - ISP - Internet Service Provider 
  - ISP caching works in much the same way as browser caching.
  - Once you have visited a website, your ISP may cache those pages so that they appear to load faster the next time you visit them.
  - The main problem with this is that, unlike your browser cache, you can not delete these temporary files; instead, you have to wait until your ISP's cache expires and requests fresh copies of the files. 

#### ```Cache Eviction Policy```
- We need to delete existing items for new resources when the cache is complete. In fact, it is just one of the most popular methods to delete the least recently used object. The solution is to optimize the probability in the cache that the requesting resource exists.
- Random Replacement: As the term suggests, we can randomly delete an entry.
- Least frequently used (LFU): We can keep a count of how frequently an item is requested and delete the least frequently used.
- Least Recently Used (LRU): In LRU, we delete the item that has been used least recently.
- First In First Out (FIFO): The FIFO algorithm holds an object queue in the order that the objects have been loaded into the cache. It evicts one or more objects from the head when a cache misses and inserts a new object into the queue tail. Upon a cache hit, the list does not shift.

#### ```Cache Invalidation```
- Cache invalidation refers to when web cache proxies declare cached content as invalid, meaning it will no longer be served as the most recent piece of content when requested. 
- The ultimate purpose, of course, is to ensure that the next time a client requests the affected content, the client receives the newest version at all times. 
- There are primarily three kinds of systems for caching:
  - **Write through cache:**
    - The writes go through the cache, and only if writes to DB and the cache both succeed, write verified as a success.
    - Between cache and storage, we will have full data consistency.
    - Nothing can get lost in case of a crash, power failure, or other system disturbances.
    - In this case, however, writing latency would be higher since two different systems are written.
    - // Seems like a choice for frequent reads - seldom writes
  - **Write around cache:**
    - The write directly goes to the DB, bypassing the cache.
    - Cache misses are increased because, in a cache error, the cache device reads the information from the Database.
    - Consequently, in applications that quickly write and re-read the data, this can lead to higher reading latency.
    - Reading must take place through slower back-end storage and higher latency.
  - **Write back cache:**
    - The write is rendered directly to the cache layer, and as soon as the write to the cache is finished, the write is verified.
    - The cache then synchronizes this writing to the DB asynchronously.
    - For write-intensive applications, this will result in rapid write latency and high write throughput.
    - However, if the caching layer dies, there is a chance of losing the data since the cache is the only single copy of the written data.
    - By having more than one replica that recognizes the writing in the cache, we can maximize this.

#### ```Advantages of Caching```
- **Improve Application Performance:**
  - Caching can be used to improve system performance and API latency. 
- **Reduce Database Cost:**
   - Caching can take up additional traffic to its cache server and reduce database traffic, eventually reducing database cost.
- **Reduce the Load on the Backend:**
  - Offloading the same request traffic from the main server to caching server would reduce the backend load.
- **Increase Read Throughput (IOPS):**
  - Caching server responds much faster than the main server for the cached key, which increases read throughput. 
- ![image](https://user-images.githubusercontent.com/57194114/202945974-33921a9d-6a1b-4dfa-8717-a4b0131f4002.png)

#### Links
- https://www.enjoyalgorithms.com/blog/caching-system-design-concept/
- https://www.youtube.com/watch?v=_F1U6Rh0wfo

---
---
## ```Load Balancing```
- A load balancer is a software or a hardware device that sits between clients and a set of servers and balances workload across resources. It saves our servers from overloading and increases the throughput of the system.
- **Problem behind load balancing**:
  - We use various web services in real life, which quickly respond to our requests. But most of us are unaware of the background process and scale of the system responsible for providing the fast response. This involves the allocation of requests across several servers when thousands of users request the service simultaneously.
  - On the other side, the load on servers keeps increasing with the traffic growth, and the website gets slower to serve the user request. To deliver a fast and reliable response, one idea would be to increase the number of servers. But this situation brings a new challenge: how to distribute the requests across several servers? We can solve this problem using the idea of a load balancer!
  - Suppose we have several clients sending requests to a single server. When the number of requests increases significantly, the server experiences an overload, leading to failure in the system.
  - ![image](https://user-images.githubusercontent.com/57194114/203491597-741b1297-53a6-4527-9104-2ea829532439.png)
  - There are two issues:
    - **Server overloading:** There is always a limitation on a server to handle requests. After growth in the number of requests, the server may get overloaded. 
    - **Single point of failure:** If the single server goes down, the whole application will be unavailable for the users for a certain period of time. It will create a bad user experience.
- **Solution**:
  - We can try to scale our system. The first way is to vertically scale our system or increase the power of our server. But, there's only so much that we can do about it to increase a single machine's power.
  - Another way is to scale the system horizontally by adding more servers to our system. Now for handling the request, we can add a load balancer and distribute the request across multiple servers. This could allow our services to handle a large number of requests by adding more servers.
  - Even if one of the servers goes offline due to some reason, the service will be available. It continuously checks the health of backend resources and prevents sending traffic to servers that cannot fulfill requests.
  - ![image](https://user-images.githubusercontent.com/57194114/203492955-43b89640-2eb9-4959-9464-9941f8ad77f0.png)
- We can add load balancers at various places in the system, especially with multiple resources like servers, databases, or caches.
  - Between the client and the server
  - Between the server and application servers
  - Between the application and cache servers
  - Between the cache and database servers

#### ```Types of Load Balancers```
- // This distinction is not clear at all(???!!!)
- There can be two types of load balancers(???): software load balancer and hardware load balancers: software load balancer and hardware load balancer.
- The main difference between them is that we can do more with a software load balancer(???). We have more power for customization and scaling with software load balancers. With hardware load balancers, we are limited to the hardware we are given.
- Pros and cons of software load balancers:
  - Flexible in adjusting to changing needs
  - Able to scale beyond initial capacity by adding more software instances
  - Lower cost than purchasing and maintaining physical hardware. The software can run on any standard device, which tends to be cheaper
  - It can allow cloud-based load balancing (???)
  - There can be some delay (???) when scaling beyond initial capacity while configuring load balancer software
  - There will be some extra costs for ongoing upgrades
- Examples of software load balancers(???):
  - HAProxy: A TCP load balancer
  - NGINX: An HTTP load balancer with SSL termination support.
  - mod_athena: Apache-based HTTP load balancer.
  - Varnish: A reverse proxy-based load balancer. 
  - Balance: Open-source TCP load balancer.
  - LVS: Linux virtual server offering layer 4 load balancing.
- Pros and cons of hardware load balancers:
  - Provide fast throughput due to software running on specialized processors.
  - Increase security because only the organization can access the servers physically.
  - Need more human resources and expertise to configure and manage the machines.
  - It fails to scale when the number of requests exceeds a specific limit.
  - It requires a higher cost for purchase and maintenance.
- Examples of hardware load balancers (??? don't know any of them):
  - F5 BIG-IP load balancer
  - CISCO system catalyst
  - Barracuda load balancer
  - Coytepoint load balancer
  - Citrix NetScaler

#### ```Advantages of load balancing```
- We use a load balancer for better user experience and uninterrupted service by distributing the client requests to an available and responsive server. In other words, it ensures the availability and scalability of the application.
- It prevents server overload and a single point of failure. In other words, it ensures that no single server bears too many requests that degrade the application's overall performance. 
- It can also offer functionalities like encryption, authentication, etc. to provide a single control point for securing, managing, and monitoring the application. It can provide efficient protection from the DoS attack.
- The end-user only needs to know the address of the load balancer, not the address of every server in the cluster. So it also provides a layer of Abstraction.
- We can roll out software updates without taking the whole service down by using the load balancer to take out one server at a time.
- It minimizes server response time and maximizes throughput.
- It can do health checks and monitor the request handling capability of servers.
- Based on the number of requests, it can add or remove the number of servers.

#### ```Load Balancing Algorithms```
- Load balancing algorithms are categorized into two parts:
  - **Static load balancing algorithms**: These algorithms works in the same way regardless of the state of the backend serving the requests. It is simpler and more efficient to implement but can lead to uneven distribution of requests. Some examples of static load balancing algorithms are:
    - Round Robin
    - Wheighted Round Robin
    - Source IP Hash
    - URL Hash
    - Randomized algorithm
  - **Dynamic load balancing algorithms**: These algorithms takes into account the state of the backend and considers server load when distributing requests. It requires communication between the load balancers and servers. So it would be a little complex but can distribute requests efficiently. Some examples of dynamic load balancing algorithms are:
     - Least connection method
     - Weighted least connections method
     - Least response time method
- **Round Robin Load Balancing Algorithm**: Round robin is one of the simplest load balancing algorithms, ensuring client requests to a different server based on a rotating list. Here load balancer maintains a list of available servers and directs the incoming request in a round-robin fashion: 1st request to the 1st server, 2nd request to the 2nd server, so on. When the load balancer reaches the end of the list, it goes to the list's beginning and starts from the first server again.
  - Easy to implement.
  - Evenly balances the traffic between the servers.
  - Doesn't consider the server's load and specifications. So there is a risk that a server with low capacity receives many requests and becomes overloaded.
  - Works best if every server in the load balancer list has roughly the exact specification. Otherwise, a low processing server may have the same load as a high processing server.
- **Weighted Round Robin Balancing Algorithm**: The weighted round-robin load-balancing algorithm is an advanced version of the simple round-robin algorithm. It distributes the incoming request based on the weighted score of the servers. Here weight can be an integer that can vary according to the server's processing power or specification. So, It considers server specifications and distributes the traffic based on that.
  - Little complex compared to a simple round-robin algorithm but works well with servers with different specifications.
  - The current load of each server and the relative computation cost of each request are not considered.
  - Based on the weighted score, some of the servers may get many requests of the overall request count.
- **Random Load Balancing Algorithm:** This algorithm randomly maps requests to the server using some random number generator. Whenever a load balancer receives requests, a randomized algorithm distributes the requests evenly to the servers. So like Round Robin, this algorithm also works well for the group of servers with similar configurations.
- **Source IP Hash Load Balancing Algorithm (???):*** Here server is selected based on a unique hash key. It combines the source and destination IP address to generate a unique hash key and allocate the request to a particular server. The key can be regenerated if the session is broken and the client request is directed to the same server it was using previously. In other words, this is useful when a dropped connection needs to be returned to the same server initially handling it.
- **URL Hash Load Balancing Algorithm (???):** Load balancer generates the hash value based on the URL present in requests coming from the clients. Based on hash value, requests will be forwarded to servers. The load balancer caches the hashed value of the URL, and subsequent requests that use the same URL make a cache hit and are forwarded to the same server.
  - It improves the capacity of backend caches by avoiding cache duplication.
  - This method is used when load-balanced servers serve mostly unique content per server. So, basically, what this means is that all requests related to one process will go to one server, say “running code,” and all requests related to another process, say “payments,” will go to another server, and so on…
- **Least Connection Method:** This algorithm considers the current load on a server and delivers better performance. Here load balancer sends requests to the server with the least number of active connections.
  - The load balancer does additional calculations to figure out the server with the least number of connections.
  - This algorithm is useful when there are many persistent connections in the traffic unevenly distributed between the servers. If the servers are busy in long computations, the connections between client and server stay alive for a long period of time.
- **Weighted Least Connections Method:** In weighted least connections, the load distribution is based on both the factors – the number of current and active connections to each server and the relative capacity of the server.
  - Some servers can handle more connections than others.
  - Servers are rated based on their processing capabilities.
- **Least Response Time Method:** This algorithm is a little advanced form of the least connection method, where a request is forwarded to the server with the fewest active connections and the least average response time.
  - It relies on the time taken by a server to respond to a health monitoring request.
  - The speed of the response is an indicator of how loaded the server is.
  - Also, consider the number of active connections on each server.
  - The backend server that responds the fastest receives the subsequent request.

#### Links
- https://www.enjoyalgorithms.com/blog/load-balancers-in-system-design/
- https://www.enjoyalgorithms.com/blog/types-of-load-balancing-algorithms/
- https://docs.aws.amazon.com/elasticloadbalancing/latest/application/introduction.html
---
---

#### Links
- https://www.educative.io/blog/what-is-cap-theorem
- 
