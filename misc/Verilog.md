![[Multiplexer-example.png|center|400]]

The code for the multiplexer above is given by:

```verilog
module example1 (x1, x2, s, f);
	input x1, x2, s;
	output f;
	not (k, s);
	and (g, k, x1);
	and (h, s, x2);
	or (f, g, h);
endmodule
```

Notice how it takes all inputs and outputs in the params. And each "function" inside takes the output variable to assign to as the final param.

Or we can use the weird verilog syntax instead of explicit gates where:

1. not = ~
2. and = &
3. or = |
4. xor = ^

and then you can combine them like this:

5. nand = ~&
6. nor = ~|
   etc..

So the above example would be:

```verilog
module example1 (x1, x2, s, f);
	input x1, x2, s;
	output f;
	assign f = (~s & x1) | (s & x2);
endmodule
```

This is equivalent to:

```ts
function example1(x1: boolean, x2: boolean, s: boolean): boolean {
  const k = !s;
  const g = k && x1;
  const h = s && x2;
  const f = g || h;
  return f;
}
```

Also you can specify wires and buses like this:

```verilog
module example2 (x1, x2, s, f);
	input [3:0] x1, x2; // 4 bit bus
	input s;
	output f;
	wire [3:0] g, h; // 4 bit wire
	assign g = (~s & x1);
	assign h = (s & x2);
	assign f = g | h;
endmodule
```
