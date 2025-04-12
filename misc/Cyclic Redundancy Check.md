

#### **Cyclic Redundancy Check (CRC)**

CRC is a more powerful error-detection method than checksums. It treats data as a large binary number and divides it by a fixed **generator polynomial**. The remainder of this division is called the **CRC**, which is sent along with the data.

CRC is widely used in Ethernet, storage devices, and wireless communication because it can detect burst errors more effectively than simple checksums.




Let’s say the **data** to send is:

```yaml
Data: 1101011011
Generator (divisor): 10011 (a 5-bit polynomial)
```

1. **Append zeros** to the data equal to the degree of the generator (4 zeros for 5-bit polynomial):
    

```yaml
Appended Data: 1101011011 0000
```

2. **Perform binary division** (just like long division but using XOR instead of subtraction).
    

For example, first few steps:

```
11010 ÷ 10011 → XOR gives 01001  
Bring down next bit → 10011 ÷ 10011 → XOR gives 00000  
... (continue)
```

3. **Final remainder** (after full division) is the **CRC**. Let’s say the result is:
    

```yaml
CRC: 0110
```

4. **Transmit:**  
    Sender sends `1101011011` + `0110` → `11010110110110`
    

---

5. **Receiver side:**
    

The receiver takes the entire received data (`11010110110110`) and divides it by the same generator (`10011`).

- If the remainder is **0**, the data is correct.
    
- If the remainder is **non-zero**, an error is detected.
    
