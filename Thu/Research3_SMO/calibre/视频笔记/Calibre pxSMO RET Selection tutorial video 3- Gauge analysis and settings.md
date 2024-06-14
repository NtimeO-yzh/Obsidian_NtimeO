![[support/img/Pasted image 20240517140324.png]]

![[support/img/Pasted image 20240517140333.png|191]]
> how many edges of particular gauge interacts with and ideally, you want it to be two and 

![[support/img/Pasted image 20240517140533.png]]
- 鼠标放在上面会有一个大图显示gauge到底是怎样建立的, 到底和多少features interact
![[support/img/Pasted image 20240517140649.png|293]]
- 红色的是gauge
- 绿色的一圈是包围gauge的guard band.  the guard band is actually what determines the interaction, so you can think of it like when you're checking for
![[support/img/Pasted image 20240517141033.png]]
- 报问题了, 说是因为the gauge Wasn't long enough, it's too close to the edge so that when the gauge gets ==**resized**==. Um. with this this um the safety margin. it no longer interacts with any edges on the target layer. 相当于是一个浮空的gauge
> [!question] 这不是和edge交互了吗, 为啥还是0?


- 解决办法: 上边有一个`center`按钮, 点开之后
> There's the center tool up here. So we'll come in here and I'll talk you through a few of these options. So the first is this Factor? I'm just going to change change this to two right now. For example, usually between 1.5 and 2 are pretty good values here. You'll want to enable gauges centering. So this will run this MDF gauges Center command. It will take. By default the way that we've configured it, it will take the drawn value here in the spreadsheet. And multiply that by 2 and use that as the desired length for the gauge. But in

- factor不能设置太大, 如果设置太大的话, **就会和超过两个边相交, 这是不允许的**

> [!question] center是什么意思?
> The gauges have all been analyzed, they've all been centered. So the center of that gauge in the long axis is now centered over the feature. 

![[support/img/Pasted image 20240517141512.png]]
- 应用之后gauge会改变一下
![[support/img/Pasted image 20240517141551.png]]
- 对于这个GID1来说, Len长度变长了. context从0→2
![[support/img/Pasted image 20240517141945.png]]
> [!question] into Memory??

> [!question] Um, this determines what gauges are used to create Clips. Clips???


![[support/img/Pasted image 20240517142550.png]]
 - 高亮显示gauge
 ![[support/img/Pasted image 20240517142635.png]]
 > measurement. So, imagine now When SMO runs it's going to, because of the gauge is turned on it, will come here, it will look at the center of this gauge like as marked with this Crosshair. And then it'll come out by a certain amount defined by the optical diameter. And draw a gauge. I believe our part of me draw a clip. I believe the clip is roughly one, **==O.D by one OD.==** Um, so Basically, it's looking out one optical diameter and taking all the geometries that lie inside that clip. And it will use that to run, SMO and optimize that image. So because it's doing that, if you have two gauges on a feature, like in this case, ![[support/img/Pasted image 20240517142907.png|201]]let's turn this back on. If you have two gauges on a feature in this case, an X-gauge and a y gauge. There's really no need to feed both of those gauges in the SMO because it will they have the same Center. The tool will find the same center, and it will basically always create a duplicate clip and run both Clips through the SMO tool. So, If you were to keep both enabled, You would essentially have. A clip weight of two for this one feature, even though no weights are made, you would have be feeding two duplicate Clips into the tool.![[support/img/Pasted image 20240517143041.png|39]]

![[support/img/Pasted image 20240517143218.png]]
 
> So let's say Let's say I ran this tool. And let's say pitch X, for instance. Reruns his tool and Pitch X just **==doesn't==** **==have as narrow of a PV band==** as the customer wants during analysis. One way that you could, you could investigate improving. This is to set a weight. So, You double click

- 讲了一下如何影响最终结果的, 但也没说很清楚
> could, you could investigate improving. This is to set a weight. So, You double click to edit and then enter the value and then carriage return, accepts it. So What will happen at this point is that all objectives calculated on Pitch X during SML. We'll be calculated normally and then that objective will get multiplied by this weight of three. So that That the objective for pitch, acts will count will. Count 3x, more than normal and because of that, it'll get more attention and more heavily optimized.
- CD tolerance
>So Cd tolerance is used during common depth of focus analysis, right now. To figure out what CD limits. A feature is allowed to have. In the future. We'll take the CD tolerance and plug it directly back into fix, SMO. So that During the SMO calculation. It knows the CD tolerance and can actually directly optimize for common up. The focus. And our goal is to Eliminate the the use of weights. Um, Uh, and and just have the, the SMO tool smartly optimized for this important. Common up, the focus. Okay. Finally, there's this *==common process window==* field here.

![[support/img/Pasted image 20240517150641.png]]

![[support/img/Pasted image 20240517151003.png]]
上边有一行FILTER
![[support/img/Pasted image 20240517151027.png]]多选set weight