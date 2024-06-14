[[Fishing/First_Sem_Master_1/数大/数大|数大]]
# L05~L06: CMOS 反相器_NOTE
## 一、引言
### 1.CMOS 反相器和版图
#### (1)基本版图
![[support/img/Pasted image 20231107142127.png|380]]
**知识点**:
1. 在模拟之中,有特殊需要的版图,需要优化的才手工画,数字电路之中一般是在 IP 的标准库直接进行 mapping(映射)
2. D 和 S 在做完,使用之后才定下来
3. 区分长度 L,宽度 W 到底是哪个方向?长度一般就是沟道的长度,宽度是沿着纸面往里延伸
4. 金属的作用?金属一般是通过打孔,进行赋予电压和电路连线的
5. 注意这个 SDDS

![[support/img/Pasted image 20231107143257.png#center|侧切视图|380]]
> [!question] 怎么看懂这个版图?
> 还有怎么侧切这个图里边没有 N well 啊

**棍棒图(stick diagram)**: 只显示拓扑结构,但是没有维度![[support/img/Pasted image 20231107142713.png#center|棍棒图|200]]

#### (2)两个 CMOS 反相器相连的版图
![[support/img/Pasted image 20231107142808.png|380]]
**知识点:**
1. 注意用 metal 吧 1.out 和 2.in 连接起来了
2. 为了工艺制造上的方便, 两个 P well 连在一起了
3. 两个共享电源和地线,**高度是一样的**,因此可以拼在一起,因此节省了空间
4. 注意,在实际的大规模的版图之中, 都是背靠背的,也就是 VDD - GND - VDD -GND 这样进行相连的
> [!question] 单元之间的空隙, filler, ic 设计与方法提到过
> 
#### (3)补充知识
![[support/img/Pasted image 20231107164028.png#center|设计者和 fab 的文件交互流程|400]]
##### A 设计规则
**Design Rule:**
1. 设计者与工艺工程师之间的接口
2. 制造mask 的指南
3. 最基本的要素:最小线宽(线间距)
###### a 层内规则
![[support/img/Pasted image 20231107143959.png|+grid]]![[support/img/Pasted image 20231107163347.png|+grid]]
**知识点:**
1. 找图中的 0 or 6的意思就是要么合在一起,要么至少是 6
2. 0or6 和 9 的区别就是就在于那个 different <span style="background:#affad1">potential</span> #question 
3. 对于这些层名称的解释详见[[Fishing/First_Sem_Master_1/数大/关键问题/版图|版图]]
4. 注意往里指的箭头和往外的箭头的区别
5. 右图指的是伸出来的必须要大于 3
###### b 层间规则
![[support/img/Pasted image 20231107163520.png|+grid]]![[support/img/Pasted image 20231107163543.png|+grid]]
**知识点:**
1. via 和 contact 的区别
2. 为什么有一些对孔的限制?
3. Substrate:衬底
##### B EDA工艺层视图
![[support/img/Pasted image 20231107143815.png|+grid]]![[support/img/Pasted image 20231107143918.png|+grid]]
**知识点:**
1. #question 和实际的工艺层不是一一对应的,由虚拟和过渡层

##### C DRC
![[support/img/Pasted image 20231107164003.png|380]]


### 2.CMOS反相器的特性简介
#### (1)开关模型
##### A 静态特性
![[support/img/Pasted image 20231107194027.png|380]]

##### B 动态特性
![[support/img/Pasted image 20231107194043.png|380]]
#### (2)VTC
##### A 坐标变换法
**思路**:你看啊,P 和 N MOS连接在了一个电路之中, 因此如果要能把两者的特性曲线通过坐标变换之后放在同一个坐标系里边, 交点就是符合两个管子特性曲线的工作曲线.![[support/img/Pasted image 20231107195017.png#right|CMOS 结构|95]]
从右图中可以看到,上边 PMOS 的坐标曲线的 x 坐标应该是$V_{DSp}$, y 坐标应该是$I_{Dp}$, 参数坐标应该是$V_{GSp}$; 下边 NMOS 的坐标曲线的 x 坐标应该是$V_{DSn}$, y 坐标应该是$I_{Dn}$, 参数坐标应该是$V_{GSn}$. 使用以下的坐标变换:$$I_{Dp}=-I_{Dn},V_{GSp}=V_{in}-V_{DD},\quad V_{DSp}=V_{out}-V_{DD}$$
依据以上的坐标变换, 就能够得到以下的图像
![[support/img/Pasted image 20231107195731.png|500]]
这样, 我们就把 PMOS 的的横纵坐标变成了以上图最右边的样子, 一看这不就是 NMOS 的横纵和参数坐标吗?这样,把这两个图画在一起就有了:![[support/img/Pasted image 20231107200002.png|400]]
以上这个图中, 参数坐标对应一样的(条件一样)的交点就是 VTC.  但是注意,要把参数坐标变成 VTC 中的横坐标.
##### B VTC曲线的分析
###### a 五个点的定性分析
首先应该在上边的交点图上,多画一对曲线. 这样就有了五个点,否则只有四个点.   
*第一个点*:此时 PMOS 是知道 D/S 之中一个为 VDD, G 的电压为 0, 因此要么是饱和区, 要么是线性区. 
	#question<span style="background:#affad1"> 如何去确定到底是线性还是饱和?</span>
	感觉还是要解方程啊. 从结果来看的话,此时的输出是 VDD, 因此$V_{DSp}$几乎是 0. 所以说这个条件就是线性的条件. 由于栅压是 0,所以说NMOS肯定是截止,  无论是和 D 还是和 S 比. <span style="background:#affad1">就是不知道如何从数学上推出来,不知道悬空如何解释?</span>  
*第二个点*:这个点对应图中斜率是-1 的点, 从交点的的图上来看, 对应的红线(NMOS)一定是饱和的, 但是对于蓝线, 在图上来看, 不一定是线性还是饱和. <span style="background:#affad1">在计算上如何确定这种不确定性?</span>  
*第三个点*:这个点一定是俩都饱和,数学上还是不知道怎么办.  
之后的第四个和第五个同理  
![[support/img/Pasted image 20231107200332.png|400]]
###### a 几个关键点的定义
![[support/img/Pasted image 20231107211306.png|+grid]]![[support/img/Pasted image 20231107211322.png|+grid]]
- $V_M$ 开关阈值: ${V}_{{M}}=f({V}_{{M}})$
- $V_{OH}$ 高输出电平:$\mathrm{V}_{\mathrm{OH}}=f(\mathrm{V}_{\mathrm{OL}})$
- $V_{OL}$ 低输出电平:$\mathrm{V}_{\mathrm{OL}}=f(\mathrm{V}_{\mathrm{OH}})$
- $V_{IH}$ :斜率等于-1时的,较低的那个输入电压
- $V_{IL}$ :斜率等于-1时的,较高的那个输入电压
###### b 噪声容限 Noise Margins
**串连的反相器**门的噪声定义  
<u>噪声容限越大，逻辑门的抗干扰能力就越强</u>![[support/img/Pasted image 20231107211621.png#right|噪声容限示意|0150]]
高噪声容限 Noise margin high:
$$NM_{H}=V_{OH}-V_{IH}$$
低噪声容限 Noise margin low:
$$NM_{L}=V_{IL}-V_{OL}$$

##### C 再生性 Regenerative Property
当第一级的输入偏离额定电平时，后面各级能否逐渐收敛回到正确的电平？如果可以的话,就是具有再生性.其中的一个例子对应 PPT 上的是课本 P15 页的例子,明白 v0,v1,v2 的含义
下图是一个反相器的链,第一级的输出是 v1,输入是 v0; 第二级的输出是 v2,输入是 v1.<span style="background:#affad1">如何放在同一个坐标之中啊</span>,我能理解 y 轴是一个,但我不能理解 x 轴啊,除非你说第一个和第三个是等效的

![[support/img/Pasted image 20231107230229.png|300]]

放在一个图之中就有了下图:
![[support/img/Pasted image 20231107230503.png|400]]
其中是否具有再生性的关键就是:<font color="#ff0000">过渡区增益的绝对值大于1</font>  
#question 什么是过渡区?
#### (3)其他特性
##### A 扇入和扇出Fan-out/in
参考:![[support/img/Pasted image 20231107233033.png#right|扇入和扇出|300]]
1. 教材的 P17.   
2. https://zhuanlan.zhihu.com/p/620111029
3. [https://blog.csdn.net/gtatcs/article/details/8922192](https://blog.csdn.net/gtatcs/article/details/8922192)
定义:
1. 扇出：输出端连接（同类）门的个数
2. 扇入: 输入端的个数
**知识点:**
1. 增加扇出,会影响逻辑输出电平
2. 使得**负载门**的*输入电阻*尽可能大(也就是*输入电流*尽可能小), 并保持**驱动门**的*输出电阻*较小(即负载电流对输出电压的影响),可以使得 1. 中的影响最小.
3. 增加扇出, 会使驱动门的动态性能变差,因此很多单元定义了最大的扇出数
4. 扇入增大也会使动态和静态性能变差
##### B 理想门的特性

![[support/img/Pasted image 20231107233204.png|400]]
#### (4)早期的反相器--NMOS 反相器
在 20 世界70 年代的时候使用 NMOS 反相器,因为 <span style="background:#affad1">NMOS 的迁移率比较高</span>
![[support/img/Pasted image 20231107233442.png|400]]
**知识点:**
1. 为什么是两个 nmos 的形式?
   - 首先看上边的这个 nmos,它的 D 和 G 是连在一起的(<span style="background:#affad1">咋区分的这里的 DS</span>), 而且都是最高的电平,因此要么是截止要么是饱和导通. 能够截止的原因是因为寄希望于能够存在某个$V_{in}$能够使得上边 nmos 的 GS 电压小于阈值电压. 
   - 下面的 nmos 也是上边是 D 下边是 S.  
	a. <font color="#ff0000">如果 in 是高电平</font>,因此下面的管子一定可以导通. <span style="background:#affad1">导通的时候相当于啥玩意啊,那个电阻咋算的?</span>.
		假设上边是截止, 那么根据分压肯定就是 out=0, 但是out 等于 0 的时候上边也是饱和导通,因此矛盾.所以上边不可能截止
		假设上边是饱和导通,那么分压不是很好说.从结果来看,既然是反相器对吧,那么就要输出低电平, 而上边又不可能截止,因此只能让上边的电阻(饱和导通)大于下边的电阻. 还是从结果来看,既然输出的是低电平,那么下边一定是线性电阻区.问题就是线性电阻区和饱和导通的 MOS 管的电阻怎么算了---详见--[[Fishing/First_Sem_Master_1/数大/第三章 器件|第三章 器件]]. 先从定性上来分析的话, 饱和导通时电压增大,电流不变,所以就是电阻增大. *所以等效电阻的话,饱和区大于线性区.* 因此,上边的分压大,所以输出的$V_{out}$就是一个接近于 0 的分压, 但是不为 0   
	b. <font color="#ff0000">如果 in 是低电平</font>, 下边的管子一定是截止的,只要上边不是也截止就能保证输出的是高电平. 所以我们假设输出的是高电平, 上边的也确实是截止了. 但这里不对, 上边截止是要求 out 一定要比 4.3V 还要大, 如果是小于 4.3V 的话, 那么上边依旧是饱和导通, 电阻小于截止区的电阻, 但是不为 0, 这样的话确实也可以输出一个高电平. 实际的情况是后一种.<span style="background:#affad1">前一种为啥不行?(不能双截止分压?)</span>
2. 综上所述, 上边的一直是处于饱和导通状态,相当于是一个电阻

#### (5)CMOS 的特点
1. CMOS输出高电平和低电平分别为VDD和GND。信号电压摆幅等于电源电压，噪声容限很大。
![[support/img/Pasted image 20231108000920.png|350]]
2. 无比逻辑
	1. 逻辑电平与器件尺寸无关，晶体管可采用最小尺寸。
	2. 电路翻转时，不会因为尺寸设计的原因而出现翻转错误。
3. 具有低输出阻抗, 稳态时在输出和VDD 或 GND™2 之间总存在一条具有有限电阻 的通路，对噪声和干扰不敏感。
![[support/img/Pasted image 20231108001110.png|400]]
4. 输入电阻极高
	1. 不消耗直流输入电流
	2. 理论上可以驱动无限多个门。
	![[support/img/Pasted image 20231108001253.png|400]]
5. 不考虑泄漏功耗的情况下，没有静态功耗
	![[support/img/Pasted image 20231108001324.png|400]]
	![[support/img/Pasted image 20231108001431.png|400]]
	虽然说那里有一个一下子的下降,但是还是回去了,<span style="background:#affad1">为啥</span>
## 二、静态特性分析
### 1.开关阈值的求解
#### (1)$V_M$的求解
$V_M$的特点是输入等于输出,在图中来看就是,NMOS 和 PMOS 的 ${V_{GS}}{=V_{DS}}$, 因此不是饱和导通就是截止. 稍微动脑子就知道肯定不是截止, <font color="#ff0000">所以两个管子都是饱和导通</font>.   
假设电源电压足够大,处于**速度饱和**,即:$$V_{DSAT}<V_{M}-V_{T}\tag1$$
由电路的串联关系可得:$$I_{Dn}=-I_{Dp}\tag2$$
 联立式(1)(2),并忽略沟长调调制效应,可得:$$k_nV_{DSATn}\left(V_M-V_{Tn}-\frac{V_{DSATn}}2\right)=-k_pV_{DSATp}\left(V_M-V_{DD}-V_{Tp}-\frac{V_{DSATp}}2\right)\tag3$$
 注:此处利用了手工分析模型(再谈速度饱和的那个式子), 把$V_{GT}$稍微改一下就行. 解得: $$V_M=\frac{\left(V_{Tn}+\frac{V_{DSATn}}2\right)+r\left(V_{DD}+V_{Tp}+\frac{V_{DSATp}}2\right)}{1+r}\tag4$$
 其中, $$r=\frac{k_pV_{DSATp}}{k_nV_{DSATn}}=\frac{\upsilon_{satp}W_p}{\upsilon_{satn}W_n}\tag5$$
#### (2)结果分析
 1. *假设*$V_{DD}>>V_{Tn}或说(|V_{Tp}|)$且$V_{DD}>>V_{DSATn}或说(|V_{DSATp}|)$, 则可以得到:$$\begin{aligned}V_M=&\frac{\left(V_{Tn}+\frac{V_{DSATn}}2\right)+r\left(V_{DD}+V_{Tp}+\frac{V_{DSATp}}2\right)}{1+r}=\frac{rV_{DD}}{1+r}=\frac{V_{DD}}{1+1/r}\end{aligned}\tag6$$
      如果令$V_M\boldsymbol{=}V_{DD}/2$,也就是阈值电压是电源的一半,和(6)联立就得到$$r=1$$
      带入 r 的定义,也就是式子(6),把 k 拆开为$k=k^{'}W/L'$可得到$$\left(W/L\right)_p/\left(W/L\right)_n=\left(V_{DSATn}k^{^{\prime}}_n\right)/\left(V_{DSATp}k^{^{\prime}}_p\right)$$
      这个式子就表明了pmos 和 nmos 的尺寸之比.
 2. *若没有这个假设*,则通过式(3)可以直接得到:$$\frac{\left(\boldsymbol{W}/\boldsymbol{L}\right)_p}{\left(\boldsymbol{W}/\boldsymbol{L}\right)_n}=\frac{k^{^{\prime}}{}_nV_{DSATn}\left(V_M-V_{Tn}-V_{DSATn}/2\right)}{k^{^{\prime}}{}_pV_{DSATp}\left(V_{DD}-V_M+V_{Tp}+V_{DSATp}/2\right)}$$
 3. 从图中了解一下n,p尺寸之比对 阈值电压$V_M$的影响
    比较直观的分析时式(5)中的第二个等号后面的东西加上简化假设之后的式(6).  
    r 不看饱和速度的话, r 可以直接当做 p/n 的尺寸比, r 变大, 分母变小, 阈值电压变大.也就是 p 管子相对于 n 管子尺寸越大, 阈值电压越大. 
    - 在直观上进行分析, 上边的尺寸变大, 电阻变小, 都饱和导通, 上边分压变小, 所以阈值电压变大
    - 在实际计算之中 P122 页有一个例子, .25微米之下, 尺寸比大概 3.5 的时候, 阈值电压为 Vdd 的一半
    - <span style="background:#affad1">Vm 时电压摆幅一半的时候, 高低电平的噪声余量相同, 噪声容限最大</span>
     ![[support/img/Pasted image 20231108111617.png|500]]
#### (3)根据$V_M$的反相器分类
如果输入信号的<span style="background:#affad1">零值</span>受到噪声的干扰,输出也会受到很大的干扰,如果采用不对称的阈值,则可以去掉这一个干扰.
- 改变阈值并不容易, 看上边的半对数坐标
- 尤其在 <span style="background:#affad1">VDD/晶体管阈值 比较小的时候</span>, 更难改变
![[support/img/Pasted image 20231108111902.png|550]]
#### (4 )长沟器件的开关阈值
对长沟器件或电源电压较低情形，不会发生速度饱和,即$V_M-V_T{<}V_{DSAT}$, 因此得到$$\begin{aligned}V_{_M}=\frac{V_{_{Tn}}+r(V_{_{DD}}+V_{_{Tp}})}{1+r}\quad\quad\text{其中}\quad r=\sqrt{\frac{-k_p}{k_n}}\end{aligned}$$

### 2.噪声容限的求解
既然已知噪声容限的公式是:$$\begin{aligned}NM_H&=V_{OH}-V_{IH}\\NM_L&=V_{IL}-V_{OL}\end{aligned}$$
那么求取问题的关键就是知道 k=-1 的时候的电压,也就是$V_{IH}$和$V_{IL}$. 
#### (1)求解方法一
1. 第一步
	**假设N管和P管的状态**, 并列出电流相等的方程$$I_{Dn}(V_{in},V_{out})=-I_{Dp}(V_{in},V_{out})\tag7$$
2. 第二步
	对式(7)两边求导得到$$\begin{aligned}&\frac{\partial I_{Dn}(V_{in},V_{out})}{\partial V_{in}}=-\frac{\partial I_{Dp}(V_{in},V_{out})}{\partial V_{in}}\\&\frac{\partial V_{out}}{\partial V_{in}}=-1\end{aligned}\tag8$$
3. 第三步
	结果代入第一步,验证假设是否成立.最后求得噪声容限
#### (2)求解方法二
使用分段线性近似的方法![[support/img/Pasted image 20231111004723.png#right|求解方法二|150]]
1. 求解$V_M$
2. 求解$V_M$处的增益 g
3. 求解$V_{IH}$和$V_{IL}$
	$$\begin{aligned}V_{IH}&=V_M+\frac{V_M}{|g|}\\V_{IL}&=V_M-\frac{V_{DD}-V_M}{|g|}\end{aligned}\tag9$$
4. 求得噪声容限
🌰
- 求解增益 g(<font color="#ff0000">VTC 曲线的斜率啊</font>)
   设NMOS和PMOS都工作在速度饱和区，考虑沟道长度调制效应
	$$\begin{aligned}
   &k_nV_{DSATn}\Bigg(V_{in}-V_{Tn}-\frac{V_{DSATn}}2\Bigg)\Bigg(1+\lambda_nV_{out}\Bigg)+ k_pV_{DSATp}\Bigg(V_{in}-V_{DD}-V_{Tp}-\frac{V_{DSATp}}2\Bigg)\Bigg(1+\lambda_pV_{out}-\lambda_pV_{DD}\Bigg)=0
   \end{aligned}$$
	$$\frac{dV_{out}}{dV_{in}}=-\frac{k_nV_{DSATn}(1+\lambda_nV_{out})+k_pV_{DSATp}(1+\lambda_pV_{out}-\lambda_pV_{DD})}{\lambda_nk_nV_{DSATn}(V_{in}-V_{Tn}-V_{DSATn}/2)+\lambda_pk_pV_{DSATp}(V_{in}-V_{DD}-V_{Tp}-V_{DSATp}/2)}\tag{10}$$

	忽略二次项,  并假设$V_{in}{=}V_{M}$. 得到
	$$g\thickapprox-\frac1{{I_D(V_M})}\frac{k_nV_{DSATn}+k_pV_{DSATp}}{\lambda_n-\lambda_p}=-\frac{1+r}{(V_M-V_{Tn}-V_{DSATn}/2)(\lambda_n-\lambda_p)}\tag{11}$$
   **实例**
   ![[support/img/Pasted image 20231111005647.png|+grid]]![[support/img/Pasted image 20231111092155.png|+grid]]
   <span style="background:#affad1">较大的误差发生于此处</span>,意思是分段线性那一步导致的误差要大于上边式10->11那一步的<span style="background:#affad1">忽略二次项和有关</span>近似假设的结果.见课本P124
### 3.稳定性的进一步分析
#### (1)器件参数的变化
1. 存![[support/img/Pasted image 20231111095310.png#right|V_T corner|200]]在哪些影响器件**数值参数**的外在条件或者工艺误差呢?
	- *温度*在一个很大的范围内变化
	- 制造过程中的偏离
2. 好在 CMOS反相器 的**直流特性**对上述的变化十分不敏感. 这里就分"好的/坏的"PMOS 和"好的/坏的"NMOS 的组合. 好坏通常用 FAST 还是 SLOW 的区别.
	- Good: $t_{ox}$小, L 小, W 大, $V_T$小
	- Bad: $t_{ox}$大, L 大, W 小, $V_T$大
		- $t_{ox}$是栅氧层的厚度
		- #question <span style="background:#affad1">这里如何知道好坏对应的这四个参数到底是大还是小</span>, *我感觉是好就是响应快对应 fast*, 是不是这样??
3. 造成的影响
	- 造成了曲线的平移,注意**平移的方向,会判断**
	- 对于形状没有什么影响
	- 对于输出的高低电平没什么影响
#### (2)电源电压
#question <span style="background:#affad1">器件尺寸的连续缩小导致电源电压也也降低, 但是期间的阈值电压(区分阈值电压和开关阈值)却没有什么变化,这对器件的工作稳定性有什么影响呢?</span>
- 电压降低的时候还能否正常工作?
	1. 首先观看式(11)
	   对于固定的晶体管尺⼨⽐r, $V_M$正比于电源电压$V_{DD}$可以发现反相器的增益 g 随着电源电压的降低而增大.
	2. 反相器在电源电压在接近*晶体管阈值*的时候仍能够很好地工作, 在图中电源电压都是 0.5V 了, 只比阈值电压高 100mV 的时候, 还能够很好地工作.并且此时的 VTC 曲线很好,过渡区的宽度只是电源电压的10%. 而⽽电源电压为2.5 V时, 它加⼤到17%. 
- 电源电压的降低是不是存在一个限制?
	1. 不加区分地降低电源电压虽然对减少能耗有正⾯影响，但它绝对会使*⻔的延时加⼤*。
	2. 一旦电源电压和本征电压(阈值电压)变得可⽐拟，*直流特性对器件参数*(如晶体管國值）的变化就变得越来越*敏感*。
	3. 降低电源电压意味着*减⼩信号摆幅*。虽然这通常可以帮助<span style="background:#affad1">减少系统的内部噪声(如由串扰引起的噪声）</span>，但它也使设计*对并不减⼩的外部噪声源更加敏感*。
- 如果进一步降低$V_{DD}$会如何?
	1. 在$V_{DD}$降低至200mV左右，还能保持反相器的特性(**200mV 怎么开启啊**)不开启, 但是看下一条中的亚阈值电流
	2. 亚阈值电流使得反相器可以在高电平和低电平间翻转，提供了足够的增益得到较理想的VTC曲线
	3. 开关电流值很低决定了反相器⼯作得⾮常慢，但这也许在某些应⽤（例如⼿表）中可以接受
	4. 当接近 100 mv 时，我们开始看到门的特性变得很差。$V_{OL}$和$V_{OH}$不再等于电源的两个电平，并且过渡区的增益接近1。为了能达到足够的增益以用于数字电路，必须使电源至少等于第3章所说的热电势的两倍. 此当低于这⼀电压时，热噪声也会成为⼀个问题⽽可能引起不可靠的⼯作。我们把这⼀关系表⽰为$$V_{DDmin}>2\cdots4\frac{kT}{q}$$
	   这个式子写出了电源电压的最小值, 大概是 2~4 倍的热电势. 上式代表了电源电压降低的实际限制。这意味着使 CMOS 反相器工作在 100mV 以下的唯一方法是降低环境温度，从而减小热电势. 
	   ![[support/img/Pasted image 20231114185900.png|500]]
### 4.小结
![[support/img/Pasted image 20231114192317.png|400]]
## 三、动态特性分析
### 1.负载电容分析与计算
CMOS反相器的传播延时由P管和N管对负载电容充放电时间决定,设所有的电容⼀起集总成⼀个单个的电容$C_L$,其处于$V_{out}$和 GND 之间,其简化模型如下![[support/img/Pasted image 20231114192737.png|200]]
一对串联的反相器的电路图和详细的寄生电容分布情况如下.
![[support/img/Pasted image 20231114192800.png|450]]
假设输入$V_{in}$由⼀个上升和下降时间均为零的理想电压源所驱动. 只考虑连⾄输出节点上的电容时，$C_L$分成以下几个部分  
#👁 这里 PPT上没有,只有课本上有
### 2.传播延时分析
#### (1)开关模型
把 CMOS![[support/img/Pasted image 20231114193711.png#right|开关模型|100]] 等效成一个开关, 右图是1->0 的时候的等效电路. 由此模型可以得出    
$$t_p=\frac{t_{pHL}+t_{pLH}}2=0.69{\left(\frac{R_{eqn}C_{L,HL}+R_{eqp}C_{L,LH}}2\right)}\approx0.69C_L{\left(\frac{R_{eqn}+R_{eqp}}2\right)}\tag{12}$$
其中已经假设$C_{H,HL}=C_{L,LH}=C_L$, 接下来计算等效电阻:$$\begin{aligned}
\boldsymbol{R}_{eq}& \begin{aligned}=\frac1{V_{DD}/2}\int_{V_{DD}/2}^{V_{DD}}\frac V{I_{DSAT}\left(1+\lambda V\right)}dV\end{aligned}  \\
&\approx\frac34\frac{V_{DD}}{I_{DSAT}}\left(1-\frac79\lambda V_{DD}\right)\approx\frac34\frac{V_{DD}}{I_{DSAT}}\left(1-\frac56\lambda V_{DD}\right)
\end{aligned}\tag{13}$$
其中$I_{DSATn}$为: $$I_{DSAT}=k^{\prime}\frac WL{\left((V_{DD}-V_T)V_{DSAT}-\frac{{V_{DSAT}}^2}2\right)}\tag{14}$$
忽略式(13)之中的括号内的部分,计算$t_{pHL}$,有$$\begin{gathered}
t_{pHL} =0.69C_LR_{eqn}\overset{\quad}{\operatorname*{=}}0.69\times\frac34\frac{C_LV_{DD}}{I_{DSATn}}=0.52\times\frac{C_LV_{DD}}{I_{DSATn}} \\
=0.52\times\frac{C_LV_{DD}}{\left(W/L\right)_nk^{^{\prime}}{}_nV_{DSATn}\left(V_{DD}-V_{Tn}-V_{DSATn}/2\right)} 
\end{gathered}\tag{15}$$
若假设$V_{DD}>>V_{Tn}+V_{DSATn}/2$, 则有$$t_{_{pHL}}=0.52\times\frac{C_{L}}{(W/L)_{n}k^{^{\prime}}{_n}V_{_{DSATn}}}\tag{16}$$
**得到结论**:
- 在以上近似前提下,高$V_{DD}$下，传播延时与电源电压基本无关
- 高$V_{DD}$下，<span style="background:#affad1">由于沟长调制</span>，提高$V_{DD}$会使传播延时略有减小
![[support/img/Pasted image 20231114195558.png|450]]
![[support/img/Pasted image 20231114195730.png|450]]
上图的$R_{eq}$与$t_p$和 V_DD 的关系基本是一样的, 因为负载电容假设是一个定值.   
#question <span style="background:#affad1">不太懂上图中为啥电源电压很高的时候, 调制了沟道, 所以才有那个变化?</span>
### 3.性能设计优化
观察公式$$t_{_{pHL}}=\frac{0.52\times C_LV_{_{DD}}}{\left(W/L\right)_nk^{^{\prime}}_nV_{_{DSATn}}\left(V_{_{DD}}-V_{_{Tn}}-V_{_{DSATn}}/2\right)}$$
可以发现为了减少延时, 有以下的方法  
1. 减小负载电容
   - 自身的扩散电容（漏区面积尽可能小）
   - 连线电容
   - 扇出电容  
	前两个和版图设计有关
   其中前两个可以通过**合理的版图设计**进行减小, 后一个没啥办法
2. 加大晶体管尺寸(W/L)
   - 优点：增加了驱动能力（增大充放电电流，降低导通电阻） 
   - 缺点：扩散电容增大，从而使C增大 (Self-loading) 
   - 缺点：栅电容增加，使前一级的扇出电容增加
3. 提高电源电压$V_{DD}$
	- $V_{DD}$增加到一定程度, 对延时优化效果不明显
	- 功耗增加
	- 出于安全的考虑, $V_{DD}$有严格的上限
#### (1)第一种情况
求解单个对称反相器 （${t_{pHL}}{=t_{pLH}}$）驱动固定负载$C_{ex}$时的延时$t_p$与反相器尺寸放大系数S的关系：![[support/img/Pasted image 20231114210225.png|300]]
该反相器的**负载电容**$C_L$分为两个部分:$$C_L=C_{int}+C_{ext}$$
其中, $C_{int}$为*本征电容*（自载电容）, 包括漏扩散电容和栅漏覆盖电容;   
	 $C_{ex}$为外部*负载电容*, 包括扇出电容和连线电容  
因此**传播延时**为:$$\begin{gathered}
_p=0.69R_{eq}(C_{int}+C_{ext})=0.69R_{eq}C_{int}(1+C_{ext}/C_{int}) \\
=t_{p0}(1+C_{ext}/C_{int}) 
\end{gathered}$$
其中, $t_{p0}$为*本征延时*, 即外部负载电容为 0 的时候的延时
<u>分析以上的式子</u>, 主要考虑面积 S 的影响  
以最小尺寸反相器作为参考，考虑一个**尺寸系数为 S** 的反相器的延时. 设最小尺寸的本征电容为$C_{iref}$,导通电阻为$R_{ref}$. 尺寸系数S的意思为<u>P管和N管的附/L相对于最小尺寸反相器均放大了S倍</u>, 则其本征电容为$S\cdot C_{iref}$, 本征电阻为$R_{ref}/S$. 因此这样计算的话, 延时为:$$\begin{aligned}t_p&=0.69(R_{ref}/S)(SC_{iref})(1+C_{ext}/\left(SC_{iref}\right))\\&=0.69R_{ref}C_{iref}(1+C_{ext}/\left(SC_{iref}\right))=t_{p0}(1+C_{ext}/\left(SC_{iref}\right))\end{aligned}$$
从以上的式子可以得到以下的结论: 
1. **本征延时与反相器尺寸无关**，而仅与工艺和版图相关。加大尺寸系数，晶体管的导通电阻减小，但是本征电容增大；
2. 当外部负载电容$C_{ex}$为零时，增加尺寸井不能减小延时。这是因为，驱动能力的提高被自身增加的电容负载抵消；
3. S越大，延时越小 (S无穷大，延时=本征延时）。当S相对${C_{ext}/C_{int}}$足够大时，进一步增加S，延时改善非常有限<font color="#ff0000">（自载效应）</font>，面积却会显著增加。
![[support/img/Pasted image 20231114211837.png|400]]
#### (2)第二种情况
如![[support/img/Pasted image 20231114233713.png#right|两级反相器|200]]右图，求解<u>一个反相器驱动另一个同样的反相器</u>时，设反相器P管的尺寸是N管的$\beta$倍，则第一个反相器的延时$t_p$与$\beta$的关系：  
*设*: 右图中P管的尺寸是N管的$\beta$倍, 即$${\beta=(W/L)_p/(W/L)_n}$$
*设*: 右图的N管电阻为$R_{eqn}$, 又设与该N管具有相同尺寸的P管的电阻为$R_{eqp}$. 则,右图中相对于N管放大了$\beta$倍的P管的电阻为$R_{eq}/\beta$.
*若要求*: 第一个反相器具有对称的上升时间和下降时间, 即$t_{pHL}=t_{pLH}$, 可以得到$$\beta{=}R_{eqp}/R_{eqn}$$
(解释一下以上的意思, 这个$R_{eq}$是相同尺寸的N和P管的等效电阻, 但是在图中是已经放大了的$\beta$倍的管子,所以要求等效电阻有这个关系)
*若要求*:第一个反相器有最小的延时, 首先计算第一个反相器的延时:  
(1)$C_L$的计算:$$\begin{aligned}
C_{L}& =(C_{dp1}+C_{dn1})+(C_{gp2}+C_{gn2})+C_{w}  \\
&=(\beta C_{dn1}+C_{dn1})+(\beta C_{gn2}+C_{gn2})+C_{w} \\
&=(1+\beta)(C_{dn1}+C_{gn2})+C_{W}
\end{aligned}$$
其中$C_{dp1}$和$C_{dn1}$是第一个反相器的NMOS的*漏扩散电容*, 而$C_{gp2}$和$C_{gn2}$为第二个反相器的栅电容, $C_w$为连线电容. 当PMOS器件为NMOS器件的的$\beta$倍的时候, 所有晶体管的电容也等比例的加大, 因此有了第一行到第二行的转化. 
(2)$R_{eq}$的计算: $$R_{eq}=\frac{(R_{eqn}+R_{eqp}/\beta)}2$$
(3)延时$t_p$的计算:$$t_p=0.69C_LR_{eq}=0.69[(1+\beta)(C_{dn1}+C_{gn2})+C_W]\frac{(R_{eqn}+R_{eqp}/\beta)}2$$
(4)求$\beta$等于什么值的时候, 能够有最小的延时:$$\frac{\partial t_p}{\partial\beta}=0\quad\Rightarrow\quad\beta=\sqrt{\frac{R_{eqp}}{R_{eqn}}(1+\frac{C_W}{C_{dn1}+C_{gn2}})}$$
若果有条件:$C_{dn1}+C_{gn2}>>C_{w}$, 则可以得到$$\beta=\sqrt{\frac{R_{eqp}}{R_{eqn}}}$$
这意味着当导线电容可以忽略时，$\beta$等于$\sqrt{\frac{R_{eqp}}{R_{eqn}}}$，这不同于在非串联情形时通常采用的比值r。如果导线电容占主导地位，那么应当采用较大的$\beta$值。这一分析令人奇怪的结果是, 当以对称性及<span style="background:#affad1">噪声容限</span>为代价时，较小的器件尺寸(因而较小的设计面积)得到了较快的设计。
![[support/img/Pasted image 20231115105013.png|400]]
#question <span style="background:#affad1">噪声容限?为啥1.9的平方不是2.4?上图中这个31K/13K是什么</span>
#### (3)第三种情况
**反相器链的设计**  
**适用的情景**: 驱动大负载, 能够是实际使用的一般简化形式  
**问题描述**: 如果给定负载电容$C_L$和第一个反相器的输入电容$C_{g1}$, 要求传输延时$t_p$
最小, 请问
1. 应该用多少级反相器进行驱动
2. 如何设计这些反相器的尺寸?
##### A 一些假设
*设*: 反相器的输入栅电容$C_g$与<span style="background:#affad1">本征输出电容</span>$C_{int}$之间的关系如下:$$C_{int}=\gamma C_{_g}$$
其中, $\gamma$是比例系数, 只与工艺有关, 对于大多数亚微米工艺$\gamma=1$
*因此*: 传播延时为$$t_p=t_{p0}\left(1+\frac{C_{ext}}{C_{int}}\right)=t_{p0}\left(1+\frac{C_{ext}}{\gamma C_{g}}\right)$$
其中, $t_{p0}$为本征延时(无外部负载), 与晶体管的尺寸无关
*设*: $C_{ext}=fC_g$, f为<font color="#ff0000">等效扇出</font>, 因此上式可以变为$$t_p=t_{p0}\Bigg(1+\frac{C_{ext}}{\gamma C_{g}}\Bigg)=t_{p0}\Bigg(1+\frac f\gamma\Bigg)$$
*结论*: 反相器的延时只取决于外部负载电容与输入电容间的比值f.
##### B 延时公式应用于反相器链
![[support/img/Pasted image 20231115111626.png|300]]
反相器链总延时: $$t_p=t_{p,1}+t_{p,2}+...+t_{p,N}\tag{15}$$
第j级反相器的延时为: $$t_{p,j}=t_{p0}\left(1+\frac{C_{g,j+1}}{\gamma C_{g,j}}\right)=t_{p0}(1+f_j/\gamma)\tag{16}$$
把式(16)带入(15)则有: $$\begin{aligned}t_p&=\sum_{j=1}^Nt_{p,j}=t_{p0}\sum_{j=1}^N\left(1+\frac{C_{g,j+1}}{\gamma C_{g,j}}\right)\end{aligned}\tag{17}$$
其中有, $C_{g,N+1}=C_L$
##### C 求每一级的f约束
==给定级数 N , 给定负载$C_L$和给定输入电容$C_{in}$的情况下,确定最优的尺寸比 f==  
式(17)中一共有N-1个未知数: $\boldsymbol{C_{g,2}}\sim\boldsymbol{C_{g,N}}$
求解方法: 求 N-1 个偏微分并令他们都等于0, 即$$\partial t_p/\partial C_{g,j}=0$$
可以得到$$C_{g,j+1}/C_{g,j}=C_{g,j}/C_{g,j-1}$$
上式的意思是, 每一级反相器的尺寸是与其相邻前后两个反相器尺寸的几何平均数, 即$$C_{g,j}=\sqrt{C_{g,j-1}C_{g,j+1}}$$
并且说明了两点
1. 每一级有相同的等效扇出f
2. <span style="background:#affad1">每一级有相同的延时</span>
若每一级具有相同的尺寸放大系数为 f, 则$$f^N=F=C_L/C_{g,1}$$
因此得到: $$f=\sqrt[N]{F}$$
带入式(17)得到延时为$$t_p=Nt_{p0}(1+\sqrt[N]{F}/\gamma)$$
<span style="background:#affad1">注意, 等效扇出变为等效放大系数, 如何变过去的</span>  
🌰![[support/img/Pasted image 20231115113028.png|+grid]]![[support/img/Pasted image 20231115113057.png|+grid]]

##### D 确定级数N和F的关系
==级数 N 未知的情况下，给定负 载$C_L$和给定输入电容$C_{in}$的情况下, 并且规定各级的尺寸系数均为$f=\sqrt[N]{F}$求f的值最优级数 N==
<span style="background:#affad1">刚才那一个是, 给定了N, 求f为什么样子, 结果是给出了表达式以上f和F的表达式. 现在是说N又为止了, 但是还用了N已知时候的f和F的关系, 不理解. </span>应该有的解释是, f和F关系的得出不依赖于N, 因此这里可以接着用.但是这样话就不能算作两种独立的求解条件了.  
各级尺寸放大系数均为 f ，则最小延时为：$$t_p=Nt_{p0}(1+\frac{\sqrt[N]{F}}\gamma)\tag{18}$$
将$t_p$表示为F，f 和γ的函数：$$t_p=Nt_{p0}(1+\frac{\sqrt[N]{F}}\gamma)~=\frac{\ln F}{\ln f}t_{p0}(1+\frac f\gamma)~=\frac{\ln F}\gamma t_{p0}(\frac\gamma{\ln f}+\frac f{\ln f})\tag{19}$$
其中利用了$C_L=F\cdot C_{in}=f^NC_{in}$和$N=\frac{\ln F}{\ln f}$  
由于F是定值,这样就完全把$t_p$表达成了f的函数,因此对f求导$$\frac{\partial t_p}{\partial f}=\frac{\ln F}\gamma t_{p0}\cdot\frac{\ln f-1-\gamma/f}{\ln^2f}=0\tag{19}$$
得到了$$f=e^{\left(1+\gamma/f\right)}\tag{20}$$
这是一个超越方程,按照课本上的叙述主要有两个情况  
1. 只有一个**收敛解**,就是$\gamma=0$的时候,此时的意思是<font color="#ff0000">忽略自载,负载电容=扇出</font>,这样就有$N=\ln\left(F\right), f=e=2.718$,以指数的形式放大各级尺寸,并且因此称为**指数锥形.**  
2. 若包含自载,也就是$\gamma$不为0的时候,只能求**数值解**,如图所示. 
3. 对于$\gamma=1$的典型情形,$f_{opt}=3.6$. 而且下图画出了在$\gamma=1$时, 传播延时(归一化延时)和f(等效扇出)的关系.
![[support/img/Pasted image 20231121104258.png|400]]
<span style="background:#ff4d4f">f=4的最终选择问题</span>($\gamma=1$)
1. 主要看右边那个图, 在f=3.6的时候确实是最小的,但是啊,f加大一些不会明显地增大延时
2. f大还有一个好处就是能够减少反相器的级数和实现面积
3. f小延时增加
综上所述最有扇出为4.  ![[support/img/Pasted image 20240102095729.png|400]]
![[support/img/Pasted image 20240102095748.png|400]]
![[support/img/Pasted image 20240102095809.png|400]]
🌰
![[support/img/Pasted image 20231121104312.png|300]]
**总结**
![[support/img/Pasted image 20231121104329.png|+grid]]![[support/img/Pasted image 20231121104340.png|+grid]]
#### (4)信号斜率对于延时的影响
此前延时的推导基于两个假设：
1. 输入信号是突变的，即上升时间或下降时间为0
2. 两个晶体管不同时导通
实际的情况: 
1. 输入信号是缓变的，因为前级门的驱动是有限的
2. 两个静态管可能同时导通
##### A 为什么会影响延时?
实际上，输⼈信号是逐渐变化的，⽽且 PMOS 和 NMOS 管会暂时同时导通⼀段时间。这会影响所得到的充(放)电总电流，从⽽影响传播延时。

那为啥又会导致充电电流的变化呢, 因为有斜率, 所以说不一直是电源经过NMOS或者PMOS给电容充电,有的时候会直接从俩mos通过对地去了,所以不全是给负载电容充电了.
##### B 斜率导致的影响(反相器延时（高->低）与输入信号上升时间的关系)
斜率的这个时间表达为: <font color="#ff0000">输入信号上升时间或者下降时间</font>  
有关系: $$t_{_{p_{HL}}}=\sqrt{t_{_{pHL}}^2(step)+(\frac{t_{_{rise}}}2)^2}$$
参数解释:
- $t_{pHL}$就是修正之后的,带有斜率的信号的延时
- $t_{pHL}(step)$是阶跃信号(理想信号)输入的时候的延时
- $t_{rise}$是信号上升的时间,表达了斜率
因此有图:![[support/img/Pasted image 20231121110506.png|300]]
从上图可以看出当$t_{\mathrm{~rise}}>t_{pHL}$,近似线性关系. <u>从这个平方相加看出来的吧.</u>   
##### C 最终表达式
<span style="background:#affad1">因此</span>修正反相器的延时为: $$t^i_p=t^i_{step}+\eta t^{i-1}_{step}$$
参数定义: 
- ${t^i}_{p}$: 第i个反相器的传播延时
- ${t^i}_{step},{t^{i-1}}_{step}$: 第 i 个反相器及其前一级门的阶跃输入延时
- $\eta$: 经验常数, 典型值0.25
🌰  ![[support/img/Pasted image 20231121110826.png|400]]
![[support/img/Pasted image 20231121110810.png|400]]
**其他事项**: ![[support/img/Pasted image 20231121110910.png|400]]

### 4. 小结
- 负载电容的计算
	- 本征电容（本级的栅漏电容和漏级的扩散电容）
	- 外部负载电容（下一级的栅电容和引线电容）
- 性能设计优化
	- 反相器延时与负载的关系（第一种情况）
	- 反相器性能最优与上下对称设计（第二种情况）
	- 反相器链的设计（第三种情况）
		- 已知 F 和 N（第一种条件），求 f
		- 已知 F（第二种条件），求 f 和 N
	- 实际设计中要考虑输入信号斜率的影响
## 四、功耗分析和低功耗设计
CMOS反相器的功耗 P 由以下3部分构成：
1. **动态功耗**(Dynamic Power)$P_{dyn}$  
   对负载电容的充电和放电造成的功耗
2. **短路功耗**(Short Circuit Currents)$P_{dp}$  
   开关过程中电源和地之间瞬间的直流通路造成的功耗
3. 静态功耗(Static Power)$P_{stat}$  
   稳定输出高电平或低电平时的直流功耗，漏电流造成
因此总功耗为:$$\color{red}{P_{tot}=P_{dyn}+P_{dp}+P_{stat}}$$
### 1. 动态功耗
#### (1)一次高低翻转
输![[support/img/Pasted image 20231121111501.png#right|等效电路|150]]出从低至高翻转期间的等效电路（忽略输入波形的下降时间） 
- 从电源之中取得的能量: 
$$\begin{gathered}
\boldsymbol{E}_{\boldsymbol{VDD}} =\int_0^\infty i_{VDD}(t)V_{DD}dt=V_{DD}\int_0^\infty C_L\frac{dV_{out}}{dt}dt \\
=C_LV_{DD}\int_0^{V_{DD}}dV_{out}=C_L{V_{DD}}^2 
\end{gathered}$$
- 储存在电容上的能量: $$\begin{aligned}E_C&=\int_0^\infty i_{VDD}(t)V_{out}dt=\int_0^\infty C_L\frac{dV_{out}}{dt}V_{out}dt\\&=C_L\int_0^{V_{DD}}V_{out}dV_{out}=\frac{C_LV_{DD}^2}2\end{aligned}$$
#### (2)时钟周期内
每次从0-->1从电源获取的能量为: $C_LV_{DD}^2$  
因此N个时钟周期的翻转消耗的电源能量为:$$E_N=C_LV_{DD}^2\times n(N)$$
  参数解释:  n(N)为N个时钟周期内0-->1的翻转次数
动态功耗就是的整体时间上的平均功耗,因此就有了$$P_{dyn}=\lim_{N->\infty}\frac{E_N}{N\times T_{clk}}=[\lim_{N->\infty}\frac{n(N)}N]\times{C_L}{V_{DD}}^2f_{clk}={P_{0->1}}{C_L}{V_{DD}}^2f_{clk}\tag{20}$$
  参数解释:$P_{0->1}$输入变化导致输出发生0->1变化的概率,又称为**翻转活动因子**
假设等效电容$$C_{EFF}=P_{0\to1}\times C_L$$
因此式(20)可以变为$$P_{dyn}=C_{EFF}{V_{DD}}^2f_{clk}$$
<font color="#ff0000">动态功耗与晶体管尺寸无关（负载电容确定的情况下）</font>  
🌰  
![[support/img/Pasted image 20231121112803.png|400]]
#### (3)优化
使得右图,能![[support/img/Pasted image 20231121113103.png#right|电路图|200]]量最小的晶体管尺寸设计, 目标如下:  
1. 待设计的参数：尺寸系数f和电源电压$V_{DD}$
2. 在满足$t_{p}\leq t_{pref}$的<span style="background:#affad1">前提</span>下,使得电路的**能耗**最小  
	其中, $y_{pref}$是f=1且$V_{DD}=V_{ref}$的参考电路的传播延时$t_{_{pHL}}=\frac{0.52\times C_{_L}V_{_{DD}}}{\left(W/L\right)_nk^{^{\prime}}{_n}V_{_{DSATn}\left(V_{_{DD}}-V_{_{Tn}}-V_{_{DSATn}}/2\right)}}$
##### A 性能约束
右图电路的延时如下(两个相加嘛)$$t_p=t_{p0}\left(\left(1+\frac f\gamma\right)+\left(1+\frac F{f\gamma}\right)\right)\tag{21}$$
  参数解释: $F=C_{ext}/C_{gl}$, 总等效栅出
而其中$$t_{_{p0}}\propto\frac{V_{_{DD}}}{V_{_{DD}}-V_{_{TE}}}\tag{22}$$$$V_{TE}=V_{T}+V_{DSAT}/2$$
由于要求$t_{p}\leq t_{pref}$, 因此求一下他俩的比值,<font color="#ff0000">最终的结果是求出</font>$\color{red}V_{DD}(f)$$$\frac{t_p}{t_{pref}}=\frac{t_{p0}}{t_{p0ref}}\frac{\left(2+f+\frac Ff\right)}{\left(3+F\right)}=\frac{V_{DD}}{V_{ref}}\frac{V_{ref}-V_{TE}}{V_{DD}-V_{TE}}\frac{\left(2+f+\frac Ff\right)}{\left(3+F\right)}\leq 1\tag{23}$$
其中第一个等号利用了上面的式(21), 其中$\gamma=f=1$.第二个等号利用了式(22).从上式可以总结出两点:
1. $V_{DD}$上升, $t_p$下降. 
2. $f=F^{0.5}$的时候, $t_p$最小.  
如果令式(23)等于1的话,则可求出一个电源电压$V_{DD}$和F, f的关系<font color="#ff0000">.(因为功耗是需要电源电压的呀)</font>, 因此有了下图![[support/img/Pasted image 20231121175540.png|300]]
怎么理解这个图呢? 首先这个图是在延时为最大允许值的条件下求出来的, 纵轴为电源电压$V_{DD}$. 在某一个确定的F之下, 电源电压都是先降后升的, 为啥呢, *因为f在到达*$F^{0.5}$之前, f的上升会导致延时的下降(观察式子23的第一个等号), 因此可以把这些延时的下降牺牲掉,换做电压的下降(电源电压下降, 延时上升.), 这样小号的**功率就下降**了. 在到达$F^{0.5}$之后, f的上升会导致延时的上升, 因此电源电压就不得上升来挽回降低的速度了.这时候, **能耗也就上升**了.  
##### B 计算能量功耗
$$\begin{aligned}E&=V_{DD}^2[\underline{C_{g1}+\gamma C_{g1}}+\underline{fC_{g1}+\eta CC_{g1}}+\underline{FC_{g1}}]\\&=V_{DD}^2C_{g1}[(1+\gamma)(1+f)+F]\end{aligned}$$
第一个下划线: 第一级的输入栅电容和本征输出电容; 第二个下划线: 第二级的输入栅电容和本征输出电容; 第三个下划线: 负载电容. 神奇的是都可以和第一级的输入栅电容建立关系, 因此就有了第二行.  
这样, 和参考两级反相器($\gamma=f=1$)的功耗相比,有了$$\frac E{E_{ref}}=\left(\frac{V_{DD}(f)}{V_{ref}}\right)^2\left(\frac{2+2f+F}{\color{red}{\boxed{4+F}}}\right)$$
太难解了, 画图方法不错,因此![[support/img/Pasted image 20231121182936.png|400]]
##### C 结论
1. 改变器件尺寸并降低电源电压是减少逻辑电路能耗的有效方法。
2. 在最优值之外过多地加大晶体管的尺寸会付出较大的能量代价。
	- 在基于标准单元的半定制设计当中，常常放大晶体管的尺寸以 满足驱动一定范围的负载的性能要求，因此能耗往往比较高。
3. 考虑能量时的最优尺寸系数小于考虑性能时的最优尺寸系数。例如，当F = 20时，$f_{opt}(\text{能量})=3.53,\text{ 而}f_{opt}(\text{性能})=20^{0.5}-4.47。$

### 2.短路功耗
- **定义**: 开关过程中电源和地之间瞬 间的直流通路造成的功耗(因为有斜率,也就是输入波形上升/下降时间不为0 ， NMOS和PMOS管同时导通，VDD 和 GND之间在短时间内出现直流通路)
#### (1)计算短路功耗
![[support/img/Pasted image 20231121183256.png|300]]
开关周期消耗的能量:$$V_{_{DD}}\frac{I_{_{peak}}t_{_{sc}}}2+V_{_{DD}}\frac{I_{_{peak}}t_{_{sc}}}2=t_{sc}V_{DD}{I_{peak}}$$
其中: $$t_{sc}=\frac{V_{DD}-2V_{T}}{V_{DD}}t_{s}\approx\frac{V_{DD}-2V_T}{V_{DD}}\times\frac{t_{r(f)}}{0.8}$$
平均功耗是这个周期内的能耗除以这个周期的时间, 就是<span style="background:#affad1">乘频率</span>$$P_{dp}=t_{sc}V_{DD}I_{peak}f=C_{sc}V_{DD}{}^{2}f$$
其中$C_{sc}=t_{sc}I_{peak}/V_{DD}$   
此外, $I_{peak}$由器件的<font color="#ff0000">饱和电流</font>决定, 因此<font color="#ff0000">直接正比于晶体管的尺寸</font>. 此外峰值电流也与<font color="#ff0000">输入和输出的上升/下降时间之比</font>密切相关.见(2)即可.
#### (2)负载对短路电流的影响
![[support/img/Pasted image 20231121183730.png|400]]
⾸先假设负载电容很⼤，所以输出的下降时间明显⼤于输⼈的上升时间.在这些情况下,输入在输出开始改变之前就已经通过了过渡区。由于在这⼀时期 PMOS 器件的源-漏电压近似为0，因此该器件甚⾄还没有传导任何电流就断开了。这种情况 下短路电流接近于零。现在考虑相反的情况，就是输出电容⾮常⼩，因此输出的下降时间明品⼩于输⼈的上升时间，PMOS 器件的源-漏电压在翻转期间的⼤部分时间内等于$V_{DD}$.从⽽引起了最⼤的短路电流(等于PMOS 的饱和电流)。这显然 代表了最坏情况的条件。

![[support/img/Pasted image 20231121210804.png|400]]

这⼀分析的结论是：使**输出**的上升/下降时间⼤于**输⼈**的上升/下降时间可以使短路功耗减到最⼩。<font color="#ff0000">但输出的上升/下降时间太⼤会降低电路的速度并在扇出门中引起短路电流。</font>
这个例⼦很好地说明了只顾局部优化⽽不管全局是如何会引起不良后果的。
#### (3)小总结
- 对![[support/img/Pasted image 20231121221941.png#right|短路功耗和斜率比值关系|150]]于一个给定尺寸的反相器: 
	- 负载电容**太小**时，功耗主要来自**短路电流**
	- 对非常**大的负载**电容，功耗来自**充/放电负载电容**。
	- 如果是输入和输出的上升/下降时间**相等**，大部分功耗来自**动态功耗**，只有小部分(< 10%)来自短路功耗。
- 电源电压降低时，短路电流的影响减小。若$V_{DD}<V_{tn}+|V_{tp}|$，则可以消除短路电流。
- 从全局优化的角度，应该要求**输入和输出信号的上升/下降时间匹配**来使短路功耗达到最小。
### 3.静态功耗
⼀![[support/img/Pasted image 20231121222142.png#right|漏电流|200]]个电路的静态(或称稳态）功耗可⽤下列关系来表示：$$P_{stat}=I_{stat}\cdot V_{DD}$$
	其中, $I_{stat}$是没有开关活动存在时在电源两条轨线之间流动的电流。
理想情况是 CMOS 反相器的静态电流为零，因为PMOS 和 NMOS 器件在稳态⼯作状况下决不会同时导通。可惜的是，总会有**泄漏电流**流过位于晶体管**源(或漏）与衬底之间的反相偏置的⼆极管结**,
- **漏电电流**与*温度*有关, *亚阈值电流*是漏电电流的主要来源
#### (1)pn结漏电
![[support/img/Pasted image 20231121222212.png|500]]
#### (2)亚阈值漏电
**亚阈值电流**是$V_{GS}$小于阈值电压的时候, 也会有一个阈值电流.  
阈值电压越是接近0v，则在$V_{GS}=0V$时的泄漏电流越⼤，因⽽静态功耗也就越⼤。为了抵消这⼀效应，器件的阈值电压⼀般应当保持⾜够⾼。标准⼯艺的特征值$V_T$V从未⼩于 0.5~0.6 V，有时甚⾄还相当⼤(~0.75 V)。  
随亚微⽶⼯艺尺⼨的缩⼩，同时出现了电源电压降低，因⽽这⼀⽅法受到挑战.我们在前⾯已经总结过(见图 5.17)，<span style="background:#affad1">降低电源电压同时保持阈值电压不变会造成性能的严重损失</span>，特别是$V_{DD}$接近二倍$V_T$的时候.  
为了解决这一问题,可以降低这⼀器件的阈值电压。它意味着由降低电源电压造成的性能损失滅⼩。可惜的是，阈值电压的最低值是由 所允许的亚阈值漏电流的数量所决定的，如图5.35 所⽰。因此阈值电压的选择代表了在性能和<font color="#ff0000">静态功耗之问的权衡取舍</font>。

如下图, $V_{GS}$越大, $V_T$ 越小, $I_D$越大. 
![[support/img/Pasted image 20231121222704.png|400]]
**工艺尺寸缩小 => 电源电压降低（动态功耗下降） => 阈值降低（静态功耗增加）**
![[support/img/Pasted image 20231121231108.png|400]]
#### 4. 综合考虑
总功耗为:$$P_{tot}=P_{dyn}+P_{dp}+P_{stat}=(C_{L}V_{DD}^2+V_{DD}I_{peak}t_s)f_{0->1}+V_{DD}I_{leak}$$
功耗延时积（PDP): $$PDP=P_{a\nu}t_p=\frac{C_L{V_{DD}}^2}{2t_p}t_p=\frac{C_L{V_{DD}}^2}2$$
  1. 衡量切换一个门所需要的的能量
  2. 又被称为电路的优值
  3. 每操作的能耗
  4. 其中假设了门以最快的速度切换
能量延时积（EDP):$$EDP=PDP\times t_p=P_{a\nu}t_p^2=\frac{C_L{V_{DD}}^2}2t_p$$
  1. 同时考虑性能和能耗的衡量
  2. 又被称为电路的品质因子
## 五、总结
![[support/img/Pasted image 20231121231533.png|450]]
