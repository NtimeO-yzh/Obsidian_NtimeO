# Problems
DRC, ORC, OPC
# Cp1--Rule-Based OPC Overview
## 1.Rule Writing for OPC
**<font color="#ff0000"><center>Three Steps for Rule-Based Corrections</center></font>**
1. Identify and classify edges that require correction.
2. ==Modify Edges== to create polygons to *remove* from (red) or *add* to (black) the original shape.
3. Combine ==correction polygons== with the original design data.

![[support/img/Pasted image 20231116112948.png|400]]
### (1)Classification of Edges that Require Correction
是OPC的第一步.根据edge的物理特征进行分类,主要包括  
	1. Distance to nearby features  
	2. Feature width  
	3. Interactions with other layers  
	4. Topology (convex corner, concave corner, jogs, and so on)  
#### A: Dimension Checks as Layer Selectors
通常需要开始使用SVRF dimension check operations, 主要包括三个维度
	1. INTERNAL  
	2. EXTERNAL  
	3. ENCLOSURE  
通常的情况是返回error data(这不是严格的几何数据,而且不能通过SVRF进行进一步的操作).但是可以override这一generic behavior,并且direct Calibre来返回edge和polygen数据,这样就把dimension check变成了layer selector.  
具体方法: through the<font color="#ff0000"> layer specification method</font> and the use of the <font color="#ff0000">Region keyword</font>.layer specifications的几种形式如下:
	1. **Layer name alone**  🌰  `INT POLY <= 2`  
	   Operations return <u>error data</u>, which cannot be manipulated further.
	2. **Layer name in square brackets**  🌰  `INT [POLY] <= 2`  
	   Operations return <u>edge data</u> comprising those edges that satisfy the constraint. These data are also referred to as positive edge data.
	3. **Layer name in parenthesis**  🌰  `INT (POLY) <= 2`  
	   Operations return <u>edge data</u> comprising those edges that do not satisfy the constraint. These data are also referred to as negative edge data.
	4. **Operation plus the keyword**   🌰  `INT POLY <= 2 REGION`  
	   Operations return <u>polygon data</u> consisting of polygons created by joining opposing edges that satisfy the constraint. This keyword functions as a layer constructor rather than a layer selector.
### (2)Use of Metrics With Dimension Checks
 Edge selection可能出现的错误:
	1. selecting the same edge multiple times  
	2. selecting too many edges  
	3. <span style="background:#affad1">selecting edges to the side of a measurement region  </span>
Use of Metrics With Dimension Checks的作用: reduces incorrect edge selection.  
#### A: Selection of the Same Edge Multiple Times
Multiple Times会出现的问题
: This could result in rule-based OPC attempting to apply two different biases to these edges.  
例子Figure 1-3  
![[support/img/Pasted image 20231116140148.png|300]]
#### B: Shielding
缺少Shielding的时候会导致多选了很多边缘,在这里给出了例子,但是我没有很懂里边的一些细节
#### C: Order of Classification
使用一定的分类顺序可以避免上述多选了很多边缘的问题,在此给出了示例
#### D: Metrics Assist With the Selection of Edges
在svrf里边的`External`操作里边有一个名为metric的参数,其能够有三个选择值(1)OPPOSITE(2)OPPOSITE EXTENDED(3)SQUARE. 意思是,操作的时候会有横向的扩展,或者仅仅是垂直方向的去进行边缘的选择,并给出了例子.<span style="background:#affad1"> (但是怎么知道垂直是0还是90)</span>
#### E: Use INTERNAL to Find Convex Corners
使用INTERNAL来找到凸角,核心的思想就是选择一条边的内侧face(面对)另一条边的内侧,并且相交,而且角度是多少<span style="background:#affad1">(凹凸的定义)(如何知道是edge A的)</span>

#### F: Use EXTERNAL to Find Concave Corners
同上,只不过是通过external找到凹角
### (3)Edge Classification
Edges must be classified correctly to provide expected <font color="#ff0000">biasing</font> results.
#### A: CONVEX EDGE
使用layer_operation里边的`CONVEX EDGE`操作选择edge.可以根据的属性有:
  1. line-end length
  2. the angle at one or both ends一端或两端的角度
  3. the length of abutting edges临边长度
这个操作是layer selection而不是dimensional checks.找到line_ends之后可以使用dimensional checks来根据这些边到其余边的距离来进行分类.
🌰![[support/img/Pasted image 20231116155924.png|400]]
<span style="background:#affad1">疑问: 上边的angle1,2是如何确定顺序的?</span>
#### B: Edge Modification
分类完了,下一步就是把这些edge变成polygen.之后再把这些polygen加到原来的多边形上或者在原来的多边形上移除. 先介绍the process you are correcting for和the options the Calibre nmDRC hierarchical engine provides you for modifying layout designs. ![[support/img/Pasted image 20231116162553.png|500]]
分别给出了三种典型的correction
  1. biasing
  2. corner serifs
  3. hammerheads
