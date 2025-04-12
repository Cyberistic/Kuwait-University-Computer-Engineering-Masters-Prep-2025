Transport Layer Protocols (UDP and TCP).




The **Transport Layer** sits above the **network layer** (like IP) and is responsible for **end-to-end communication** between processes on different hosts.

Its job is to **break messages into segments**, **send them**, and **ensure they’re reassembled correctly** on the other side.


## Main Goals
- **Process-to-process delivery** (e.g., from one application to another).
    
- **Multiplexing and demultiplexing** using **port numbers**.
    
- **Reliable or unreliable** data transfer.
    
- **Flow and congestion control** (TCP only).
    


## UDP (User Datagram Protocol)

UDP is a **simple, connectionless**, and **fast** transport protocol.

### Features:

- **No connection setup** (connectionless).
    
- **No reliability**: No acknowledgment, no retransmission.
    
- **No ordering**: Packets may arrive out of order.
    
- **Lightweight** and **low overhead**.
    
- Often used for **real-time** applications where speed is more important than reliability.
    

### Common Uses:

- **Streaming (audio/video)** like YouTube live.
    
- **Online gaming**.
    
- **DNS** lookups.

> [!Note] Services like Facetime and Twitch streams use UDP. Lose a few packets? no problem! The stream will keep sending you packets. 
> better than breaking the connection completely!

### UDP Segment Format:

- Source Port
    
- Destination Port
    
- Length
    
- Checksum
    

---

## TCP (Transmission Control Protocol)

TCP is a **connection-oriented**, **reliable** transport protocol.

### Features:

- **Connection setup** (3-way handshake).
    
- **Reliable**: Acknowledges received data, retransmits lost packets.
    
- **In-order delivery**: Ensures data is assembled in the correct order.
    
- **Flow control**: Avoids overwhelming the receiver.
    
- **Congestion control**: Avoids flooding the network.
    

### Common Uses:

- **Web browsing (HTTP/HTTPS)**
    
- **Email (SMTP, IMAP)**
    
- **File transfers (FTP)**
    

### 3-Way Handshake (Connection Setup):

1. Client → Server: SYN
    
2. Server → Client: SYN-ACK
    
3. Client → Server: ACK
    

After this, data transfer begins.

### TCP Segment Format:

- Source Port
    
- Destination Port
    
- Sequence Number
    
- Acknowledgment Number
    
- Flags (SYN, ACK, FIN, etc.)
    
- Window Size
    
- Checksum
    
- Data




> [!Note] If you send a file, for example an image, you want to make sure that file arrives fully with all its data! 
> This is where we use TCP to validate data received.


> [!Warning] Important..
> I did not include the way TCP resolves lost packets etc as they are probably outside the scope of this exam.
> But maybe they are not? ¯\\_(ツ)_/¯ 
> ![[TCP-JOKE.png | center | 300]]

## 🔍 Comparison Table

|Feature|TCP|UDP|
|---|---|---|
|Connection|Connection-oriented|Connectionless|
|Reliability|Reliable (ACK + Retransmit)|Unreliable|
|Order|In-order delivery|No guarantee|
|Speed|Slower (more overhead)|Faster (less overhead)|
|Use Cases|Web, email, file transfer|Streaming, gaming, DNS|
|Flow/Congestion Ctrl|Yes|No|



## tl;dr: 

- **UDP** is fast, simple, and used where **speed > reliability**.
    
- **TCP** is reliable and ordered, used where **accuracy > speed**.
    

> 🎯 Use UDP when you can **tolerate loss** but need speed.  
> 📦 Use TCP when **every byte matters**, even if it’s slower.

