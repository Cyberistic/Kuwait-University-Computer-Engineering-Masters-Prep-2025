![[Multiplexer-example.png|center|400]]


The code for the multiplexer above is given by:
```verilog
module example1 (x1, x2, s, f); 
	input x1, x2, s; 
	output f; not (k, s); 
	and (g, k, x1); 
	and (h, s, x2); 
	or (f, g, h); 
endmodule
```

Notice how it takes all inputs and outputs in the params. And each "function" inside takes the output variable to assign to as the final param.

This is equivalent to: