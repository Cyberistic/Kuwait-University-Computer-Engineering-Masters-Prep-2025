MSI circuits (Muxes, Cross-bar switches, Decoders, Encoders, Priority Encoders), and the Verilog coding of those circuits.

Before we start, if you don't remember the verilog syntax, here's a quick overview: [[Verilog]]

Note that I won't explain all the details since you should have a general idea of how they work, instead, I will give a short description, and both typescript and verilog codes.

## MUX

![[MUX-Symbol.png | center | 500]]

Based on select line `s`, connect inputs 0 or 1 to f.

_Note that you need log(n) select lines for n inputs. _

### Typescript

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

> [!Important] Fun fact..
> I recently used a 16-to-1 MUX to make a 16 buttons keyboard.
> In my code I just loop through the inputs and use the select lines to determine which button is pressed.
> Instead of connecting 16 wires to the esp32, I only needed 5 (4 select and 1 output).
> ![[ESP32-MUX.png|center|200]]

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
$$\( f(x,y) = \overline{x}y + x\overline{y} = (\overline{x} + y)(x + \overline{y}) \)$$
This can be implemented using a 2-to-1 MUX as follows:

![[Weird Mux thing.png | center | 300]]

In shannon's theorem, we can use the MUX to implement the function by using the inputs as select lines and the output as the function.

Say we have this table:

| w1  | w2  | w3  | f(w1,w2,w3) |
| --- | --- | --- | ----------- |
| 0   | 0   | 0   | 0           |
| 0   | 0   | 1   | 1           |
| 0   | 1   | 0   | 1           |
| 0   | 1   | 1   | 0           |
| 1   | 0   | 0   | 1           |
| 1   | 0   | 1   | 0           |
| 1   | 1   | 0   | 0           |
| 1   | 1   | 1   | 1           |

The output in term of f(w1,w2,w3) is:
f(w1,w2,w3) = w1'w2'w3 + w1'w2w3' + w1w2'w3' + w1w2w3
To solve, we take 2 inputs, say w1 and w2, as common denominators _"عامل مشترك"_:

$$
\begin{aligned}
\ f(w_1, w_2, w_3) &= \overline{w_1}\overline{w_2} \cdot f(0, 0, w_3) + \overline{w_1}w_2 \cdot f(0, 1, w_3) + w_1\overline{w_2} \cdot f(1, 0, w_3) + w_1w_2 \cdot f(1, 1, w_3)) \\
&= \overline{w_1}\overline{w_2}(w_3) + \overline{w_1}w_2(\overline{w_3}) + w_1\overline{w_2}(\overline{w_3}) + w_1w_2(w_3))
 \end{aligned}
$$

![[Shannons-theorem.png| center | 300]]

Given the folloing equation:

$$
f(x,y,z) = m1 + m2 + m3 + m5 + m7
$$

Draw the truth table:

| m   | x   | y   | z   | f(x,y,z) |
| --- | --- | --- | --- | -------- |
| 0   | 0   | 0   | 0   | 0        |
| 1   | 0   | 0   | 1   | 1        |
| 2   | 0   | 1   | 0   | 1        |
| 3   | 0   | 1   | 1   | 0        |
| 4   | 1   | 0   | 0   | 1        |
| 5   | 1   | 0   | 1   | 0        |
| 6   | 1   | 1   | 0   | 0        |
| 7   | 1   | 1   | 1   | 1        |

Then the equation is as follows (taking y and z as common):

$$
\begin{aligned}
f(x,y,z) &= m1 + m2 + m3 + m5 + m7\\
&= \overline{x}\overline{y}z + \overline{x}y\overline{z} + x\overline{y}\overline{z} + x\overline{y}z + xyz \\
&= \overline{y}z \cdot f(x, 0, 0) + \overline{y}z \cdot f(x, 0, 1) + y\overline{z} \cdot f(x, 1, 0) + yz \cdot f(x, 1, 1)\\
\end{aligned}
$$

Now you have to "derive" a solution based on x,
You can see from the table:

1. when y' and z', f is always 0
2. when y' and z: f is x' or x
3. when y and z': f is x'
4. when y and z: f is always 1

so continuing the formula:

$$
\begin{aligned}
f(x,y,z) &= \overline{y}z \cdot [0] + \overline{y}z \cdot [x + \overline{x}] + y\overline{z} \cdot [\overline{x}] + yz \cdot [1]\\

&= \overline{y}z \cdot [0] + \overline{y}z \cdot [1] + y\overline{z} \cdot [\overline{x}] + yz \cdot [1]\\
\end{aligned} \\
$$

and the result:
![[Shannons-theorem-1.png|center|300]]

and we can write the verilog like this:

```verilog
// f(x,y,z) = m1 + m2 + m3 + m5 + m7
module shannon (x, s, f);
  input x;
    input [1:0] s; // y and z variables
    output reg f;

    always @(x or s)
        case (s)
            0: f <= 0;
            1: f <= 1;
            2: f <= ~x;
            3: f <= 1;
        endcase
endmodule
```

Again, idk if shannon is even in the exam heh.

You're almost there! The explanation is good, but a few grammatical and clarity tweaks will make it more polished and accurate. Here's an improved version:

---

## Crossbar Switch

![[Crossbar-symbol.png | center | 300]]

A **crossbar switch** is a circuit with `n` inputs and `k` outputs, designed to connect any input to any output.  
When there are two inputs and two outputs, it's called a **2×2 crossbar**.

Based on the **select lines**, the crossbar connects a specific input to the desired output.

The crossbar switch is typically implemented using **2-to-1 multiplexers (MUXes)**.  
The number of MUXes required is `n × k`, where:

