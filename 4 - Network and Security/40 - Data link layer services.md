`Kurose and Ross, the Data Link Layer services are discussed in Chapter 6
____
### 1. Link Layer Review

The **Link Layer** is the second layer in the OSI model and is responsible for establishing, maintaining, and terminating connections between devices on the same network. It handles communication between directly connected devices, like computers or routers, over physical links (e.g., Ethernet, Wi-Fi protocols).

Key responsibilities of the Link Layer:

- **Framing**: It packages data into frames for transmission.
    
- **Addressing**: It uses MAC (Media Access Control) addresses to identify devices on the same network.
    
- **Error Detection and Correction**: It detects errors in transmitted frames and can correct some errors (e.g., using checksums or CRC).
    
- **Access Control**: It manages how devices access the shared physical medium (like the Ethernet protocol).
_____
### 2. Error Detection 
#### 2.1 Bit Error

- When only a single bit is changed during the transmission in the link.

	SENDER: 0, 0, 1 --> RECIEVER: 0, **<span style="color: red;">1 </span>**,1

#### 2.2 Burst Error

- When multiple bits are changed during the transmission in the link.
  For example:

	SENDER: 0, 0, 0, 1, 1, 1 --> RECIEVER: 0, **<span style="color: red;">1, 0, 0, 0 </span>**,1
	
	`Even though bit number 2 (with a value of 0) was received correctly as 0, it is still highlighted in red. This happens because the burst treats all the bits between the valid ones as invalid, marking them as errors.
### 2.3 Error Detection Techniques 

1. **Parity Bits**: A single bit added to the data to make the number of 1s either even (even parity) or odd (odd parity). If the number of 1s is incorrect, an error is detected.

2. **Checksums**: Data is divided into segments, and a checksum value is computed. The receiver recalculates the checksum and compares it with the received value to detect errors.

3. **Cyclic Redundancy Check (CRC)**: A more robust error detection method that treats the data as a large binary number, divides it by a pre-determined polynomial, and appends the remainder (CRC) to the data. The receiver performs the same division and checks if the remainder is zero, indicating no errors.

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

> 🖊️ Author: Asmaa Alazmi