# 折叠
## 运算单元折叠
### (1)基本步骤
#### 1. 确定折叠集
#### 2. 列出折叠方程并检查
$$D_F(U{\to}V){=}Nw(e){-}P_u{+}v{-}u$$

- $D_{F}$代表Folding之后的图中, $U\to V$的延迟数
- N折叠因子, 折叠集中单元的数量
- $w(e)$, 原DFG之中$U\to V$的延时
- $P_{u}$节点U的计算时间(几级流水)
- v, u 折叠集中U和V的下标
#### 3. 画出折叠图
### (2)例子
#### 1. 例子1
![[support/img/Pasted image 20240413132122.png|500]]
1. 确定折叠集$$S1=\{4, 2, 3, 1\}  S2=\{5, 8, 6, 7\}$$
2. $D_F(U{\to}V){=}Nw(e){-}P_u{+}v{-}u$$$\begin{gathered}
D_{F}(1\rightarrow2) =4(1)-1+1-3=1 \\
D_{F}(1\rightarrow5)=4(1)-1+0-3=0 \\
D_{F}(1\rightarrow6)=4(1)-1+2-3= \text{=2} \\
D_{F}(1\rightarrow7)=4(1)-1+3-3= =3 \\
D_{F}(1\rightarrow8)=4(2)-1+1-3 =5 \\
D_{F}(3\rightarrow1)=4(0)-1+3-2 =0 \\
D_{F}(4\rightarrow2)=4(0)-1+1-0=0 \\
D_{F}(5\rightarrow3)=4(0)-2+2-0 =0 \\
D_{F}(6\rightarrow4)=4(1)-2+0-2 =0 \\
 D_{F}(7\rightarrow3)=4(1)-2+2-3 \text{=1} \\
D_{F}(8\rightarrow4)=4(1)-2+0-1 \text{=1} 
\end{gathered}$$
3. 画图
- 首先画出两个功能单元(折叠集)
- 画出若干个D, 根据算出后的$D_{F}$确定
- 从原图中一条一条边来, 包括输入边和输出边
例如: 按照式子中的, $1\to 2$的结果是1, 1和2都是加法器, 都属于集合 $S_1$, 因此需要画一个从加法器出来, 经过一个D再连回加法器线. 这条线的开关时刻是2被使用的时刻, 也就是2在 $S_1$之中的下标是多少, 是1(从0开始的).
- 比较难的是输入和输出的时间, 输入是 $S_1|3$的输入, 输出是 $S_1|1$的输出, 因此应该输入时刻是3, 输出时刻是2
![[support/img/Pasted image 20240413133040.png]]
### (3)重定时和折叠
![[support/img/Pasted image 20240413224028.png]]
## 寄存器折叠
# Q
折叠不一定降低性能,咋看, 咋看什么迭代边界