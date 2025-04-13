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

> [!Note] _Synchronous_ means that the clock signal is shared between the sender and receiver, while _asynchronous_ means that each device has its own clock.
> In asynchronous communication, the sender and receiver must agree on a communication protocol (how do we talk to each other). The baud rate is the speed of communication, and it must be the same for both devices.

## UART Communication

_SIPO (Serial-In-Parallel-Out)_

- Used for receiving data serially and converting it to parallel format.
- Commonly used in microcontrollers for serial communication.
- Example: Receiving data from a serial device and storing it in a register.

- **PISO (Parallel-In-Serial-Out)**
- Used for sending data in serial format from parallel inputs.
- Commonly used in microcontrollers for serial communication.
- Example: Sending data from a register to a serial device like an LCD.

````mermaid

### SIPO (Serial-In-Parallel-Out)

### PISO (Parallel-In-Serial-Out)

Used for reducing output pins:

```mermaid
graph LR
    A[8 Parallel Inputs] --> B[Shift Register] --> C[Serial Data]
````

> [!tip] Common Applications
>
> - SIPO: LED displays, input expansion
> - PISO: Keyboard scanning, output compression

Basic shift register operation:

```assembly
SETB CLOCK     ; Clock high
MOV DATA, P1.0 ; Send data bit
CLR CLOCK      ; Clock low
```

### Examples

1. LED control using shift register:

```assembly
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

```assembly
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
