Design and Analysis of Synchronous Finite State Machines (S-FSM) Mealy and Moore. Serial Adder implementations using S-FSM. Verilog coding of the designs. Formal design steps, and informal design techniques.



## Synchronous Finite State Machines (FSMs)

FSMs are sequential circuits used to model systems that transition between states based on inputs and current state. There are two main types:

### Moore Machine

![[Moore.png | center|300 ]]

- Outputs depend **only** on the current state.
- More stable, as output changes only on state changes.
- So if you are on A, and the input is 1, you go to B and the output is 0 (inside circle of A). Meaning, the output is always going to be 0 if you are on A and going anywhere else, no matter what the input is.

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
- So if you are on A, the output condition changes if you go to B or A again. Unlike Moore where the output is always the same based on the state you're in.

```verilog
module mealy_fsm (
    input Clock, Resetn, w,
    output reg z
);
    reg [1:0] y, Y;

    parameter A = 2'b00, B = 2'b01, C = 2'b10;

    // Combinational logic for next state and output
    always @(*) begin
        case (y)
            A: begin
                if (w) begin
                    Y = B;
                    z = 0;
                end else begin
                    Y = A;
                    z = 0;
                end
            end
            B: begin
                if (w) begin
                    Y = C;
                    z = 0;
                end else begin
                    Y = A;
                    z = 1;  // output happens during transition
                end
            end
            C: begin
                if (w) begin
                    Y = C;
                    z = 1;
                end else begin
                    Y = A;
                    z = 0;
                end
            end
            default: begin
                Y = A;
                z = 0;
            end
        endcase
    end

    // Sequential block for state update
    always @(posedge Clock or negedge Resetn) begin
        if (!Resetn)
            y <= A;
        else
            y <= Y;
    end
endmodule

```
> [!Tip] 
> Mealy: Input/Output value on transition arc
> Moore: Output value inside circle
---

## Serial Adder using FSM

A **serial adder** adds two binary numbers bit by bit using one flip-flop to store the carry.

Example FSM States:

- `S0`: Carry = 0
- `S1`: Carry = 1

Inputs: `A`, `B` (1-bit at a time)  
Outputs: `Sum`

![[FSM-Adder.png]]

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

![[Example.png]]
Special thanks to Eng. Zainab Behbahani, truly one of the best engineers in the department.


## Informal FSM Design Techniques
uhh I have no idea what this means?




