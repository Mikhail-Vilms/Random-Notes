# System Design Problems

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
