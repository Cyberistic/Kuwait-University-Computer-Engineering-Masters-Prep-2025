Design and Analysis of Synchronous Finite State Machines (S-FSM) Mealy and Moore. Serial Adder implementations using S-FSM. Verilog coding of the designs. Formal design steps, and informal design techniques.

Sure! Here’s a concise and friendly write-up that matches the tone and style of your sequential circuits doc. It covers Synchronous FSMs, Mealy vs Moore, Serial Adders, Verilog examples, and design techniques:

---

## Synchronous Finite State Machines (FSMs)

FSMs are sequential circuits used to model systems that transition between states based on inputs and current state. There are two main types:

### Moore Machine

- Outputs depend **only** on the current state.
- More stable, as output changes only on state changes.

### Mealy Machine

- Outputs depend on **current state + input**.
- Usually more compact and reacts faster.

> [!Tip]
> If the output needs to change instantly with input, use Mealy.
> If you want stability and clean timing, go Moore.

---

## Serial Adder using FSM

A **serial adder** adds two binary numbers bit by bit using one flip-flop to store the carry.

Example FSM States:

- `S0`: Carry = 0
- `S1`: Carry = 1

Inputs: `A`, `B` (1-bit at a time)  
Outputs: `Sum`

### Moore Serial Adder Diagram

![[Moore-Serial-Adder.png]]

### Mealy Serial Adder Diagram

![[Mealy-Serial-Adder.png]]

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
