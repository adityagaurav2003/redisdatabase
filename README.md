Redis Server (C++)

A Redis-like in-memory key–value server built from scratch using C++17.
Implements TCP networking, RESP protocol parsing, concurrency, TTL, and persistence.

This project exists to understand how Redis works internally, not to compete with Redis.

1. What This Project Does

Accepts multiple TCP clients

Parses Redis RESP protocol

Executes Redis-like commands

Stores data in memory

Persists data to disk

Supports TTL (key expiry)

Compatible with redis-cli.

2. Core Concepts Used

TCP/IP & Socket Programming

Multithreading (thread per client)

Mutex & synchronization

Data structures (hash tables, vectors)

RESP protocol parsing

File I/O persistence

Signal handling (graceful shutdown)

Command parsing & response formatting

Singleton pattern (database)

Bitwise operators (|=)

C++ standard libraries

3. Architecture Overview
Client (redis-cli)
        |
     TCP Socket
        |
   RedisServer
        |
 RedisCommandHandler
        |
   RedisDatabase (Singleton)

Concurrency Model

One thread per client

Single shared database

Global mutex protects data

Simple, correct, predictable

4. Main Classes

RedisServer
Handles sockets, accepts clients, manages threads

RedisCommandHandler
Parses RESP, validates commands, formats responses

RedisDatabase
Stores all data, TTL info, persistence logic

5. Folder Structure
.
├── include/
│   ├── RedisServer.h
│   ├── RedisCommandHandler.h
│   └── RedisDatabase.h
│
├── src/
│   ├── RedisServer.cpp
│   ├── RedisCommandHandler.cpp
│   ├── RedisDatabase.cpp
│   └── main.cpp
│
├── Makefile
├── test_all.sh
└── README.md

6. Setup
Requirements

Linux / macOS

g++ (C++17)

make

redis-cli (for testing)

Ubuntu
sudo apt update
sudo apt install g++ make redis-tools

macOS
brew install gcc make redis

7. Build
make


Clean:

make clean


Manual (optional):

g++ -std=c++17 -pthread -Iinclude src/*.cpp -o my_redis_server

8. Run
Default Port (6379)
./my_redis_server

Custom Port
./my_redis_server 6379

9. Connect
redis-cli -p 6379


Test:

PING


Expected:

PONG

10. Commands & Use Cases
Common Commands

PING
Use case: Health check before performing operations

ECHO
Use case: Network/debug testing

FLUSHALL
Use case: Reset cache or clear data during development

Key / Value Operations

SET / GET
Use case: Cache sessions, tokens, config values

KEYS
Use case: Inspect stored keys (KEYS session:*)

TYPE
Use case: Identify stored data type

DEL / UNLINK
Use case: Remove invalid or stale keys

EXPIRE
Use case: Auto-expiring cache entries

RENAME
Use case: Key migration without data loss

List Operations

LPUSH / RPUSH
Use case: Task queues, message buffers

LPOP / RPOP
Use case: Dequeue jobs

LLEN
Use case: Pending job count

LGET
Use case: Read entire list (like LRANGE 0 -1)

LINDEX
Use case: Inspect element without removal

LSET
Use case: Update list element in place

LREM
Use case: Remove duplicates or cancelled tasks

Hash Operations

HSET / HMSET
Use case: Store structured objects (user profiles)

HGET
Use case: Fetch a specific field

HEXISTS
Use case: Validate field existence

HDEL
Use case: Remove outdated fields

HLEN
Use case: Count object attributes

HKEYS / HVALS
Use case: Iterate over object fields or values

HGETALL
Use case: Retrieve full object

11. Example Usage
Key / Value
SET session:1 "data"
GET session:1
EXPIRE session:1 5

List
RPUSH L a b c
LGET L
LPOP L

Hash
HSET user:1 name Alice age 30 email a@x.com
HGETALL user:1

12. Persistence

Data saved to dump.my_rdb

Loaded on startup

Auto-save every 300 seconds

Saved on Ctrl + C

⚠️ Custom format. Not Redis RDB compatible.

13. Testing
Start Server
./my_redis_server 6379

Connect
redis-cli -p 6379

Run Script
./test_all.sh

14. Internal Data Structures

Strings → unordered_map<string, string>

Lists → unordered_map<string, vector<string>>

Hashes → unordered_map<string, unordered_map<string,string>>

TTL → unordered_map<string, time_t>

Locking → single global mutex

No abstractions. No overengineering
