Design and Analysis of Synchronous Finite State Machines (S-FSM) Mealy and Moore. Serial Adder implementations using S-FSM. Verilog coding of the designs. Formal design steps, and informal design techniques.



## Synchronous Finite State Machines (FSMs)

FSMs are sequential circuits used to model systems that transition between states based on inputs and current state. There are two main types:

### Moore Machine

![[Moore.png | center|300 ]]

- Outputs depend **only** on the current state.
- More stable, as output changes only on state changes.

```verilog
module moore_fms (Clock, Resetn, w, z);
    input Clock, Resetn, w;
    output z;
    reg [1:0] y, Y;
    parameter A = 2'b00, B = 2'b01, C = 2'b10;
    // Define the next state combinational circuit
    always @(*) case (y)
        A: if (w) Y = B; else Y = A;
        B: if (w) Y = C; else Y = A;
        C: if (w) Y = C; else Y = A;
        default: Y = 2'b00;
    endcase
    // Define the sequential block
    always @(posedge Clock, negedge Resetn)
        if (!Resetn) y <= A;
        else y <= Y;
    // Define output
    assign z = (y == C);
endmodule
```

### Mealy Machine

![[Mealy.png | center | 200]]

- Outputs depend on **current state + input**.
- Usually more compact and reacts faster.

> [!Tip] 
> Mealy: state/input
> Moore: Output value inside circle
---

## Serial Adder using FSM

A **serial adder** adds two binary numbers bit by bit using one flip-flop to store the carry.

Example FSM States:

- `S0`: Carry = 0
- `S1`: Carry = 1

Inputs: `A`, `B` (1-bit at a time)  
Outputs: `Sum`

![[FMS-Adder.png]]

### Moore Serial Adder Diagram

![[Moore-Adder.png]]
![[Moore-Adder-1.png]]

### Mealy Serial Adder Diagram

![[FSM-Adder.png]]
![[Mealy-Adder.png]]


---

## Verilog Example – Mealy Serial Adder

```verilog
module mealy_serial_adder (
    input clk, reset,
    input A, B,
    output reg SUM
);
    reg CARRY;

    always @(posedge clk or posedge reset) begin
        if (reset)
            CARRY <= 0;
        else begin
            SUM <= A ^ B ^ CARRY;
            CARRY <= (A & B) | (CARRY & (A ^ B));
        end
    end
endmodule
```

## Verilog Example – Moore FSM (2 states)

```verilog
module moore_serial_adder (
    input clk, reset,
    input A, B,
    output reg SUM
);
    reg [1:0] state;

    parameter S0 = 0, S1 = 1;

    always @(posedge clk or posedge reset) begin
        if (reset)
            state <= S0;
        else begin
            case (state)
                S0: begin
                    SUM <= A ^ B;
                    if (A & B) state <= S1;
                end
                S1: begin
                    SUM <= ~(A ^ B); // plus carry
                    if (!(A | B)) state <= S0;
                end
            endcase
        end
    end
endmodule
```

---

## FSM Design: Formal Steps

1. **Problem Definition**  
   Understand inputs, outputs, and desired behavior.

2. **State Diagram**  
   Draw states, transitions, inputs, and outputs.

3. **State Table**  
   Translate diagram into a truth table.

4. **State Assignment**  
   Give binary values to each state.

5. **Flip-Flop Selection**  
   Choose D, T, JK, etc.

6. **Next-State Logic**  
   Use K-maps or logic equations.

7. **Output Logic**  
   Define outputs (based on Moore or Mealy).

8. **Verilog Implementation**  
   Write and simulate your code.

---

## Informal FSM Design Tips

- Start with a drawing, even a napkin sketch helps.
- Use meaningful state names: `IDLE`, `WAIT`, `ADD`, `DONE` instead of `S0`, `S1`...
- Simulate early. Don't wait until everything is perfect.
- Try one-hot encoding if your FSM is small—it’s easier to debug.
- Write a testbench early. FSM bugs are often timing-related.

---

> [!Quote]
> “FSMs are like storytelling with logic. Each state is a scene, inputs are choices, outputs are consequences.”
