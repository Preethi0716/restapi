**Cache-Library**

Cache-Library is a high-performance caching library developed in Go, designed to support multiple backend caches including in-memory with LRU eviction, Redis, and Memcached. 
This library is built to provide efficient caching solutions with intuitive APIs for cache operations.

**Features**

**In-Memory Cache:** Implements an LRU (Least Recently Used) cache with support for TTL (Time-to-Live) and eviction policies.

**Redis Cache:** Integration with Redis for distributed caching.

**Memcached Cache:** Integration with Memcached for distributed caching.

**Concurrent Operations:** Supports concurrent cache operations with thread safety.

**TTL Support:** Expiry of cached entries based on TTL.

**Installation**

To get started with the Cache-Library, clone the repository and use Go modules to install the dependencies.

```
git clone https://github.com/Preethi0716/Cache-Library.git

cd Cache-Library

go mod tidy
```

**Usage**

**In-Memory Cache**

This provides the lru package which implements a fixed-size thread safe LRU cache. It is based on the cache in Groupcache.

```
package main

import (
	"fmt"
	"time"
	"github.com/Preethi0716/Cache-Library/preethi/restapi/pkg/cache"
)

func main() {
	// Create a new LRU cache with a capacity of 2
	lruCache := cache.NewLRUCache(2)

	// Set a value in the cache
	lruCache.Set("key1", "value1", time.Minute)

	// Get the value from the cache
	value, err := lruCache.Get("key1")
	if err != nil {
		fmt.Println("Error:", err)
	} else {
		fmt.Println("Value:", value)
	}
}
```

**Redis Cache**

go-redis/cache library implements a cache using Redis as a key/value storage. It uses MessagePack to marshal values.

```package main

import (
	"fmt"
	"time"
	"github.com/Preethi0716/Cache-Library/preethi/restapi/pkg/cache"
)

func main() {
	// Create a new Redis cache
	redisCache, err := cache.NewRedisCache("localhost:6379")
	if err != nil {
		fmt.Println("Error:", err)
		return
	}

	// Set a value in the cache
	err = redisCache.Set("key1", "value1", time.Minute)
	if err != nil {
		fmt.Println("Error:", err)
		return
	}

	// Get the value from the cache
	value, err := redisCache.Get("key1")
	if err != nil {
		fmt.Println("Error:", err)
	} else {
		fmt.Println("Value:", value)
	}
}
```

**Memcached cache**

Memcached can serve cached items in less than a millisecond, and enables you to easily and cost effectively scale for higher loads.

```
package main

import (
	"fmt"
	"time"
	"github.com/Preethi0716/Cache-Library/preethi/restapi/pkg/cache"
)

func main() {
	// Create a new Memcached cache
	memcachedCache, err := cache.NewMemcachedCache("localhost:11211")
	if err != nil {
		fmt.Println("Error:", err)
		return
	}

	// Set a value in the cache
	err = memcachedCache.Set("key1", "value1", time.Minute)
	if err != nil {
		fmt.Println("Error:", err)
		return
	}

	// Get the value from the cache
	value, err := memcachedCache.Get("key1")
	if err != nil {
		fmt.Println("Error:", err)
	} else {
		fmt.Println("Value:", value)
	}
}
```

**Testing**

Our basic test code looks like

```
package tests

import (
	"fmt"
	"sync"
	"testing"
	"time"

	"github.com/Preethi0716/Cache-Library/preethi/restapi/pkg/cache"
)

func TestLRUCache_SetGet(t *testing.T) {
	cache := cache.NewLRUCache(2)
	cache.Set("key1", "value1", time.Minute)

	value, err := cache.Get("key1")
	if err != nil || value != "value1" {
		t.Fatalf("Expected value1, got %v", value)
	}
}

func TestRedisCache_SetGet(t *testing.T) {
	cache, err := cache.NewRedisCache("localhost:6379")
	if err != nil {
		t.Fatalf("Failed to create Redis cache: %v", err)
	}

	err = cache.Set("key1", "value1", time.Minute)
	if err != nil {
		t.Fatalf("Failed to set value: %v", err)
	}

	value, err := cache.Get("key1")
	if err != nil || value != "value1" {
		t.Fatalf("Expected value1, got %v", value)
	}
}B

func TestMemcachedCache_SetGet(t *testing.T) {
	cache, err := cache.NewMemcachedCache("localhost:11211")
	if err != nil {
		t.Fatalf("Failed to create Memcached cache: %v", err)
	}

	err = cache.Set("key1", "value1", time.Minute)
	if err != nil {
		t.Fatalf("Failed to set value: %v", err)
	}

	value, err := cache.Get("key1")
	if err != nil || value != "value1" {
		t.Fatalf("Expected value1, got %v", value)
	}
}
```

To run the tests for the library, use the following command:

```
go test ./tests/ -v
```

**Benchmark**

A benchmark is a type of function that executes a code segment multiple times and compares each output against a standard, assessing the code’s overall performance level. 
Golang includes built-in tools for writing benchmarks in the testing package and the go tool, so you can write useful benchmarks without installing any dependencies.

To run the benchmark for the library, use the following command

```
go test -bench=.
```

**Contributing**

If you would like to contribute to this project, please fork the repository and create a pull request with your changes. Ensure that your contributions are well-documented and include tests where applicable.

**License**

This project is licensed under the MIT License. See the LICENSE file for details.