之后给出了这三种操作具体的事怎么通过:边->correct shape->删减->修正完成这一操作进行的
#### C: EXPAND EDGE
讲了`EXPAND EDGE`操作
### (4) Convex and Concave Corner Biasing
`EXPAND EDGE` and `CORNER FILL` provide quick and accurate biasing of convex and concave corners.这不是两个独立的操作,corner fill是expand edge的一个参数, 可以fills the square gap![[support/img/Pasted image 20231116164211.png]]
#### A: Corner Corrections
你通过一些操作把边变成了派生层之中的多边形,你需要把这些多边形和原始的多边形进行加减,需要进行一系列的布尔操作
#### B: Convex Corner Biasing
![[support/img/Pasted image 20231116165104.png|400]]
#### C: Concave Corner Biasing
![[support/img/Pasted image 20231116165122.png|400]]

### (5) Proper Data Storage
给出了不同目的下,储存数据的基本形式
#👁 

## 2.Examples
#👁 
### (1)Via Biasing
Via和Contact通常在密度方面存在偏差。可以使用两种SVRF操作来增加独立孔的尺寸。第一种操作是找到与其他孔之间的距离超过200纳米的孔。然后，每个独立孔都会被放大10纳米。  
之后给了实现这同一个目的的两种操作.  
### (2)Hammerhead Construction
常常应用于几何形状的末端.之后给了3个例子:
  1. Adding Hammerheads
  2. Width-Dependent Hammerheads
  3. Corner-Based Hammerheads
# Cp2--Calibre v
主要包括三个操作:
  1. OPCBIAS(covers variable isolated or dense line, and via biasing)
  2. LITHO NMBIAS™(covers line and via biasing)
  3. OPCLINEEND(covers line-end extension generation in the form of either simple lineend extensions or hammerheads with serifs)
这三个操作能够实现快速搞笑的OPC,其根据每一个多边形边缘上的属性来进行.这也是为什么称为"Table-Driven OPC".因为以<font color="#ff0000">rules table</font>作为批处理工具的输入.
已经假设熟悉SVRF文件和程序的执行.*这里有个介绍Hierarchy的东西*
## 1.Table Practices
OPCBIAS or OPCLINEEND 可以 **convert a rules table into a set of rules**.  
两个看起来一样的表格可能产生不同的rule set.(假设不同,实践不同)  
之后给出了很多实践中需要考虑的参数值和问题.  
## 2.Calibre TDopc Execution
先编写SVRF,之后调用calibre进行运行.
### (1)Running OPCLINEEND
<font color="#ff0000">OPCLINEEND performs biasing using width and height of line-ends</font> (with no consideration for spacing violations) applied to the entire poly layer.

**An exhaustive SVRF file** would possibly include rules to correct spacing violations introduced by the default line-end treatment, and preprocessing the poly layer to specify shapes requiring correction.
SVRF文件需要包括先读取target layer的需要处理的lineend.之后为每个宽度和高度组合编写一条规则，定义应如何处理符合标准的lineend。之后使用`calibre -drc -hier lineend.svrf`运行svrf文件.之后再在Calibre WORKbench里边查看结果![[support/img/Pasted image 20231116211903.png|400]]
🌰给出了一个实际的svrf文件  
#👁 
<span style="background:#affad1">但是没介绍语法啊</span>,只给出了例子![[support/img/Pasted image 20231116212106.png|500]]
### (2)Running OPCBIAS
<font color="#ff0000">OPCBIAS applies bias based on space and widths</font>. Biasing can be positive or negative, a<u>dding to or reducing</u> the width of a target shape, respectively.这个是space和widths, 上一个是widths和hight.
做法也和上边一样,编写文件,奇怪的是为什么这里也是width and height combination而不是width and space combination.![[support/img/Pasted image 20231116222218.png|400]]![[support/img/Pasted image 20231116224510.png]]
### (3)Running LITHO NMBIAS
Calibre nmBIAS applies bias based on <font color="#ff0000">space and widths</font>. Biasing can be positive or negative, adding to or reducing the width of a target shape, respectively. <span style="background:#affad1">Either rule or table mode may be used.</span>

# Cp3--LITHO NMBIAS Command
## 1.  `LITHO NMBIAS`
### Usage

### Arguments
### Description
### Examples
## 2. `setlayer nmbias`
## 3. `option`
## 4. Calibre nmBIAS Tcl Scripting Commands
