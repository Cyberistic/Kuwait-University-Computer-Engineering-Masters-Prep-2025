MSI circuits (Muxes, Cross-bar switches, Decoders, Encoders, Priority Encoders), and the Verilog coding of those circuits.

Before we start, if you don't remember the verilog syntax, here's a quick overview: [[Verilog]]

Note that I won't explain in details since you should have a general idea of how they work, instead, I will give a short description, typescript code and verilog.

## MUX

![[MUX-Symbol.png | center | 500]]

Based on select line `s`, connect inputs 0 or 1 to f.

_Note that you need log(n) select lines for n inputs. _

```typescript
function mux2to1(input1: boolean, input2: boolean, select: boolean): boolean {
  return select ? input2 : input1;
}
```

### Verilog

```verilog
module mux2to1 (input1, input2, select, output);
  input input1, input2, select;
  output output;
  assign output = select ? input2 : input1;
endmodule
```

![[4-to-1-MUX.png]]

You can even combine them to make a bigger MUX. For example, a 4-to-1 MUX can be made by combining two 2-to-1 MUXes.

```typescript
function mux4to1(
  x0: boolean,
  x1: boolean,
  x2: boolean,
  x3: boolean,
  s0: boolean,
  s1: boolean
): boolean {
  const mux1 = mux2to1(x0, x1, s0);
  const mux2 = mux2to1(x2, x3, s0);
  return mux2to1(mux1, mux2, s1);
}
```

There is no "end" to this.

### DEMUX

A DEMUX is the opposite of a MUX. It takes one input and routes it to one of the multiple outputs based on the select lines.
In reality, they are the same thing! Just flip the mux and you get a demux. (Since it's basically connecting a pin to another pin based on the select lines.)

### Shannon's Theorem

Shannon's theorem states that any boolean function can be implemented using a MUX.
This means that you can use a MUX to implement any logic circuit. The theorem is based on the idea that any boolean function can be expressed as a combination of AND, OR, and NOT gates.

> [!Note] I'm not sure if this theorem is required for this exam, so you might want to skip it.

First, let's see how to implement a boolean function using a MUX.

Suppose you have this boolean table:

| x   | y   | f(x,y) |
| --- | --- | ------ |
| 0   | 0   | 0      |
| 0   | 1   | 1      |
| 1   | 0   | 1      |
| 1   | 1   | 0      |

The output in term of f(x,y) is:
f(x,y) = x'y + xy' = (x' + y)(x + y')
This can be implemented using a 2-to-1 MUX as follows:

![[Weird Mux thing.png | center | 300]]

In shannon's theorem, we can use the MUX to implement the function by using the inputs as select lines and the output as the function.

Say we have this table:

| w1 | w2 | w3 | f(w1,w2,w3) |
| -- | -- | -- | ----------- |
| 0 | 0 | 0 | 0 |
| 0 | 0 | 1 | 1 |
| 0 | 1 | 0 | 1 |
| 0 | 1 | 1 | 0 |
| 1 | 0 | 0 | 1 |
| 1 | 0 | 1 | 0 |
| 1 | 1 | 0 | 0 |
| 1 | 1 | 1 | 1 |

The output in term of f(w1,w2,w3) is:
f(w1,w2,w3) = w1'w2'w3 + w1'w2w3' + w1w2'w3' + w1w2w3
To solve, we take 2 inputs, say w1 and w2, as common denominators *"عامل مشترك"*:
$$ \begin{aligned} 
\ f(w_1, w_2, w_3) &= \overline{w_1}\overline{w_2} \cdot f(0, 0, w_3) + \overline{w_1}w_2 \cdot f(0, 1, w_3) + w_1\overline{w_2} \cdot f(1, 0, w_3) + w_1w_2 \cdot f(1, 1, w_3)) \\
&= \overline{w_1}\overline{w_2}(w_3) + \overline{w_1}w_2(\overline{w_3}) + w_1\overline{w_2}(\overline{w_3}) + w_1w_2(w_3)) 
 \end{aligned}
$$


This can be implemented using a 4-to-1 MUX as follows:
![[Shannon's Theorem.png | center | 300]]

> [!Important] Fun fact..
> I recently used a 16-to-1 MUX to make a 16 buttons keyboard.
> In my code I just loop through the inputs and use the select lines to determine which button is pressed.
> Instead of connecting 16 wires to the esp32, I only needed 5 (4 select and 1 output).
> ![[ESP32-MUX.png|center|200]]
