# VL1
## 描述
制作一个四选一的多路选择器，要求输出定义上为线网类型
状态转换：  
  
d0    11  
d1    10  
d2    01  
d3    00  


**信号示意图：**

![|400](https://uploadfiles.nowcoder.com/images/20220720/0_1658303260872/3E9DFE8C955BEE03B3B7D8CA84F47CF3)

**波形示意图：**

![|400](https://uploadfiles.nowcoder.com/images/20220315/110_1647317901277/456F6192D239C94DA01BA49C3179CA1A)  

### 输入描述：

输入信号   d1,d2,d3,d4 sel  
类型 wire  

### 输出描述：

输出信号 mux_out  
类型  wire
## AVL1
```verilog
`timescale 1ns/1ns
module mux4_1(
input [1:0]d1,d2,d3,d0,
input [1:0]sel,
output[1:0]mux_out
);
//*************code***********//
    assign mux_out = sel[1] ? (sel[0] ? d0 : d1) : (sel[0] ? d2 : d3);
 
//*************code***********//
endmodule
```

# VL2
## 描述

**题目描述：**           

用verilog实现两个串联的异步复位的T触发器的逻辑，结构如图：

**信号示意图：**

![](https://uploadfiles.nowcoder.com/images/20220315/110_1647318016110/C6C3BAE69CC46580B2052396C38859BE)

**波形示意图：**

![](https://uploadfiles.nowcoder.com/images/20220315/110_1647318068736/48657953373D8219BA33FFD608A16019)  

  

### 输入描述：

  
输入信号   data, clk, rst  
类型 wire  
在testbench中，clk为周期5ns的时钟，rst为低电平复位  

### 输出描述：

输出信号 q   
类型  reg
## AVL2
```verilog
`timescale 1ns/1ns
module Tff_2 (
input wire data, clk, rst,
output reg q 
);
//*************code***********//
    reg tmp;
    always @(posedge clk, negedge rst) begin
        if(~rst) begin
            q <= 1'b0;
            tmp <= 1'b0;
        end else begin
            tmp <= data ? ~tmp : tmp;
            q <= tmp ? ~q : q;
        end
    end
 
//*************code***********//
endmodule
```
# VL3
## 描述

**题目描述：**           

现在需要对输入的32位数据进行奇偶校验,根据sel输出校验结果（1输出奇校验，0输出偶校验）

**信号示意图：**

![](https://uploadfiles.nowcoder.com/images/20220315/110_1647318173987/03BD93CCCE5C155084CA2CAEB9D5D73B)

**波形示意图：**

**![](https://uploadfiles.nowcoder.com/images/20220315/110_1647318183585/42CD08E9562FE1299E66878122EABE6E)  

  

### 输入描述：

输入信号   bus sel  
类型 wire  

### 输出描述：

输出信号   check          
类型  wire
## AVL3
```verilog
`timescale 1ns/1ns
module odd_sel(
input [31:0] bus,
input sel,
output check
);
//*************code***********//
    reg check;
    always @(*) begin
        if(^bus)
            check = sel;
        else
            check = ~sel;
    end
 
//*************code***********//
endmodule
```
# VL4
## 描述

**题目描述：**           

已知d为一个8位数，请在每个时钟周期分别输出该数乘1/3/7/8,并输出一个信号通知此时刻输入的d有效（d给出的信号的上升沿表示写入有效） 

**信号示意图：**

![](https://uploadfiles.nowcoder.com/images/20220315/110_1647318289820/1A8A63B78DE007D1361C622558D99390)

**波形示意图：**

![](https://uploadfiles.nowcoder.com/images/20220315/110_1647318319333/C0A9FB65EE4D95CB5989F19D204C8C3D)  

  

### 输入描述：

输入信号   d, clk, rst  
类型 wire  
在testbench中，clk为周期5ns的时钟，rst为低电平复位  

### 输出描述：

输出信号 input_grant    out  
类型  reg
## AVL4
```verilog
`timescale 1ns/1ns
module multi_sel(
input [7:0]d ,
input clk,
input rst,
output reg input_grant,
output reg [10:0]out
);
//*************code***********//
    reg [1:0] count;
    reg [7:0] din;
    always @(posedge clk, negedge rst) begin
        if(~rst) begin
            count <= 2'd0;
            din <= 8'd0;
            input_grant <= 1'b0;
            out <= 11'd0;
        end else begin
            case(count)
                2'b00: begin
                    din <= d;
                    input_grant <= 1'b1;
                    out <= d;
                    count <= count + 1'b1;
                end
                2'b01: begin
                    input_grant <= 1'd0;
                    out <= (din<<1) + din;
                    count <= count + 1'b1;
                end
                2'b10: begin
                    input_grant <= 1'b0;
                    out <= (din<<3) - din; 
                    count <= count + 1'b1;
                end
                2'b11: begin
                    input_grant <= 1'b0;
                    out <= (din<<3);
                    count <= 2'b00;
                end
            endcase
        end
    end

//*************code***********//
endmodule
```
# VL5
## 描述

**题目描述：**           

现在输入了一个压缩的16位数据，其实际上包含了四个数据[3:0][7:4][11:8][15:12],

现在请按照sel选择输出四个数据的相加结果,并输出valid_out信号（在不输出时候拉低）

0:   不输出且只有此时的输入有效  

1：输出[3:0]+[7:4]

2：输出[3:0]+[11:8]

3：输出[3:0]+[15:12]

**信号示意图：**

![](https://uploadfiles.nowcoder.com/images/20220315/110_1647324367041/4A059B7D0CE23BE806749CB7347973DE)

**波形示意图：**

**![](https://uploadfiles.nowcoder.com/images/20220315/110_1647324392924/C99F5E49906B9082CAE3EE6B236EE6D9)  
**

  

### 输入描述：

  
输入信号   d, clk, rst  
类型 wire  
在testbench中，clk为周期5ns的时钟，rst为低电平复位  

### 输出描述：

输出信号 validout    out  
类型  reg

## AVL5
```verilog
`timescale 1ns/1ns

module data_cal(
input clk,
input rst,
input [15:0]d,
input [1:0]sel,

output [4:0]out,
output validout
);
//*************code***********//
    reg[15:0] d_in;
    reg[4:0] out;
    reg validout;
    always @(posedge clk, negedge rst) begin
        if(~rst) begin
            out <= 5'd0;
            validout <= 5'd0;
        end else begin
            case(sel)
                2'b00: begin
                    validout <= 1'b0;
                    out <= 5'd0;
                    d_in <= d;
                end
                2'b01: begin
                    validout <= 1'b1;
                    out <= d_in[3:0] + d_in[7:4];
                end
                2'b10: begin
                    validout <= 1'b1;
                    out <= d_in[3:0] + d_in[11:8];
                end
                2'b11: begin
                    validout <= 1'b1;
                    out <= d_in[3:0] + d_in[15:12];
                end
            endcase
        end
    end

//*************code***********//
endmodule
```
# VL6
## 描述

根据指示信号select的不同，对输入信号a,b实现不同的运算。输入信号a,b为8bit有符号数，当select信号为0，输出a；当select信号为1，输出b；当select信号为2，输出a+b；当select信号为3，输出a-b.  
接口信号图如下：  
 ![](https://dev-private-public.oss-cn-hangzhou.aliyuncs.com/images/20220217/110_1645094518724/B873FAD020D7F2541D23F71D9E74C826)  
使用Verilog HDL实现以上功能并编写testbench验证。  
  

### 输入描述：

clk：系统时钟

rst_n：复位信号，低电平有效

a,b：8bit位宽的有符号数

select：2bit位宽的无符号数

### 输出描述：

c：9bit位宽的有符号数
## AVL6
```verilog
`timescale 1ns/1ns
module data_select(
	input clk,
	input rst_n,
	input signed[7:0]a,
	input signed[7:0]b,
	input [1:0]select,
	output reg signed [8:0]c
);
    always @(posedge clk, negedge rst_n) begin
        if(~rst_n)
            c <= 9'd0;
        else begin
            case(select)
                2'b00: c <= a;
                2'b01: c <= b;
                2'b10: c <= a+b;
                2'b11: c <= a-b;
            endcase
        end
    end
endmodule
```
# VL7
## 描述

根据输入信号a,b的大小关系，求解两个数的差值：输入信号a,b为8bit位宽的无符号数。如果a>b，则输出a-b，如果a≤b，则输出b-a。

接口信号图如下：  
 ![](https://dev-private-public.oss-cn-hangzhou.aliyuncs.com/images/20220217/110_1645097870529/7B1E6B1087B60D6B90C3D454A067DF06)  
使用Verilog HDL实现以上功能并编写testbench验证。  

  

### 输入描述：

clk：系统时钟

rst_n：复位信号，低电平有效

a,b：8bit位宽的无符号数

### 输出描述：

c：8bit位宽的无符号数
## AVL7
```verilog
`timescale 1ns/1ns
module data_minus(
	input clk,
	input rst_n,
	input [7:0]a,
	input [7:0]b,

	output  reg [8:0]c
);
    
    always @(posedge clk, negedge rst_n) begin
        if(~rst_n)
            c <= 9'd0;
        else begin
            if(a > b)
                c <= a - b;
            else
                c <= b - a;
        end
    end
endmodule
```
# VL8
## 描述

在某个module中包含了很多相似的连续赋值语句，请使用generata…for语句编写代码，替代该语句，要求不能改变原module的功能。  
使用Verilog HDL实现以上功能并编写testbench验证。  
  
module template_module(   
    input [7:0] data_in,  
    output [7:0] data_out  
);  
    assign data_out [0] = data_in [7];  
    assign data_out [1] = data_in [6];  
    assign data_out [2] = data_in [5];  
    assign data_out [3] = data_in [4];  
    assign data_out [4] = data_in [3];  
    assign data_out [5] = data_in [2];  
    assign data_out [6] = data_in [1];  
    assign data_out [7] = data_in [0];  
      
endmodule

  

  

### 输入描述：

data_in：8bit位宽的无符号数

### 输出描述：

data_out：8bit位宽的无符号数

## AVL8
```verilog
`timescale 1ns/1ns
module gen_for_module( 
    input [7:0] data_in,
    output [7:0] data_out
);
    genvar i;
    generate
        for(i=0; i<8; i=i+1) begin : gen
            assign data_out[7-i] = data_in[i];
        end
    endgenerate
 
endmodule
```
# VL9
## 描述

在数字芯片设计中，通常把完成特定功能且相对独立的代码编写成子模块，在需要的时候再在主模块中例化使用，以提高代码的可复用性和设计的层次性，方便后续的修改。

请编写一个子模块，将输入两个8bit位宽的变量data_a,data_b，并输出data_a,data_b之中较小的数。并在主模块中例化，实现输出三个8bit输入信号的最小值的功能。

子模块的信号接口图如下：

![](https://dev-private-public.oss-cn-hangzhou.aliyuncs.com/images/20220217/110_1645098525780/BB179DEC99C1B8B62E634496E1AA541F)  

  

主模块的信号接口图如下：

![](https://dev-private-public.oss-cn-hangzhou.aliyuncs.com/images/20220217/110_1645098529601/FB6D0B65196295F7F507AD80AD254FF8)  

使用Verilog HDL实现以上功能并编写testbench验证。

  

### 输入描述：

clk：系统时钟

rst_n：异步复位信号，低电平有效

a,b,c：8bit位宽的无符号数

### 输出描述：

d：8bit位宽的无符号数，表示a,b,c中的最小值
## AVL9
```verilog
`timescale 1ns/1ns
module main_mod(
	input clk,
	input rst_n,
	input [7:0]a,
	input [7:0]b,
	input [7:0]c,
	
	output [7:0]d
);

    wire [7:0] m, n;
    sub_mod sub1(clk, rst_n, a, b, m);
    sub_mod sub2(clk, rst_n, a, c, n);
    sub_mod sub3(clk, rst_n, m, n, d);

endmodule

module sub_mod(
    input clk,
    input rst_n,
    input [7:0]a,
    input [7:0]b,
    output reg [7:0]c
);
    always @(posedge clk, negedge rst_n) begin
        if(~rst_n)
            c <= 8'd0;
        else
            c <= a>b?b:a;
    end
    
endmodule
```
# VL10
## 描述

在数字芯片设计中，经常把实现特定功能的模块编写成函数，在需要的时候再在主模块中调用，以提高代码的复用性和提高设计的层次，分别后续的修改。

请用函数实现一个4bit数据大小端转换的功能。实现对两个不同的输入分别转换并输出。

程序的接口信号图如下：

![](https://uploadfiles.nowcoder.com/images/20220310/110_1646903148285/9299D3D3D3C8AED96206146854EA2C54)  

使用Verilog HDL实现以上功能并编写testbench验证。

### 输入描述：

clk：系统时钟

rst_n：异步复位信号，低电平有效

a,b：4bit位宽的无符号数

### 输出描述：

c,d：8bit位宽的无符号数
## AVL10
```verilog
`timescale 1ns/1ns
module function_mod(
	input clk,
	input rst_n,
	input [3:0]a,
	input [3:0]b,
	
	output [3:0]c,
	output [3:0]d
);
    reg [3:0]c;
    reg [3:0]d;

    function [3:0] bit_rev(
        input [3:0]bits
    );
        begin
        bit_rev[0] = bits[3];
        bit_rev[1] = bits[2];
        bit_rev[2] = bits[1];
        bit_rev[3] = bits[0];
        end
    endfunction
    
    always @(posedge clk, negedge clk, negedge rst_n) begin
        if(~rst_n) begin
            c <= 4'd0;
            d <= 4'd0;
        end else begin
            c <= bit_rev(a);
            d <= bit_rev(b);
        end
    end
endmodule
```