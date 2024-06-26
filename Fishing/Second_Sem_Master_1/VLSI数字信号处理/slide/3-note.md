流水线与并行处理的优点：
1.  提高速度
2. 降低功耗
基本公式：CMOS电路的两个简化了的公式
1. 传播延时$$T_{pd}=\frac{C_{charge}V_{0}}{k(V_{0}-V_{t})^{2}}$$
2. 动态功耗$$P=C_{total}V_{0}^{2}f
$$其中：$V_0$为电源电压，$V_t$为MOS管阈值电压；$k$为跨导因子， $C_{charge}$为关键路径的负载电容；$C_{total}$为系统总负载电容；$f$为系统时钟频率
流水线用于降功耗(不用于提高速度)：仅做宏观估计
- 原始时序系统的功耗：$$P_{seq}=C_{total}V_{0}^{2}f$$
### 1.对M级流水线系统
由于被流水线锁存器分割，关键路径==**可能会减少为**==原始的1/M；关键路径的负载电容也减少为原始$C_{charge}$ 的1/M
- 注意：影响功耗的$C_{total}$不会减少，反而因插入锁存器而有所增加.
假设$T_{seq}$是原始时序的时钟周期, 如果流水线系统的时钟周期保持不变, 那么对电容$C_{charge}$充放电同样的时间内, 我们只需要对$\frac{C_{charge}}{M}$进行放电, 这就意味着我充电的电源, 也就是电源电压可以降低, 降低为$\beta V_{0}$**=={不是直接$÷M$}==**
这样根据动态功耗的公式, 我们可以得到$$P=C_{total}{\beta}^2V_{0}^{2}f$$
#### (1)求$\beta$
![[support/img/Pasted image 20240317092535.png|500]]
由于时钟周期不变, 并且时钟周期被设置为等于两个时序系统的最大延时, 因此以上两者相同, 因此有$$M\left(\beta V_{0}-V_{t}\right)^{2}=\beta(V_{0}-V_{t})^{2}$$
### 2.对L路并行系统
充电电容不变, 总电容增大L倍, 时钟周期增加$L$倍变为$LT_{seq}$, 这也就意味着对于$C_{charge}$的充电时间要增加$L$倍, 电压同样可以降低到$\beta V_{0}$, 因此求法一样![[support/img/Pasted image 20240317093107.png|400]]
带入公式得到并行处理系统的功耗为: $$\begin{aligned}
P_{par}& =\quad(LC_{tota},)(\beta V_{0})^{2}\frac{f}{L}  \\
&=\quad\beta^{2}C_{total}V_{0}^{2}f \\
&=\quad\beta^{2}P_{seq}
\end{aligned}$$
### 3.流水线和并行系统同时使用
- 流水线减少在一个时钟内的充/放电电容
- 并行处理允许增加对原始电容充/放电时钟周期
$$LT_{pd}=\frac{(C_{charge}/M)\beta V_{0}}{k(\beta V_{0}-V_{t})^{2}}=\frac{LC_{charge}V_{0}}{k(V_{0}-V_{t})^{2}}$$
因此求解$$M\overline{L}(\beta V_{0}-V_{t})^{2}=\beta(V_{0}-V_{t})^{2}$$
