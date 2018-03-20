# COMP3331 - Topic 2: Application Layer

## 2.1 Principles of Network Application
### Creating a network app
- Write programs that run on different end systems, communicate over network.
  - eg. web server software communicates with browser software
- Varying degrees of integration
  - Loose: email, web browsing
  - Medium: chat, Skype
  - Tight: process migration, distributed file systems
- No need to write software for network-core devices
  - Network-core devices do not run user applications

### Interprocess Communication (IPC)
- Processes talk to each other through IPC
- On a single machine: shared memory
- Across machine: we need other abstractions

### Sockets
- Process sends/receives messages to/from its **socket**
- Socket is analogous to door

### Addressing process
- To receive messages, process must have **identifier**
- Host device has a unique 32-bit IP address
- Many process can be running on the same host, so we need more information
- **Identifier** includes both **IP address** and **port numbers** associated with process on host
- Example port numbers:
  - HTTP server: 80
  - mail server: 25

### Client-server architecture
**Server**:  
- Exports well-defined request/response interface
- Long-lived process that waits for requests
- Upon receiving request, carries it out
Characteristics:  
- Always-on host
- Permanent IP address
- Static port conventions

**Clients**:  
- Short-lived process that makes requests
- "User-side" of application
- Initiates the communication
Characteristics:  
- May be intermittently connected
- May have dynamic IP address
- Do not communicate directly with each other

### P2P architecture
- No always-on server, no permanent rendezvous involved
- Arbritrary end systems (peers) directly communicate
- Symmetric responsibility (unlike client/server)
- Often used for: file sharing, games, video distribution

**Pros**  
- Peers request service from mother peers, provide service in return to other peers
- Speed: parallelism, less contention
- Reliability: redundancy, fault tolerance
- Geographic distribution

**Cons**  
- State uncertainty: no shared memory or clock
- Action uncertainty: mutually conflicting decisions
- Distributed algorithms are complex

### App-layer protocol defines
- Types of messages exchanged
- Message syntax
- Message semantics
- Rules

Type of protocols:  
- Open protocols: defined in RFC, eg. HTTP, SMTP
- Proprietary protocols: eg. Skype

#### Transport service an app needs
- Data integrity:
  - Some apps require 100% reliable data transfer. eg. File transfer, web transaction
  - Other apps can tolerate some loss. eg. audio, video streaming
- Timing:
  - Some apps require low delay to be "effective". eg. Internet telephone, interactive games
- Throughput:
  - Some apps require minimum amount of throughput to be "effective". eg. multimedia
  - Other apps ("elastic apps") make use of whatever throughput they get
- Security:
  - encryption, data integrity, etc

### Internet trasport protocols services
TCP service:
- Reliable transport between sending and receiving process
- Flow control: sender won't overwhelm receiver
- Congestion control: throttle sender when network overloaded
- Does not provide: Timing, minimum throughput guarantee, security
- Connection-oriented: setup required between client and server processes

UDP service:  
- Unreliable data transfer between sending and receiving process
- Does not provide: reliability, flow control, congestion control, timing, thtoughput guarantee, security, or connection setup

## 2.2 Web and HTTP
### Review
- Web page consists of objects
- Object can be HTML, JPG, Java applet, audio file, etc
- Web page consists of base HTML-file which includes several referenced objects- Each object is addressable by a URL

### Uniform Record Locator (URL)
```
protocol://host-name[:port]/directory-path/resource
```
- protocol: http, ftp, smtp, etc
- hostname: DNS name, IP address
- port: defaults to protocol's standard port; eg http: 80, https: 443
- directory path: hierarchical, reflecting file system
- resource: Identifies the desired resource

### Hypertext Transfer Protocol (HTTP) overview
- Web's application layer protocol
- Client/server model:
  - Client: browser that requests, receives, (using HTTP) and "displays" Web objects
  - Server: web server sends (using HTTP) objects in response to request
- Uses TCP:
  - Client initiates TCP connection (creates socket) to server, port 80
  - Server accepts TCP connection from client
  - HTTP messages (application-layer protocol messages) exchanged between browser (HTTP client) and web server (HTTP server)
  - TCP connection closed

**HTTP is "stateless"**  
Server maintains no information about past client requests

#### HTTP messages
- ASCII (human readable format) - ALL TEXT
- HTTP Request message
- HTTP Response message

#### HTTP response status codes
- 200: OK
- 301: Moved Permanently
- 400: Bad Request
- 404: Not Found
- 505: HTTP Version Not Supported
- 451: Unavailable for Legal
- 429: Too Many Requests
- 418: I'm a Teapot

