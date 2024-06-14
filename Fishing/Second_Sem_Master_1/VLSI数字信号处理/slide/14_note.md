# DSP系统的低功耗设计及可编程数字信号处理器
## 低功耗设计绪论
![[support/img/Pasted image 20240602121904.png]]
# 数字CMOS电路的功耗表示
![[support/img/Pasted image 20240602121925.png]]
蓝色的是静态功耗, 红色的和绿色的是动态功耗
## 2.1闲置功耗$I_{STANDBY}V_{DD}$(静态功耗)
- 闲置电流$I_{STANDBY}$是指电源到地的连续电流
![[support/img/Pasted image 20240602123038.png]]
> [!question] 三台输出out端引起下一级电荷泄露

## 2.2泄露功耗$I_{LEAKAGE}V_{DD}$(静态功耗)
来源有: 
- 反向偏置二极管漏电流
	- 引起的功耗正比于扩散区面积和泄漏电 流密度
- 亚阈值漏电流
	- 越来越突出的泄漏功耗来源
- Punchthrough 漏电流
- 栅极热电子注入效应漏电流
- 栅氧化层隧道效应漏电流
- Narrow-Width Effect漏电流

### 2.2.1反向偏置二极管电流
![[support/img/Pasted image 20240602124511.png|385]]
### 2.2.2亚阈值漏电
![[support/img/Pasted image 20240602124534.png|431]]
![[support/img/Pasted image 20240602124601.png|500]]
![[support/img/Pasted image 20240602124628.png|546]]

## 2.3 短路功耗
![[support/img/Pasted image 20240602124717.png|523]]
![[support/img/Pasted image 20240602124742.png|408]]
忘记了输出上升时间和下降时间是怎么影响的了, 具体参见[[Fishing/First_Sem_Master_1/数大/L05/L05_L06_CMOS 反相器_NOTE#(2)负载对短路电流的影响|L05_L06_CMOS 反相器_NOTE]]
# 功耗分析
# 功耗降低技术
# 小结