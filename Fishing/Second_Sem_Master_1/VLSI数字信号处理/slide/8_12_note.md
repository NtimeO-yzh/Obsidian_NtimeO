# 第七章 脉动阵列
> [!Summary] 处理的关键问题
> 一个无限的处理流程如何以时间为代价, 放在特定的处理单元上进行处理, 并且再合适的时间调度这些PE

### 脉动阵列与各种变换技术的结合
利用==*相同的投影矢量$d$和PE空间矢量$p^T$、但是不同的调度矢量$\mathbf{s}^{T}$*==得到的FIR脉动架构，可以通过各种变换(边反转、结合、减速、重定时和流水线)互相导出. 
 - 例子
- ![[support/img/Pasted image 20240514155714.png]]
## 调度矢量的选择
基于**调度不等式**选择调度矢量 $s^T$
调度不等式由依赖关系得出, 考虑节点X→Y依赖关系的映射: $I_{x}→I_{y}$, 即$$\left.{X}=\left(\begin{array}{c}{i_{x}}\\{j_{x}}\\\end{array}\right.\right)\longrightarrow Y:I_{y}=\left(\begin{array}{c}{i_{y}}\\{j_{y}}\\\end{array}\right)$$
根据以上依赖关系, 调度不等式为: $$S_y\geq S_x+\left\lceil T_x/T_{cycle}\right\rceil $$
- 其中$T_x$是节点X的计算时间, $T_{cycle}$是时钟周期, $S_{x}和S_y$是节点X和Y的调度时刻
### 两类调度方程
#### (1)线性调度
$$\left.S_{x}\quad=\quad\mathbf{s}^{T}I_{x}=\left(\begin{array}{cc}s_{1}&s_{2}\end{array}\right.\right)\left(\begin{array}{c}i_{x}\\j_{x}\end{array}\right)$$
$$\left.S_{y}\quad=\quad\mathbf{s}^{T}I_{y}=\left(\begin{array}{cc}{s_{1}}&{s_{2}}\\\end{array}\right.\right)\left(\begin{array}{c}{i_{y}}\\{j_{y}}\\\end{array}\right)$$
#### (2)仿射调度
$$\left.S_{x}\quad=\quad\mathbf{s}^{T}I_{x}+\gamma_{x}=\left(\begin{array}{cc}{s_{1}}&{s_{2}}\\\end{array}\right.\right)\left(\begin{array}{c}{i_{x}}\\{j_{x}}\\\end{array}\right)+\gamma_{x}$$
$$\left.S_{y}=\mathbf{s}^{T}I_{y}+\gamma_{y}=\left(\begin{array}{cc}{s_{1}}&{s_{2}}\end{array}\right.\right)\left(\begin{array}{c}{i_{y}}\\{j_{y}}\end{array}\right)+\gamma_{y}$$

---
最一般形式的调度不等式为: 
$$s^{T}e_{x-y}+\gamma_{y}-\gamma_{x}\geq\left\lceil T_x/T_{cycle}\right\rceil,\text{其中}e_{x-y}=I_{y}-I_{x}$$
### 调度矢量的选择方法
1. 用简化依赖图(Reduced DG, RDG)找出所有基本边。 RDG利用相应问题 的规则迭代算法(Regular Iterative Algorithm, RIA)描述来构造
2. 建立所有边的调度不等式方程组，求解得到可用的$s_T$
#### (1)两种RIA标准形式的定义
- 标准输入RIA形式：所有方程的输入下标即方程右侧下标都相同
- 标准输出RIA形式：所有输出下标即方程左侧下标都相同
#### (2)例子
![[support/img/Pasted image 20240514183446.png]]
右下的图的来历就是根据标准RIA的模式, 从谁来的就画一个谁到谁的箭头, 洗漱就是i-a的那个a.
> [!question] PE单元怎么得出?????

![[support/img/Pasted image 20240514204956.png|500]]

> [!question] $T_{x}$计算时间怎么得出?

-  R2和双R2
![[support/img/Pasted image 20240514210105.png|454]]
### 矩阵乘法与二维脉动阵列设计
从c的表达式到a的表达式难以看出, 更难的本质是从表达式到右下角图的表示, 但是有了RIA形式之后就可以较为简单的得到下面的结果了.
> [!question] $P^T$是怎么得出的

