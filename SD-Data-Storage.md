# System Design: Data Storage

## Key-value Storage: Riak
- Let’s say our data storage consists only of appending to a file
- Then the simplest possible indexing strategy is this: keep an **in-memory hash map** where every key is mapped to a byte offset in the **data file**—the location at which the value can be found, as illustrated in Figure 3-1
  - ![image](https://user-images.githubusercontent.com/57194114/207240278-635a6ce8-f83e-4f0f-a502-fde4a84c3b0a.png)
- Whenever you append a new key-value pair to the file, you **also update the hash map** to reflect the offset of the data you just wrote (this works both for inserting new keys and for updating existing keys).
- When you want to look up a value, use the hash map to find the offset in the data file, seek to that location, and read the value.
- This may sound simplistic, but it is a viable approach. In fact, this is essentially whatBitcask (the default storage engine in Riak) does.
