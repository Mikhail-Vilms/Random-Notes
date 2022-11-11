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
---
![image](https://user-images.githubusercontent.com/57194114/201296359-1ac5726b-6de3-4275-a061-0a009b97e0b7.png)
---