> [!question] 如何从表达式到RDG

![[support/img/Pasted image 20240514210800.png|523]]
![[support/img/Pasted image 20240514210814.png|454]]
![[support/img/Pasted image 20240514210839.png|523]]

# 第十三章 位级运算架构
## 13.1引言
![[support/img/Pasted image 20240514215950.png]]

- 位级架构和运算架构有什么关系
- 先介绍为什么统一用补码表示
## 13.2数的定点表示
数制分类：解决值与表示的关系问题
采用固定基r
![[support/img/Pasted image 20240514220207.png]]
![[support/img/Pasted image 20240514220225.png]]
![[support/img/Pasted image 20240514220238.png]]
![[support/img/Pasted image 20240514220251.png]]


## 13.3并行乘法器
### 13.3.1Horner法则
关于**乘法到底怎么计算**: 是一个Paper and pencil rule, 即右移和
$$\begin{aligned}P&=A\times(-b_{w-1}+\Sigma b_{w-1-i}2^{-i})\\&=\boxed{-Ab_{w-1}}+[Ab_{w-2}+[Ab_{w-3}+[...+[Ab_1+Ab_02^{-1}]2^{-1}]...]2^{-1}]2^{-1}\end{aligned}$$
- 例子![[support/img/Pasted image 20240515104108.png|385]]
#### (1)表格形式
A、B相乘展开为部分积(Partial Product, PP)阵列，乘法结果通过加法器阵列完成部分积求和得到
**非符号数**![[support/img/Pasted image 20240515104218.png|362]]
> [!question] 这个权重对吗?
> 感觉只是相对的

**符号数**
最高位是符号位, 因此关键就是处理$-Ab_{w-1}$
$\text{数}A=-a_{w-1}+\Sigma a_{w-1-i}2^{-i},\text{ 表示为}(a_{w-1}.a_{w-2}...a_1a_0)$
对上式求负值, 则有$$\begin{aligned}-A&=a_{w-1}-\Sigma a_{w-1-i}2^{-i}\\&=a_{w-1}-\Sigma a_{w-1-i}2^{-i}+1-1\\&=-(1-a_{w-1})+1-\Sigma a_{w-1-i}2^{-i}\\&=-(1-a_{w-1})+(\Sigma2^{-i}+2^{-w+1})-\Sigma a_{w-1-i}2^{-i}\\&=-(1-a_{w-1})+\Sigma(1-a_{w-1-i})2^{-i}+2^{-w+1}\\&=-\overline{a_{w-1}}+\Sigma\overline{a_{w-1-i}}2^{-i}+2^{-w+1}\end{aligned}$$
因此$$-Ab_{w-1}=-\overline{a}_{w-1}b_{w-1}+\sum\overline{a}_{w-1-i}b_{w-1}2^{-i}+b_{w-1}2^{-w+1}$$
这样Horner表格变为![[support/img/Pasted image 20240515121052.png|454]]
其中最下边的五项就是$-Ab_{w-1}$得到的结果, 其余的都是正常的部分积.
> [!question] 为什么不直接使用定义得到表格, 而是进行一步转换?把$-Ab_{w-1}$挑出来? 
> 例子如下: 
> 1. 非符号数不能直接使用符号数的Horner表格
> ![[support/img/Pasted image 20240515134854.png|224]]
> 2. 非符号数的最后一行不能和前三行使用一样的形式
> ![[support/img/Pasted image 20240515135231.png|339]]
> 3. 按照算法规则$\begin{aligned}A\times B&=(-a_{w-1}+\sum_{i=1}^{w-1}a_{w+i}\cdot2^{-i})\cdot(-b_{w-1}+\sum_{i=1}^{w-1}b_{w+-i}\cdot2^{-i})\\&=A\cdot(-b_{w-1})+A\cdot(\sum_{i=1}^{w-1}b_{w-i-i}\cdot2^{-i})\\&=a_{w-1}b_{w-1}-b_{w-1}\cdot\sum_{i=1}^{w-1}a_{w-1-i}\cdot2^{-i}+A\cdot(\sum_{i=1}^{w-1}b_{w-1-i}\cdot2^{-i})\end{aligned}$![[support/img/Pasted image 20240515135735.png|400]]
> 最后一行和前几行是相反的符号
> ![[support/img/Pasted image 20240515135544.png|328]]
> 4. 该算法规则, 可以转换为以上按位求反的结果, 这样的好处是最后一行变为一个减法和若干个加法![[support/img/Pasted image 20240515135840.png|400]]
> ![[support/img/Pasted image 20240515135939.png|281]]
> 5. 还是有减法怎么办: 下面的符号位扩展

