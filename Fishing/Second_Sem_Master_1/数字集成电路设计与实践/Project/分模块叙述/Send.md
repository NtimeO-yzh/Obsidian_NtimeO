# 前期探索
## pc模块
本周期给了一个地址, 这个地址下一个周期就会在id_ex模块被译码
## id模块
译码成功, 给出了下一次的pc_jump,其实就是加1, 得写全,避免出现latch
## ex模块
接收到了send指令, 打开send模块(`send_start`)
- 由于ex模块执行访存, 因此`mem_we=0`, `address=30000004`, 此时的`mem_data`就是串口是否busy.
- send模块由于需要知道什么时候发送下一位学号, 因此,其需要知道`mem_data`是什么, 如果此时send=1, data和addres都符合条件, 那么下一时刻, addrss被制成3000000c, we=1,data=学号, 此时send模块检测到 data和addres不符合条件, 那么send出来的学号就不变, we重新回0, addrsee
> [!question] 如何保证, we=1, send的下一周期切换we=0之后, 这段时间内, uart可以接收到这个总线上的数. 写入数组合逻辑吗

## send
- 输入: 
	- send_start 等于1的时候激活send模块, 否则关闭send
	- mem_we 写内存信号, =1的时候表明是写往mem的, =0的时候表明是在读mem
	- mem_addr 写or读的地址是什么
	- 写or读的数据是什么
- 输出: 
	- ID: 八位的mem_data
	- implicit : address, 探讨一些输出的形式, 要不要输出一个address, 我感觉是可以的, 就这么设计, 当进来的地址符合\[uart_ctrl, 1, 0], 下一时刻ID输出8, address输出uart的八位串口.总感觉时序不太对, 检测到符合条件的这一时刻, wad改变, 但是这个上升沿的时候, uart写不进去, 因为w还没变, 下一时刻, 检测的时候wad是对的, 只不过此时的wad需要变了, 但是

---
遇到了问题: ex模块从mem只from mem : `input wire[MemBus] mem_rdata_i, // 内存输入数据`, 我们还需要此时的地址和mem_we信号, 还有那个奇怪的访存请求信号mem_req有没有什么岔劈
1. 有关mem_req信号请见[[Fishing/Second_Sem_Master_1/数字集成电路设计与实践/Project/分模块叙述/rib|rib]]
2. 整理完rib大概懂了, 也就是有下面一个框图, 我输出的是address和we,还有写data, 读入一个读data, 这个读data就是组合逻辑上边那仨那个输出直接得到的, 所以我send模块直接去连ex模块的address,we,和写进来的
3. `assign rib_ex_addr_o = (ex_mem_we_o == WriteEnable)? ex_mem_waddr_o: ex_mem_raddr_o;`
# 输入输出
![[Fishing/Second_Sem_Master_1/数字集成电路设计与实践/Project/pic/send_1.svg]]
![[support/img/Pasted image 20240526220523.png]]
