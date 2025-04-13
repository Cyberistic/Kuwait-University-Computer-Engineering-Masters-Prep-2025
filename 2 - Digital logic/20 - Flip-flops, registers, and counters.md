Flip flops (SR, D, T, Master-Slave, JK, latches and edge triggered), Registers, counters (synchronous, and asynchronous) and the Verilog coding.

These are all sequential circuits that store and process binary information. Unlike combinational circuits, these have memory - their outputs depend on both current inputs and previous state.

> [!Note] tl;dr:
> Combinational: output = f(input)
> Sequential: output = f(input, previous_output)
> So sequential has memory of the past

![[Memory-symbol.png]]

## Basic Latches

Latches are basic memory elements that can store one bit of information.

### SR Latch (Set-Reset)

![[SR Latch.png]]

The most basic memory element. Has two inputs and clock:

- S (Set): Sets output to 1
- R (Reset): Sets output to 0
- Only happens when clock is 1

So you set, and it stores that "active" state, you reset, and it stores that "inactive" state. When either go to 0, it stays in the last state.

- Q: Output
- Qbar: Inverted output

Truth Table:

| CLK | S   | R   | Q   | Q'  | Comment   |
| --- | --- | --- | --- | --- | --------- |
| 0   | x   | x   | Q   | Q'  | No change |
| 1   | 0   | 0   | Q   | Q'  | No change |
| 1   | 0   | 1   | 0   | 1   | Reset     |
| 1   | 1   | 0   | 1   | 0   | Set       |
| 1   | 1   | 1   | x   | x   | Invalid   |

```verilog
module sr_latch(
    input S, R,
    output reg Q, Qbar
);
    always @(S or R) begin
        if (S && !R) begin
            Q <= 1;
            Qbar <= 0;
        end else if (!S && R) begin
            Q <= 0;
            Qbar <= 1;
        end
        // S=R=1 is invalid state
        // S=R=0 maintains previous state
    end
endmodule
```

### D Latch

![[D-Latch-Symbol.png]]

Improved version of SR latch that prevents invalid states. Has:

- D (Data): Input data
- CLK (Clock): Controls when data is stored

When CLK=1, D is stored in Q. When CLK=0, Q retains its value.

So instead of having two inputs, it has one input and a clock. It only changes state when the clock is high.

```verilog
module d_latch(
    input D, CLK,
    output reg Q
);
    always @(D or CLK) begin
        if (CLK)
            Q <= D;
    end
endmodule
```

> [!Note]
> It's not necessary for latches to include the clock input. The simplest latch does not have a clock input, and the output changes state whenever the inputs change.

## Flip-Flops

Flip-flops are edge-triggered devices that change state on clock edges. They are built using latches.
They are used in synchronous circuits to store data.
They are more reliable than latches because they only change state on clock edges, reducing the chance of glitches.

### D Flip-Flop

Similar to D latch but changes state only on clock edge.

The verilog is literally a single word different (`posedge`):

```verilog
module d_flipflop(
    input D, CLK,
    output reg Q
);
    always @(posedge CLK) begin
        Q <= D;
    end
endmodule
```

#### Master-Slave

![[Master-Slave-Symbol.png]]

By chaining multiple D flip-flops, and inverting the clock for the second one, we create a master-slave flip-flop. The first flip-flop (master) captures the input on the rising edge of the clock, and the second flip-flop (slave) captures the output of the master on the falling edge of the clock.

> [!Important] Important Note
> There exists an SR-Flip-Flop which acts the same as the D flip-flop but has an invalid state when both S and R are 1. So we don't usually use it.

```verilog
module master_slave_flipflop(
    input D, CLK,
    output reg Q
);
    reg D_master;

    always @(posedge CLK) begin
        D_master <= D; // Capture input on rising edge
    end

    always @(negedge CLK) begin
        Q <= D_master; // Capture output on falling edge
    end
endmodule
```

We can chain this as many times as we want, this is how we create registers.

### T Flip-Flop (Toggle)

![[T-Flip-Flop-Symbol.png]]

Similar to D flip-flop BUT it _flips_ its current state on every clock edge if T=1. If T=0, it retains its state.

Especially useful for counters.

Truth Table:
| T | CLK | Q(next) | Comment |
|---|-----|---------|-----------|
| 0 | 0 | Q | No change |
| 0 | 1 | Q | No change |
| 1 | 0 | Q | No change |
| 1 | 1 | ~Q | Toggle |

```verilog
module t_flipflop(
    input T, CLK,
    output reg Q
);
    always @(posedge CLK) begin
        if (T)
            Q <= ~Q;
    end
endmodule
```