![[support/img/Pasted image 20240515140440.png]]
![[support/img/Pasted image 20240515140456.png]]
- 为什么拓展最高位可以不要符号了, 具体如下
![[support/img/Pasted image 20240515142041.png]]
> [!question] p6啥意思, 为啥还有俩?
> 

### 13.3.2串行进位加法器(CRA)阵列乘法器
![[support/img/Pasted image 20240515155311.png]]
![[support/img/Pasted image 20240515155326.png]]
注意
- 会解释为什么会有一个RDG, 怎么来的
- 尤其是那一个符号位扩展的线
- 原本的关键路径和改进后的关键路径
- 字长就是几位加法

### 13.3.3进位保留加法器(CSA)阵列乘法器
参考[[Fishing/First_Sem_Master_1/数大/L11/L11_L12_算术运算电路_NOTE#(2)进位保留乘法器 ( Carry-Save Multiplier)|L11_L12_算术运算电路_NOTE]]
上边那个笔记中说的左下方的加法器,  由于规则布局成是正方形的, 因此就是现在这个向下的虚线
![[support/img/Pasted image 20240515155455.png]]
![[support/img/Pasted image 20240515155504.png]]
![[support/img/Pasted image 20240515155517.png]]

### 13.3.4Wallace Tree(WT)乘法器
![[support/img/Pasted image 20240515160441.png]]
![[support/img/Pasted image 20240515160450.png]]
> [!question] 隔行并行加法器阵列 乘法器是什么?

![[support/img/Pasted image 20240515160530.png]]
### 13.3.5Dadda乘法器
这个其实才是数大中的华莱士树[[Fishing/First_Sem_Master_1/数大/L11/L11_L12_算术运算电路_NOTE#(3)树形乘法器|L11_L12_算术运算电路_NOTE]]
![[support/img/Pasted image 20240515160634.png]]
![[support/img/Pasted image 20240515160643.png]]
### 13.3.6Bough-Wooley 乘法器
Baugh-Wooley乘法器提出另一种**符号位处理方法**
$$A=-a_{n-1}2^{n-1}+\sum_{i=0}^{n-2}a_{i}2^{i},\quad B=-b_{n-1}2^{n-1}+\sum_{j=0}^{n-2}b_{j}2^{j}$$$$AB=a_{n-1}b_{n-1}2^{2(n-1)}+\sum_{i-0}^{n-2}\sum_{i=0}^{n-2}a_{i}b_{j}2^{i+j}-\sum_{i=0}^{n-2}b_{n-1}a_{i}2^{i+n-1}-\sum_{i=0}^{n-2}a_{n-1}b_{j}2^{j+n-1}$$
后两个负项可以写为: $$-\sum_{i=0}^{n-1}b_{n-1}a_{i}2^{i+n-1}=-2^{2(n-1)}+\sum_{i=0}^{n-2}\overline{b_{n-1}a_{i}}2^{i+n-1}+2^{n-1}$$$$-\sum_{j=0}^{n-2}a_{n-1}b_{j}2^{j+n-1}=-2^{2(n-1)}+\sum_{j=0}^{n-2}\overline{a_{n-1}b_{j}}2^{j+n-1}+2^{n-1}$$
代会原来的式子有$$\begin{aligned}
AB=& \boxed{-2^{2n-1}}+a_{n-1}b_{n-1}2^{2(n-1)}+\sum_{i-0}^{n-2}\sum_{j-0}^{n-2}a_{i}b_{i}2^{i-i}  \\
&+\sum_{i=0}^{n-2}\overline{b_{n-1}a_{i}2^{i+n-1}}+\sum_{j=0}^{n-2}\overline{a_{n-1}b_{j}}2^{j+n-1}+2^{n}
\end{aligned}$$
> [!question] 最高两位是符号位
> $-2^{2n-1}$可以舍去?????==*这可是算出来的数值啊大哥*==

![[support/img/Pasted image 20240515162203.png|408]]
![[support/img/Pasted image 20240515162256.png]]
==*注意反相器和那一个1*==
### 13.3.7布茨重编码(Booth’s recoding)乘法器
#### (1)思路
![[support/img/Pasted image 20240515162704.png]]
由于是冗余数, 因此一个数值可以有很多表示方法, 于是有了以下的过程
设乘数B为定点基2符号数，且字长为偶数$w =2k$
$$B=(b_{w-1}.b_{w-2}...b_1b_0)=-b_{w-1}+b_{w-2}2^{-1}+...+b_12^{-(w-2)}+b_02^{-(w-1)}$$
采用以4为基的玻兹重编码, 结果导致一半(如奇权重)数位变成0，即$$B=(b’_k.0b’_{k-1}0...b’_l...b’_10)_2;b’_k\in\{-2,-1,0,1,2\}$$
> [!question] 没懂后边的这个范围啥意思
> 看后边编码之后的, 只有-2, -1, 0, 1, 2这五个数

具体操作过程如下
1. 为使奇权重数位消失，做如下处理$$\begin{array}{c}2^{-1}=2^0-2^*2^{-2}\\2^{-3}=2^{-2}-2^*2^{-4}\end{array}$$
2. 代入B，合并
![[support/img/Pasted image 20240515180734.png]]
3. 补完之后可以叙述为: 重编码时, 需要在符号位之前补一位, 三位一编, 最后不够的补0. 
> 注: 如果是一般的的, 不是小数点前边只有一位的话, 要从$b_{1}b_{0}.b_{-1}$为一组开始编

4. 因此公式为: $$b’_{i-1}=-2b_{2i}+b_{2i-1}+b_{2i-2}$$
对应表格![[support/img/Pasted image 20240515191813.png|431]]
#### (2)无符号数
![[support/img/Pasted image 20240515162348.png|454]]
- 证明![[support/img/Pasted image 20240515162504.png]]![[support/img/Pasted image 20240515162540.png|431]]
#### (3)符号数
符号位拓展
![[support/img/Pasted image 20240515162401.png|546]]
#### (4)例子
![[support/img/Pasted image 20240515162618.png]]
![[support/img/Pasted image 20240515192459.png]]
#### (5)底层架构
![[support/img/Pasted image 20240515194208.png]]
> [!question] 没太懂这个Mux是什么玩意儿
## 交织布局规则与基于位平面的数字滤波器
目的: FIR柱子滤波器的高效实现和布局技术(减少布线复杂度).
FIR 滤波器：多个乘法累加。 3 阶例子$$y(n)=x(n)+fx(n-1)+gx(n-2)\quad\text{设}\underline{x(n-1)}=x’;\underline{x(n-2)}=x’’$$$$\text{即: }y(n){=}x(n){+}f\underline{x}{'}{+}g\underline{x}{''}\quad\text{如何并行?}$$
> [!question] 为啥不能并行啊?

信号6位，系数4位. 结果取6位：1符号位， 5有效位，尾数截断. 因此信号的最后两位是不需要的. 
![[support/img/Pasted image 20240515203015.png]]
![[support/img/Pasted image 20240515203026.png]]
如何看懂这个架构: 右上角的$g_{0}x_{2}''+f_{0}x_{2}'$相加, 左边是$g_{0}x_{3}''+f_{0}x_{3}'$. 第一行有六个, 就是把horner表格中的第五个和第六个移上来了. 
![[support/img/Pasted image 20240515203754.png]]
刚刚的布局架构是不同系数的部分积在同一行出现了, 也有$f_{0}和g_{0}的组合, 还有f_{1}和g_{1}$的组合. 右边的这个结构一行只对应部分积的一行, 所有的进位是规则往下的, 因此红线直接插入就是流水线.
## 位串行乘法器
### 利用Horner法则的位串行Lyon乘法器
- 位串行含义：**被乘数**按位串行计算(即位级折叠变换)
- 具体操作: 就是一个字长w内的被乘数一位一位的输进去.
- 计算依据: Horner法, 考虑2的补码表示$$\begin{aligned}&A=a_{w-1}a_{w-2}\cdots a_{1}a_{0}=-a_{w-1}+a_{w-2}2^{-1}+\cdots+a_{1}2^{-(w-2)}+a_{0}^{-(w-1)}\\&B=b_{w-1}b_{w-2}\cdots b_{1}b_{0}=-b_{w-1}+b_{w-2}2^{-1}+\cdots+b_{1}2^{-(w-2)}+b_{0}^{-(w-1)}\\&P=A\times(-b_{w-1}+b_{w-2}2^{-1}+\cdots+b_{1}2^{-(w-2)}+b_{0}^{-(w-1)})\\&=-A\cdot b_{w-1}+A\cdot b_{w-2}2^{-1}+\cdots+A\cdot b_{1}2^{-(w-2)}+A\cdot b_{0}2^{-(w-1)}\\&=-A\cdot b_{w-1}+[A\cdot b_{w-2}+[A\cdot b_{w-3}+[\cdots+[A\cdot b_{1}+A\cdot b_{0}2^{-1}]2^{-1}\cdots]2^{-1}]2^{-1}\end{aligned}$$
有一个减法很不爽, 希望能够按照位取反的方法实现, 因此继续化简为$$\begin{aligned}
&Ab_{w-1}=-\overline{a}_{w-1}b_{w-1}+\Sigma\overline{a}_{w-1-i}b_{w-1}2^{-i}+b_{w-1}2^{-w+1} \\
&\text{P} =A\times(-b_3+b_22^{-1}+b_12^{-2}+b_0^{-3})  \\
&=-A\cdot b_3+A\cdot b_22^{-1}+A\cdot b_12^{-2}+A\cdot b_02^{-3} \\
&=-A\cdot b_3+[A\cdot b_2+[A\cdot b_1+A\cdot b_02^{-1}]2^{-1}]2^{-1} \\
&=(-\overline{a}_3+\overline{a}_22^{-1}+\overline{a}_12^{-2}+\overline{a}_02^{-3}+2^{-3})\cdot b_3+[A\cdot b_2+[A\cdot b_1+A\cdot b_02^{-1}]2^{-1}]2^{-1}
\end{aligned}$$![[support/img/Pasted image 20240515222509.png]]
上图绝对不是标准的对应结构, 因为无法实现符号位扩展和对应的补码的选择输入, 但大体的意思是对的. 
但是遇到了一个问题, 缩放算子在原理上确实是×$\frac{1}{2}$, 但是, 操作起来确是右移一位, 因此, 每一个缩放起作用的时候, 必须知道这一位的左边是什么, 因此纯粹的组合逻辑是无法实现的. 因此需要插入延迟原件.
![[support/img/Pasted image 20240515222839.png]]
实现过程的硬件和计算时间如下: ![[support/img/Pasted image 20240515222941.png]]
详细的结构如下: 

![[support/img/Pasted image 20240515222145.png]]
具体的工作细节: ![[support/img/Pasted image 20240515223033.png]]
![[support/img/Pasted image 20240515223043.png]]
![[support/img/Pasted image 20240515223056.png]]
![[support/img/Pasted image 20240515223117.png]]
#### 利用脉动映射的位串行乘法器设计
![[support/img/Pasted image 20240515224742.png]]
![[support/img/Pasted image 20240515225135.png]]
![[support/img/Pasted image 20240515225149.png]]
![[support/img/Pasted image 20240515225202.png]]**==只能给出一个架构, PE的具体形式还不不可以啊, 而且开关如何映射出来呢==**
## 位串行滤波器的设计与实现
### (1)设计思路
首先需要明确的是: **上一大节讲的是乘法器, 这一节是滤波器**. 
滤波器的基本形式: 
- 例子: $y(n)=-\frac78x(n)+\frac12x(n-1)$
需要处理的是系数部分, 利用符号数的表达方法, 我们可以把以上的例子**改写为**:
$y(n)=-x(n)+x(n)2^{-3}+x(n-1)2^{-1}$
对应的DFG我们就有了![[support/img/Pasted image 20240516230017.png|339]]
==真正关键的问题是==: 由于缩放运算在底层的实现是移位操作, 但是, 移位的话一定需要知道相邻位是什么, 但是由于是串行输入, 不知道相邻位是什么, 只有在下个时钟周期才知道, 因此需要插入D.
- 插入D的方法: 前馈各级保证功能不错(**==也就是之前的功能其实是将信将疑的对==**)
- (n-1)是对应的**字延时**, 其由于是串行输入, 需要字长$w$个位延时.
- 改写后负号: 利用补码的形式, 每一位取反, 之后$+1$, 因此上图下面的加法器那里就有一个1的输入, 上边对应有一个反相器
- 加法器的实现: 由于加法器产生的进位还需要加到这一级之中, 因此, 需要保留一次进位, 这样, 加法器进位输入部分需要一个D.
- 加法器的开关时刻: 第一位的时候没有进位, 此时的输入应该为0或者1, 分别对应加和减.
> [!question] 那个输出的D是什么, 是加法器后边跟着一个D, 作用如下:
> 

![[support/img/Pasted image 20240516230832.png]]
> [!note] 缩放因子的开关原理
> 其实就是逃离, 你会发现, 移位其实就是对不齐. 要是按照原来的, 原封不动的输入的话, 结果就是对齐的, 因此, 我需要一个D后边保证, 我该输入的时候, 我躲在D的后边, 导致你没法输入对齐的数.
> - 还有一个累加的D的效应, 比如说后边的缩放因子$2^{-1}$, 其中$8l+3$的时候在左边, 就是为了躲避3,11,19时刻进来的数, 这些数被挡住了, 其中这个$+3$就是因为前边的缩放因子前馈割集插入的那个$3D$.

### (2)具体工作原理
![[support/img/Pasted image 20240516233540.png]]
![[support/img/Pasted image 20240516233548.png]]
![[support/img/Pasted image 20240516233600.png]]
![[support/img/Pasted image 20240516233609.png]]
![[support/img/Pasted image 20240516233619.png]]
![[support/img/Pasted image 20240516233628.png]]
![[support/img/Pasted image 20240516233637.png]]
![[support/img/Pasted image 20240516233646.png]]
![[support/img/Pasted image 20240516233654.png]]
![[support/img/Pasted image 20240516233701.png]]
### (3)寄存器共享
![[support/img/Pasted image 20240516233709.png]]
为啥可以啊: 上边那个3D可以不要了, 我只需要改一下输入$x(n)$的位置就可以了啊.
![[support/img/Pasted image 20240516234013.png|293]]
#### (4)位串行IIR滤波器
> FTR和IIR?
> FIR滤波器（Finite Impulse Response）是一种线性相位滤波器，其输出只由输入的有限数量的最近样本组成。这意味着FIR滤波器没有反馈回路，因此它们具有稳定的特性和线性相位响应。FIR滤波器的设计通常使用窗函数方法或频率采样方法。
   相比之下，IIR滤波器（Infinite Impulse Response）是一种具有无限冲激响应的滤波器，其输出不仅由当前的输入样本组成，还包含过去的输出样本。这种反馈结构使得IIR滤波器可以更有效地实现滤波器，但也会导致它们的不稳定性和非线性相位响应。IIR滤波器的设计通常使用双线性变换方法、极点和零点放置等方法。
   一句话就是: IIR有反馈了

例子如下: 
信号字长8$$y(n)=-\frac78y(n-1)+\frac12y(n-2)+x(n)$$
利用结合律改写: $$\begin{aligned}&w(n)=-\frac78y(n-1)+\frac12y(n-2)\\&y(n)=w(n)+x(n)\end{aligned}$$因此DFG为:
> 内外环
> 都是从y出发回到y, 也就是两条反馈回路, 对应的$(n-1)和(n-2)$

以上是字级延时, 串行输入位, 需要变换为位级延时, **==变换过程有一个可以灵活的点==**, 其需要去一个合适的值使$得外,内环延时为16, 8$
![[support/img/Pasted image 20240517192107.png]]

>[!question] 灵活
> 为啥可以灵活???不导致功能改变吗??功能确定框架下, 重定时导致的确定结果, 并不是灵活的.

---
我的推导过程
![[support/img/Pasted image 20240517230704.png]]
![[support/img/Pasted image 20240517230757.png|408]]
![[support/img/Pasted image 20240517231016.png]]

---
官方过程, 由于要求了全流水, 所以加法器不能直接相连, 需要每一个后面跟一个D, 对我的结果进行重定时即可.
![[support/img/Pasted image 20240517231056.png]]
- 这是对我的结果后面进行重定时的结果
- $T_{c}=1$全流水的加法器, 串行输入, 八个cycle处理一个字
> [!question] 使环延迟能够匹配的最小字长？($T_{c}=1$)
> 算法: 首先是那个8D, 本质为wD, 但是缩放因子是不能改变的,所以是$\frac{{3+1+2}}{3+1+2+wD}=1:2$ 

![[support/img/Pasted image 20240517232452.png]]
![[support/img/Pasted image 20240517232504.png]]

## 正则符号数运算
**正则符号数制(CSD, Canonic Signed Digit)表示法**
![[support/img/Pasted image 20240517233705.png]]
![[support/img/Pasted image 20240517233718.png]]
> [!question] 这个平均比例是啥?

![[support/img/Pasted image 20240517235234.png]]
> [!question] 自学的考不考
> 但是意思是知道的, 就是递推一下

![[support/img/Pasted image 20240518001817.png]]
- means 1少就是加法器少, 加法器少就是流水插入的D少, 导致最小匹配字长变小
![[support/img/Pasted image 20240518002702.png]]
![[support/img/Pasted image 20240518003033.png]]
> [!question] 推迟缩放因子什么意思, 又为什么减少截断误差?
> 右上角那个图还是很明显的, 直接截断后相加考虑的有效位小于先省略公共缩放因子相加, 之后再移位的情况


![[support/img/Pasted image 20240518003309.png]]
- 树形排列咋来的:
1. Horner进行重新排列
2. 按照2,4,8,一级一级拆分组织结构 嵌套各层
## 分布式运算
> [!question] 分布式运算的概念?? 

![[support/img/Pasted image 20240518003449.png]]
![[support/img/Pasted image 20240518003506.png]]
![[support/img/Pasted image 20240518003520.png]]
![[support/img/Pasted image 20240518003531.png]]
> [!question] 最大的问题, 如何从$D_j得到Y$


![[support/img/Pasted image 20240518003547.png]]
![[support/img/Pasted image 20240518003600.png]]
![[support/img/Pasted image 20240518003615.png]]

## 小结
![[support/img/Pasted image 20240518003653.png]]

# 第十四章 冗余计算

## 引言
![[support/img/Pasted image 20240518095517.png]]

> [!question] 冗余数制和无进位
## 冗余数表示
![[support/img/Pasted image 20240518120114.png]]
![[support/img/Pasted image 20240518120615.png|431]]
![[support/img/Pasted image 20240518120643.png|684]]
- $α的最大取值就是r-1$
- $最小冗余就是, α取什么值, 仍旧能保证满足ρ>1/2$, 注意这是取整, 每一位必须是整数
![[support/img/Pasted image 20240518121606.png]]

## 无进位基2加法与减法
![[support/img/Pasted image 20240518122502.png]]
> [!question] 这个结论的意思
> 冗余数的和与差可以通过求混合基2加法与减法（冗余数与非冗余数的和与差）得到

#### (1)混合基2加法
考虑一个r-2有符号数位数字$X_{<2,1>}$和一个==*无符号*==普通数字$Y$的加法
$$S_{<2,1>}=X_{<2,1>}+Y$$
$$X^{+}=x^{+}{}_{w-1}+x^{+}{}_{w-2}2^{-1}+...+x^{+}{}_{1}2^{-w+2}+x^{+}{}_{0}2^{-w+1}$$$$X^{-}=x^{-}{}_{w-1}+x^{-}{}_{w-2}2^{-1}+...+x_{-1}2^{-w+2}+x_{-0}2^{-w+1}$$$$Y^{+}=y^{+}{}_{w-1}+y^{+}{}_{w-2}2^{-1}+...+y^{+}{}_{1}2^{-w+2}+y^{+}{}_{0}2^{-w+1}$$
- 其中: $x_i^+,x_i^-,y_i^+\in\{0,1\}$
加法可以分两步进行
1. 第一步
	以并行的方式在所有位上进行
	$$p_{i}=x_{i}+y_{i}$$
	其中: $x_{i}=x^+_{i}-x^+_{i}$, $y_{i}=y^+_{i}$
	因此$p_{i}$的值在$\{-1, 0, 1 ,2\}$之中
	把这个加法表示成$$p_i=x_i^+-x_i^-+y_i^+=2t_{i}+u_{i}=2t_i^+-u_i^-$$
	则对应的真值表为: 
	![[support/img/Pasted image 20240518181949.png|184]]
	对应的有$$\begin{aligned}&t_i^+=x_i^+y_i^++\overline{x_i^-}(x_i^+\oplus y_i^+)\\&u_i^-=x_i^+\oplus x_i^-\oplus y_i^+\end{aligned}$$
	
2. 把$t^+_{i-1}和u^-_{i}$组合, 得到==**和的数位**==$$s_{i}=t^+_{i-1}-u^-_{i}$$
> [!note] 
> 这个$s_{i}$就是最终的结果, 是某一位的值. 因为你看$p_{i}$的式子, 二倍就代表这个数要进位到下一位, 因此, 本位的值就是下一位进位再减去留在本位的$u_{i}$

![[support/img/Pasted image 20240518211905.png]]
> [!question] 串行架构讲的好不清楚啊?
> 什么叫插一个0, 还是对于并行架构的理解不深, 并行架构好说啊, 就是一堆组合逻辑直接得到结果, *==不存在一个进位链woc==*
#### (2)混合基2减法
![[support/img/Pasted image 20240518213315.png]]
> [!warning] 还是打错了p和s

![[support/img/Pasted image 20240518213403.png]]
#### (3)混合r-2加减法器
![[support/img/Pasted image 20240518224540.png]]
加减法器不是同时做加减法, 而是可以选择做加法还是做减法
#### (5)两个r-2冗余数的加法、减法和加减法运算
![[support/img/Pasted image 20240518224819.png]]
![[support/img/Pasted image 20240518224829.png]]
> [!question] 这个结构没有仔细看
> 
## 基 2 混合冗余乘法架构
#### (1)r-2 冗余数位串行在线乘法的架构： P = AB
![[support/img/Pasted image 20240518225041.png]]
![[support/img/Pasted image 20240518230517.png]]
> [!question] 没懂最后一行为什么只有这五个, 那右边的三位结果不也应该有一个空操作吗?
> 定w, 截断

![[support/img/Pasted image 20240518230908.png]]
![[support/img/Pasted image 20240518231359.png]]
> [!question] 没太仔细看架构
> $b_{0}$进来的时候, 和$a_0$相乘传到D前边, 等下个cycle, 和$a_{1}\times b_{1}$的结果相加

![[support/img/Pasted image 20240519012456.png]]
![[support/img/Pasted image 20240519012520.png]]
![[support/img/Pasted image 20240519012530.png]]
![[support/img/Pasted image 20240519012541.png]]
![[support/img/Pasted image 20240519012615.png]]
![[support/img/Pasted image 20240519012631.png]]
![[support/img/Pasted image 20240519012641.png]]
![[support/img/Pasted image 20240519012657.png]]

> [!question] sign(b)咋来的
>  
## 数据格式转换
![[support/img/Pasted image 20240519014401.png]]
> [!note] 借位一直借到最高位 nb

![[support/img/Pasted image 20240519014546.png]]
![[support/img/Pasted image 20240519014558.png]]
![[support/img/Pasted image 20240519014607.png]]
![[support/img/Pasted image 20240519014616.png]]
![[support/img/Pasted image 20240519014626.png]]
- 注意: 借位之后, 下一位上有一个1, 但只有下一位上有一个1, 两个0的时候就要细看了, ==*一直借下去*==
![[support/img/Pasted image 20240519014638.png]]
![[support/img/Pasted image 20240519014648.png]]
![[support/img/Pasted image 20240519014657.png]]
这个图怎么看
- 首先需要知道 c往下的是直接传递选择信号m和p, 所以看第二行第二个c, 传出来的实际是乡下的x2的选择, 选择了最高位经过3的mp选完后, 在经过选一轮, 而x2 append的结果只需要x1和x0的mp进行选择, 其中x0的一数列都是选择信号
![[support/img/Pasted image 20240519014707.png]]
> [!question] 没看咋来的??????

## 小结