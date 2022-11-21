# System Design: Basic Concepts

### List of Basic Concepts
- **[Process Management in Operating System](#Process-Management-in-Operating-System)**
  - **[Process Memory](#Process-Memory)**
  - **[Process Control Block](#Process-Control-Block)**
  - **[States of a Process in Operating System](#States-of-a-Process-in-Operating-System)**
- **[Client Server Architecture](#Client-Server-Architecture)**
- **[Peer-to-Peer Networks]** - TODO
- **[Latency](#Latency)**
- **[Horizontal VS Vertical Scaling](#Horizontal-VS-Vertical-Scaling)**
- **[Distributed System](#Distributed-System)**
- Consistent Hashing
- **[CAP Theorem](#CAP-Theorem)**
- **[HTTP Long Polling](#HTTP-Long-Polling)**
- **[Domain Name Service](#domain-name-service)**
- **[Caching](#Caching)**
  - **[Types of Caching](#Types-of-Caching)**
  - **[Cache Eviction Policy](#Cache-Eviction-Policy)**
  - **[Cache Invalidation](#Cache-Invalidation)**
---
## Process Management in Operating System

#### Process
- In simple words, a program in execution is called a process
- It is an instance of a program that actually runs, i.e., an entity that can be assigned and executed on a processor
- Two essential process elements are **program code** and a **set of data** associated with that code.

#### Process Memory
- Process memory is divided into four sections:
  - The **program memory** stores the compiled program code, read in form of non-volitile storage when the program is launched
  - The **data section** stores global and static variables, allocated and initialized before executing the main program.
  - The **heap section** is used to manage dynamic memory allocation inside our program. In other words, it is the portion of memory where dynamically allocated memory resides i.e., memory allocation via new or malloc and memory deallocation via delete or free, etc.
  - The **stack section** stores local variables defined inside our program or function. Stack space is created for local variables when declared, and the space is freed up when they go out of scope.
- ![image](https://user-images.githubusercontent.com/57194114/202915150-ee07a87b-5b6b-4e58-8ee6-a5349925bd0a.png)
- Note: The stack and heap sections start at opposite ends of the process's free space and grow towards each other. When they meet, a stack overflow error will occur, or a call to new memory allocation will fail due to insufficient memory available in a heap section.

#### Process Control Block
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

#### Horizontal VS Vertical Scaling

#### Distributed System
- A distributed system is a collection of computers that work together to form a single computer for end users. 
- All of the distributed machines have one shared state and operate concurrently. 
- With distributed systems, users must be able to communicate with any of the distributed machines without knowing it’s only one machine.
- The distributed system network stores its data on more than just a single node, using multiple physical or virtual machines at the same time.

#### CAP Theorem



### Domain Name Service

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
  -  Write through cache:
---

#### Links
- https://www.educative.io/blog/what-is-cap-theorem
- 
