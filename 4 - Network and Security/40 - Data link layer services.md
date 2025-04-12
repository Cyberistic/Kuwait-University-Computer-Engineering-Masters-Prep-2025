Data link layer services (error detection, error correction, and multiple access).  

[[James Kurose, Keith Ross - Computer Networking_ A Top-Down Approach, 7th Edition.pdf#page=493]]


The **Link Layer** is the second layer in the OSI model and is responsible for establishing, maintaining, and terminating connections between devices on the same network. It handles communication between directly connected devices, like computers or routers, over physical links (e.g., Ethernet, Wi-Fi protocols).

## Key responsibilities 

- **Framing**: It packages data into frames for transmission.
    
- **Addressing**: It uses MAC (Media Access Control) addresses to identify devices on the same network.
    
- **Error Detection and Correction**: It detects errors in transmitted frames and can correct some errors (e.g., using checksums or CRC).
    
- **Access Control**: It manages how devices access the shared physical medium (like the Ethernet protocol).

## Error Detection 
How does it know the packets arrived correctly? huh? HUH??

First let's establish the error types:
1. Bit Error

- When only a single bit is changed during the transmission in the link.

	SENDER: 0, 0, 1 --> RECIEVER: 0, **<span style="color: red;">1 </span>**,1

2. Burst Error

- When multiple bits are changed during the transmission in the link.
  For example:

	SENDER: 0, 0, 0, 1, 1, 1 --> RECIEVER: 0, **<span style="color: red;">1, 0, 0, 0 </span>**,1
	
> [!Important] Even though bit number 2 (with a value of 0) was received correctly as 0, it is still highlighted in red. 
> This happens because the burst treats all the bits between the valid ones as invalid, marking them as errors.

### Detection Techniques 

#### **Parity Bits**
A single bit is added to the data, telling you if the sum of the bits (including the added bit) are *even* (even parity) or *odd* (odd parity). If the number of 1s is incorrect, an error is detected.

Example: with even parity, suppose you have 3 ones `1010100`, so the parity bit is 1 as well making you have 4 ones `10101001 ` (an even amount of bits). Computer will see it's a 1, indicating the sum of the number before it (excluding it) is odd. If not, an error occured?


<br/>

 ![[Even-Parity.png | center | 300]]


<br/>

> [!Note] But what if more than one bit flips?!
> You can use two-dimensional parity checks, mentioned in the book: [[James Kurose, Keith Ross - Computer Networking_ A Top-Down Approach, 7th Edition.pdf#page=503]]
> Or Hamming codes (Mentioned below)!
> ![https://www.youtube.com/watch?v=X8jsijhllIA](https://www.youtube.com/watch?v=X8jsijhllIA)


#### **Checksums**
Data is divided into segments, and a checksum value is computed. The receiver recalculates the checksum and compares it with the received value to detect errors.

In the internet, the receiver checks the checksum by taking the 1s complement of the sum of the received data (including the checksum) and checking whether the result is all 1 bits. If any of the bits are 0, an error is indicated.


Let’s say we’re sending **two 8-bit words**:

```yaml
Word 1: 01010101  
Word 2: 01100110
```

1. Add the words

```yaml
  01010101  
+ 01100110  
------------
  10111011  
```

✅ No overflow, so we don’t need to wrap around.

2. Take the 1’s complement of the result (flip all bits)

```yaml
Original Sum:     10111011  
1's complement:   01000100  ← This is the **checksum**  
```

The sender now sends:

- Word 1: `01010101`
    
- Word 2: `01100110`
    
- Checksum: `01000100`
    


3. Receiver side:

The receiver adds all 3 values:

```
  01010101  
+ 01100110  
+ 01000100  
------------
  11111111  ← All bits are 1s = no error!
```

If the result is **not** all 1s (`11111111`), an error is detected.


#### **Cyclic Redundancy Check (CRC)**
A more robust error detection method that treats the data as a large binary number, divides it by a pre-determined polynomial, and appends the remainder (CRC) to the data. The receiver performs the same division and checks if the remainder is zero, indicating no errors.

>[!Important] Important..
> We won't go into details assuming it's out of the exam's scope.


#### tl;dr
| **Error Detection Technique**     | **Type of Error Detected**             | **Corrects Errors?** | **Used In**                                                | **Advantages**                                                                            |
| --------------------------------- | -------------------------------------- | -------------------- | ---------------------------------------------------------- | ----------------------------------------------------------------------------------------- |
| **Parity Bit**                    | Single-bit errors                      | No                   | Simple systems, memory storage, and transmission protocols | Simple and low overhead but only detects single-bit errors                                |
| **Checksum**                      | Errors in blocks of data               | No                   | **IPv4, UDP, TCP**, application-level protocols            | Detects errors in larger data blocks, better than parity                                  |
| **Cyclic Redundancy Check (CRC)** | Errors in data transmission            | No                   | **Ethernet**, HDLC, ATM, data storage systems              | Very effective, **detects burst errors** and is widely used in Ethernet                   |
| **Hamming Code**                  | Single-bit errors & <br>two-bit errors | Yes (single-bit)     | Memory storage, error-correcting codes in communication    | Corrects single-bit errors and detects two-bit errors, but requires extra bits for parity |

> These techniques help maintain **data integrity** by ensuring that corrupted or altered frames are detected and can be retransmitted.

_________
### 3. Error Correction 

There are multiple ways to correct errors:

1. The receiver asks the sender to retransmit the entire data unit.

2. The receiver can use an error correcting code, which automatically corrects certain errors. 

__________

> [!Important] Author: Asmaa Alazmi