> [!Important]- Fun fact..
> I learned about T-flip-flops looong (2012-ish) before joining the CE world; because of Minecraft...
> It was the simplest way to make a redstone button act as a toggle switch.
> ![[Minecraft-T-Flip-Flop.png | center | 300]]
>
> As I wrote this, I got curious so I spent like 15 minutes looking for the video.. here is where I learned it (1:15 to 3:40):
> ![Redstone stuff](https://www.youtube.com/watch?v=4Y2oPNSdqNI&t=75s)

### JK Flip-Flop

![[JK-Flip-Flop-Symbol.png]]

It combines the behaviors of SR and T flip-flops in a useful way. It behaves as the SR flip-flop, where J = S and K = R, for all input values except J = K = 1. For the latter case, which has to be avoided in the SR flip-flop, the JK flip-flop toggles its state like the T flip-flop.

Truth Table:

| J   | K   | Q(next) | Comment   |
| --- | --- | ------- | --------- |
| 0   | 0   | Q       | No change |
| 0   | 1   | 0       | Reset     |
| 1   | 0   | 1       | Set       |
| 1   | 1   | ~Q      | Toggle    |

```verilog
module jk_flipflop(
    input J, K, CLK,
    output reg Q
);
    always @(posedge CLK) begin
        case ({J,K})
            2'b00: Q <= Q;    // No change
            2'b01: Q <= 0;    // Reset
            2'b10: Q <= 1;    // Set
            2'b11: Q <= ~Q;   // Toggle
        endcase
    end
endmodule
```

## Registers

A register is a group of flip-flops that store multiple bits.

A flip-flop stores one bit of information. When a set of n flip-flops is used to store n bits of information, such as an n-bit number, we refer to these flip-flops as a register.

### Basic Register

Where each register has a different input line connected to D

```verilog
module register #(
    parameter WIDTH = 8
)(
    input [WIDTH-1:0] D,
    input CLK, RESET,
    output reg [WIDTH-1:0] Q
);
    always @(posedge CLK or posedge RESET) begin
        if (RESET)
            Q <= 0;
        else
            Q <= D;
    end
endmodule
```

### Shift Register

![[Simple-Shift-Register.png]]
Can shift bits left or right.

```verilog
module shift_register #(
    parameter WIDTH = 8
)(
    input CLK, RESET,
    input SHIFT_RIGHT, // 1 for right, 0 for left
    input IN_BIT,     // New bit to shift in
    output reg [WIDTH-1:0] Q
);
    always @(posedge CLK or posedge RESET) begin
        if (RESET)
            Q <= 0;
        else if (SHIFT_RIGHT)
            Q <= {IN_BIT, Q[WIDTH-1:1]}; // Right shift
        else
            Q <= {Q[WIDTH-2:0], IN_BIT}; // Left shift
    end
endmodule
```

### Parallel Reads Register

![[Parallel-Register.png]]

Same as above, but has output at every "stage" or flip-flip.

> [!Important] Fun fact..
> Parallel Shift Registers (TPIC6B595) are perhaps my **FAVORITE** ESP32 components. It always you to control an (almost) INFINITE number of output GPIO pins using only 3 ESP32 pins!!!
> Each Register gives you 8 outputs AND THEY ARE CHAINABLE, IT IS SO GOOD AND I HATE HOW NO ONE KNOWS ABOUT THEM!!
> [![[TPIC6B595N.png|center|300]]

## Counters

Counters count.

### Up counter

Example:
3 T-Flip-flops give you 3 bits, counting up to 7, which is what we call a modulo-8 counter:

![[Up-counter.png]]
![[Up-Counter-Timing.png]]

Notice how the first bit corresponds directly to the clock, while the other bits change after a delay.

### Down counter

Slight modification of the above counter gives you a down counter:

![[Down-Counter.png]]
![[Down-Counter-Timing.png]]

### Synchronous Counters

The counters above are **asynchronous**. They are easy to build but very slow. As more bits are added, delays from the chained clocking become too long. To make faster counters, we can clock all flip-flops at the same time:

![[Synchronous-Counters.png]]

> [!Warning] Note..
> I am ending it here, but the book [[Stephen Brown, Zvonko Vranesic - Fundamentals of Digital Logic with Verilog Design-McGraw-Hill (2014).pdf#page=273]] goes into more details about:
>
> 1.  Inclusion of Enable and Clear capability
> 2.  Synchronous Counter with D Flip-Flops
> 3.  Counters with Parallel Load
> 4.  Reset Synchronization
> 5.  BCD Counter
> 6.  Ring Counter
> 7.  Johnson Counter
> 8.  and so on..
>     I don't remember studying most of them before, so i'm skipping them; however you may want to take a quick look? idk im not ur mother
