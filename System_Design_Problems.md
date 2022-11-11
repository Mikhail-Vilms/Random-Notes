# System Design Problems

## Distributed Cache
#### Stepping Into Distributed World
- We can start with a really straightforward idea: we move LRU cache that we just implemented to it's own host
- One of the benefits of this approach: we can make each host to store only chunk of data - called shard. Because data is split across several hosts, we can store much more data in memory.
- Service hosts know about all shards and they forward PUT and GET requests to a particular shard.
- The same idea, but slightly different realization is to use service hosts for tha cache:
  - We run cache as a separate process on a service host 
  - and data is also split into shards
