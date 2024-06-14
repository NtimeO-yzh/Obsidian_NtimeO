# 流程
## 1.1 Creat Layout And Gauge Pair
### 1.1.1 Layout
> [!question] 问题1: Layout起什么作用?
> 一个Layout有很多层, 其中需要有Target Layer. Target Layer是否有至少一个, 可以更多吗, 多选如何处理?
> - Target Layer的意思是不是Critical Pattern, 也就是包含那十几个Pattern

### 1.1.2 Gauge
> [!question] 问题1: Gauge的基本知识
> 1. Gauge的基本类型和插入方法是可以变的吗?
> 2. 那几个什么gg文件啥的有什么区别?
> 3. Gauge的创建方法?

> [!question] Q2: Gauge的影响?
> 1. 最终模型收敛到一定的值是否是根据Gauge的位置进行测量EPE?
> 2. 如果还有别的指标的话, 是根据怎样的公式来进行判断收敛的?
> 3. Threshold和Gauge有什么联系?

### 1.1.3 LithoModel
### 1.1.4 Choose Layers
![[support/img/Pasted image 20240516121359.png]]
> [!question] Q1: 为什么分Mask\Layout\SMO Layer?
> -  Mask Layer: 
> 看上去是选项的样子, TRANS dark啥意思? 
> - Layout Layers: 
> 就是Layout中的number
> - SMO Layer Type: 
> 有如下选择:
> 	1. Target
> 	2. Smooth Target
> 	3. ...
> 这些type有什么用? 在哪里讲的? 有点像OPC里边的那个Layer, 但是那个我也不很懂. 


## 2.1 Creat Session
> [!question] Q1: 创建完LG Pair之后会自动生成一个Session, 这个是什么?

### 2.1.1 Select Lithomodel/Setup File
![[support/img/Pasted image 20240516154215.png]]
> [!question] Q1: 为什么两者是并列出现的, 是等效的
> 如果是等效的话, Setup File的内容是怎样的

> [!question] Q2: 有好几个Setup File, 分别是Setup什么?
### 2.1.2 Choose Layers
![[support/img/Pasted image 20240516160203.png]]
> [!question] Q1: 这些选项都有什么用, 和之前的LG Pair中的有什么区别?

