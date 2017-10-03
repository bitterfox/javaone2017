# Day3
## Collections Refueled [CON5965]
- New APIs
  - List.of with varargs
    - `Collections.unmodifiableList(Arrays.asList(""))` -> `List.of("")`
  - Set.of with varargs ver
    - `Collections.unmodifiableSet(new HashSet<>(Arrays.asList("")))` -> `Set.of("")`
  - Map.of
    - Map.ofEntries is a varargs ver
      - more than 10 entries
    - Map.entry
  - properties
    - unmodifiable
      - UnsupportedOE on add, set , or remove
      - Difference between unmodifiable collections and unmodifiable wrappers
        - `Collections.unmodifiableList(inner)` is an unmodifiable view of inner
        - `inner = Arrays.asList()` can be modified, and modifications to it are visible to list1
        - `List.of` cannot be modified at all
          - all of them throws UnsupportedOE by invocation of add. remove, set, etc...
    - null dissallowd
      - throws NPE at create time
      - no collection in Java 5 or later (j.u.concurrent) has permitted nulls
      - classic collection like ArrayList still allow nulls
    - randomized iteration order(Sets and Maps)
      - order for HashSet, HashMap is not officially unspecified
      - it will be changed for long periods of time
      - Lot of code breaks when iteration order is changed
      - Depending code to latent iteration order will cause of bugs!
        - just change this HashMap to a LinkedHashMap
      - We want to break your code early like at test-phase to find bug early
        - iteration order is same in a JVM instance but not in other instance
          - randomized when every JVM waked
        - Just only new APIs
          - No existing code have effect
    - duplicate disallowed(Sets and Maps)
      - Throws IllegalArgumentError because it's mostly like programming error
    - space efficient
      - less space overall
      - fewer objects result in improved locality of reference
      - field based implementation for 0, 1, 2 elements
      - array-based with closed hashing for > 2 elements
      - can be changed compatibly even in minor release
        - easy to change because static factory hide implementation
      - ```
        new HashSet<>(3)
        set.add("foo")
        set.add("bar")
        set = Collections.unmodifiableSet(set)
        ```
        - 1 modifiable wapper, 1 hashset, 1 hashmap, 1 object[] table of size 3
        - Totally 152 bytes to store just two object references!
      - `Set.of("foo", "bar")`
        - just 20 bytes!!
        - lower fixed cost
        - lower variable cost
    - serialization
  - Future
    - copyOf
      - for making shallow copyOf
      - short circuit copying if not necessary
    - Stream collectors
      - Collectors.toUnmodifiableList, etc


## Enabling Java for Persistent Memory Hardware [CON7656]
- Intel Persistent Memory
  - New type of memory
    - Persistent
    - 6TB per socket
- Java enabling for vlolatile use of persistent Memory
  - heterogeneous memory arch
  - lower cost and higher capacity
  - Two usage
    - whole heap on pm: coarse granularity
      - allocation of java heap
        - heap memory on persistent memory, non heap memory on DRAM
      - for big data application and in-memory db
      - JDK8171181
      - -XX:HeapDir=/XPointFS/heap
    - Heterogeneous heap: Finer granularity
      - support within java vm for a heterogeneous heap
      - frequently accessed objects reside in DRAM
      - user can directed allocation, or automatic object movement based on access profile
      - Two kinds of HeapAreas
        - Normal or DRAM HeapAreas
        - COLD HeapArea
      - -Xmp500g
      - ```
      Heap.setAllocationTarget(PM);
      ...
      HEAP.resetAllocationTarget(); // reset to DRAM
      ```
      - Deap cloning using Kryo
  - Case study
    - Apark
      - To avoid need to use disk
      - prototype with COLD HeapAreas
        - DRAM acts as L1 cache
        - overflow partition saved in PM
