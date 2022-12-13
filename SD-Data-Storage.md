# System Design: Data Storage

## Key-value Storage: Riak
- Let’s say our data storage consists only of appending to a file
- Then the simplest possible indexing strategy is this: keep an **in-memory hash map** where every key is mapped to a byte offset in the **data file**—the location at which the value can be found, as illustrated in Figure 3-1
  - ![image](https://user-images.githubusercontent.com/57194114/207240278-635a6ce8-f83e-4f0f-a502-fde4a84c3b0a.png)
