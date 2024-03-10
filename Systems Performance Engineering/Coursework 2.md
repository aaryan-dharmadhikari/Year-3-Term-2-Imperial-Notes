## 1 Synchronisation Primitives
### 1.1 Question: Execution order (1) [5 marks]
Possible orderings are:
```cpp
1122
2211
2121
1212
1221
2112
```
### 1.2 Question: Execution order (2) [5 marks]
Possible orderings are:
```cpp
1122
2211
```
### 1.3 Question: User-level Implementation [20 marks]
#### Constructor
```cpp
mysem(uint32_t init_value): counter(init_value) {};
```
#### Acquire
```cpp
void acquire() {
  while (true) {
    auto current = counter.load();
    if (counter != 0 && counter.compare_exchange_weak(current, current - 1)) {
      return;
    }
  }
};
```
#### Release
```cpp
void release() { counter.fetch_add(1); };
```
### 1.4 Question: Hybrid Implementation [15 marks]
#### Constructor
```cpp
mysem(uint32_t init_value): counter(init_value) {};
```
#### Acquire
```cpp
void acquire() {
  while (true) {
    for (auto i = 0; i < 100; i++) {
      auto current = counter.load();
      if (counter != 0 && counter.compare_exchange_weak(current, current - 1)) {
          return;
      }
    }
    uint32_t* counter_ptr = reinterpret_cast<uint32_t*>(&counter);
    syscall(SYS_futex, counter_ptr, FUTEX_WAIT, 0, nullptr, nullptr, 0);
  }
};
```
#### Release
```cpp
void release() {
  counter.fetch_add(1);
  uint32_t *counter_ptr = reinterpret_cast<uint32_t *>(&counter);
  syscall(SYS_futex, counter_ptr, FUTEX_WAKE, 1, nullptr, nullptr, 0);
};
```
### 1.5 Question: A Better Hybrid Implementation [5 marks]
Add an atomic integer `std::atomic<uint32_t> sleeping`, which tracks the number of threads currently asleep in the futex. This can be incremented in `acquire` before waiting on the mutex and decremented after waking up. In the case `sleeping` is 0, we can skip calling `FUTEX_WAIT`.
## 2 Networked Service Design
### 2.1 Network Modelling [15 marks]
Let us first look at requests:
- 90:10 GET vs SET
- Thus $0.9 * (512 + 1) + 0.1 * (1 + 512 + 1024) = 615.4\:Bytes$ is the average size of a request
- Given $10\:Gbps = 10 * 1024^{3} = 10,737,418,240\: bps$ throughput, this means request throughput is $10,737,418,240 / 615.4 = 17,447,868.4432889178 \approx 17,447,868\:requests/sec\:(taken\; floor)$ 
Now responses:
- Similarly, $0.9 * 1024 + 0.1 * 1 = 921.7\:Bytes$ is the average size of a response
- Thus throughput is $10,737,418,240 / 921.7 = 11,649,580.3840729088 \approx 11,649,580\:responses/sec\:(taken\; floor)$
Since we are limited by the larger average size, responses are going to limit throughput, thus the maximum throughput is $11,649,580\:requests\; and\; responses/sec$
### 2.2 CPU Modeling [20 marks]
A single core can process:
- $\frac{10^{6}}{200} = 5000\: GETs/sec$ 
- $\frac{10^{6}}{500} = 2000\: SETs/sec$ 
- Given the previous 90:10 ratio, that means average no requests processed is given by $5000 * 0.9 + 2000 * 0.1 = 4,700\:requests/sec$ 
Therefore, to get no of cores required, we take $\lceil \frac{130,000}{4,700} \rceil = \lceil 27.6595744681 \rceil = 28\: cores$
### 2.3 Cutting Corners [15 marks]
#### 1000 req/sec, 100% GETs → Average Latency: 200 Μsec
For this particular experiment, results are unlikely to vary with a lock-free implementation. 1000 requests is far less than the 10,000 buckets in our system and thus we should expect basically no contention for buckets; we should therefore expect the same average latency.
#### 100000 req/sec, 100% GETs → Average Latency: 207 Μsec
We should expect slightly lower average latency in a lock-free implementation. As the number of requests is now much higher, we can expect multiple requests per bucket and as such the overhead from acquiring a read lock will be slightly higher than in a lock-free implementation.
#### 100000 req/sec, 50% GETs, 50% SETs → Average Latency: 475 Μsec
We would expect significantly lower latency in a lock-free implementation: due to SET requests requiring exclusive access to a bucket, there will be lots of contention and waiting for a given bucket- a problem which would not exist in a lock-free implementation.