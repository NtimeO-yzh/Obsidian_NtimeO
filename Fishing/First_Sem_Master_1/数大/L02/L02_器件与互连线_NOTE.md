[[Fishing/First_Sem_Master_1/数大/数大|数大]]
## 壹---器件
### 一、二极管
#### 1.二极管简介
存在一个内建电势：$\phi_{0}=\phi_{T}\ln\biggl[\frac{N_{A}N_{D}}{n_{t}^{2}}\biggr]$

- 热电势$\phi_{T}=\frac{kT}{q}=26\mathrm{mV}\quad(300\mathrm{K}\mathrm{时})$
- $N_A、N_D$分别是P型材料和N型材料的掺杂浓度
- $n_t$为半导体纯样品的固有载流子浓度，300K是Si的$n_t$值约为$1.5\times10^{10}{cm}^{-3}$

#### 2.静态特性

##### （1） 理想的二极管方程

![[Pasted image 20231027150802.png]]

描述二极管的电流: $I_D=I_s(e^{V_D/\phi_T}-1)$. ^1
- $I_s$二极管的饱和电流，是一个常数值。与二极管的面积成正比，并其余掺杂水平和中性区的宽度有关没大多数情况下是根据经验确定的。对于一个典型的硅pn结，饱和电流通常在$10^{-17}A/{{\mu}m}^2$。实际的反向电流（漏电流）要比这个大三个数量级。
- $\phi_T$是热电势，室温下等于**26mV。**
##### （2） 人工分析模型
![[Pasted image 20231027150848.png|#center|423]]

固定压降0.6～0.8V。
#### 3.动态特性

这里讲的是对于偏置情况改变的响应，决定了器件的最大工作速度。在二极管之中，动态特性很大程度上取决于空间电荷区能够移动的多快。而在数字电路之中，二极管都是反向偏置的。

##### （1）耗尽区电容

理想模型之中，耗尽区没有可移动的载流子，耗尽区的电荷取决于不能移动的施主和受主离子。正偏置的时候，势垒降低，耗尽区变窄，电荷变少。反向偏置就是反之。在宏观上，就像是一个电容。

1. 耗尽区电荷（正向偏置）
   
    $$ Q_{j}=A_{D}\sqrt{(2\varepsilon_{si}q\frac{N_{A}N_{D}}{N_{A}+N_{D}})(\phi_{0}-V_{D})} $$
    
2. 耗尽区宽度
   
    $$ W_{j}=W_{2}-W_{1}=\sqrt{\Big(\frac{2\varepsilon_{si}N_{A}+N_{D}}{q}\frac{N_{A}+N_{D}}{N_{A}N_{D}}\Big)(\phi_{0}-V_{D})} $$
    
3. 最大电场
   
    $$ E_{j}=\sqrt{\Big(\frac{2q}{\varepsilon_{si}N_{A}+N_{D}}\Big)(\phi_{0}-V_{D})} $$
    
    - $\varepsilon_{si}$是硅的介电常数，大概是真空介电常数的11.7倍，约为$1.053\times10^{-10} F/m$。
4. 等效电容
    $$ \begin{aligned}C_{j}&=\frac{dQ_{j}}{dV_{D}}=A_{D}\sqrt{\left(\frac{\varepsilon_{si}q}{2}\frac{N_{A}N_{D}}{N_{A}+N_{D}}\right)(\phi_{0}-V_{D})^{-1}}\\&=\frac{C_{j0}}{\sqrt{1-V_{D}/\phi_{0}}}\end{aligned} $$
    
    其中$$C_{j0}=A_{D}\sqrt{\left(\frac{\varepsilon_{si}q}{2}\frac{N_{A}N_{D}}{N_{A}+N_{D}}\right)\phi_{0}^{-1}}$$
##### （2）大信号耗尽区电容

3.8图告诉我们啊：电容取决于外加偏置电压。在数字电路之中，工作电压的变化范围很大很快，希望用一个**等效的线性电容$C_{eq}$来替代和电压有关的非线性电容$C_{j0}$**。

- 定义$C_{eq}$：对于一个给定的电压摆幅，$V_{high}$→$V_{low}$，电荷的改变量和非线性模型一样滴。====

因此有了

$$ C_{eq}=\frac{\Delta Q_{j}}{\Delta V_{D}}=\frac{Q_{j}(V_{high})-Q_{j}(V_{tow})}{V_{high}-V_{low}}=K_{eq}C_{j0} $$

其中$K_{eq}$为

$$ K_{eq}=\frac{-\phi_{0}^{m}}{(V_{high}-V_{low})(1-m)}[(\phi_{0}-V_{high})^{1-m}-(\phi_{0}-V_{low})^{1-m}] $$

#### 4.实际二极管的二次效应

- 电流比理想的要小：中性区总会有一些电压降，所以外加偏置电压不会全部出现在结两端。（中性区的电阻比较小，1～100欧）。模拟的时候，n和p区接触点上串联一个电阻。
- 反向偏置电压太大的时候会出现雪崩击穿，可以恢复，但是不要一直击穿。（高掺杂的情况下会出现齐纳击穿） 
- ![[Pasted image 20231027151030.png| #center|500]]
- 温度的影响:
	- 1出现在电流方程指数项上的热电势$\phi_t$与温度成线性关系。[[#^1]]可以观察出,$\phi_t$增加使电流下降。
	- 2.饱和电流$I_s$也与温度有关，因为热平衡时的载流子浓度随温度的升高面增加。理论上，<font color="#ff0000">温度上升5°C 跑和电流约增加一倍</font>。实验测得的反向电流每 8°C增加一倍。
	这一双重关系对数字电路的工作有很大影响．首先，温度上升, 电流（从而功耗）会显著加大。例如
1. 对于一个在300°K 时0.7V的正偏置，温度每上升1°C电流约增加6%，每上升12°C 电流会翻一倍。当电流值固定时，温度每上开1°C 二极管的电压V，下降2mV
2. 第二，集成电路高度依赖于把反向偏置的二极管作为**绝缘体**来用。温度的升高会引起漏电流的增加，因而降低绝缘的质量。
(功耗和绝缘质量)
#### 5.二极管的 SPICE 模型
![[Pasted image 20231028000426.png|#center|300]]
##### (1)稳态特性
稳态特性⽤⼀个⾮线性的电流源$I_D$来模拟，它对理想的⼆极管公式做了⼀些修改：$$I_{D}=I_{S}(\mathrm{e}^{V_{D}/n\cdot\phi_{T}}-1)$$
- **n称为发射系数**，对最普通的⼆极管它等于1，但对另⼀些它可以⽐1 ⼤些。
- **电阻R**,⽤来模拟结两边中性区的串联电阻。在电流较⼤时，该电阻使⼆极管的内部电压 V不同于外加电压，因此所产⽣的电流⽐从⼀个理想⼆极管公式中预期的电流要低。
##### (2)动态特性
![[support/img/Pasted image 20231028002214.png|500]]
`动态特性使用非线性电容`$C_D$来模拟,它同时考虑了⼆极管中两个不同的**电荷存储效 应**：
1. 空间(或耗尽区)电荷
2. <span style="background:#affad1">过剩的少⼦电荷(只在正向偏置条件下)</span>
非线性电容$C_D$的定义如下:$$C_{D}=\frac{C_{j0}}{(1-V_{D}/\phi_{0})^{m}}+\frac{\tau_{T}I_{S}}{\phi_{T}}\mathrm{e}^{V_{D}/n\phi_{T}}$$

### 二、MOSFET

从数字设计⾓度来看，它的主要优点是**作为⼀个开关**它有良好的性能以及**引起的寄⽣效应很⼩**。并且制造工艺简单,有高密度的集成。
- 叙述步骤:概述-->静态-->动态-->spice
#### 1.MOS 晶体管简介
![[support/img/Pasted image 20231028132354.png|286]]
MOSFET 是一个四端器件. 在 G 的电压决定了 D S 之间的电流. Body 是第四个端口,作用相对小一些.
- 分类:
	1. PMOS:n 衬底和 p 的源区和漏区
	2. NMOS:p 衬底和 n 的源区和漏区
	- 注意 PN 结的<span style="background:#affad1">电流是电子和空穴构成的</span>,但是 MOS 只能由一种构成.
#### 2.静态下的 MOSFET
集中讨论 NMOS
##### (1)阈值电压
1. **$V_{GS}=0$, B G S 均接地.**
	此时漏和源由<font color="#c00000">背靠背的pn 结(衬底-源及衬底-漏)</font>所连。在上述情况下，两个结均具有0v的偏置，因此可以被考虑为断开，这导致了漏和源之间极高的电阻。
2. $V_{GS}>0$,  **B S 均接地.**
   此时G 和 B 相当于电容器的两个极板. SiO2 为电介质. 由于栅压＞0,因此正电荷下去,负电荷上来.<span style="background:#affad1">起初,负电荷下去是通过排斥可移动的空穴形成的.</span>于是在 G 下面形成了一个<font color="#c00000">耗尽区</font>,<span style="background:#affad1">它与 pn 结中的耗尽区十分像</span>.
   其宽度和储存的电荷分别为$W_{d}=\sqrt{\frac{2\varepsilon_{si}\phi}{qN_{A}}}$, ${Q}_{d}=-\sqrt{2qN_{A}\varepsilon_{si}\phi}$. 
	其中,$N_A$ 为衬底掺杂,$\phi$为耗尽层上的电压,也就是 sio2 和 si 边界上的电势
	- **<font color="#ffc000">临界情况</font>**: 电压等于两倍费米势$\phi_F$的时候.此时,发生强反型现象.$p$型衬底的(<u>衬底的费米势,注意表述</u>)的费米势为:$$\phi_{F}=-\phi_{T}\mathbf{ln}\Big(\frac{N_{A}}{n_{i}}\Big)$$其典型值为$0.3V$.
	- 进一步提高$V_G$提高的是反型层中的电子数量,而不是耗尽层的宽度.如何做到的呢?<font color="#ff0000">从掺杂的 D 和 S 区拽出来的.</font>拽的足够多的时候,就形成了n 型导电沟道.
	当存在反型层时,耗尽区的电荷是固定:$$Q_{B0}=-\sqrt{2qN_A\epsilon_{si}|2\phi_F|}$$
3. $V_{GS}>0,V_{SB}>0$,  **B S 均接地.**
	它使强反型所要求的表面电势增加并且变为$|2\phi_{F}+V_{SB}|$,对应的耗尽区的电荷变为$$Q_{B}=-\sqrt{2qN_{A}\varepsilon_{si}(\left|(-2)\phi_{F}+V_{SB}\right|)}$$<font color="#ff0000">我们称发生强反型时对应的</font>$V_{GS}$<font color="#ff0000">的值为阈值电压</font>$V_T$.
	<span style="background:#d3f8b6">从前⾯的讨论中可以清楚地看到，源-体电压$V_s$对阈值也有影响。</span>与其依赖于⼀个复杂的(并且多半也是不精确的)阈值解析表达式，还不如依靠⼀个经验参数$V_{T0}$，它是$V_{SB}=0$时的國值电压，并且主要与制造⼯艺有关。于是在不同体偏置条件下的阈值电压可由下式确定：$$V_{T}=V_{T0}+\gamma(\sqrt{|(-2)\phi_{F}+V_{SB}|}-\sqrt{|2\phi_{F}|})$$
	- $\gamma$<font color="#ff0000">为衬底偏执效应系数</font>
	以下$\phi_F=0.6V$, $\gamma = 0.4 V^{0.5}$,注意$V_{SB}$<span style="background:#d3f8b6">必须大于 0.6,否则二极管SB就会正偏.</span>
	![[support/img/Pasted image 20231030085448.png|300]]
##### (2)电阻工作区
4. $V_{GS}>V_T, V_{DS}>0$但不算太大,B接地,S电压由$V_T$来表征
	- 目标:求 $I_D(V_{GS}, V_{DS})$
	- 推导过程
	1. 设在沿沟道长度方向的$x$处,相对于沟道起点 S 的电压为$V{(x)}$. 因此栅极相对于这一$x$点的电压差为$V_{GS}-V(x)$.假设整个沟道的$V_{GS}-V(x)$都超过了阈值电压,那么在$x$点处感应出的每单位面积的沟道电荷可以用一下方法计算$$Q_{i}(x)=-C_{ox}[V_{GS}-V(x)-V_{T}] \tag1$$
	 $C_{ox}$为栅氧的单位面积电容$C_{ox}=\frac{\varepsilon_{ox}}{t_{ox}}$ , 其中e${\mathcal E}_{ox}=3.97\times{\mathcal E}_{o}=3.5\times10^{-11}\mathrm{F/m}$为氧化层的介电常数,$t_{ox}$为氧化层厚度. ^8c3f2e


> [!note] 一些注意的点
> 1. 这里的**每单位面积的沟道电荷**是指的从上往下看,每单位面积的电荷,就是已经在厚度方向上做乘法了. 要不就是说每单位体积内的了. 
> 2. 在现代工艺之中,$t_{ox}$的值大概是 10nm,很容易被击穿.
> 3. 对于一个 5nm 厚的氧化层,相当于一个 $7fF/μm^2$ 

 
2. 在微观的意义上计算电流.$$I_{D}=-\upsilon_{n}(x)\cdot Q_{i}(x)\cdot W\tag2$$
其中, $W$是垂直于电流方向(垂直于直面往里)的**沟道宽度**. 由于$Q_{i}(x)$和$W$的乘积就代表了垂直于l(或者说电流)方向上的总面积的电荷, 之后再与$\upsilon_{n}(x)$相乘就是电流了,在整过道上就是一个常数了.(<span style="background:#d3f8b6">课本上说的:由于电荷守恒，所以它在沟道的全长上是⼀个常数。</span>)
> [!question]
> 不懂这里的常数是啥意思,你要说电流在整个沟道上的分布是一个常数还是电流与 L 的乘积是一个常数啊,我感觉后者是对的,但没啥用啊

   其中电子的速度通过一个称为**迁移率**的参数$μ_n$与电场相关和晶体结构相关,一般简化为$$\upsilon_{n}=-\mu_{n}\xi(x)=\mu_{n}\frac{\mathrm{d}V}{\mathrm{d}x}\tag3$$
	联立以上的式子(1)(2)(3)就可以得到$$I_{D}\mathrm{d}x=\mu_{n}C_{ox}W(V_{GS}-V-V_{T})\mathrm{d}V$$在整个沟道 L 上积分得到晶体管的电压和电流关系,即$$I_{D}=k_{n}^{\prime}\frac{W}{L}\bigg[(V_{GS}-V_{T})V_{DS}-\frac{V_{DS}}{2}^{2}\bigg]=k_{n}\bigg[(V_{GS}-V_{T})V_{DS}-\frac{V_{DS}}{2}^{2}\bigg]\tag4$$
	其中,$k_{n}^{\prime}$称为工艺跨导参数, $k_{n}^{\prime}=\mu_{n}C_{ox}=\frac{\mu_{n}\varepsilon_{ox}}{t_{ox}}$
		$k_{n}^{\prime}\cdot W/L$ 为这一期间的增益因子$k_n=k_{n}^{\prime}\cdot W/L$.
	注意,当$V_{DS}$较小的时候, $I_D$和$V_{DS}$成线性关系.因此称为<font color="#ff0000">电阻区或者线性区</font>.它的⼀个主要特点是它在源区 和漏区之间表现为**⼀条连续的导电沟道**。
> [!note]
> 式(4）中的参数以和L代表晶体管的有效沟宽和沟长。这些值与版因上所画的尺⼨不同，这是由于源区与漏区的横向扩散作⽤(对L)和隔离场氧被侵独的影响(对W）等造成的。 在本书的其余部分，W 和 L将总是代表有效尺⼨，⽽下标d则⽤来说明是所画的尺⼨。

##### (3)饱和区
5. $V_{GS}>V_T, V_{DS}>0$且继续上升,B接地,S电压由$V_T$来表征
   当漏源电压值进⼀步提⾼时，<font color="#ff0000">沿全⻓沟道电压都⼤于阈值的假设就不再成⽴，</font>这通常发⽣在当$V_{GS}-V(x)<V_{T}$时。。在这⼀点上，**被感应的电荷为零**，⽽导电沟道消失或者说它已被**夹断**(pinch off）。显然,由公式(1)为使这⼀现象发⽣，主要是在漏区要满⾜夹断条件，也就是：$$V_{GS}-V_{DS}\leqslant V_{T}$$![[support/img/Pasted image 20231031143357.png|400]]
   > [!summary] 理解夹断
> 	这里的$V_{DS}$就是公式1里边的$V(x)$. 形成反型层的第二步不是在重掺杂的区域拽出来电子嘛,你 D 的电压提高了之后不就拽不出来的吗.
> 	那这一步主要影响的是哪个公式呢,是(4).因为不可以在全长 L 上对式子进行积分了.

<font color="#ff0000">由于在感应形成的沟道上的电压差(从夹断点到源)保持固定在</font>$V_{GS}-V_T$,上，<font color="#ff0000">结果使电流保持为常数(或饱和）.</font>⽤$V_{GS}-V_T$,代 替式（4)中的$V_{DS}$，得到了饱和模式时的漏极电流$$I_{D}=\frac{k_{n}}{2}\frac{W}{L}(V_{GS}-V_{T})^{2}\tag5$$注意，就⼀阶近似⽽⾔，漏电流不再是$V_{DS}$的两数。还要注意的是，漏电流与控制电压$V_{GS}$之间存在平⽅关系
> [!question] 电压差一定怎么得到的电流一定?
> 我只能理解说,电压$V_{DS}$升高了之后,虽然电压(推动力)变大了,但是整个沟道里边的电子不会增加了(感觉甚至减少了),所以电流不会增加特别多.但要是直接说夹断点的 V(x)是不变的,所以电流不变,那表示难以理解.V(x)不变的时候,总电荷不变?总电荷不变 $V_{DS}$增加的话那也应该电流加大啊.
> 
###### A:沟道⻓度调制
饱和模式下晶体管的作⽤像⼀个理想的电流源——在D与S间的电流相对于电压$V_{DS}$是恒定的.但这并不完全正确。导电沟道的有效⻓度实际上由所加的$V_{DS}$调制：增加$V_{DS}$将使漏结的耗尽区加⼤，从⽽缩短了有效沟道的⻓度。正如从(5)中可以看到的，当⻓度⼯滅⼩时电流会增加。因此关于 MOS 管电流的⼀个更精确的描述为:$$I_{D}=I_{D}{}^{\prime}(1+\lambda V_{DS})$$
其中,$I_{D}{}^{\prime}$是(5)推导出的表达式.$\lambda$是沟长调制系数,是一个经验参数.
	⼀般来说$\lambda$与沟长成反⽐。在沟长较短的晶体管中，漏结耗尽区占了沟道的较⼤部分，沟道的调制效应也更加显著。<span style="background:#affad1">因此在需要⾼阻抗的电沆源时 建议采⽤⻓沟道晶体管。</span>
###### B:速度饱和假设
沟道⾮常短的品体管（称为**短沟器件**）的特性与上⾯介绍的电阻⼯作区和饱和区的模型有很 ⼤的不同。这⼀差别的主要原因就是**速度饱和效应**。**(3)式**表明载流⼦的速度正⽐于电场，这⼀关系与电场强度值的⼤⼩⽆关.换⾔之，<font color="#ff0000">载流⼦的迁移率是⼀个常数</font>。然⽽在（⽔平⽅向）电场强度很⾼的情况下，载流⼦不再符合这⼀线性模型。当沿沟道的电场达到某临界值$\xi_c$.时，<font color="#ff0000">载流⼦的速度将由于<font color="#ff0000">散射效应</font>(即载流⼦间的碰撞)⽽趋于饱和</font>。意思就是:电场(推动力)再怎么增加,但由于碰撞散射,速度不增加了
![[support/img/Pasted image 20231031105702.png|300]]
<span style="background:#affad1">电⼦和空⽳的饱和速度⼤致相同，</span>即$10^5m/s$.速度饱和发⽣时的临界电场强度,取决于:**摻杂浓度和外加的垂直电场强度**。对于电⼦，临界电场在 1~5 V/pm 之问。<span style="background:#affad1">在n型硅中的空⽳需要稍⾼⼀些的电场才能达到饱和，因此在 PMOS 晶体管中速度饱和效应不太显著。</span>
> [!question] 电子和空穴还不一样?

**a:假设**
图3.17 所画出的速度与电场的关系可以⽤带有条件的下式来⼤致近似：$$v=\begin{cases}\frac{\mu_{n}\xi}{1+\xi/\xi_{c}},&\xi\leqslant\xi_{c}\\v_{sat},&\xi\geqslant\xi_{c}\end{cases}$$
为了使这两个区域间连续，要求$$\xi_{_{c}}=2v_{_{sat}}/\mu_{_{n}}$$
**b:电阻工作区**
如果⽤修正后的速度公式重新对式(1)和式(2)求值，就得到了在*电阻⼯作区*漏极电流的修正式：$$\begin{aligned}I_D&=\frac{\mu_nC_{ox}}{1+(V_{DS}/\xi_cL)}\Big[(V_{GS}-V_T)V_{DS}-\frac{V_{DS}}{2}\Big]\\\\&=\mu_nC_{ox}\Big[(V_{GS}-V_T)V_{DS}-\frac{{V_{DS}}^2}{2}\Big]\kappa(V_{DS})\end{aligned}\tag6$$
- 其中,$\kappa(V_{DS})$考虑速度饱和的程度,其定义为: $$\kappa(V)=\frac{1}{1+(V/\xi_{c}L)}\tag7$$.
	- $V_{DS}/L$可以被解释为在沟道中的平均电场。
	- 在长沟器件(L值较⼤）或$V_{DS}$值较⼩的情况下，$\kappa$接近1，于是式(6)就简化为通常情况下*电阻⼯作模式*的电流公式。对于短沟器件，$\kappa$⼩于1，这意味着所产⽣的电流将⼩于通常所预期的值。
	**c:速度饱和**
	*当增加漏源电压到$V_{DSAT}$时*，沟道中的电场最终达到了临界值$\xi_{_{c}}$，于是在漏端的载流⼦出现速度饱和。首先计算临界时的电流$$\begin{aligned}I_{DSAT}&=\upsilon_{sat}C_{ox}W(V_{GT}-V_{DSAT})\\\\&I_{DSAT}=\kappa(V_{DSAT})\mu_{n}C_{ox}\frac{W}{L}\biggl[V_{GT}V_{DSAT}-\frac{{V_{DSAT}}^2}{2}\biggr]\end{aligned}\tag8$$式子中$V_{GT}=V_{GS}-V_T$,在合并有关项并简化之后得到电压$$V_{DSAT}=\kappa(V_{GT})V_{GT}\tag9$$进⼀步增加DS电压并,载流子速度不再增加,因此不能产⽣更多的电流(就⼀阶近似⽽⾔)，即晶体管的电流饱和在$I_{DSAT}$上。
> [!note] 推导见 Page1~2


> [!summary] 我说几个点
> 1. 这里速度饱和是由于载流子速度达到了一定值呀,之前长沟是因为沟道夹断了.所以你不能用$V_{GT}$代替$V_{DS}$
> 2. 公式(9)并没有什么现实的意义,就是可以量化的看一下
> 3. 这里的思路是,先推导电流,之后通过(9)看一下$V_{DSAT}$和$V_{GT}$的关系.因为这个$V_{GT}$就是之前长沟饱和的$V_{DS}$,所以有了以下课本上的话

![[support/img/Pasted image 20231031115028.png]]
![[support/img/Pasted image 20231031115400.png|inlR|200]]以上这些公式都忽略了⼀个事实，即进⼀步增加$V_{DS}$会使更⼤部分的沟道变成速度饱和。从建⽴模型的⻆度来看，它仿佛就像是有效沟道长度随着$V_{DS}$的增加⽽缩短，其效果⾮常类似于沟⻓调制.所引起的电流增加可以很容易地通过引⼈另⼀乘数项$(1+\lambda\times V_{DS})$来考虑.



**d:手工分析**
简化假设
1. 速度饱和即刻发⽣在$\xi_{_{c}}$$$\upsilon=\begin{cases}\mu_n\xi,&\xi\le\xi_c\\\upsilon_{sat}=\mu_n\xi_c,&\xi\geqslant\xi_c\end{cases}$$
2. 达到临界电场且出;现饱和速度时的漏源电压$V_{DSAT}$是⼀个常数，并可以近似为$$V_{DSAT}=L\xi_{c}=\frac{L\upsilon_{sat}}{\mu_{n}}$$
<font color="#ff0000">可以得出这⼀假设对于较⼤的</font>$V_{GT}$<font color="#ff0000">值是合理的</font>.在这些条件下，电阻⼯作区的电流式与⻓沟模型相⽐保持不变。⼀旦达到$V_{DSAT}$，电流就即刻饱和。此时的$I_{DSAT}$值可以通过把饱和电压代⼈电阻区的电流式得到：$$\begin{aligned}
I_{DSAT}& =I_{D}(V_{DS}=V_{DSAT})  \\
&=\mu_{n}C_{ox}\frac{W}{L}\Big((V_{GS}-V_{T})V_{DSAT}-\frac{V_{DSAT}^{2}}{2}\Big) \\
&=\upsilon_{sat}C_{ox}W\biggl(V_{GS}-V_{T}-\frac{V_{DSAT}}{2}\biggr)
\end{aligned}$$
这⼀模型完全是⼀阶近似并且是经验模型。这<span style="background:#affad1">种简化的速度模型在线性区和速度饱和区之间的过渡区上会引起明显的偏差。</span>
![[support/img/Pasted image 20231031125457.png#center|400]]
![[support/img/Pasted image 20231031125611.png#center|400]]

---

省略了课本上的:漏极电流与电压的关系图、亚阈值情形

---
##### (4)手工分析模型
###### A 第一次简化
这⼀模型把晶体管表示成⼀个电流源(⻅图3.23)。它的值由该图给出。因3.24表示了这个通⽤模型如何把品体管的整个⼯作空间划分成三个区城：线性区、速度 饱和区 及饱和区，并且标明了在这些区域之间的边界。⽐较重要的是，模型应当在**最有关的区城内匹配最好**。<span style="background:#affad1">这在数字电路中就是</span>$V_{DS}$和$V_{GS}$<span style="background:#affad1">较⾼的区域</span>.
![[support/img/Pasted image 20231031131221.png#center|]]
![[support/img/Pasted image 20231031131350.png|300]]
这个图一定要知道那个*min*的式子和为啥*饱和区和速度饱和区能在一个图上*.$V_{DSAT}和 V_{GT}$哪个小就说明了到底是长沟还是短沟,只有前者小的时候,或者说 $V_{GS}$大的时候,才会有速度饱和,否则就没有.(可以想 GS 之间的电压会提高载流子浓度,从而容易发生速度饱和,太低的时候就没有,饱和就是普通饱和.)DS 之间电压的取值说明了到底是不是线性区,至于是什么饱和,就看那俩的比较值了.所以图中分成了三个区域.
###### B第二次简化
它以在⼤多数数宇设计中的基本假设作为基础，即晶体管只不过是⼀个开关，它有⽆穷⼤的“断开”电阻和有限的“导通”电阻（⻅图3.26）。
![[support/img/Pasted image 20231031134411.png|300]]
这⼀模型的主要问题是$R_{on}$是时变的、⾮线性的并取决于晶体管的⼯作点。对此⼀个合理的⽅法是采⽤在所关⼼ 的⼯作区城上电阻的平均值，或过⾄更为简单地，采⽤在瞬态过程的起点和终点处两个电阻的平均值。后⼀个假设能很好地成⽴，只要电阻在进⾏平均的问隔范围内并不出现任何⾼度的⾮线性。与此相应，有：$$\begin{aligned}
\text{Req}& =\mathrm{average}_{t=t_{1}\cdots t_{2}}(R_{on}(t))=\frac{1}{t_{2}-t_{1}}\int_{t_{1}}^{t_{2}}R_{on}(t)\mathrm{d}t=\frac{1}{t_{2}-t_{1}}\int_{t_{1}}^{t_{2}}\frac{V_{DS}(t)}{I_{D}(t)}\mathrm{d}t  \\
&\approx\frac{1}{2}(R_{on}(t_{1})+R_{on}(t_{2}))
\end{aligned}$$

##### (5)等效电阻
这里一定是晶体管*开关模型*的等效电阻. 
#### 3.动态特性
一个 MOSFET 管的动态响应只取决于它*充放电的这个器件的本征寄生电容*和互连线及负载引起的*额外电容*所需要的时间. 其中本证电容主要有三个来源:
1. 基本的 MOS 结构
2. 沟道电荷
3. 漏源反向偏置 pn 结的耗尽区
除了结构带内容之外的两个,都是非线性的,并且与外加的电压相关
##### (1)结构电容
在线性电阻区已经定义了单位面积的栅氧电容$C_{ox}$[[#^8c3f2e]], 这个电容的总值称为栅电容$C_g$, 其可以分为两个部分. 他一部分会影响沟道电荷, 另一部分完全来自于晶体管的拓扑结构.  
![[support/img/Pasted image 20231106133426.png|300]]
在理想状态下,D和 S 扩散应该恰好终止在栅氧的边上. 现实中,D 和 S 都会往栅氧下面延伸一小段距离, 这称为**横向扩散**. 因此晶体管的有效沟长比实际沟长短一个$\Delta L=2x_{d}$的距离, 这也正是引起了 G 和 S 与 G 和 D 之间的寄生电容, 称为**覆盖电容**.这个电容是线性的并具有固定的值:$$C_{GSO}=C_{GDO}=C_{ox}x_{d}W=C_{o}W$$
由于$x_d$是由⼯艺决定的，习惯上把它与每单位⾯积的栅氧电容$C_{ox}$相乘,得到的是每单位晶体管宽度的覆盖电容$C_o$（更具体地说为 $C_{gso}$和 $C_{gdo}$）。
> [!question] 这里对应存储的电荷是什么?
> 
##### (2)沟道电容
G 到沟道的电容$C_{GC}$是最重要的MOS寄生电容部分,它的**大小**和它在$C_{GCS}、C_{GCD}$和$C_{GCB}$(分别为栅⾄源、栅⾄漏和栅⾄体的电容)这三部分之间的**划分**取决于*⼯作区域和端⼝电压*。这种会产⽣不同分布的情况可以⽤图3.30的简图来解释。
1. 截⽌区域(a），没有任何沟道存在，所以,总电容$C_{GC}$出现在栅和体之间。
2. 电阻区(b), 形成了⼀个反型层，它的作⽤像源与漏之间的⼀个导体。结果$C_{GCB}=0$, 因为体电极与栅之间被沟道所屏蔽。对称性使这⼀电容在源与漏之问平均分布。
3. 饱和区(c), 沟道被夹断。栅与漏之间的电容近似为0，⽽且栅⾄体电容也为0。因此所有的电容是在栅与源之间的。
![[support/img/Pasted image 20231106135846.png]]
![[support/img/Pasted image 20231106165935.png]]
![[support/img/Pasted image 20231106173420.png]]
**疑问**
1. 没有沟道存在的时候怎么计算出的沟道电容,因为不存储电子啊?  
   - 肯定是存储的,只不过是存储在耗尽区之中了.此时就是 G 和 B 共同组成的等效电容, 随着 $V_{GS}$的增加, 耗尽层的厚度增加,电荷不知道怎么变,反正电容就是减小了
2. G 下的耗尽区到底是栅氧之下的还是G 下的?  
   - 肯定是栅氧之下的
3. 如果是栅氧之下的话,那存储的电荷还有意义吗?  
   - 肯定有意义啊
4. 导电沟道是一下子形成的吗?  
   - 是的!
5. 为什么导通就是平分?
   - 因为电荷的分布是平分的
6. 屏蔽的意思是什么?
   - 还不是很清楚,但大概的意思就是慢慢的电容都是由 S 和 D 提供的了,B 的作用很小了,来自B 的电荷几乎没有了
7. 导电沟道在这里究竟是什么作用才导致的 G 与 S、D 之间的电容这样分布?
   - 就是看来自于谁的电荷多
8. 为什么是 2/3?
	- 因为是一个论文中推导出来的



##### (3)结电容
> [!info] 首先回顾一下 pn 结电容
> 结电容为什么是势垒电容和扩散电容之和：
> 1. 势垒电容：当二极管截止时候，在PN结处由于扩散会天然形成电荷的富集(耗尽区)，此时的电容就是势垒电容；  
  2. 扩散电容：二极管从截止到导通需要一个“饱和”的过程，就是说载流子从密集在PN结附近到扩散到全部半导体，这个过程就是产生了扩散电容。
> 二极管从截止到导通，必须要经历这个过程，扩散电容的大小和P型半导体的掺杂浓度有关，和P型半导体的厚度有关。这过程也叫做二极管的反向恢复。

后⼀部分电容是由反向偏置的**源⼀体**和**漏-体**之间的pn 结引起的。耗尽区电容是⾮线性的，并且正如前⾯讨论过的，当反向偏置提⾼时它会减⼩。为了理解结电容(常称为**扩散电容**)的构成，我们看⼀下源(漏）区及其周围的情形。图3.33 显示源区pn结是由两部分组成的：
- 底板 pn 结:S 和 B 组成的,一般为<span style="background:#affad1">突变类型</span>的
- 侧壁 pn 结:S 和 $P^+$沟道阻挡层,<span style="background:#affad1">阻挡层</span>的掺杂浓度通常⼤于衬底的掺杂，于是形成了每单位⾯积较⼤的电容。侧壁结⼀般都是缓变的，梯度系数从0.33⾄0.5不等
因为这些都是<span style="background:#affad1">⼩信号电容</span>，所以我们通常把它们线性化并采⽤类似式(3.10)中那样的平均电容。
> [!info] 沟道阻挡层
> 在p型硅衬底上通过热氧化等[平面工艺](https://baike.baidu.com/item/%E5%B9%B3%E9%9D%A2%E5%B7%A5%E8%89%BA?fromModule=lemma_inlink)来制作器件和电路时，往往由于SiO2薄膜中存在有正电荷（Na离子沾污等所致），使得p型硅衬底表面上出现n型寄生沟道而造成短路；就在p型硅衬底的器件有源区以外的表面上注入硼离子（剂量约为1013/cm2），形成一层较高掺杂浓度的p型层，这样p型层就称为沟道阻止层。

![[support/img/Pasted image 20231107094433.png|500]]
![[support/img/Pasted image 20231107105329.png|400]]

##### (4)器件电容模型
整个 MOS 的电容可以表达为:(按照产生的电极分类)
$$\begin{array}{c}{C_{GS}=C_{GCS}+C_{GSO};\quad}&{C_{GD}=C_{GCD}+C_{GDO};\quad}&{C_{GB}=C_{GCB}}\\{C_{SB}=C_{Sdiff};\quad}&{C_{DB}=C_{Ddiff}}\\\end{array}$$

![[support/img/Pasted image 20231107105521.png|300]]
如果按照种类来划分的话,就看刚刚的那三种
##### (5)电阻
一个CMOS电路的性能可能进一步受另一组寄生元件的影响，这就是与漏区和源区相串联的电阻，如图3.35（a）所示。当晶体管的尺寸缩小时这一影响变得更为显著，因为尺寸缩小使结变浅，接触孔变小。漏(源）区的电阻可以表示为:$$R_{S,D}=\frac{L_{S,D}}{W}R_{\square}+R_{C}$$
其中, $R_c$是接触电阻,$W$是晶体管的宽度,而$L_{S,D}$是源或漏区的长度,见图3.35(b)。
$R_{\square}$是漏-源扩散区每⽅块的薄层电阻，它的范围为$20-100\Omega/\square$。 注意，材料的⽅块电阻是⼀个常数，它与⽅块的尺⼨⽆关。
![[support/img/Pasted image 20231107110052.png]]
#### 4.实际的 MOS 管二阶效应
#👁
##### (1)阈值变化
详细看课本,意思就是: 阈值电压变成与L、⽐和$V_{DS}$有关。
##### (2)热载流⼦效应
短沟道器件的阈值电压除了与设计间会有偏差之外，它也往往会**随时间漂移**。这是由于热 载流⼦效应的结果.
##### (3)CMOS 闩锁效应
MOS工艺会包含许多内在的双极型(BJT)管。它们在CMOS工艺中特别会引起麻烦，因为同时存在的阱和衬底会形成寄生的**n-p-n-p**结构。这些类似于*闸流管*的器件一旦激发即会导致$V_{DD}$和$V_{SS}$线短路，这通常会破坏芯片，或至少会使系统无法工作而只能断电后重新启动。

如下图所示:n-p-n-p结构是由 NMOS 的源、p衬底、n阱和由 PMOS 的源所形成。<u>当两个双极型管中的⼀个变为正向偏置时（例如由流 过阱或衬底的电流所引起），它提供了另⼀个双极型管的基极电流。这⼀正反馈使电流增加直⾄该电路失效或烧坏。</u>

![[support/img/Pasted image 20231107123036.png]]
因此,为了避免闩锁效应,应该使得电阻$R_{nwell}$和$R_{psubs}$最小。这可以通过提供很多阱和村底的接触并把它们放在 NMOS/PMOS 器件的源端接线附近来实现。传送大电流的器件(如在I/O驱动器中的晶体管)应当在周围有保护环(guard ring)。这些放置在晶体管周围的环形阱/衬底接触可以进一步减小电阻，从而减小寄生双极型管的增益。闩锁效应在早期的 CMOS 工艺中特别严重。近年来工艺上的创新和设计技术的改进已经几乎消除了闩锁的危险。
#### 5.MOS管的 SPICE 模型
#👁
短沟 MOS 晶体管特性的复杂性以及它的许多寄生效应已促使开发了许多模型，它们针对不同的精度和计算效率。一般来说，比较精确也意味着比较复杂，并因此增加了运行时间。在本节中简短地讨论比较常用的 MOSFET 模型的特点并介绍如何在电路说明中例举一个 MOS 晶体管。

课本上的3.4 工艺偏差; 3.5 综述: 工艺尺寸缩小; 3.6 小结
## 贰---互连线模型
随着深亚微⽶半导体⼯艺的出现,它们已开始⽀配数字集成电路⼀些相关的特性指标，如速度、能耗和可靠性。
这可以⽤⼀个简单的例⼦做最好的说明,⼀个总线⽹络中的每条导线把⼀个(或几个）发送器连⾄⼀组接收器。每条导线是由⼀系列具有不同长度和⼏何尺⼨的导线段构成的。假设所有的导线段都在同一互连层上实现，并且通过一层绝缘材料与硅衬底隔离及相互间隔离（注意现实情况比这更复杂）。右边是电路图,左边是加粗部分的实际视图. 
![[support/img/Pasted image 20231107125512.png|inLL|+grid|a|400]]![[support/img/Pasted image 20231107125431.png|+grid]]
### (1)集总模型Lumped Model
一条导线的电路奇生参数是沿的长度分布的，因此不能把它集总在一点上。然而当只有一个奇生元件占支配地位时，当在这些寄生元件之间的相互作用很小时或者当只考虑电路特性的一个方面时，把各个不同的(寄生元件）部分集总成单个的电路元件常常是很有用的。这一步骤的优点是寄生效应因此可以用**常微分方程**来描述。正如我们在后面将要看到的，描述一个分布元件需要偏微分方程。  
<u>只要导线的电阻部分很小并且开关频率在低至中间的范围内</u>，那么就可以很合理地只考虑该导线的电容部分，并把分布的电容集总为单个电容，如图4.11 所示。注意在这一模型中导线仍表现为一个等势区，因而导线本身并不引人任何延时。对于**性能的唯一影响是由电容对于驱动门的负载效应引起的**。这一电容集总的模型很简单但很有效，因此常常选用该模型来分析数宇集成电路中的大多数互连线。  
三种分析方法:
1. 多电阻,多电容,因此使用一组常微分方程
2. SPICE 模拟
3. Elmore 延时(70 年代的方法,比较准确)
![[support/img/Pasted image 20231107130310.png]]
#### A电阻电容网络---RC树
##### a特点:
1. 一个输入节点
2. 电容都在节点和地之间
3. 无电阻回路(因此叫树结构)
![[support/img/Pasted image 20231107130704.png|300]]

##### b参数定义:
- **路径电阻**$R_{ii}$: 源节点 s 与电路的任何节点 i 之间存在唯一的电阻路径，路径的总电阻称为路径电阻，如:$R_{44}\mathbf{=}R_1\mathbf{+}R_3\mathbf{+}R_4$
- **共享路径电阻**$R_{ik}$: 源节点 s 与电路的一个节点 i 和另一个节点 k 的这两条路径的共享电阻$R_{ik}=\sum R_j\Rightarrow(R_j\in[path(s\rightarrow i)\cap path(s\rightarrow k)])$
	<font color="#ff0000">例如:</font>$\color{red}{R_{i4}}\color{red}{=}R_1\color{red}{+}R_3,\color{red}{R_{i2}}\color{red}{=}R_1$
##### c时间常数计算
假设在 t=0 的时候加一个阶跃输入,则节点 i 的 Elmore 延时为:$$\tau_{Di}=\sum_{k=1}^NC_kR_{ik}$$
- 例子,上图的 i 点:$$\tau_{Di}=R_1C_1+R_1C_2+(R_1+R_3)C_3+(R_1+R_3)C_4+(R_1+R_3+R_i)C_i$$
- 注意:这个只是时间常数,还得 'x0.69'
#### B电阻电容网络---RC链
是 RC 树的特殊情形,没有分支了,因此其**共享路径电阻=路径电阻**
![[support/img/Pasted image 20231107132128.png]]
节点 N:$$\tau_{DN}=C_1R_1+C_2(R_1+R_2)+\cdots+C_N{\left(R_1+R_2+\cdots+R_N\right)}$$
节点 i:$$\tau_{Di}=C_{1}R_{1}+C_{2}(R_{1}+R_{2})+\cdots+\begin{aligned}(C_i+C_{i+1}+\cdots+C_N)(R_1+R_2+\cdots+R_i)\end{aligned}$$
- <font color="#ff0000">注意!!</font>这里的$\tau_{Di}$和$\tau_{i}$是有区别的,详见课本  
- 例子:长度为 L 的导线分割成长度相等的 N 段，每段长度为 L / N, 单位长度导线的电阻和电容分别为 r 和 c，每段的电阻和电容分别为 rL / N 和 cL / N
  利用Elmore公式，计算导线的时间常数:  $$\tau_{DN}=\left(\frac LN\right)^2\left(rc+2rc+\cdots+Nrc\right)=L^2(rc)\frac{N\left(N+1\right)}{2N^2}=RC\frac{N+1}{2N}$$
  其中R=rL和C=cL是导线的集总电阻和电容
  结论:
	- 当 N 很大的时候, 代入以上式子得到$\tau_{DN}=\frac{RC}2=\frac{rcL^2}2$, 集总 RC 链趋向于分布式 rc 线, 导线的时间常数是长度 L 的二次函数
	- 分布rc线(**N=无穷**)的时间常数是按集总 RC 模型(**N=1**)预测的时间常数的一半

### (2)分布模型
![[support/img/Pasted image 20231107134939.png#center|分布线的电路符号|100]]
![[support/img/Pasted image 20231107135614.png|400]]
电路中节点i处的电压可以通过求解以下⼀组偏微分⽅程来确定：$$c\Delta L\frac{\partial V_i}{\partial t}=\frac{(V_{i+1}-V_{i})+(V_{i-1}-V_{i})}{r\Delta L}$$
对于$\boldsymbol{\Delta}L\to0$, 以上方程就变成了扩散方程:$$rc\frac{\partial V}{\partial t}=\frac{\partial V}{\partial x^2}$$
其中, V是导线上⼀个特定点的电压，x是该点和信号源之问的距离。  
这个⽅程不存在收敛解，但可以推导6出以下的近似表达式$$\left.V_{out}(t)=\left\{\begin{matrix}2\mathrm{erfc}\left(\sqrt{\frac{RC}{4t}}\right)&&t\ll RC\\\\1.0-1.366e^{-2.5359\frac{t}{RC}}+0.366\mathrm{e}^{-9.4641\frac{t}{RC}}&&t\gg RC\end{matrix}\right.\right.$$
这些公式很难⽤来进⾏通常的电路分析。然⽽我们已知分布式⼼线可以⽤**集总的 RC梯形⽹络**
来近似，后者可以很容易地⽤在计算机辅助分析中。
![[support/img/Pasted image 20231107140313.png|400]]

