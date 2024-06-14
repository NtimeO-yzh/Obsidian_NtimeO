# 和mem交互
## 1.1输入输出
```
// from mem
input wire[`MemBus] mem_rdata_i, // 内存输入数据
// to mem
output reg[`MemBus] mem_wdata_o, // 写内存数据
output reg[`MemAddrBus] mem_raddr_o, // 读内存地址
output reg[`MemAddrBus] mem_waddr_o, // 写内存地址
output wire mem_we_o, // 是否要写内存
output wire mem_req_o, // 请求访问内存标志
```

写内存地址和读内存地址只能活下来一个: tinyrisc.v中, 有`assign rib_ex_addr_o = (ex_mem_we_o == WriteEnable)? ex_mem_waddr_o: ex_mem_raddr_o;`, 意思就是, 通过写信号来判断到底这个地址是写地址还是读地址
### 1.1.2 输出信号的组成
**mem_wdata_o**
直接赋值输出, 没有一级筛选
**mem_raddr_o**
直接赋值输出, 没有一级筛选
**mem_waddr_o**
直接赋值输出, 没有一级筛选
**mem_we_o**

```
// 响应中断时不写内存
assign mem_we_o = (int_assert_i == `INT_ASSERT)? `WriteDisable: (mem_we || send_we);
```

意思是中断的时候不写内存, 不中断的时候写的是mem_we||send_we, *==因此需要控制mem_we和send_we只有一个为1==*
. 继续看下方的*执行块*, 发现we信号都是给mem_we赋值, 因此我们才写了一个send_we, 但好像没有从根本上解决同时写入的问题. 比如下面的执行块中, 就在`INST_TYPE_S`部分的we有一个enable.
 所以需要看hold的时候, pc会不会加1, 发现在*pc_reg.v*中有
 ```verilog
always @ (posedge clk) begin
,
// 复位

if (rst == `RstEnable || jtag_reset_flag_i == 1'b1) begin

pc_o <= `CpuResetAddr;

// 跳转

end else if (jump_flag_i == `JumpEnable) begin

pc_o <= jump_addr_i;

// 暂停

end else if (hold_flag_i >= `Hold_Pc) begin

pc_o <= pc_o;

// 地址加4

end else begin

pc_o <= pc_o + 4'h4;

end

end
```

说明要是不跳转, hold的时候, pc不动, 那我总感觉ex模块中hold和jump同时生效了, 回来一看确实是, 要是这样的话, jump优先级更高, 一旦jump, 我的指令取出来给到ex模块, 我直接下一周期的译码都错了啊.'(续)确实, 但此时进入else, 又不jump,, 只hold了..., 但是也会译出别的码, 假设别的码也需要访存, 那不就错了吗????[[Fishing/Second_Sem_Master_1/数字集成电路设计与实践/Project/分模块叙述/pc_reg#有关暂停流水线]]的部分保证了下一时刻进来的指令是$NOP$, 因此不会出错.
仔细看ex出去的jump, 给了ctrl, ctrl直接输出jump给pc_reg, jump的优先级又更高, 所以我只能先hold, 之后再
### 1.1.3 信号细节及连接

