# Lecture-16 Blocking and Non-blocking statements ( PART-1)

## Procedural assignment

### Introduction and types

- These statements are used to update variables like "reg" , "integer" ,"real" , or "time". 
- Basically these are assignments in the procedural block like initial or always. 
- net type is not allowed on LHS of procedural assignment.
- This is different from continuous assignment (assign).
- net type does not store a value and continuosly driven. but in contrast, the value assigned to value remains unchanged unless a new value is assigned again.
- Two types are possible 
	1. Blocking "="
	2. Non - Blocking "<="

- LHS can be:
  1. a register type variable ( reg, integer, real, time)
  2. a bit of the variable (a[15])
  3. a part of the variable (a[31:26])
  4. concatenation of any of the above
- RHS can be net or reg type.

### Blocking Assignment 

- Syntax : variable = [delay or event control ] expression;
- These statements are executed in the order they are specified.
- One by one sequentially statements are executed. basically the statement executing 'blocks' the statements it follows.
- The target of an assignment is updated before next statement is executed in the procedural block. 
- They do not block execution of statements in other blocks.
- Recommended for modeling combinational logic, can be used for sequential too.

```
integer a,b,c;
initial 
	begin
		a= 10; b=20 ; c=15;
		a = b+c;
		b = a+5;
		c = a-b;
	end
```
```
a becomes 35
b becomes 40
c becomes -5
```
- statements execute in the order they appear.

```
module blocking_example;
	reg X,Y,Z;
	reg [31:0] A,b; integer sum;
	initial
		begin 
			X=1'b0; Y=1'b0 ; Z=1'b1;   // At t=0
			sum = 1;                   // At t=0
			A=31'b0 ; B= 31'habababab; // At t=0
			#5 A[5] = 1'b1;            // At t =5
			#10 B[31:29] = {X,Y,Z};    // At t=15
			sum  = sum + 5             // At t=15
		end
endmodule
```

```
module blocking_assgn;
integer a,b,c,d;
always @ (*)
	repeat (4)
	begin 
		#5 a = b+c;
		#5 d = a-3;
		#5 b = d+10;
		#5 c = c+1;
	end

initial 
	begin 
	 $monitor ($time, "a=%4d, b=%4d, c=%4d, d=%4d" , a,b,c,d);
	 a= 30; b=20 ;c=15 ; d=5;
	 #100 $finish;
	 end
endmodule 
// Do simulate and see the output results.
```
### Non - Blocking assignment

- Syntax : variable <= [delay or event control ] expression;
- The statements inside the block can execute concurrently.
- Basically, this assignment statement does not block execution of statements in the block.
- The assignment to the target is scheduled at the end of the procedural block. The statements all execute together.
- These statements are used for modelling sequential logic.

```
integer a,b,c;
initial
	begin 
		a=10; b=20; c=15;
	end
initial
	begin 
		a <= #5 b+c;
		b <= #5 a+5;
		c<= #5 a-b;
	end
	
// a becomes 35 at t=5
// b becomes 15 at t=5
// c becomes -10 at t=5
// All RHS expressions are calculated according to the prev values not according to the new values. They are assigned concurrently at t=5.
```

```
always @ (posedge clk)
	begin
		a<= b & c;
		b<= a^d;
		c <= a|b;
	end
// all assignments take place synchronously at the rising edge of the clock.
```

- Recommended for modelling synchronous circuits with a clock.
#### Swapping values of two variables "a " and "b"