- Persistent Collections for Java Project
  - Library of persistent classes
    - state stored on persistent heap
    - easy to understand data consistency model
    - Ex
      - PersistentByteArray, PersistentArrayList, etc...
      - Primitive types, BoxedPrimitives, PersistentString
  - API for defining persistent classes
  - Low-level accessor API(MemoryRegion)
    - bytecode level API
  - https://github.com/pmem/pcj
  - usage
    - In databases or data grids
      - replace Cassandra storage layer
      - faster and simpler than current solution

    - Replace simple database use with persistent Collections
    - Long-lived caches
  - High level API
    - `ObjectDirectory.put("Application_data", data)`
    - `ObjectDirectory.get("Application_data", PersistentIntArray.class)`
    - Transaction supports
      ```
      Transaction.run(() -> ...)
      ```
  - Define their own class for persistent memory
    - extends `PersistentObject`
      - `setObjectField(NAME, name)`
    - `ObjectType<E> = ObjectType.withFields(E.class, ID, NAME)`
    - `LongField`, `StringField`
    - Interface-based class definition
      - http://cr.openjdk.java.net/~jrose/panama/using-interfaces.html
    - Low level API
      - MemoryRegion
        - Interface from OpenJDK Panama Project
        - get and set for byte, short, int long on persistent MemoryRegion
        - Add Heap API to allocate and free memory region
        - `RawMemoryRegion` -> useful for volatile use or when caller provides data consistency
        - `Flushable MemoryRegion` -> includes flush and fail-safe is flushed state
        - `TransactionalMemoryRegion` -> write are transactional
- QA
  - Can we emulate the behaivior without persistent memory
    - Yes. It will write to RAM disks?


## Adventures in Big Data Scaling Bottleneck Hunt [CON7682]
- DataCenter workloads trends
  - Data explosion continues
  - Focus on putting data to work
  - Distributed.scale out becoming the norm
  - Emergin focus on machine/deep learning
- Hardware to meet work load demands
  - Increace CPU Core
  - SSD & NVMe economics drive IO requirements higher
  - Larger memory capacities for big data sets and in- memory DB
  - Networking standards evolving faster 10G -> 25G -> 100G with RDMA
  - Accelerator ASICs and FPGAs to otimize computationally expensive work
- Dilemma
  - Stay compatible vs. access new feature or performance
  - incompatibilities like using Unsafe to reduce GC

- Case study
  - Checksum
    - CRC32 implementation kicks JNI
    - Frequently invoked method in Hadoop(~10% of total runtime)
    - Hadoop community implemented a pure java version, PureJavaCRC32, 10x faster
    - Built in Java Checksum
      - Intel improved CRC32 in Java, 10x faster than PureJavaCRC32 in JDK8
      - In JDK9 Intel introduces a CRC32c which is faster than the previous CRC32
  - Lexicographic Array Compare
    - Commonly used in key lookup or serch application like HBase
    - HBase spend 20% of runtime to lexicographic Compare
    - They have own unsafe version, comparing 8bytes as a single operation with 8x faster
    - In JDK9 introduce a standard Lexicographi array comparator (vectorizedMismatch)
    - Intel implements 64bytes as a single operation, 12x vs JDK, 2x over Hadoop one
    - JDK-8033148
- Apache Cassandra data insert performance
  - Run it and mesure
  - JFR Profile
    - SlabAllocator.java
      - bump-the-poiter
      - combat heap fragmentation
      - It hack to avoid GC
      - But many of thread need to wait by lock
    - Fix it
      - Use G1GC
      - Make it ThreadLocal
  - Finding root cause is difficult
- Genomics Scalability
  - Disabling 2nd Super cache improved Cassandra writes by 60%
  - Unreported JVM issue that affects scalability of many existing application
- Puzzling IO Behavior
  - kernel frequently flushing the file system cache
    -
  - Introducing Direct IO
    - Self caching application have better semantics of the data than the OS
    - Modern SQL Database often only use direct IO, but not Java
    - Provides more consistent throughput and latencies
    - Avoid double caching of data
  - In JDK9+ to contain direct IO API
  - Cassandra and HBase are improved 2x throughput and responce time improvement


## JDK 9 Language, Tooling, and Library Features [CON2702]

## Migrating to Modules [CON6122]

## OpenJ9: Under the Hood of the Next Open Source JVM [CON3573]

## Developer Keynote [KEY7383]

## JUnit 5 [BOF7382]

## Modules Are Coming...to Libraries?? [BOF3707]
