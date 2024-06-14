# 输入输出
## 1.1主设备交互
包括master0~3四个主设备, 
![[support/img/Pasted image 20240523001931.png]]
有四个输入
- 主设备是否访问rib
- 主设备是写还是读
- 主设备写或读的地址
- 主设备写的数据
一个输出
- 主设备读到的数据
## 1.2 从设备交互
![[support/img/Pasted image 20240523002126.png]]
一个输入
- 从从设备读到的数据
三个输出
- 往从设备读还是写
- 往从设备的读或写的地址
- 往从设备写的数据
## 1.3 信号连接
实例: rib.v
例化: riscv_soc.v
- master_0 : ex![[support/img/Pasted image 20240524010510.png|L|254]]
- master_1 :pc_reg![[support/img/Pasted image 20240524010557.png|L|206]]可以发现地址和读进来的数据是不固定的, 对应pc和对应的指令. 其余的三个是固定的
- master_2 :jtag![[support/img/Pasted image 20240524010826.png|L|214]]
- master_3: uart_debug![[support/img/Pasted image 20240524010919.png|L|180]]
# 基本逻辑
## 2.1 主设备仲裁
![[support/img/Pasted image 20240523002326.png]]
根据每个主设备发来的req信号, 仲裁确定到底是哪个主设备可以和接下来的从设备交互
*==3>0>2>1==*
## 2.2 从设备交互
根据仲裁结果, 给grant赋值, case grant其实也就是选中了主设备到底是谁, 接下来根据`m0_addr_i`的高四位决定到底和哪个从设备交互.
## 2.3 主从数据通路
1.  m0_we_i → s0_we_o : 主设备决定了到底是写还是读从设备
2. `s0_addr_o = {{4'h0}, {m0_addr_i[27:0]}};`: 访问从设备的地址也由主设备的输入直接连上
3.  m0_data_i →s0_data_o 主设备写的数据作为写往从设备的数据
4.  s1_data_i → m0_data_o : 从从设备读到的数据作为主设备读到的数据
# 经验
1. 是一个组合逻辑
2. 根据req和mem_addr的高四位分别决定把which master和which slave的四根线直接相连