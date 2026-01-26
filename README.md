Redis Server (C++)

A lightweight Redis-like in-memory database server implemented in C++17.
The project focuses on understanding low-level systems concepts such as TCP networking, concurrency, protocol parsing, and data persistence.

The server is compatible with redis-cli and supports a subset of Redis commands.

Overview

This project implements a custom key–value database server with the following goals:

Learn how Redis works internally

Build a TCP server from scratch

Implement the RESP protocol

Handle concurrent clients safely

Design in-memory data structures

Persist data to disk

This is not a production replacement for Redis.
It is a systems-level learning project.

Key Features

TCP server with multiple client support

RESP protocol parsing

Thread-per-client concurrency model

Mutex-protected shared database

Support for Strings, Lists, and Hashes

Key expiration (TTL)

Periodic and shutdown persistence

Compatible with redis-cli

Architecture
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

One thread per client connection

Single shared database instance

Global mutex ensures data consistency

Simple, predictable, and safe design

Core Concepts Used

TCP/IP and socket programming

Multithreading and synchronization

Mutexes and critical sections

RESP protocol parsing

In-memory data structures

File I/O and persistence

Signal handling

Singleton design pattern

Bitwise operators (|=)

C++ standard library usage

Main Components
RedisServer

Initializes socket

Accepts client connections

Spawns client handler threads

RedisCommandHandler

Parses RESP input

Validates commands

Dispatches operations

Formats responses

RedisDatabase

Stores all data

Manages TTL

Handles persistence

Implemented as a singleton

Project Structure
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

Setup
Requirements

Linux / macOS / Windows (WSL recommended)

g++ with C++17 support

make

redis-cli (for testing)

Ubuntu
sudo apt update
sudo apt install g++ make redis-tools

macOS
brew install gcc make redis

Build
make


Clean build files:

make clean

Run
Default Port (6379)
./my_redis_server

Custom Port
./my_redis_server 6380

Connect Using redis-cli
redis-cli -p 6379


Test connection:

PING


Expected response:

PONG

Supported Commands and Use Cases
Common Commands

PING
Used to check server availability

ECHO
Used for connectivity and debugging

FLUSHALL
Clears all stored data (useful during development)

Key / Value Operations

SET / GET
Cache sessions, tokens, configuration values

KEYS
Inspect stored keys or patterns

TYPE
Determine data type of a key

DEL / UNLINK
Remove unused or expired keys

EXPIRE
Automatically expire cached data

RENAME
Rename keys without losing data

List Operations

LPUSH / RPUSH
Task queues, message buffers

LPOP / RPOP
Job or message consumption

LLEN
Queue size monitoring

LGET
Retrieve all list elements

LINDEX
Inspect specific list elements

LSET
Modify list entries in place

LREM
Remove specific list values

Hash Operations

HSET / HMSET
Store structured objects (user profiles)

HGET
Fetch individual fields

HEXISTS
Check field existence

HDEL
Remove outdated fields

HLEN
Count stored fields

HKEYS / HVALS
Iterate over fields or values

HGETALL
Retrieve full object data

Persistence

Data saved to dump.my_rdb

Loaded automatically on startup

Auto-save every 300 seconds

Saved on graceful shutdown (Ctrl + C)

Note:
Persistence format is custom and not Redis RDB compatible.

Testing
Start Server
./my_redis_server 6379

Connect
redis-cli -p 6379

Run Test Script
./test_all.sh

Internal Data Structures

Strings → unordered_map<string, string>

Lists → unordered_map<string, vector<string>>

Hashes → unordered_map<string, unordered_map<string, string>>

TTL tracking → unordered_map<string, time_t>

Synchronization → single global mutex
