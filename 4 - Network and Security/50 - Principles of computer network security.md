Principles of computer network security (cryptography and message integrity).

Once again, suuuper vague description, so I will only go through the basics. This is like an ENTIRE field!
check the book for more details I guess. 

[[James Kurose, Keith Ross - Computer Networking_ A Top-Down Approach, 7th Edition.pdf#page=655]]


Quick reminder of terms:


- **Confidentiality** – Preventing unauthorized access (e.g., encryption)
	`Can anyone see my data except the reciever?`
    
- **Integrity** – Detecting unauthorized modification
	`Is data sent same as data recieved or did someone change it?`
	
- **Authentication** – Verifying identity of the sender
	`Is the person or system I'm communicating with really **who they claim to be**?`
	
- **Non-repudiation** – Preventing denial of sent messages
	`Will the user be able to *deny* sending this message? For example, they will deny that they signed the paper? `



###Principles of Computer Network Security


## Cryptography ✨

Cryptography is the art of scrambling (encrypting) data so that only authorized parties can read it.

### Types of Cryptography:

####  **Symmetric Key Encryption**

- Both sender and receiver use the **same secret key** to encrypt and decrypt.
    
- Fast, but key distribution is a problem.
    

> Example: AES, DES  
> Used in VPNs, secure messaging apps

```plaintext
Encrypt(Message, Key) → Ciphertext  
Decrypt(Ciphertext, Key) → Message
```

> [!Note] Another example of this is a cesar cipher
#### **Public Key Encryption (Asymmetric)**

- Two keys: a **public** key (shared with everyone) and a **private** key (kept secret).
    
- One key encrypts, the other decrypts.
    

> Example: RSA, ECC  
> Used in HTTPS, digital signatures

```plaintext
Encrypt(Message, PublicKey) → Ciphertext  
Decrypt(Ciphertext, PrivateKey) → Message
```

---

## ✅ 2. **Message Integrity**

Message integrity ensures that a message hasn’t been **altered** during transmission.

### Techniques:

#### 🔘 **Message Digests / Hashing**

A fixed-length hash (digest) is generated from a message.

- If even **1 bit changes**, the hash will be different.
    
- Common Hash Algorithms: **MD5**, **SHA-256**
    

> Example:  
> `hash("hello") → 2cf24d...`  
> `hash("Hello") → 185f8d...` ← different!

#### ✍️ **Message Authentication Code (MAC)**

A hash of the message **plus a shared secret key**.

```plaintext
MAC = hash(message + secret_key)
```

- Verifies both **integrity** and **authenticity**
    
- Used in SSL/TLS, Wi-Fi (WPA2)
    

#### 🔏 **Digital Signatures**

Used with **public key cryptography**.

- Sender signs a message digest with their **private key**.
    
- Receiver verifies it using the sender’s **public key**.
    

> Guarantees integrity + authenticity  
> Often combined with public-key encryption

---

## 📎 Real-World Example: HTTPS

When you connect to a secure website via **HTTPS**:

1. Your browser gets the **public key** from the website’s certificate.
    
2. It uses it to securely exchange a **symmetric key** (for speed).
    
3. All messages are encrypted and include **MACs** for integrity.
    
4. The certificate is signed by a **Certificate Authority (CA)** to ensure authenticity.
    

---

## 🧠 tl;dr

|**Concept**|**Purpose**|**Tools**|
|---|---|---|
|Encryption|Protect message content (confidentiality)|AES, RSA, TLS|
|Hashing|Detect changes in message (integrity)|MD5, SHA-256|
|MAC|Integrity + authenticity|HMAC|
|Digital Signature|Integrity + authentication|RSA + SHA|
|Certificates (PKI)|Verify identity of websites|X.509, CA|

---

Want me to follow this up with a note on **Firewalls, VPNs, or TLS Handshake**?