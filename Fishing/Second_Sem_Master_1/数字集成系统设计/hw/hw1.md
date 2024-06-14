作业要求：1.4，1.9，1.16，3.11，3.27（VHDL/Verilog，并在作业中添加仿真波形）
![[support/img/Pasted image 20240304231339.png]]

![[support/img/Pasted image 20240304231400.png]]

![[support/img/Pasted image 20240304231415.png]]

![[support/img/Pasted image 20240304231425.png]]

![[support/img/Pasted image 20240304231510.png]]

![[support/img/Pasted image 20240304231617.png]]
``` verilog
`timescale 1ns/1ps
`timescale 1us / 1ns

module tb;

reg x;
reg y;
reg z;
wire o;

hw1_yzh uut(x,y,z,o);


initial 
begin
x = 0;
#40 x = 1;
end

initial 
begin
y = 0;
#20 y = 1;
#20 y = 0;
#20 y = 1;
end

initial 
begin
z = 0;
#10 z = 1;
#10 z = 0;
#10 z = 1;
#10 z = 0;
#10 z = 1;
#10 z = 0;
#10 z = 1;
end

initial
#100 $finish;

endmodule
```

``` verilog
`timescale 1ns/1ps

module hw1_yzh(
  input x,y,z,
  output reg o
);
always @* begin
  if (x == 1) o = y;
  else o = z;
end

endmodule
```
![[support/img/Pasted image 20240305205056.png]]