# Cache

Ta đã biết đến Redis là 1 dạng cache. Nay chúng ta tìm hiểu thêm về guava cache. Guava được phát triển bởi google nên chắc chắn cũng rất đảm bảo về mặt an toàn và hiệu nặng
Vậy Guava cache có gì?


# Guava Cache save in L1 or L2? and why

Guava Cache is an in-memory cache that can be used to store frequently accessed data. It is designed to be used as a local cache and is not intended to be used as a distributed cache. Guava Cache is stored in the L1 cache by default. The L1 cache is a local cache that is stored in the same process as the application. The L2 cache is a distributed cache that is stored on a remote server. The L2 cache is used when the data needs to be shared across multiple processes or servers1.

In summary, Guava Cache is stored in L1 by default because it is designed to be used as a local cache and not intended to be used as a distributed cache.


The maximum size of L1 cache is determined by the amount of memory available to the application. Guava caches are local to a single run of your application and do not store data in files or on outside servers1. You can limit the size of your cache using maximumSize(). If the cache reaches the limit, it evicts the oldest items2.

In summary, the maximum size of L1 cache is determined by the amount of memory available to the application and can be limited using maximumSize().

https://github.com/google/guava/wiki/CachesExplained


https://www.baeldung.com/spring-cache-tutorial



# how to determine maximum size of L1 cache for app with 16Gb RAM

The maximum size of L1 cache is determined by the amount of memory available to the application. In your case, since you have 16GB of RAM, you can allocate a maximum of 16GB of memory to your application1. However, it is not recommended to allocate all available memory to your application as it can cause other applications to slow down or crash2.

In summary, the maximum size of L1 cache for an app with 16GB RAM is 16GB but it is not recommended to allocate all available memory to your application.


You can set the maximum size of the cache in bytes using the CacheBuilder.maximumWeight(long) method. This method sets the maximum weight of the cache. The weight of an entry is determined by the Weigher function that you provide when you create the cache.

Here’s an example:
```java
Cache<String, String> cache = CacheBuilder.newBuilder()
    .maximumWeight(1000000)
    .weigher((key, value) -> value.length())
    .build();
```
In this example, we create a cache that supports up to 1000000 bytes using the CacheBuilder class. We also set the weight of each entry to be its length using the weigher method.

# how to solve read an write in a same time in piece of memory with guava cache

Guava Cache is thread-safe and supports concurrent reads and writes. You can use the CacheBuilder class to create a cache that supports concurrent reads and writes. Here’s an example:

``` java
import com.google.common.cache.Cache;
import com.google.common.cache.CacheBuilder;

public class Example {
  public static void main(String[] args) {
    Cache<String, String> cache = CacheBuilder.newBuilder()
        .concurrencyLevel(4)
        .maximumSize(10000)
        .build();

    // Write to cache
    cache.put("key", "value");

    // Read from cache
    String value = cache.getIfPresent("key");
  }
}

```

In summary, you can solve read and write in a same time in piece of memory with Guava Cache by using the CacheBuilder class to create a cache that supports concurrent reads and writes.

The concurrencyLevel method in the CacheBuilder class is used to guide the allowed concurrency among update operations. It is used as a hint for internal sizing and the table is internally partitioned to try to permit the indicated number of concurrent updates without contention.

In the example I provided earlier, we set the concurrency level to 4 using the concurrencyLevel method. `This means that the cache is optimized for up to 4 threads performing updates concurrently.`

I hope this helps!


# how to invalid key in guava cache

You can invalidate a key in Guava Cache using the Cache.invalidate(key) method1. This method removes the entry for the specified key if it exists in the cache. Here’s an example:
```java
Cache<String, String> cache = CacheBuilder.newBuilder()
    .maximumSize(1000)
    .build();

cache.put("key1", "value1");
cache.put("key2", "value2");

// Invalidate key1
cache.invalidate("key1");
```
In this example, we create a cache that supports up to 1000 entries using the CacheBuilder class. We then add two entries to the cache using the put method and invalidate one of them using the invalidate method.

# how to restrict number of threads for read cacheBuilder

You can restrict the number of threads for read operations in Guava Cache using the CacheBuilder.maximumSize(long) method. This method sets the maximum number of entries that the cache may contain. When the cache reaches its maximum size, the least recently used entry is evicted.

Here’s an example:

```java
Cache<String, String> cache = CacheBuilder.newBuilder()
    .maximumSize(1000)
    .concurrencyLevel(4)
    .build();
```
In this example, we create a cache that supports up to 1000 entries using the CacheBuilder class. We also set the concurrency level to 4 using the concurrencyLevel method.





