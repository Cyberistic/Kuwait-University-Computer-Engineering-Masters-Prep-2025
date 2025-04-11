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

A DEMUX is the opposite of a MUX. It takes one input and routes it to one of the multiple outputs based on the select lines.
In reality, they are the same thing! Just flip the mux and you get a demux. (Since it's basically connecting a pin to another pin based on the select lines.)

> [!Important] Fun fact..
> I recently used a 16-to-1 MUX to make a 16 buttons keyboard.
> In my code I just loop through the inputs and use the select lines to determine which button is pressed.
> It's such a useful piece
