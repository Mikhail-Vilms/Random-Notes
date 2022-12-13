# System Design: Data Storage

## Key-value Storage: Riak
- Let’s say our data storage consists only of appending to a file
- Then the simplest possible indexing strategy is this: keep an **in-memory hash map** where every key is mapped to a byte offset in the **data file**—the location at which the value can be found, as illustrated in Figure 3-1
  - ![image](https://user-images.githubusercontent.com/57194114/207240278-635a6ce8-f83e-4f0f-a502-fde4a84c3b0a.png)
- Whenever you append a new key-value pair to the file, you **also update the hash map** to reflect the offset of the data you just wrote (this works both for inserting new keys and for updating existing keys).
- When you want to look up a value, use the hash map to find the offset in the data file, seek to that location, and read the value.
- This may sound simplistic, but it is a viable approach. In fact, this is essentially whatBitcask (the default storage engine in Riak) does.
---
---
## Compaction
- **Problem:** As described so far, we only ever append to a file—so how do we avoid eventually running out of disk space?
- A good solution is to break the log into segments of a certain size by closing a segment file when it reaches a certain size, and making subsequent writes to a new segment file.
- We can then perform compaction on these segments.
  - Compaction means throwing away duplicate keys in the log, and keeping only the most recent update for each key.
  - ![image](https://user-images.githubusercontent.com/57194114/207241752-7bc169c3-4b48-4d2d-922e-9ffc9c1bee66.png)
  - Since compaction often makes segments much smaller (assuming that a key is overwritten several times on average within one segment), we can also merge several segments together at the same time as performing the compaction
  - ![image](https://user-images.githubusercontent.com/57194114/207242489-ed5ac8b3-c3c6-4f77-8713-1206e94cc7a5.png)
  - Segments are never modified after they have been written, so the merged segment is written to a new file.
    - The merging and compaction of frozen segments can be done in a background thread, and while it is going on, we can still continue to serve read and write requests as normal, using the old segment files
  - After the merging process is complete, we switch read requests to using the new merged segment instead of the old segments—and then the old segment files can simply be deleted. 
