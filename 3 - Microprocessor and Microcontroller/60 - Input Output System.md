Input/Output System.

Again, super vague topic!! Will give headlines.

## Ports

8052 has 8 8-bit bidirectional ports (P0-P7). Each bit can be individually addressed.

```NASM
SETB P1.0    ; Set bit 0 of Port 1
CLR  P2.7    ; Clear bit 7 of Port 2
MOV  P3, #0FFH ; Set all bits of Port 3

MOV A, P0    ; Read Port 0 into accumulator
```

## Basic I/O Devices

1. 7-Segment Display

Common-anode or common-cathode configuration:

2. LCD Interface

Standard 16x2 LCD uses 8-bit or 4-bit mode:

```NASM
MOV P2, #38H    ; 8-bit, 2 lines
MOV P2, #0CH    ; Display ON, cursor OFF
MOV P2, #01H    ; Clear display
```

## Serial Communication

### UART (Serial Port)

Uses TxD (P3.1) and RxD (P3.0) pins.

Baud Rate calculation:

$$
\text{Baud Rate} = \frac{\text{Oscillator Frequency}}{32 \times 12 \times (256-\text{TH1})}
$$

Example for 9600 baud with 11.0592MHz crystal:

$$
\begin{align*}
9600 &= \frac{11059200}{384 \times (256-\text{TH1})} \\
\text{TH1} &= 256 - \frac{11059200}{384 \times 9600} \\
\text{TH1} &= 253 \text{ (FDH)}
\end{align*}
$$

### Types of Serial Communication

1. **UART**: Asynchronous, two-wire
2. **SPI**: Synchronous, 4-wire (MOSI, MISO, SCK, SS)
3. **I2C**: Synchronous, 2-wire (SDA, SCL)

### Communication Modes

1. **Simplex**

   - One-way communication only
   - One device transmits, other only receives
   - Example: Television broadcast

2. **Half-Duplex**

   - Two-way communication, but one direction at a time
   - Devices take turns transmitting
   - Example: Walkie-talkie

3. **Full-Duplex**
   - Two-way simultaneous communication
   - Both devices can transmit and receive at the same time
   - Example: Telephone conversation, UART

> [!tip] Most microcontroller serial communications use:
>
> - UART: Full-duplex (RX and TX lines)
> - SPI: Full-duplex (MOSI and MISO)
> - I2C: Half-duplex (single data line - SDA)

> [!Note] _Synchronous_ means that the clock signal is shared between the sender and receiver, while _asynchronous_ means that each device has its own clock.
> In asynchronous communication, the sender and receiver must agree on a communication protocol (how do we talk to each other). The baud rate is the speed of communication, and it must be the same for both devices.

## Serial Communication with Shift Registers

We need to convert bytes into bits for serial communication. Shift registers are used for this purpose.

1. _SIPO (Serial-In-Parallel-Out)_:

- Receiver
- Used for receiving data serially and converting it to parallel format.
- LED displays, input expansion

2. _PISO (Parallel-In-Serial-Out)_:

- Transmitter
- Used for sending data in serial format to parallel inputs.
- Keyboard scanning, output compression

### Examples

1. LED control using shift register:

```NASM
ShiftByte:
    MOV R2, #8        ; 8 bits
Next:
    RLC A            ; Rotate left through carry
    MOV P1.0, C     ; Output data bit
    SETB P1.1       ; Clock pulse
    CLR P1.1
    DJNZ R2, Next   ; Next bit
    RET
```

2. Reading from parallel input:

```NASM
ReadByte:
    SETB P1.2       ; Load parallel data
    CLR P1.2
    MOV R2, #8      ; 8 bits
    MOV A, #0       ; Clear accumulator
Next:
    MOV C, P1.0     ; Read serial input
    RLC A           ; Rotate into accumulator
    SETB P1.1       ; Clock pulse
    CLR P1.1
    DJNZ R2, Next
    RET
```
