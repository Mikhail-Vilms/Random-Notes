# System Design Problems

### List of problems
- **TinyURL**
- **Instagram**
- **Twitter**
- **[Distributed Cache](#Distributed-Cache)**

## Distributed Cache
---
#### Stepping Into The Distributed World
- We can start with a really straightforward idea: we move LRU cache that we just implemented to it's own host
  - One of the benefits of this approach: we can make each host to store only chunk of data - called shard. 
  - Because data is split across several hosts, we can store much more data in memory.
  - Service hosts know about all shards and they forward PUT and GET requests to a particular shard.
- The same idea, but slightly different realization is to use service hosts for tha cache:
  - We run cache as a separate process on a service host 
  - And data is also split into shards
  - And, similar to the first option, when service needs to make a call to the cache, it picks the shard that stores data and makes a call.
- Let's call these options as *"dedicated cache cluster"* and *"co-located cache"*
- Benefits of each option:
  - Dedicated cluster hepls to isolate cache resources from the service resources
  - Both the cache and the service do not share memory and CPU anymore and can scale on their own
  - Dedicated cluster can be used by multiple services and we can utilize the same cluster across several microservices our team owns
  - And dedicated cluster also gives us flexibility in chosing hardware: we can shoose hardware host with a lot of memory and high network bandwith. Public clouds nowdays provide variety of memory-optimized hardware
  - As for co-located cache, the biggest benefit is that we do not need a separate cluster: this helps to save on hardware cost and usually less operationally intensive than a separate cluster
  - And with co-location, both the service and cache scale out at the same time: we just add more hosts to the service cluster when needed
- ![image](https://user-images.githubusercontent.com/57194114/201296359-1ac5726b-6de3-4275-a061-0a009b97e0b7.png)
---
#### Choosing a Cache Host (Naive Approach)
- We have implemented a LRU cache and made it runnable as a separate process, we told cache clients to call the cache process using either TCP or UDP connection.
  - But how do cache clients decide which cache shard to call?
- Let's discuss a naiva approach: a MOD function
  - Based on the item key and some hash function we compute a hash
  - We divide this hash number by a number of available cache hosts and take a reminder
  - We treat this remainder as an index in the array of the cache hosts
  - For example, we have 3 cache hosts and hash is equal to8; 8 mod 3 is 2, so the cache host with index 3 will be selected by the service to store this item in the cache and while retrieving the item from the cache.
  - *Problem*: But what happens when we add a new cache host (or some host dies due to hardware failures)? The MOD function will start to produce completely different results
  - Service hosts will start choosing completely different cache hosts than they did previuosly - resulting in a high percentage of cache misses.
  - This is rarely acceptable in production systems and usually used for testing purposes only
- ![image](https://user-images.githubusercontent.com/57194114/201452558-39c20ae4-e816-4d4a-895f-fb07f2fe29eb.png)
---
#### Choosing a Cache Host (Consistent Hashing)
- A much better option is to use "consistent hashing"
  - Consistent hashing is based on mapping each object to a point on a circle
  - We pick an arbitrary point on this circle and assign as 0 number to it
  - We move clockwise along the circle and assign values
  - We then take a list of cache hosts and calculate a hash for each host based on a host identifier - for example IP address or name
  - The hash value tells us where on the consistent hashing circle that host lives
  - And the reason we do all that, is that we want to assign a list of hash ranges each cache host owns
  - Specifically, each host will own the cache items that live between this host and the nearest clockwise neighbor
- ![image](https://user-images.githubusercontent.com/57194114/201453015-905072a4-425b-4ba9-9f96-b0746772b174.png)
---
- So, for a particular item, we need to look uo what cache host stores it, we calculate a hash and move backwards to identify the host.
  - In this case, host 4 is storing the item
- ![image](https://user-images.githubusercontent.com/57194114/201453326-146c4285-5bad-4209-964c-64d5c2afc43b.png)
---
- And what happens when we add new host to the cache cluster?
  - Same as before: we calculate a hash for a new host and this new host becomes responsible for its range of keys on the circle.
  - While its counter clockwise neighbor (host 4 in this case) becomes responsible for a smaller range
  - In other words, host 6 took responsibility for a subset of what was formerly owned by host 4
  - **And nothing has changed for all the other hosts** - which is exactly what we wanted: to minimize a number of keys we need to re-hash
- **Consistent hashing is much better than MOD hashing, as significantly smaller fraction of keys is re-hashed when new host is added or host is removed from the cache cluster**
- ![image](https://user-images.githubusercontent.com/57194114/201453656-554d2c0c-488e-44e0-9938-5e50588eff01.png)
---
#### Cache Client
- *Question*: Now we know how cache cluster host is selected for both put and get, but who is actually doing this selection? On the service side, who is responsible for running all these hash calculations and routing requests to the selected cache host?
  - Cache client is that component. It’s a small and lightweight library, that is integrated with the service code and is responsible for the cache host selection.
  - Cache client knows about all cache servers.
  - And all clients should have the same list. Otherwise, different clients will have their own view of the consistent hashing circle and the same key may be routed to different cache hosts.
  - Client stores list of cache hosts in sorted order (for a fast host lookup) and binary search can be used to find a cache server that owns the key.
  - Cache client talks to cache hosts using TCP or UDP protocol.
  - And if cache host is unavailable, client proceeds as though it was a cache miss.
- ![image](https://user-images.githubusercontent.com/57194114/201478007-dbf98373-0f27-43fb-8ed3-38a535d219a5.png)
---
#### Maintaining a List of Cache Servers
- *Question*: As you may see, list of cache hosts is the most important knowledge for clients. And what we need to understand, is how this list is created, maintained and shared among all the clients.
- Let’s discuss several options:
- Option 1:
  - In the first option we store a list of cache hosts in a file and deploy this file to service
hosts using some continuous deployment pipeline.
  - We can use configuration management tools such as chef and puppet to deploy file to
every service host. 
  - This is the simplest option. But not very flexible.
  - *Problem*: Every time list changes we need to make a code change and deploy it out to every service
host.
- Option 2:
  - What if we keep the file, but simplify the deployment process?
  - Specifically, we may put the file to the shared storage and make service hosts poll for the
file periodically.
  - All service hosts try to retrieve the file from some common location, for example S3
storage service.
  - To implement this option, we may introduce a daemon process that runs on each service
host and polls data from the storage once a minute or several minutes.
  - *Problem*: The drawback of this approach is that we still need to maintain the file manually. 
    - We have to make changes and deploy it to the shared storage every time cache host dies or new host is
added. 
    - It would be great if we could somehow monitor cache server health and if something bad happens to the cache server, all service hosts are notified and stop sending any requests to the unavailable cache server.
    - And if a new cache server is added, all service hosts are also notified and start sending
requests to it.
- Option 3:
  - To implement this approach, we will need a new service, configuration service, whose purpose is to discover cache hosts and monitor their health.
  - Each cache server registers itself with the configuration service and sends heartbeats
to the configuration service periodically.
  - As long as heartbeats come, server is keep registered in the system. If heartbeats stop coming, the configuration service unregisters a cache server that is no longer alive or inaccessible.
  - And every cache client grabs the list of registered cache servers from the configuration service.
- The third option is the hardest from implementation standpoint and its operational cost is higher. But it helps to fully automate the list maintenance. 
- ![image](https://user-images.githubusercontent.com/57194114/201479082-c4581126-0a02-4bde-be06-cef7f3085835.png)
---
#### Summary
- Let’s quickly summarize what we have discussed so far:
  - To store more data in memory we partition data into shards. And put each shard on its own server.
  - Every cache client knows about all cache shards. And cache clients use consistent hashing algorithm to pick a shard for storing and retrieving a particular cache key.
- Let’s recall non-functional requirements we defined in the beginning of our design:
  - We wanted to build fast, highly scalable and available distributed cache.
  - *Have we built a highly performant cache?* 
    - Yes. Least recently used cache implementation uses constant time operations.
    - Cache client picks cache server in log n time, very fast.
    - And connection between cache client and cache server is done over TCP or UDP, also fast.
    - So, performance is there.
  - *But what about other two: scalability and availability?*
    - Scalability is also there. We can easily create more shards and have more data stored in memory.
    - Although those of you who did data sharding in real systems know that **common problem for shards is that some of them may become hot: meaning that some shards process much more requests then their peers, resulting in a bottleneck**.

---
