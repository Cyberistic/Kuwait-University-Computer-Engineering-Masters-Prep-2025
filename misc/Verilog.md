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

if statements like this:
(Notice how we're using "reg" to specify a changing variable)

```verilog
module example5 (x1, x2, s, f);
	input x1, x2, s;
	output f;
	reg f;
	always @(x1, x2, s)
		if (s == 0)
			f = x1;
		else
			f = x2;
endmodule
```

and switch cases:

```verilog
module mux16to1 (W, S16, f);
 	input [0:15] W;
	input [3:0] S16;
	output reg f;

	// Function that specifies a 4-to-1 multiplexer
 	function mux4to1;
		input [0:3] X;
		input [1:0] S4;
		case (S4)
			0: mux4to1 = X[0];
			1: mux4to1 = X[1];
			2: mux4to1 = X[2];
			3: mux4to1 = X[3];
			endcase
	endfunction
	always @(W, S16)
	case (S16[3:2])
		0: f = mux4to1 (W[0:3], S16[1:0]);
		1: f = mux4to1 (W[4:7], S16[1:0]);
		2: f = mux4to1 (W[8:11], S16[1:0]);
		3: f = mux4to1 (W[12:15], S16[1:0]);
	endcase
endmodule
```
