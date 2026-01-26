# Redis Server (C++)

A Redis-like in-memory database server implemented in **C++17**.  
Built to understand networking, concurrency, protocol parsing, and data storage at a low level.

> Educational systems project. Not production-ready.

---

## Features

- Redis-cli compatible
- TCP/IP socket-based server
- Multiple client support
- RESP protocol parsing
- Thread-per-client concurrency model
- Mutex-based synchronization
- In-memory storage
- Key expiration (TTL)
- Periodic and shutdown persistence

---

## Architecture

Client (redis-cli)
|
TCP
|
RedisServer
|
RedisCommandHandler
|
RedisDatabase (Singleton)

yaml
Copy code

---

## Concepts Used

- TCP/IP and Socket Programming
- Concurrency and Multithreading
- Mutex and Synchronization
- Data Structures (Hash Tables, Vectors)
- RESP Protocol Parsing
- File I/O Persistence
- Signal Handling
- Command Processing & Response Formatting
- Singleton Pattern
- Bitwise Operators (`|=`)
- C++ Standard Libraries

---

## Core Classes

### RedisServer
- Initializes TCP socket
- Accepts client connections
- Spawns client threads

### RedisCommandHandler
- Parses RESP input
- Validates commands
- Dispatches operations
- Formats responses

### RedisDatabase
- Stores all data
- Manages TTL
- Handles persistence
- Implemented as Singleton

---

## How to Build and Run

```bash
make
./my_redis_server        # default port 6379
./my_redis_server 6380   # custom port
Connect Using redis-cli
bash
Copy code
redis-cli -p 6379
PING
Expected response:

nginx
Copy code
PONG
Commands & Use Cases
Common Commands
PING
Use case: Check if the server is alive before performing operations.

ECHO
Use case: Debugging and connectivity testing.

FLUSHALL
Use case: Clear all stored data during development or cache reset.

Key / Value Operations
SET
Use case: Cache session data or configuration values.

GET
Use case: Retrieve cached data.

KEYS
Use case: Inspect keys or perform maintenance tasks.

TYPE
Use case: Identify the stored data type.

DEL / UNLINK
Use case: Remove stale or invalid keys.

EXPIRE
Use case: Auto-expiring cached entries.

RENAME
Use case: Rename keys without data loss.

List Operations
LGET
Use case: Retrieve all list elements.

LLEN
Use case: Check queue length.

LPUSH / RPUSH
Use case: Add tasks to a queue.

LPOP / RPOP
Use case: Consume tasks from queue.

LREM
Use case: Remove specific list values.

LINDEX
Use case: Inspect a list element without removal.

LSET
Use case: Update a list entry.

Hash Operations
HSET
Use case: Store structured objects (user profiles).

HGET
Use case: Retrieve a specific field.

HEXISTS
Use case: Check field existence.

HDEL
Use case: Remove outdated fields.

HGETALL
Use case: Retrieve full object data.

HKEYS / HVALS
Use case: Iterate over fields or values.

HLEN
Use case: Count number of fields.

HMSET
Use case: Set multiple fields at once.

Testing
1. Start Server
bash
Copy code
./my_redis_server 6379
2. Connect
bash
Copy code
redis-cli -p 6379
3. Common Commands
Command	Example	Expected
PING	PING	PONG
ECHO	ECHO "Hello"	Hello
FLUSHALL	FLUSHALL	OK

4. Key / Value Tests
Command	Example	Expected
SET/GET	SET a x → GET a	"x"
KEYS	KEYS *	key list
TYPE	TYPE a	string
DEL	DEL a	(integer) 1
EXPIRE	EXPIRE a 5	OK
RENAME	RENAME a b	OK

Tip: After EXPIRE, access the key after TTL to verify deletion.

5. List Tests
Command	Example	Expected
LGET	RPUSH L a b c	list
LLEN	LLEN L	(integer) 3
LPOP	LPOP L	"a"
LSET	LSET L 1 x	OK

6. Hash Tests
Command	Example	Expected
HSET	HSET user:1 name Alice	(integer) 1
HGET	HGET user:1 name	"Alice"
HEXISTS	HEXISTS user:1 name	(integer) 1
HGETALL	HGETALL user:1	fields + values

Persistence
Data stored in dump.my_rdb

Loaded on startup

Auto-save every 300 seconds

Saved on graceful shutdown

