[[Fishing/First_Sem_Master_1/数大/数大|数大]]
# 一、引言
# 二、静态CMOS设计
## 1.互补CMOS
### (1)互补CMOS逻辑门的构造规则
#### (1)PUN 和  PDN
- PUN: 上拉网络, 每当(输入决定的)逻辑门的输出意味着逻辑 1 时, 其将提供一条在输出和$V_{DD}$之间的通路.
- PDN: 下拉网路, 每当(输入决定的)逻辑门的输出意味着逻辑 1 时, 其将提供一条在输出和$V_{SS}$之间的通路.
PUN 和 PDN 是以相互排斥的方式构成的: 在稳定状态下, 两个网络中有且只有一个导通. 也就是说, <span style="background:#affad1">稳定状态时的输出节点总是一个低阻节点. </span>(这里的意思是不是, VDD 或者 GND 直通 $V_{out}$)
![[support/img/Pasted image 20231113001145.png|+grid]]![[support/img/Pasted image 20231113001204.png|+grid]]
#### (2)设计逻辑
因为上拉网络只是 PMOS, 下拉网络只是 NMOS.
##### A 或非
###### a NMOS
![[support/img/Pasted image 20231113130733.png#right|NMOS与非|100]]由于这是 NMOS ,因此用作下拉, 就是把Y下拉为 0,这只在 A=1,B=1 的情况下才成立,因此就有$$\mathbf{Y}=\mathbf{\overline{AB}=0}$$
###### b PMOS
![[support/img/Pasted image 20231113131242.png#right|PMOS 与非|100]]PMOS 因此是上拉网络, 所以有输入为 1, 当 A=0, B=0 的时候有 Y=1, 因此就是$$\mathbf{Y}=\mathbf{\overline{AB}=0}$$
$$P_{0\to1}=\frac{N_0}{2^N}\cdot\frac{N_1}{2^N}=\frac{N_0\cdot\left(2^N-N_0\right)}{2^{2N}}$$
##### B 与非
思路同上![[support/img/Pasted image 20231113141857.png|300]]
#### (3)构造简单的门
通过上面的就知道了能够通过上边的 PMOS 和下边的 NMOS 分别构造一个逻辑, 上下就能构成一个完整的逻辑了. 这里的关键就是, 上边只负责 1, 下边只负责 0. ![[support/img/Pasted image 20231113142210.png|200]]
**一些经验规则**:
1. 晶体管看作是由其栅端信号控制的开关。
2. PDN用NMOS器件，PUN用PMOS器件。
3. 推导构造逻辑功能的规则：NMOS串联对应与（AND）功能，NMOS并联对应或（OR）功能。
4. PUN和PDN是对偶网络。构造CMOS门，可用串并联器件的组合实现其中一个网络（如PDN），然后根据对偶性原理得到另一个网络。
5. 互补门自然求反功能。单级只能实现NAND，NOR和XNOR等，非反相布尔函数需要额外的反相器。
6. 实现N输入的逻辑门需要晶体管数目为2N。  
🌰![[support/img/Pasted image 20231113142354.png|300]]
对于上边的这个例子来说, 其实可以根据上下两个网络分别写出两个式子来, 他们是等价的, 但是是分别而直观的. 
### (2)互补CMOS门的静态特性
- 高噪声容限
	- 标称电平部分
    标称高低电平就是电源和 GND
	- $V_{IH}和V_{IL}$部分
	  与输入信号的情况有关
- 没有静态功耗
  稳定的时候没有 VDD 到 GND 的通路
- VTC
  与输入信号的情况有关
#### (1)输入图案的影响
与反相器不一样, PUN 和 PDN 的**电阻**是输入的图案, 因此不同的输入组合有不同的 **VTC**, 从而有不同的**噪声容限**.   
#👁 #notebook

### (3)互补CMOS门的传播延时
#### (1)一阶分析
在这里, 输入模式也会对延时有不同的影响. 首先我们先进行一阶分析, 也就是忽略内部节点的电容.   
以下面的 NAND 为例子, 从输出的变化进行考虑延时大小:
1. **输出低到高转换**
	1. 两输入都变低: ${0.69\times(R_p/2)\times C_L}$
	2. 一个输入变低: ${0.69\times R_p\times C_L}$
	这里的*关键*就是: 有三种情况都能使输出从 0 变成 1, 但是他们对应的上拉(充电的)电路的走过的电阻是不同的, 假设哈这里的 PMOS 都是一样的, 那么明显以这种输出的变化为例子的话, 仅有一个输入变化的话会导致有更大的延时
2. **输出高到低转换**
   两输入都变高: $0.69\times(2R_n)\times C_L$
#### (2)二阶分析
![[support/img/Pasted image 20231113184315.png#right|NAND门|130]]
分析时应当考虑的点:
1. 输入使输出上升还是下降？
2. 内部节点的电容如何充放电？
3. 串联管子中哪个网值电压较高？
以右边的 NAND 门为例子来看的话, 还是按照输出的变化进行分类的话, 可以导致其输出从 1 到 0 的输入端的变化组合形式一共有三种(当然了, 另一个也有三种, 但一共有16 种).    
通过以上的三种情况考虑延时
##### A 输出从 0 到 1
(1) **A=B=1->0**：2个P管同时充电，同时不用对2个N管间的<span style="background:#affad1">寄生电容</span>充电，所以最快。  
(2) **A=1，B＝1->0**：关断B所驱动的N管相对（3）中关断A所驱动的N管困难，同时要对2个N管之间的寄生电容充电，所以最慢。  
(3) **A=1->0，B=1**：关断A所驱动的N管相对（2）中关断B所驱动的N管容易，同时不用对2个N管之间寄生电容充电，所以速度居中。  
#notebook #👁 
##### B 输出从 1 到 0
(1)**A=0->1，B=1**：虽然开启A所驱动N管相对(2)中开启B所驱动的 N管困难，但是2个N管之间寄生电容上的电荷早就泄放掉了，所以速度最快。  
(2)**A=1，B=0->1**：虽然开启B所驱动的N管相对(1)中开启A所驱动的N管容易，但是2个N管之间寄生电容上的电荷直到B所驱动的N管开启才泄放，因此速度居中。  
(3)**A=B=0->1**：A所驱动的N管的电阻相对(2)中A所驱动的N管电阻大，因此2个串联的N管的总电阻相对(2)中的大，放电慢，所以速度最慢。![[support/img/Pasted image 20231113214116.png#center|延时与输入模式的关系|400]]
### (4)互补CMOS门的尺寸设计
目标设计**对称响应**的电路结构  
*(1)对称响应的概念*  
对称就是上升和下降, 响应就是响应时间 t. 因此就是$t_{pLH}$和$t_{pHL}$相等(为什么 PPT 上写的$R_P=R_N$)  
这里的意思应该是, 反相器的 NMOS 和 PMOS 的等效电阻相同的时候, 才有反相器的上升和下降延时是相同的.  
那么问题来了  
1. 为什么反相器的$R_P=R_N$的时候才有$t_{pLH}=t_{pHL}$.   
2. NMOS 和 PMOS 的*长宽比的比值*大概是什么值的时候才有$R_P=R_N$, 这个比值是 1 吗?

*(2)参照反相器*  
意思就是, 使**逻辑组合器件的延时大概等于这个反相器的延时**. 此外, 这个反相器的bb是给出的, 反正这个 bb 能够使得$t_{pLH}=t_{pHL}$, 这样也都等于$t_p$了. 
*(3)例子*  
更好的例子是课本上的例子, 不仅给出了 W/L 的比值, 还给了单位. 要是只给出了比值的话,我们无法得到具体的两个值.   
直接进行截图:![[support/img/Pasted image 20231113221227.png]]
再给一个课件上的例子:![[support/img/Pasted image 20231113221511.png|400]]
### (5)互补CMOS门扇入对延时的影响
之前考虑的时候,都忽略了节点内部的电容. 但是在扇入很大的时候, 不能这么考虑. 于是使用下面的 4 输入 NAND 门进行分析
![[support/img/Pasted image 20231114102020.png|400]]
(1)$t_{pHL}$  
	每一个电容都要通过<span style="background:#affad1">电阻放电</span>(为啥对应下面的加号), 因此通过 **Elmore 模型**就有$$\begin{aligned}
t_{pHL}=& 0.69[{R_1}C_1  \\
&{+(R_1+R_2)C_2} \\
&+(R_1+R_2+R_3)C_3 \\
&+(R_1+R_2+R_3+R_4)C_L]
\end{aligned}$$
如果NMOS 的尺寸都是一样的,则有$$t_{pHL}=0.69R\left(C_1+2C_2+3C_3+4C_L\right)$$
<font color="#ff0000">注意:</font>$\color{red}M_1$<font color="#ff0000">电阻出现在所有的项之中, 所以在优化延时的时候显得尤为重要. </font>因此在设计的的时候M1～M4的尺寸应该递减（电阻递增）.  
仔细观察上边的式子,其实就是 1+2+...+n, 因此是 n 的二次方的函数.  
(2)$t_{pLH}$  
使连至输出节点的PMOS 器件数呈线性增加，我们可以预见门的由低至高的延时将随扇人数线性增加-因为虽然电容线性增加，但上拉电阻保持不变。
$$t_{pLH}=R(C_1+C_2+C_3+C_4+C_L)$$
(3)$t_{p}$  
![[support/img/Pasted image 20231114103228.png|400]]
### (6)互补CMOS组合逻辑链的性能优化
**目的:** 确定逻辑链路径中各级的尺寸以优化路径速度  
**一般来说**: 
- 一条逻辑路径的*原始输入电容*往往是确定的；
- 这条逻辑路径的*末端必须驱动的电容负载*也是确定的；  

**问题:**  
- 如何确定逻辑链路径中的尺寸来达到最快？
- 能否把反相器链的情况推广一下?
#### (1) 求解过程
![[support/img/Pasted image 20231114103631.png#right|对称的 NAND 与参考 CMOS|150]]考虑输出下降的情景，反相器同与非门具有相同的<font color="#ff0000">驱动能力（相同的输出电阻）</font>
##### A 原始延时表达式
###### a 反相器的延时原始表达式
$$\begin{aligned}
tp(INV)& =t_{P0}\left(1+{\frac{C_{L}}{C_{\mathrm{int}}}}\right) \\&=t_{P0}\left(1+{\frac{C_{L}/C_{gin}}{C_{\mathrm{int}}/C_{gin}}}\right)  \\

&\text{=} =t_{p0}\left(1+{\frac{f}{\gamma}}\right) 
\end{aligned}$$
参数(1):$\gamma=\frac{C_{\mathrm{int}}}{C_{gin}}$  
参数(2):$f=\frac{C_{\mathrm{L}}}{C_{in}}$
###### b 逻辑门的延时
$$\begin{aligned}
t_{P}(ND2)& =t_{p0(ND2)}\left(1+\frac{C_{L}}{C_{\mathrm{int}(ND2)}}\right)=  \\
& =k\bullet t_{P0}\left(1+\frac{C_{L}}{kC_{\mathrm{int}}}\right)=t_{p0}\left(k+\frac{C_{L}}{C_{\mathrm{int}}}\right)=  \\
&=t_{p0}\Bigg(k+\frac{C_{in(ND2)}}{C_{gin}}\bullet\frac{C_{L}/C_{in(ND2)}}{C_{int}/C_{gin}}\Bigg)= \\
&=t_{p0}\Bigg(k+g_{ND2}\frac{f_{ND2}}{\gamma}\Bigg)= \\
&=t_{p0}\Bigg({P_{ND2}}+g_{ND2}\frac{f_{ND2}}{\gamma}\Bigg)
\end{aligned}$$
参数(3)(4)定义如下, 并且假设$k\equiv P_{ND2}$
$$\begin{array}{l}{t_{p0(ND2)}=k\bullet{t_{P0}}}\\\\{C_{\mathrm{int}(ND2)}=k\bullet C_{\mathrm{int}}}\\\end{array}$$$$g=\frac{C_{in(ND2)}}{C_{gin}}$$

###### c 综上所述
- 反相器的延时为:  $t_{p}=t_{p0}\left(1+\frac{f}{\gamma}\right)$
- 一般逻辑门的延时为:  $t_{p}=t_{p0}\Bigg(p_{i}+\frac{g_{i}\cdot f_{i}}{\gamma}\Bigg)$
又因为 $\gamma=\frac{C_{\mathrm{int}}}{C_{gin}}$一般取值是 1, 因此有:  
**反相器:** $$t_{p}=t_{p0}\cdot(1+{f})$$**逻辑门:**$$t_{p}=t_{p0}\cdot(p_{i}+{g_{i}\cdot f_{i}})$$
以上就得到了<font color="#ff0000">延时的原始表达式</font>
##### B 参数的定义和意义
- **p  归一化本征延时,只与类型有关,连到Out电容÷3**  
	- $p=k=t_{P0(ND2)}/t_{P0}=C_{int(ND2)}/C_{int}$  
	- 该复合门和简单反相器的本征延时的⽐(即⽆负载)
	- *本征延时与门的类型有关，但它与门的尺寸（晶体管宽度的加倍）无关.* 
	- ![[support/img/Pasted image 20231114110558.png|500]]
- **g 逻辑努力,电流一样时,信号输入栅电容的比,只与类型有关**
	- $g=\frac{C_{in(ND2)}}{C_{gin}}$  
	- 逻辑努力 (Logical effort）是对于给定的负载，一个门的输入电容和与它具有相同输出电流的反相器的输入电容的比。  
	- *本征延时与门的类型有关，但它与门的尺寸（晶体管宽度的加倍）无关.* 
	- ![[support/img/Pasted image 20231114110625.png|500]]
- **f 等效扇出 电气努力**
	- $f=\frac{C_{\mathrm{L}}}{C_{in}}$  
	- 该门的外部负载电容和输⼈电容之间的⽐.  
	- “电气努力”（即等效扇出）与（负载电容/栅输入电容）的比值有关

**其他知识点:**
1. 反相器在所有的静态 CMOS 门中具有最小的逻辑努力和本征延时。
2. 逻辑努力 (Logical effort）随门的复杂度而加大。
##### C 链式的简化和求解
###### a 进一步简化(h 和 d)
因![[support/img/Pasted image 20231114111132.png#right|d和h的定义|300]]为扇出和逻辑努⼒是以类似的⽅式来影响延时的, 因此将两者乘积做定义:$$h=fg$$
因此复合门的延时变为:
$$t_p=t_{p0}\cdot(p+h)$$
再做定义: $$d=p+h$$
则有最简化的某一级的复合门延时:$$t_p=t_{p0}\cdot d$$
###### b 路径分支对于 f 的影响
![[support/img/Pasted image 20231114111612.png|300]]  
对于上图公式的解释, 还要计算分支上<span style="background:#affad1">所有的负载电容</span>对于对于我这一级$C_{gin}$  
$$f=\frac{C_{on-path}+C_{off-path}}{C_{gin}}$$
规定参数b为分支努力,表达式为: $$b=\frac{C_{on-path}+C_{off-path}}{C_{on-path}}$$
则能递推的链上有: $$\frac{f}{b}=\frac{C_{on-path}}{C_{gin}}$$
##### D 最终求解
某一条线路上的所有的逻辑门的延时求和, 则有:$$Delay=t_{p_{0}}\sum\limits_{i=1}^{N}\bigl(p_{i}+g_{i}\cdot f_{i}\bigr)$$因此目的就是使得以上式子的取值最小, 通过求偏导数(得把g和f写开才能看出来)得到: *当每⼀级应当具有相同的门努⼒*的时候, 有着最小的总延时, 即:$$f_{1}g_{1}=f_{2}g_{2}=\cdots=f_{N}g_{N}$$
沿电路中一条路径的总逻辑努力可以通过把这条路径上所有门的逻辑努力相乘求得，由此得到了**路径逻辑努力( path logical effort) G**:$$G=\prod_{1}^Ng_i$$
我们也可以定义**路径的有效扇出(或电气努力)P**，它建立了路径中最后一级负载电容与第一级输人电容之间的关系：$$F=\frac{C_{L}}{C_{g1}}$$
为了把各个门的等效扇出联系起来，必须引人另一个系数来考虑电路内部的逻辑扇出。当一个节点的输出上有扇出时，总驱动电流中的一部分沿我们正在分析的路径流动，而有些则离开这条路径。我们把给定路径上一个**逻辑门的分支努力(branching eftort)b**定义为:$$b=\frac{C_{\mathrm{on-path}}+C_{\mathrm{off-path}}}{C_{\mathrm{on-path}}}$$
式中，$C_{on-path}$是该门沿我们正在分析的路径上的负载电容，⽽$C_{off-path}$是离开这条路径的连线上的电容。
如果路径没有分⽀(如在逻辑门链中的情形），则分⽀努⼒为1.   
**路径分⽀努⼒**(**path branching effort**)定义为该路径上每⼀级分⽀努⼒的积，即$$B=\prod_{1}^{N}b_{i}$$
于是<u>路径电⽓努⼒</u>现在就可以与<u>各级的电⽓努⼒</u>和<u>分⽀努⼒</u>联系起来：$$F=\prod_{1}^{N}\frac{f_{i}}{b_{i}}=\frac{\prod f_{i}}{B}$$
最后，可以确定**总路径努⼒H**:$$H=\prod_{1}^{N}h_{i}=\prod_{1}^{N}g_{i}f_{i}=GFB$$
只要这一条路径之上每个门的类型确定了, 那么H就可以知道了, 知道了之后由于要求每一个h都相等,因此有: $$h=\sqrt[N]{H}$$
带入延时表达式, 得到在这一约束条件下, 最小延时为:$$D=t_{p0}\bigg(\sum_{j=1}^{N}p_{j}+\frac{N(\sqrt[N]{H})}{\gamma}\bigg)$$
###### a 得到h之后求解尺寸放大系数
1. 对于尺寸放大系数的理解:![[support/img/Pasted image 20231213102400.png|500]]
2. 求解  
由于:$h=g_{1}f_{1}=g_{2}f_{2}=\cdots=g_{i}f_{i}$, 因此有$f_i=\frac h{g_i}$. 由于:$b_{i}=\frac{C_{g(i+1)}+C_{bi}}{C_{g(i+1)}}$, 因此
有$\frac{f_i}{b_i}=\frac{C_{g(i+1)}}{C_{gi}}$.   
根据上图, 有公式$$\begin{aligned}
{s_i}& =\frac{C_{gi}}{g_{i}C_{ref}}=\frac{1}{g_{i}C_{ref}}\cdot\frac{f_{i-1}}{b_{i-1}}C_{g(i-1)}=\frac{1}{g_{i}C_{ref}}\cdot\frac{f_{i-1}}{b_{i-1}}\cdot\frac{f_{i-2}}{b_{i-2}}C_{g(i-2)}  \\
&=\frac{1}{g_{i}C_{ref}}\cdot\frac{f_{i-1}}{b_{i-1}}\cdot\frac{f_{i-2}}{b_{i-2}}\cdots\frac{f_{1}}{b_{1}}C_{g1}=\frac{1}{g_{i}C_{ref}}\cdot\frac{f_{i-1}}{b_{i-1}}\cdot\frac{f_{i-2}}{b_{i-2}}\cdots\frac{f_{1}}{b_{1}}\cdot g_{1S1}C_{ref} \\
&=\frac{g_{1}s_{1}}{g_{i}}\cdot\frac{f_{i-1}}{b_{i-1}}\cdot\frac{f_{i-2}}{b_{i-2}}\cdots\frac{f_{1}}{b_{1}}=\frac{g_{1}s_{1}}{g_{i}}\prod_{j=1}^{i-1}\!\left(\frac{f_{j}}{b_{j}}\right)\quad\textbf{186页公式6.18}
\end{aligned}$$
h知道了, g也知道了, f就知道了, s就知道了
##### E 例子
![[support/img/Pasted image 20231213102911.png|500]]
![[support/img/Pasted image 20231213102925.png|500]]
![[support/img/Pasted image 20231213102937.png|500]]
##### F 总结
**利用逻辑努力确定速度最优时尺寸的步骤**  
1. 计算路径的努力：H=GBF
2. 求出最优级数和每一级的努力h
	1. 若已经知级数 N，则 $h=H^{1/N}$
	2. 若未知级数 N，h=4, $N=\ln H/\ln4$
3. 画出具有这一级数 N 的路径的草图，并求出各级门的等效扇出$f_i=h/g_i$
4. 从任意一边开始，结合各级的分支情况，求出各级 门的输入电容（即栅电容）：$C_{g,i}=C_{out,i}/f_i$
5. 根据各级门的栅电容和逻辑努力，求出各级门的尺寸：$S_{g,i}=C_{g,i}/g_i$
6. 
#### (2)结论
##### A 逻辑努力方法的结论:   
1. **逻辑努力的概念可以用来快速比较各种电路结构的延时特性。**  
例如：在互补CMOS结构中，NAND门比NOR门好。
2. **逻辑链中当各级的<span style="background:#d3f8b6">努力延时（h）相同并且接近等于4</span>时，整个逻辑链路径的延时最快。**  
采用“较少”级数（逻辑门的数目较少）时，逻辑链未必最快。  
采用“大尺寸”逻辑门时，逻辑链未必最快，却会增加面积和功耗。  
3. **逻辑链的路径总延时对于级数偏离“最优级数”的敏感程度不大。**  
使每级的努力延时稍大于 4 可减少面积与功耗，但速度減慢不多。  
但当每级的努力延时大于6~8时，速度会明显变慢。
4. **当单个逻辑门的输入数目增多时，它的逻辑努力也增大，一般限制单个逻辑门的输入数目为4个。**
当输入数超过4时，一般需要把这个复杂门分解成多级的简单门。
##### B 逻辑努力方法的优点:  
1. 较小的逻辑优化问题可以很容易地转化为一组解析表达式。
2. 易于植入优化软件（如Matlab）。
3. 仔细推导可以转化为“凸函数”（convex）的优化问题。
4. 易于增加功耗/能量方面的考虑。
##### C 逻辑努力方法的局限:  
1. 没![[support/img/Pasted image 20231213103858.png#right|d和p,g的关系|200]]有考虑输入信号速率（Input Slope）。
2. 忽略了速度饱和与衬偏效应的影响。 PMO 因此实际的逻辑努力应通过实验求得。
3. 难以考虑互连线延时的影响。因此适合规则版图而且互连线的延时不占主要地位的情形，如数据通路和存储单元。
4. 难以分析具有分支情况复杂的路径。
5. 没有考虑如何设计电路使面积最小和功耗最小。
### (7)互补CMOS门的功耗优化
CMOS逻辑门功耗与下列因素相关
- 器件尺寸（影响电容）
- 输入输出上升下降时间（短路功耗）
- 器件阈值和温度（漏电功耗）
- 开关活动性（翻转概率）
#### (1)动态功耗
表达式: $$P_{0\to1}C_LV_{DD}{}^2f_{clk}$$
复杂的逻辑门比反相器和其余的逻辑门， P受影响最大
1. 逻辑电路拓扑结构（静态部分）
2. 电路时序特性（动态部分）
##### A 逻辑功能
 统计学上相互独立的静态CMOS逻辑门的静态翻转概率: $$P_{0\rightarrow1}=P_{0}{\cdot}P_{1}=P_{0}\cdot(1-P_{0})$$
 若输入是独立的，并且均匀分布:$$P_{0\to1}=\frac{N_0}{2^N}\cdot\frac{N_1}{2^N}=\frac{N_0\cdot\left(2^N-N_0\right)}{2^{2N}}$$
 其中, $N_0$为真值表中输出为“0”项的数目, $N_1$为真值表中输出为“1”项的数目
🌰: ![[support/img/Pasted image 20231213104558.png|300]]
##### B 信号统计特性
二输入静态NOR门，$P_{A}$和$P_{B}$分别为输入$A$和$B$等于1的概率，并设互不相关，则输出节点为1的概率为：
$$
P_1=(1-P_A)\cdot(1-P_B)
$$
输出由0→1的翻转概率为

$$
\begin{aligned}
\boldsymbol{P_{0\rightarrow1}}& =P_{0}P_{1}=(1-P_{1})P_{1}  \\
&=(1-(1-P_A)(1-P_B))(1-P_A)(1-P_B)
\end{aligned}
$$
![[support/img/Pasted image 20231213104709.png|300]]
##### C动态或虚假翻转
逻辑块的非零传播延时会引起毛刺（Glitch）和虚假翻转，即一个节点 在一次有效翻转中在稳定到正确的逻辑电平之前可能出现多次翻转。
例如在下面的NAND门之中, 所有的所有输入同时从0变为1![[support/img/Pasted image 20231213104851.png|500]]
结果: 
1. 所有奇数级输出翻转到0
2. 所有偶数级输出本来应该保持为1
问题: 由于非零的传播延时，<font color="#ff0000">较后的偶数级出现了毛刺</font>，造成了实现逻辑功能所需功耗之外的额外功耗
![[support/img/Pasted image 20231213105010.png|500]]
我的分析:
1. 首先, 全都是0的时候, 输出全是1,这是AND
2. 所有输入同时变为1, 所有奇数级输出翻转到0, 所有偶数级输出本来应该保持为1
3.  由于非零的传播延时, 本来该保持为1的偶数级,在翻转为1的时候, 上一个的0还没到, 所以差点下来, 所以出现了毛刺
4. <span style="background:#affad1">前边那个小疙瘩是啥啊</span>
## 2.有比逻辑（伪NMOS和DCVSL）
### (1)有比逻辑基础
**对比互补CMOS和有比逻辑:**
- 互补CMOS
	- 高度的稳定性：无比逻辑，噪声容限大，无静态功耗
	- 需要2N个晶体管实现N扇入的逻辑门，芯片面积和负载电容较大
- 有比逻辑
	- 目的：减少互补CMOS中的器件数, 减少面积
	- 方法：不用PDN和PUN组合，<font color="#ff0000">而用NMOS的PDN实现逻辑功能，用简单的负载器件实现上拉</font>(注意是NMOS和PDN)
	- 缺点：降低了稳定性、增加功耗
**结构展示:**
右图采⽤⼀个栅极接地的 PMOS 负载，这样的门称为伪 NMOS门。
![[support/img/Pasted image 20231213110510.png|400]]
**伪NMOS特点:**
- 晶体管数目N + 1个
- 输出高电平$V_\textit{он}{ = V _ { D D }}$
- 输出低电平$V_{OL}=\frac{R_{PDN}}{R_L+R_{PDN}}$不为 0，降低了噪声容限，增加静态功耗
- 负载器件相对于下拉器件的尺寸比，会影响噪声容限、传播延时、功耗等，甚至是逻辑功能
**计算伪NMOS的$V_{OL}$**
<span style="background:#d3f8b6">假设NMOS线性，PMOS饱和</span>,则有$$k_n\left((V_{DD}-V_{Tn})V_{OL}-\frac{V_{OL}}2\right)+k_p\left(\left(-V_{DD}-V_{Tp}\right)V_{DSATp}-\frac{V_{DSATp}}2\right)=0$$
设$V_{OL}$相对于$V_{GT}$很小，$V_{Tn}=-V_{Tp}$ 

$$
V_{\underline{oL}}\approx\frac{k_p(V_{DD}+V_{T_p})\cdot V_{DSAT_p}}{k_n(V_{DD}-V_{T_n})}=\frac{\mu_p\cdot W_p}{\mu_n\cdot W_n}\cdot V_{DSAT_p}
$$


$要使V_{oL}\text{尽可能低,PMOS尺寸应当明显小于NMOS尺寸,但会增加 }t_{pLH}$
> [!question] 为什么会增加$t_{pLH}$?
> 由于是LH,所以是输出变高, 那输出变高是VDD经过PMOS给输出充电, 但由于PMOS的尺寸减小了,<span style="background:#d3f8b6">因此k减小了</span>,因此电流减小了,所以给电容充电变慢了.
> 如果从公式的角度来理解的话, 则参考[[Fishing/First_Sem_Master_1/数大/L05/L05_L06_CMOS 反相器_NOTE#3.性能设计优化|L05_L06_CMOS反相器-开关模型]], 可以发现, 为了减少延时,可以增大晶体管尺寸, 但是这里是减少了W(<span style="background:#d3f8b6">W减少,L不变,出链也是HL不是LH,不知道为啥三倍</span>),因此延时变大


**低电平输出模式时的静态功耗：**
这是伪NMOS门的⼀个主要缺点
$$P_{low}=V_{DD}\cdot I_{low}\approx V_{DD}\cdot\left|k_p\left(\left(-V_{DD}-V_{T_p}\right)\cdot V_{DSAT_p}-\frac{V_{DSAT_p}^2}2\right)\right|$$
**伪NMOS反相器的 VTC（193页例6.7）**
![[support/img/Pasted image 20231213111424.png|400]]
**设计伪NMOS，要折中考虑**
![[support/img/Pasted image 20231229215631.png|400]]
1. 减少静态功耗，负载PMOS管要小
2. 得到较大的$NM_L$, $V_{OL}$要低, <span style="background:#d3f8b6">因此</span>$\left(W/L\right)_{\mathrm{n}}/\left(W/L\right)_{\mathrm{p}}$<span style="background:#d3f8b6">要大</span>, 负载PMOS管要小
3. 减小$t_{pLH}$, 负载PMOS管要大
4. <font color="#ff0000">1,2和3矛盾</font>,速度快的门消耗更多的静态功耗，且会减小噪声容限。
**用伪NMOS设计大扇入的复合门具有吸引力的原因**
1. N+1个晶体管，面积小，寄生电容小
2. 对前级负载小，每个输入只接到一个晶体管
3. 输出低电平时有静态功耗，适合大多数情况下输出为高电平的情况，如存储器的地址译码电路等
> [!question] 思考题6.5
> ![[support/img/Pasted image 20231229215751.png]]

**改进伪NMOS的负载，目标**
1. 消除静态电流
2. 提高输出摆幅,降低$V_{OL}$
3. 减小$t_{pLH}$
确实,就是把缺点都弄没
### (2)差分串联电压开关逻辑
![[support/img/Pasted image 20231213111935.png|300]]
#### A:工作原理:
主要应用了**两个原理**:
1. 差分: 差分要求所有的输入有互补的形式,因此输出也有互补的形式
2. 正反馈: ==保证了在不需要负载器件的时候要将其关断(关断:之前上边的PMOS一直是接地的,现在让他的栅极是反馈回去的了)==
**具体工作原理**:
假设一开始PDN1是导通的, PDN2不导通, 此时Out应该是0(虽然还没有分析机理), $\overline {Out}$是1.
考虑<font color="#ff0000">转换为PDN1不导通, PDN2导通的状态</font>. 由于PDN1导通, 就要把Out拉到0, 但是, 此时的M1仍然是导通的, 还没有关断, 所以此时M1仍然吧Out拉至$V_{DD}$, <font color="#ff0000">因此PDN1和M1存在着竞争</font>. 只有PDN1足够强, 下拉么么合理足够, 使得Out低于$V_{DD}-|V_{T_{P}}|$, 此时的M2才可以导通.(之前M2和$\overline {Out}$都处于高阻态), 一旦导通, 就来到了<font color="#ff0000">正反馈</font>状态, 先是对$\overline {Out}$充电至$V_{DD}$,而$\overline {Out}$又控制着M1的栅极, M1开始关断, 导致${Out}$更容易变低, M2更容易打开, 形成正反馈了. 最终就是$\overline {Out}=0,Out=1$ .
#### B:特点
**DCVSL的特点：**![[support/img/Pasted image 20231230144010.png#right|XOR|200]]
1. 输入具有互补形式，同时产生互补输出，消除了反相信号所需要额外反相器
2. <span style="background:#d3f8b6">输出节点电容小（和伪NMOS相同）(为啥???上边只有一个PMOS是吧)</span>
3. 注意在两个下拉网络之间有可能共用晶体管，从而降低了实现的面积开销。
**优点:**  
- 反馈机制保证了能够关断不需要的负载器件
- 消除静态功耗 
- 下拉网络PDN1和PDN2互补，实现逻辑功能的互补
- <font color="#ff0000">有比逻辑</font>，全摆幅 <font color="#ff0000">( 转换的时候是有比逻辑, 竞争)</font>
**缺点:**
- 额外面积开销(有两个下拉网络)
- 布线复杂
- 动态功耗高: 增加了<span style="background:#d3f8b6">转换功耗</span>, 这⼀电路类型还有因渡越电流所引起的功耗问题。在翻 转期间 PMOS 和PDN 会同时导通⼀段时间，从⽽产⽣⼀条短路路径。
#### C:例子
##### a:AND-NAND门
![[support/img/Pasted image 20231213112318.png|500]]
- 结构：互补NMOS 下拉网络，交叉连接的PMOS上拉管。
- 静态逻辑：<font color="#ff0000">所有逻辑由NMOS 管实现</font>，具有伪NMOS优点. 上拉PMOS管的尺寸选择很重要。
- 差分型：同时要求正反输入，面积大，<span style="background:#d3f8b6">但在要求互补输出或两个下拉网络能共享逻辑时比较有利（如下一页所示）</span>。
- 性能：<font color="#ff0000">速度比静态CMOS逻辑慢（因Latch 反馈作用有滞后现象）</font>。
- 功耗：无静态功耗，但同时有上升和下降过渡，有较大的翻转（Cross-over）电流和功耗。
> [!question] **为啥Out比Out'的延时要小**
> 因为这里在这种翻转的情况下, 直接影响的是Out, 给他充电, 才进而影响M1的栅极, 才会影响$\overline {Out}$, 因此, 注意,<font color="#ff0000"> 延时是对应输入图形的</font>. 
> 此外, 注意和<font color="#ff0000">互补CMOS延时的比较, 会比较大</font>, 因为竞争完才有正反馈

##### b:XOR-XNOR门  
![[support/img/Pasted image 20231213112514.png|500]]

**关键就是如何共享部分晶体管的**: 
这是异或($Y=A\overline B+\overline AB$)和同或($Y=AB+\overline{A} \overline B$. 这里是复用了A, 也就是异或的AB和同或的$A\overline B$的A用了一个管子, 这就是<font color="#ff0000">复用</font>.


## 3.传输晶体管逻辑（Pass-Transistor Logic）
在之前, 所有的输入端只连接到栅极, 但是在这里, <font color="#ff0000">原始输入端不仅给栅极供电, 还给源级和漏极供电</font>, 因此会减少使用的晶体管数量.![[support/img/Pasted image 20231228093347.png|400]]
特点有: 
1. 需要的器件数量减少, 只需要N个
2. <span style="background:#d3f8b6">没有静态功耗</span>
3. 但是, NMOS传输高电平有阈值损失, 其中$V_x$<span style="background:#d3f8b6">满足表达式</span>: $V_{x}=V_{DD}\left.-\left(V_{Tn0}+\gamma\Bigg(\sqrt{\left|2\phi_{f}\right|+V_{x}}-\sqrt{\left|2\phi_{f}\right|}\right)\right)$
**🌰:** ![[support/img/Pasted image 20231228093653.png|300]]
在这个例子之中, 上边那个通路实现的作用是: B=1的时候, F=A, 因此F=AB. B=0的时候, A就传不过去, 但要求是传过去一个0.  
需要细细的考虑这个事,传过去一个0,不需要下面那一条路,这个也可以完成F=AB的功能, 但是需要保证, <span style="background:#d3f8b6"><font color="#ff0000">任何时刻必须存在⼀条通向电源线(GND或VDD)的低阻抗路径</font></span>。因此, 下面的一条线路是必不可少的.
### (1)互补晶体管传输逻辑(Complementary Pass Transistor Logic，CPL)
⾼性能设计中常常使⽤称为 CPL 或DPL的差分传输管逻辑系列。其基本思想(类似于 DCVSL）<span style="background:#d3f8b6"><font color="#ff0000">是接受真输⼈及其互补输⼈并产⽣真输出及其互补输出</font></span>。图6.37是⼏个CPL门(AND NAND, OR/NOR 及 XOR/NXOR)。这些门具有⼀些有趣的特点：
- **CPL属于静态⻔**类型，因为定义为输出的节点总是通过⼀个低阳路径连到Vdd或 GND。这 有利于避免噪声千扰。
- **CPL的设计具有模块化的特点**。事实上，它所有的门都采⽤完全相同的拓扑结构。只是输入的排列不同⽽已。这使得这类门单元库的设计⾮常简单。较复杂的门可以通过串联标准的传输管模块来构成。![[support/img/Pasted image 20231228094358.png|500]]
理解上边的两点
1. 属于静态门的意思是输出不会根据存储的电荷来输出高低电平, 而会直接连接到GND或者VDD的线给其供电
2. 模块化的意思是: 上边的四个图把最基本的模块都给写出来了, AND/NADN, OR/XOR..., 因此,比如(ABCD)=(AB)(CD),因此就有下图,很奇妙的,产生了X和Y之后作为新的输出![[support/img/Pasted image 20231228094928.png|400]]
### (2)阈值损失问题
#### A:阈值损失问题的根本原因
阈值损失问题是为什么: 因为在之前的互补CMOS的时候,没有衬偏效应,为啥呢,因为那里的NMOS只下拉,但是这里的NMOS不仅下拉, 还上拉, 上拉的时候就有问题了, 一给NMOS的S的电压整上来, 栅源电压小于阈值, 就不能充电了, 电压就上不去了.![[support/img/Pasted image 20231228095515.png]]因此有了上图, 其公式大概遵循$$V_{_x}=V_{DD}-(V_{Tn0}+\gamma((\sqrt{|2\phi_{f}|+V_{x}})-\sqrt{|2\phi_{f}|}))$$
#### B:串联传输管的要求(驱动问题)
第一个输出去驱动M2的栅极的话, 就会出问题, 因为栅极电压还要减去一个, 右边这一个取max需要理解一下, 结果上来看确实好多了.
![[support/img/Pasted image 20231228095702.png]]
在右图之中, ![[support/img/Pasted image 20231228095829.png#right|驱动问题|200]]节点B的电压不能被拉到2.5V,只能被拉到$2.5V-V_{Tn}$. 由于衬偏效应, <span style="background:#d3f8b6">NMOS的阈值电压比PMOS高</span>,因此M2不能完全关断,产生静态功耗




#### C: 解决方案
##### a:用电平恢复晶体管（Level Restoring Transistor）
![[support/img/Pasted image 20231228100247.png|400]]
- 思路: 损失的是高电平是吧, 只要传过来一个弱1, 经过反相器,OUT=0,电平恢复管$M_r$就能导通, 直接一个VDD给你拉上去.
- 优点: 
	- 电压恢复至全摆幅
	- 反相器不再有静态功耗
- 缺点:   
(1) <span style="background:#d3f8b6">恢复管增加节点 X 的电容，X 点放电时间增加</span>；  
<span style="background:#d3f8b6">节点电容是什么?多的电容通过什么支路放电?</span>![[support/img/Pasted image 20231229195911.png]]
(2) 有比问题  
![[support/img/Pasted image 20231229194710.png]]
意思就是说,当传输过来一个0的时候,反相器的输入为分压,只有让这个分压小于开关阈值的时候,反相器才能输出1,从而把上拉的$M_r$关断.
- **确定电平恢复管的尺寸**
![[support/img/Pasted image 20231229195940.png]]
因此有:
- 恢复管尺寸存在上限
- <span style="background:#d3f8b6">传输管下拉可能有几个晶体管堆叠，恢复管尺寸应更小</span>![[support/img/Pasted image 20231229195807.png]]
##### b:传输管采用低阈值晶体管($V_T=0$)
![[support/img/Pasted image 20231229200445.png|]]
![[support/img/Pasted image 20231229200618.png|300]]
这里阐明了一个事实: <span style="background:#d3f8b6">亚阈值电流和静态功耗,漏电路径的关系</span>.可能导通的意思还需要有一个完整串通的VDD-->GND通路.
##### c:传输晶体管逻辑
![[support/img/Pasted image 20231229201501.png]]
![[support/img/Pasted image 20231229201511.png]]
![[support/img/Pasted image 20231229201529.png]]
![[support/img/Pasted image 20231229201554.png]]
- 注意有关低阻节点的叙述
- XOR这个例子是两边往中间传,那个左边那一列还是很神奇的   
<span style="background:#d3f8b6">如何设计传输门电路????????????</span>
- **传输门的性能**
以以输出端充电为例,$${R_{eq}}{=R_n}\|{R_p}$$即等效电阻近似为常数,等效电阻是NMOS和PMOS的电阻并联,推导见课本
![[support/img/Pasted image 20231229202450.png|400]]
- **等效电阻的应用**![[support/img/Pasted image 20231229202707.png]]
![[support/img/Pasted image 20231229202731.png]]
如果想要, **减少延时**的话, 一个重要的手段是<font color="#ff0000">打断链条</font>: ![[support/img/Pasted image 20231230191900.png|500]]
### (3)总结
![[support/img/Pasted image 20231229202854.png]]

# 三、动态CMOS设计
静态和动态的区分: 
1. 静态: 在静态逻辑电路中，每一个时间点（开关瞬态除外） <font color="#ff0000">输出都通过一条</font><font color="#ff0000">低阻的路径连接到</font>$V_{DD}$和$GND$.
	- 对于**静态互补逻辑**, 扇入为N时, 需要**2N**个器件(包括N型器件和P型器件个N个). 无静态功耗（==不考虑漏电==）。
	- 对于**伪NMOS逻辑**, 扇入为N时, 需要**N+1**个器件(包括N型器件N个和P型器件1个). 有静态功耗(==分压, 通路, 对哈==)。
2. 动态逻辑电路依赖于信号值在<font color="#ff0000">高阻节点的电容上暂时存储</font>。
	- 扇入为N 时，需要N + 2个晶体管（n+1 个 N-型器件 ＋ 1个 P-型器件，其中一个 N 型的求值管，1个 P 型的预充管）。 无静态功耗（不考虑漏电）。
### (1) 动态门的基本原理
![[support/img/Pasted image 20231230193050.png|500]]
#### A: 具体工作分析
- 分为两个工作状态(两相), 处于何种⼯作模式由<font color="#ff0000">时钟信号 CLK 决定</font>。
	- **预充(Precharge)**: CLK = 0，Mp 导通，Me 关断, 下拉路径不工作
	- **求值(Evaluate)**:  CLK = 1，Mp 关断，Me 导通， ==根据输入值和PDN的拓扑结构有条件放电(有条件的变为0和1, 所以实现了功能)==
针对右边那个例子就是: Clk=0的时候, Mp 导通，Me 关断,  此时给Out充上电了. 当\[(A且B)或C\]等于的1的时候, Out=0, 则有$\overline {Out}=\overline {AB+C}$. 
#### B:对于输入输出的要求
- 一旦动态门的输出被放电，直到进行下一次预充电之前不可能再次被充电(这是事实)。
- 动态门的输入在求值期间最多只能有一次变化。
- 在求值期间或求值后，输出<span style="background:#d3f8b6">可能处于</span>高阻状态，==状态存储在电容 C L 上==。
#### C:特性
##### a:静态特性
**以N-logic为例**
1. 逻辑电平: <font color="#ff0000">由于是无比逻辑</font>, 所以$V_{OL}=0,V_{OH}=V_{DD}$
2. 其他VTC参数, 例如 : 噪声容限, 阈值电压<span style="background:#d3f8b6"><font color="#ff0000">都是和时间相关</font></span>. 由于动态电路需要周期性的预充, 求值，因此纯粹的静态分析对动态电路不再适用。
	1. **阈值电压**：在求值相位，<font color="#ff0000">当输入信号超过NMOS下拉晶体管的开启电压</font>$V_{Tn}$时: 动态反相器的下拉网络开始导通，如果等待足够长时间，输出最终会达到 GND。(1)阈值电压 $V_{}M$ (2) 输入高电平 $V_{IH}$ (3) 输入低电平 $V_{IL}$ 都被设为$V_{Tn}$ 
	2.  **噪声容限**：<font color="#ff0000">输出高电平时，动态逻辑门的输出阻抗很大</font>。<span style="background:#d3f8b6">因此</span>， <span style="background:#d3f8b6">输出电平对噪声和干扰很敏感！</span><span style="background:#d3f8b6">其</span><span style="background:#d3f8b6">它信号的电容性耦合，可能造成节点电荷损失，而且不能恢复。</span>
##### b:动态特性
**以N-logic为例**
1. 开关速度快. 因为动态电路结构简单，晶体管数目较少，$C_{L}$小, 每个扇入对前级只表现为一个负载晶体管。
	1. $t_{pLH}=0$: 输出高电平时没有额外的开关动作发生==(开关动作产生延时)==。此外, 在伪NMOS 中，$t_{pLH}$是弱点，比 $t_{pHL}$ 还要大<span style="background:#d3f8b6">(为什么, 一定吗)</span>。
	2. $\boldsymbol{t}_{\boldsymbol{p}HL}\boldsymbol{\propto}\boldsymbol{C}_L$: ==取决于PDN下拉电流的能力==。$M_{E}$ 的存在，会使得放电变慢一些<span style="background:#d3f8b6">(为什么, 能否去掉)</span>。
2. 要考虑预充时间
	1. MOS预充管的尺寸选择。
		1. 对于逻辑来说, PMOS预充管的尺寸可以自由选择，不会影响直流特性；
		2. PMOS管增大，可以减少预充时间,  减少$t_{pLH}$,  <span style="background:#d3f8b6">但同时</span>$C_L$<span style="background:#d3f8b6">增加</span>，$t_{pHL}$会增大。
		3. 对时钟 CLK 的负载也增加, 因为栅极电容是时钟的负载. 
##### c:其他特性
**以N-logic为例**, 面积小, 速度快, 功耗大
1. 逻辑功能由 PDN实现。
	1. 构成PDN 的过程与静态 CMOS 完全⼀样
	2. 晶体管的数⽬（对于复杂门）明显少于静态情况：为**N+2**⽽不是2N。
2. **⽆⽐的逻辑门**。PMOS 预充电器件的尺⼨对于实现门的正确功能并不重要。
3. 动态逻辑门**只有动态功耗**。理想情况下，在VDD和 GND 之间从不存在任何静态电流路径。 但是，它的总功耗仍可以明显⾼于静态逻辑门, <span style="background:#d3f8b6">原因: (1)时钟总是在翻转(2)此外输出节点的开关活动因子相对大</span>
5. 动态逻辑门具有较快的开关速度，主要有两个原因。
	1. 第⼀个(明显的）原因是由于减少了<span style="background:#d3f8b6">每个门晶体管的数⽬</span>(2N--->N+2????)，并且<span style="background:#d3f8b6">每个扇⼈对前级只表现为⼀个负载晶体管</span>(AB+A'C呢, 对前级什么意思, 这里的意思是不互补了, 只有一个PDN???逻辑表达式中, 一个字母只出现一次? )，因⽽降低了负载电容，这相当于降低了逻辑努⼒。例如，⼀个两输⼈动态 NOR 门的逻辑努⼒是2/3，大⼤⼩于相应静态 CMOS 门的 5/3, 因为2|(2+2)\*2. 
	2. 第⼆个原因是动态门没有<span style="background:#d3f8b6">短路电流(啥叫短路电流??是不是静态的上下同时导通的那一瞬间?有的话为啥会变慢?)</span>，并且由==下拉器件提供的所有电流==都⽤来对负载电容放电<span style="background:#d3f8b6">(不然呢?)</span>。
7. 输入电容与伪NMOS相同(<span style="background:#d3f8b6">谁的输入电容, 某一个信号的?</span>)
##### c:优势劣势
1. 优势
	1. 晶体管少，$C_L$ <span style="background:#d3f8b6">小，每个扇入对前级只表现为一个负载晶体管</span>
	2. <span style="background:#d3f8b6">每个周期最多只能翻转一次，没有毛刺和虚假翻转</span>
	3. <span style="background:#d3f8b6">不存在短路功耗</span>
2. 劣势: 
	1. <span style="background:#d3f8b6">时钟功耗大，时钟节点每个时钟周期都要翻转</span>
	2. <span style="background:#d3f8b6">增加抗漏电器件时可能会有短路功耗</span>
	3. <span style="background:#d3f8b6">较高的开关活动性</span>
		1. $\text{静态门}0\to1\text{的翻转概率:}P_0P_1=P_0(1-P_0)$
		2. $\color{green}\text{动态门}0\to1\text{的翻转概率:}P_{0\to1}=P_0>P_0P_1$
#### D: 例子
![[support/img/Pasted image 20231230213701.png|500]]
1. 第一个是组合下拉
2. 第二个是组合上拉
3. 第三个是第一个的例化, 1的时候下拉, 所以是反相器
4. 第三个是第二个的例化, 0的时候下拉, 所以是反相器
5. 第三个是第一个的例化, 1的时候下拉, 逻辑功能见那个
---
![[support/img/Pasted image 20231231010017.png|600]]
- $t_{pre}$是啥????
- Out那个小疙瘩是啥????
- 图中可以看出下降的慢, 上升得快
### (2)动态门的信号完整性
#### A:电荷泄漏（ Charge Leakage）
**泄![[support/img/Pasted image 20231231011152.png#right|泄露|200]]露来源**
- 反偏二极管
- 亚阈值漏电
**影响:** 动态电路的时钟频率有一个下限, 原因是如果太低, 高电平就直接漏没了(<font color="#ff0000">当然, 是在求值等于1的时候吧</font>)


![[support/img/Pasted image 20231231011332.png|300]]
**解决方案:**
增加一个泄漏晶体管，类似传输晶体管逻辑中的电平恢复器方案. ![[support/img/Pasted image 20231231011518.png|500]]
- 发生在求值阶段等于1的时候
- 反馈补充
- 有比逻辑, <span style="background:#d3f8b6">存在短路功耗(竞争的时候??)</span>
#### B:电荷分享（ Charge Leakage）3
##### a:原理
![[support/img/Pasted image 20231231012143.png|500]]
##### b:计算
![[support/img/Pasted image 20231231012256.png]]
![[support/img/Pasted image 20231231012330.png|500]]
##### c:解决方案
由![[support/img/Pasted image 20231231012852.png#right|预充内部节点|200]]于上一次求值的时候, 泄露了A的电荷, 现在为0, 而充的时候只给$C_{L}$. 那既然如此, 我们在预充的时候给内部节点也充电! 
缺点: 面积和功耗增加



#### C:电容耦合（Capacitive Coupling）

![[support/img/Pasted image 20231231095605.png]]
![[support/img/Pasted image 20231231095828.png]]
- 背栅耦合
- 电容耦合
- 明白耦合的原理和要求: <span style="background:#d3f8b6">为啥高阻抗节点容易被电容耦合</span>
#### D:时钟馈通（Clock Feedthrough）
电![[support/img/Pasted image 20231231100413.png#right|电容耦合|300]]容耦合的⼀种特殊情况是时钟馈通，它是由在预充电器件的时钟输⼈和动态输出节点之 问的电容耦合引起的效应。<font color="#ff0000">耦合电容由预充电器件的栅-漏电容组成，包括了覆盖电容和沟道电容。</font>当下拉⽹络不导通时，这⼀电容耦合会在时钟由低⾄⾼翻转时引起动态节点的输出上升到 Vdd以上。其次，快速上升和下降的时钟边沿会耦合到信号节点上，这在图 6.62(b)的模拟结果 中⾮常明显。
**产生的危险:** 
1. <span style="background:#d3f8b6">它可能使预充电管正常情况下的反偏结⼆极管变为正向偏置。这会使 电⼦注⼈衬底中，它们可能为附近处于“1”（⾼电平）状态的⾼阻抗节点所收集，最终导致出错。</span>
2. <span style="background:#d3f8b6">CMOS 闩锁则是这⼀注⼈的另⼀种可能结果。</span>
因此在任何情况下，⾼速动态电路都应当仔细进⾏模拟，以确保时钟馈通的影响处在允许的范围之内。
### (3)动态门的串联
#### A: 简单串联存在的问题
![[support/img/Pasted image 20231231101423.png|500]]
首先看(a)中的电路结构, 第一级, In=1的时候, 放电, 输出为0, 所以应该是一个反相器, 因此第二级也是一个反相器, 所以为两个反相器的串联. 因此, 输入为1的时候, $Out_{1}$和$Out_{2}$应该分别为0和1. 
问题出现在, In从0变1,Out1不能立马变成0, 为1会持续一段时间, 等于1又会让第二个放电, 第二个又被放电至0的可能性, $Out_{1}下降至=V_{Tn}$的时候放电停止. 
- 导致的问题: 电荷损失导致噪声容限降低并可能引起功能出错。
- 出现的直接原因: 每⼀个⻔的输出（并且也是下⼀个⻔的输⼈）被预充电⾄1。
- <font color="#ff0000">解决方案</font>: 在预充电期间置所有的输⼈为0 可以解决这个问题。这样， 在预充电之后下拉⽹络中所有的晶体管都被关断，因此在求值期问不会发⽣存储电容的错误放电。换⾔之，<font color="#ff0000">只要在求值期间输⼈只能进⾏单个的0⼀1 翻转就能保证正确⼯作, 即晶体管只有在需要的时候才导通，并且每周期最多⼀次。</font>
#### B: 多米诺逻辑（Domino Logic）
![[support/img/Pasted image 20231231103132.png|500]]
给输出加一个反相器就好了, 优点如下
1. 无论是输出从1变为1还是1变为0, 第二级的输入只会从0变为0或者0变为1.(==逻辑需要改一下==)
2. 扇出由一个低阻抗输出的静态反相器驱动，提高了抗噪声能力
3. 缓冲器隔离了内部和外部电容，减少了动态输出节点的电容
4. 可以利用反相器驱动一个泄漏器件抵抗漏电和电荷重新分布
##### a:多米诺门串联链
当预充期间所有多米诺逻辑块输出都置 0时, 求值期间第一个多米诺块的输出保持 0 或从0->1翻转，从而影响 第二个门，<font color="#ff0000">这一影响可以在整个多米诺链上传播</font>，一个接一个， 象崩塌的多米诺骨牌![[support/img/Pasted image 20231231103604.png|500]]
##### b:多米诺逻辑的特点
- ==逻辑求值的传播如同多米诺骨牌的倾倒==
- <span style="background:#d3f8b6">只能实现非反相的逻辑（所有的多米诺门均为非反相逻辑门）</span>
- <span style="background:#d3f8b6">只有一个过渡被优化</span>
- ==多米诺门为无比逻辑，但电平恢复电路为有比逻辑==
- ==动态节点必须在预充电期间完成预充电（这限制了PMOS 的最小尺寸）== 
- 求值期间，==输入必须稳定（对 nlogic 只能有一个上升的过渡）==
- 速度非常快：
	- 在多米诺门中，动态门后面的静态反相器<span style="background:#d3f8b6">可以设计成不对称</span>：因为在求值阶段，反相器的输入端只有1—>0的过渡
	- <span style="background:#d3f8b6">输入电容减小</span>：因而 logical effort 较小
	- <span style="background:#d3f8b6">加大多米诺门中反相器的 PMOS 管可使反相器的VM 上移</span>
	- <span style="background:#d3f8b6">可根据扇出（Fan-out） 情况优化设计多米诺门中的反相器</span>
- 增==加电平恢复电路可以减少漏电和电荷分享问题==
##### c:是否可以取消求值器件(Me)
![[support/img/Pasted image 20231231111713.png]]
**结构和特点:** 
1. 第一级需要求值管
2. 预充需要一级一级进行
3. 避免短路电流，时钟逐级延时
**好处:**
- 减少时钟负载
- 提高下拉驱动能力
**坏处**: 会![[support/img/Pasted image 20231231142640.png#right|避免短路电流|300]]增加预充电周期
	第二个门等到第一个门预充好之后, 因为只1的输出充好电(延时是两个门延时), 才能把第二级的下拉网络关断, 否则充电的电荷全都通过第二级的上下通路短路到GND中去了,  又为了避免这个短路电流, 所以要增加==两级==buffer
##### d:非反相逻辑
多米诺逻辑的⼀个主要局限是只能实现⾮反相逻辑。这⼀ 要求限制了⼴泛采⽤**纯多⽶诺逻辑**。
> [!question] 非反相逻辑
> 为了解决串联动态电路的问题, 我们提出了多米诺逻辑, 即: 动态信号的门只能产生0-->1的变化. 
> ==注意!!!!!!!!!!!初始, 也就是第一级的输入信号是可以从1变成0的, 因为输入的信号比较陡峭, 而且相对于时钟(也就是时钟变为1了, 下边的求值管打开了的时候, 我应该保证已经跳过了NMOS的阈值电压, 否则就有可能漏电).==
> <font color="#ff0000">但是, 不能从之后的级输出反相信号.</font>

**解决方案**
1. **逻辑变换**
    ![[support/img/Pasted image 20231231131528.png|400]]
    在这个例子里边, $\{(ABCD)' \cdot E' \cdot[(G+H)\cdot F]'\}'=ABCD+E+GF+HF$意思就是, 通过逻辑表达式的转化, 最后写成的与或的形式之中, 不会出现$AB'$的情况. 不能在中间使用单纯的反相器产生一个信号的非 只能是处理非反相的信号(这就叫做纯多米诺逻辑).
2. **双轨多⽶诺(Dual-rail domino)**
    最主要的想法就是, 把所有的<font color="#ff0000">初始信号</font>, ==都加一个反相器==, 这样就有了A和A', 我只要利用我的双轨多米诺, 就可以把互补信号, 做成互补输出, 因此就能满足后面的逻辑. 这里可以提现多米诺的原理, 就是, ==后面要用非信号, 那么我就从第一级就开始引过来, 不在中间产生.==
    电路事先原理类似于前⾯讨论的 DCVSL 结构，<span style="background:#d3f8b6"><font color="#ff0000">但它采⽤⼀个预充电负载⽽不是⼀个静态交叉耦合的PMOS 负载。</font></span>图6.67为⼀个 AND/NAND 差分逻辑门的电路图。注意，所有输⼈来⾃其他的差分多⽶诺门。它们在预充电阶段为低电平，⽽在求值期间完成有条件的0⼀1 翻转。
	1. 采⽤差分逻辑<span style="background:#d3f8b6">有可能</span>实现任何随意的功能。
	2. ==但这是以增加功耗为代价的，因为不论输入为何值, 每个时钟周期必定有⼀次翻转. 这是因为 $O$或者$\overline O$定有⼀个要发⽣0--->1 的翻转。==
	3. 晶体管$M_{f1}$和$M_{f2}$的作用是: 在时钟较长时间处于⾼电平时仍保持该电路为静态(泄漏器)。
	4. 这⼀电路<span style="background:#d3f8b6">不是有⽐电路</span>，尽管存在 PMOS 上拉器件！
	![[support/img/Pasted image 20231231140542.png]]
##### d:NP-CMOS
![[support/img/Pasted image 20231231142838.png]]
![[support/img/Pasted image 20231231142948.png]]
# 四、总结                                                                                                    
**静态CMOS设计**
1. 互补CMOS
	- PDN和PUN的对偶关系
	- N管和P管的阈值损失
	- 大扇入时的设计技巧
	- <font color="#ff0000">组合逻辑链的性能优化（逻辑努力的概念）</font>
2. 有比逻辑
	- 伪NMOS逻辑
	- DCVSL逻辑
3. 传输晶体管逻辑（Pass-Transistor Logic）
	- 传输管输出驱动的问题
	- 互补传输管逻辑（CPL）
	- 传输门逻辑（TG）
**动态CMOS**
1. <font color="#ff0000">动态逻辑门的特点</font>
2. <font color="#ff0000">动态逻辑门的静态、动态特性</font>
3. <font color="#ff0000">动态逻辑门功耗上的优劣势</font>
4. <font color="#ff0000">动态逻辑门的信号完整性分析</font>
5. 动态逻辑门的串联和多米诺逻辑
参考网站:
1. [Cornell University ECE4740](https://ocw.ece.cornell.edu/ece-4740-course-details/ece-4740-lecture-notes-and-handouts/)
2. [墨魂](https://mohun-8052.github.io/2022/05/01/数字集成电路（8）动态CMOS/#2-多米诺逻辑)