- `n` is the number of inputs, and
- `k` is the number of outputs.

The number of select lines required per MUX is `log₂(n)`.

Here is an example:

Inputs: `x1`, `x2`  
Outputs: `y1`, `y2`

Each output (`y1` and `y2`) is connected to a multiplexer that selects between the two inputs (`x1`, `x2`).

| Select Lines | y1 Output | y2 Output |
| ------------ | --------- | --------- |
| 00           | x1        | x1        |
| 01           | x1        | x2        |
| 10           | x2        | x1        |
| 11           | x2        | x2        |

**How It Works:**

- `y1` gets its value from either `x1` or `x2`, depending on a select line (say `s1`)
- `y2` does the same independently (with `s2`)
- The select lines can be configured to connect each output to whichever input is desired

So if:

- `s1 = 0` → `y1 = x1`
- `s1 = 1` → `y1 = x2`
- `s2 = 0` → `y2 = x1`
- `s2 = 1` → `y2 = x2`

You get total flexibility between inputs and outputs.

### Typescript

```typescript
function crossbarSwitch(
  x1: boolean,
  x2: boolean,
  s1: boolean,
  s2: boolean
): [boolean, boolean] {
  const y1 = s1 ? x2 : x1;
  const y2 = s2 ? x2 : x1;
  return [y1, y2];
}
```

### Verilog

```verilog
module crossbarSwitch (x1, x2, s1, s2, y1, y2);
  input x1, x2;
  input s1, s2;
  output y1, y2;

  assign y1 = s1 ? x2 : x1;
  assign y2 = s2 ? x2 : x1;
endmodule
```

or with MUXes:

```verilog
module crossbarSwitch (x1, x2, s1, s2, y1, y2);
  input x1, x2;
  input s1, s2;
  output y1, y2;

  wire mux1_out, mux2_out;

  mux2to1 mux1 (x1, x2, s1, mux1_out);
  mux2to1 mux2 (x1, x2, s2, mux2_out);

  assign y1 = mux1_out;
  assign y2 = mux2_out;
endmodule
```

## Decoder

![[Decoder-symbol.png]]

A **decoder** is a circuit that converts binary information from `n` input lines to a maximum of `2^n` unique output lines.
It essentially "decodes" the binary input into a specific output line.
For example, a 2-to-4 decoder has 2 input lines and 4 output lines.  
When the input is `00`, the first output line is activated; when the input is `01`, the second output line is activated, and so on.

> [!Note] By adjusting the output behavior, you can create _code converters_ or _address decoders_.
> These are often used in things like 7-segment displays, where the decoder converts binary input into a specific output pattern to display numbers.

Usually, it also has an `EN` input.

You can use the `EN` input to combine encoders to build bigger decoders!
![[combined-decoders.png|center|300]]
![[combined-decoder.png|center|300]]

```typescript
function decoder2to4(
  input: number,
  enable: boolean
): [boolean, boolean, boolean, boolean] {
  if (!enable) return [false, false, false, false];

  switch (input) {
    case 0:
      return [true, false, false, false];
    case 1:
      return [false, true, false, false];
    case 2:
      return [false, false, true, false];
    case 3:
      return [false, false, false, true];
    default:
      return [false, false, false, false];
  }
}
```

### Verilog

```verilog
module decoder2to4 (input, enable, output);
  input [1:0] input;
  input enable;
  output reg [3:0] output;

  always @(input or enable) begin
    if (!enable) begin
      output = 4'b0000;
    end else begin
      case (input)
        0: output <= 4'b0001;
        1: output <= 4'b0010;
        2: output <= 4'b0100;
        3: output <= 4'b1000;
        default: output <= 4'b0000;
      endcase
    end
  end
endmodule
```

## Encoder

![[Encoder-symbol.png|center|300]]
An **encoder** is a circuit that converts `2^n` input lines into `n` output lines.
It essentially "encodes" the input into a binary representation.
For example, a 4-to-2 encoder has 4 input lines and 2 output lines.  
When the first input line is activated, the output will be `00`; when the second input line is activated, the output will be `01`, and so on.

The output is usually in binary form, and encoders usually have a `Z` output to indicate if no input is activated. If no input is activated, the output will be `disabled` (usually `00`) and Z is `0`, otherwise Z is `1`.

### Priority Encoders

What if multiple inputs are activated at the same time?
A **priority encoder** resolves this by assigning priority to the inputs where the highest priority input will be encoded (usually the last input).
For example, in a 4-to-2 priority encoder, if inputs `1` and `3` are activated, the output will be `11` (indicating input `3`).

Truth table:
| w1 | w2 | w3 | w4 | f(w1,w2,w3,w4) |
| --- | --- | --- | --- | ------------- |
| 0 | 0 | 0 | 0 | 00 |
| 0 | 0 | 0 | 1 | 00 |

### Typescript

```typescript
function encoder4to2(
  input: [boolean, boolean, boolean, boolean],
  enable: boolean
): [boolean, boolean] {
  if (!enable) return [false, false];

  if (input[0]) return [false, false];
  if (input[1]) return [false, true];
  if (input[2]) return [true, false];
  if (input[3]) return [true, true];

  return [false, false]; // Default case
}
```

### Verilog

```verilog
module encoder4to2 (input, enable, output);
  input [3:0] input;
  input enable;
  output reg [1:0] output;

  always @(input or enable) begin
    if (!enable) begin
      output = 2'b00;
    end else begin
      case (input)
        4'b0001: output <= 1;
        4'b0010: output <= 2;
        4'b0100: output <= 3;
        4'b1000: output <= 4;
        default: output <= 0;
      endcase
    end
  end
```
