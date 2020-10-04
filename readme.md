# Technical documentation

[Knowledge map (SVG)](https://raw.githubusercontent.com/mialkin/documentation/master/knowledge%20map.svg)

## Table of cоntents

- [Computer science](#computer-science)
- [Databases](#databases)
- [DevOps](#devops)
- [dotNET](#dotnet)
- [Miscellaneous](#miscellaneous)
- [Networking](#networking)
- [Programming](#programming)
- [Software design](#software-design)
- [Unix](#unix)
- [Web](#web)

## Computer science

- Data types
  - Primitive data types
    - Character
    - Integer
    - Floating-point number
    - Boolean
    - Reference or pointer
  - Composite data types
    - Array
    - Object
    - Record
    - Set
    - Union
  - Abstract data types
    - Associative array, or map, or dictionary
      - Implementation
        - Hash table
        - Self-balancing binary search tree
    - Graph
    - List
      - Implementation
        - Array
        - Variable length array
        - Singly linked list
        - Doubly linked list
    - Queue
    - Stack
    - Tree
- Data structures
  - Arrays
    - Array
    - Dynamic array
    - Circular array
    - Matrix
  - Graphs
    - Graph
    - Adjacency matrix
    - Directed acyclic graph
  - Hash-based structures
    - Hash table
    - Hash tree
  - Lists
    - Singly linked list
    - Doubly linked list
  - Trees
    - Tree
    - Binary trees
      - Binary search tree
        - Red-black tree
        - AVL tree
      - B-trees
      - Heaps
- Algorithms
  - Recursion
  - Sorting algorithms
    - Insertion sort
    - Selection sort
    - Merge sort
    - Quick sort
    - Bubble sort
  - Graph traversal
    - Tree traversal
      - Depth-first search
      - Breadth-first search
    - Dijkstra's algorithm
  - Analysis of algorithm
    - Time complexity
    - Space complexity
    - Big O notation
- Object-oriented programming (OOP)
  - Abstraction
  - Incapsulation
  - Inheritance
  - Polymorphism
- Cryptography
  - Symmetric cryptography
  - Assymetric cryptography
    - Public key
    - Private key
    - Fingerprint
    - Digital certificate

## Databases

- ACID
  - Atomicity
  - Consistency
  - Isolation
  - Durability
- Transaction
  - Isolation levels
    - Read uncommitted
    - Read commmitted
    - Repeatable read
    - Serializable
- Indexes
  - Clusterd indexes
  - Nonclustered indexes
    - Filtered indexes
- Joins
- Execution plan
- Trigger
- Stored procedure
- Function
- Cursor
- SQL
  - T-SQL
  - PL/SQL
- Document databases
  - CouchDB
  - Elasticsearch
  - MongoDB
- Relational databases
  - Microsoft SQL Server
  - Oracle Database
  - SQLite
  - PostgreSQL
- In-memory databases
  - Memcached
  - Redis
- IDE
  - SQL Server Managment Studio
  - DataGrip

## DevOps

- Continious integration (CI)
- Continious delivery (CD)
- Continious deployment (CD)
- Tools
  - Confluence
  - [Docker](unix/docker.md)
    - Kubernetes
  - GitLab
  - Jenkins
  - Jira
  - Markdown
  - TeamCity

## dotNET

- C#
  - [Access modifiers](dotnet/csharp/access%20modifiers.md)
  - [Constructor execution order](dotnet/csharp/constructor%20execution%20order.md)
  - [Covariance and contravariance](dotnet/csharp/covariance%20and%20contravariance.md)
- .NET API
- Common Language Infrastructure (CLI)

## Miscellaneous

- [Reading list](reading%20list.md)
- [Study list](study%20list.md)
- [Перевод технических терминов](translation.md)

## Networking

- OSI model
- Internet protocol suite
  - Transmission Control Protocol (TCP)
  - Internet Protocol (IP)
- HTTP
  - gRPC
    - Protobuf
- WebSocket
- FTP
- TLS
- SSH

## Programming

- Designing
- Coding
- Testing
  - Unit testing
    - Unit testing libraries
      - NUnit
    - Mock objects
  - Integration testing
  - Load testing
  - Stress testing

## Software design

- [Design patterns](software%20design/design%20patterns/design%20patterns.md)
- SOLID
  - [Dependency Inversion Principle](software%20design/dependency%20inversion/dependency%20inversion.md)
- [Composition over inheritance](software%20design/composition%20over%20inheritance.md)

## Unix

- macOS
  - Z shell
  - Homebrew
- Linux
  - CentOS
    - [YUM](unix/yum.md)
  - [Ubuntu](unix/ubuntu.md)
    - APT
- [shell](unix/shell.md)
  - Bourne shell (sh)
  - [Bourne again shell (bash)](unix/bash.md)
  - Commands
    - System
      - [crontab](unix/crontab.md)
      - date
      - [df](unix/df.md)
      - [du](unix/du.md)
      - free
      - [htop](unix/htop.md)
      - sudo
      - su
      - [systemctl](unix/systemctl.md)
      - kill
      - [tmux](unix/tmux.md)
      - uptime
      - w
      - watch
      - who
      - whoami
    - Files
      - cat
      - chmod
      - chown
      - grep
      - [less](unix/less.md)
      - ls
      - mkdir
      - mv
      - nano
      - pwd
      - [rg](unix/rg.md)
      - rm
      - [tail](unix/tail.md)
      - [tar](unix/tar.md)
      - [vim](unix/vim.md)
    - Network
      - curl
      - nc
      - [rsync](unix/rsync.md)
      - [scp](unix/scp.md)
      - [ssh](unix/ssh.md)
      - wget
- [Nginx](unix/nginx.md)
  - Certbot

## Web

- HTML
- CSS
- JavaScript
  - TypeScript
  - Libraries
    - React
  - Frameworks
    - Angular
    - Ext JS
  - Runtime environments
    - Node.js
      - npm
- Web services
  - Representational State Transfer (REST)
    - Cross-origin resourece sharing (CORS)
    - JSON
    - JSON Web Token (JWT)
    - OpenAPI
      - Swagger UI
    - OAuth 2.0
  - Simple Object Access Protocol (SOAP)
    - XML
    - WSDL
  - Microservices