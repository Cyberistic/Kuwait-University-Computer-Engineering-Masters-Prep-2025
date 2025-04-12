

Principles of network applications and its protocols (HTTP, Electronic Mail, DNS).  

Super vague description, will provide general knowledge about the above.

Remember the 7-layered OSI model which 

![[OSI-Model.png]]
[OSI Model Source](https://www.imperva.com/learn/application-security/osi-model/)



##🌐 What is the internet?

Network applications let devices **communicate over the internet**. These applications (like web browsers, email clients, or video apps) rely on **application-layer protocols** that define how data is sent and received.

Unlike lower layers (like TCP/IP or Ethernet, which will be discussed in the next notes), the application layer is what users interact with directly.

---

## 💬 Application Architecture

Two main types of communication structures:

- **Client-Server Model**
    
    - One central **server** responds to many **clients**.
        
    - E.g., web servers and browsers.
        
- **Peer-to-Peer (P2P) Model**
    
    - Devices (peers) both request and provide services.
        
    - E.g., file sharing like BitTorrent.


> [!Important] Fun fact..
> [Tailscale](https://tailscale.com/) allows you to establish a P2P connection by using their service as the server for the initial "handshake" only.
> I've used it recently while traveling to a country which blocks many common websites and social media apps; by making my computer in 🇰🇼 act as a free VPN. :D



## 📡 Key Application Protocols

### 1. **HTTP (Hypertext Transfer Protocol)**

Used by **web browsers and servers** to transfer web pages and data.

- **Client-Server** based (browser is the client).
    
- Runs over **TCP** (usually port 80 or 443 for HTTPS).
    
- **Stateless**: Server doesn't remember past requests.
    
- Methods:
    
    - `GET`: Request a resource (e.g., a web page).
        
    - `POST`: Send data (e.g., form submission).
        
    - `PUT`, `DELETE`, etc. for RESTful APIs.
        

**Example**:

```http
GET /index.html HTTP/1.1
Host: example.com
```

> [!Note] Whenever you use websites in your favorite browser, it serves you the HTML files using the HTTP protocol. 
> HTML = Hyper-Text Markup Language
> HTTP = Hyper-Text Transfer Protcol
> Coincidence? I think not.

### 2. **Electronic Mail (Email Protocols)**

Email is delivered through multiple protocols:

- **SMTP (Simple Mail Transfer Protocol)**
    
    - Sends emails from client → server 
    - or server → server.
        
    - Uses TCP (port 25, 587).
        
- **POP3 (Post Office Protocol v3)** and **IMAP (Internet Message Access Protocol)**
    
    - Used by clients to fetch and manage emails from servers.
        
    - POP3: Downloads and deletes from server.
        
    - IMAP: Keeps email on server, supports folders and syncing.

> [!Question] Which is more secure? POP3 or IMAP?
> Which is more reliable (gives you backup in case you lose your emails)

**Email flow**:

1. User writes email → Client sends it using SMTP.
    
2. Server stores email.
    
3. Recipient fetches email using POP3 or IMAP.
    

---

### 3. **DNS (Domain Name System)**

Translates **domain names** (like `google.com`) into **IP addresses** (like `142.250.64.78`).

- Works like a **phonebook** for the internet.
    
- Uses **UDP** (port 53).
    
- Hierarchical system with:
    
    - **Root servers**
        
    - **Top-level domain (TLD) servers** (.com, .org, etc.)
        
    - **Authoritative name servers** (e.g., Google’s DNS)
        

**Example**: Typing `www.example.com` triggers a DNS lookup: → gets IP address → browser connects to that IP using HTTP.



## tl;dr

|Protocol|Purpose|Transport|Port|
|---|---|---|---|
|HTTP|Web pages & APIs|TCP|80/443|
|SMTP|Sending emails|TCP|25/587|
|POP3|Receiving (download)|TCP|110|
|IMAP|Receiving (sync)|TCP|143|
|DNS|Resolving domain names|UDP|53|



> [!Note] These protocols are just the **tip of the iceberg** — the internet uses a LOT MORE!
> But maybe that's out of the context of this exam? idk

