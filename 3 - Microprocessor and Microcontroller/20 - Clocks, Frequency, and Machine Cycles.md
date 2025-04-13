Clocks, Frequency, and Machine Cycles

## Clock

A clock syncs all internal operations of a microcontroller. It uses an on-chip oscillator to generate a square wave signal.

Assume that 1 machine cycle = 12 oscillator cycles for this note.

### Calculations

1. **Execution Time**: Time taken to execute an instruction

   - Execution Time = # of Machine Cycles × Machine cycle Time
     $$$ T_{exec} = \text{# cycles} \times T_{machine cycle} $$$

2. System Clock Frequency: Number of clock cycles per second

   - System Clock Frequency = 1 / Machine cycle Time
     $$$ f_{clock} = \frac{1}{T_{machine cycle}} $$$
     $$$ T_{machine cycle} = \frac{1}{f_{clock}} $$$

3. **Machine Cycle**:

- 1 Machine Cycle = 12 Oscillator Cycles

Example: Assume microcontroller with 11.0592 MHz crystal, what is the cycle time? How long to execute a MOV DPTR, #200H?
assume 1 cycle = 12 oscillator cycles and MOV = 2 machine cycles.
use align for latex on equals sign

First, system clock frequency:

$$
\begin{align}
F_{SYS} &= F_{OSC} / x  \\
&= 11.0592 \text{ MHz} / 12 \\
&= 0.9216 \text{ MHz} \\
\end{align}
$$

Then, machine cycle time:

$$
\begin{align}
T_{SYS} &= 1 / F_{SYS} \\
&= 1 / 0.9216 \text{ MHz} \\
&= 1.084 \mu s \\
\end{align}
$$

Finally, execution time:

$$
\begin{align}
T_{exec} &= \text{# of Machine Cycles} \times T_{machine cycle} \\
&= 2 \times 1.084 \mu s \\
&= 2.168 \mu s \\
\end{align}
$$

### Clock Frequency

The speed of a processor is primarily determined by its clock frequency.

- 1 Hz = 1 cycle/second
- 1 MHz = 1,000,000 cycles/second
- 1 GHz = 1,000,000,000 cycles/second

> [!example] Example
> A 4GHz processor performs 4 billion cycles per second.
> Period = 1/4,000,000,000 = 0.25 nanoseconds per cycle

> [!note] Important
> Not all instructions take the same number of machine cycles!
> Simple operations might take 1-2 cycles, while complex ones can take many more.

### Example Instruction Timing

```assembly
MOV AX, BX    ; 2 machine cycle
ADD AX, [SI]  ; 3 machine cycles (memory access)
MUL CX        ; 3+ machine cycles (complex operation)
```
