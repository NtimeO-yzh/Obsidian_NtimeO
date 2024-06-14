Step 1: INITIALIZE A NEW LAYOUT
![[support/img/Pasted image 20240517003530.png]]
![[support/img/Pasted image 20240517003552.png]]
![[support/img/Pasted image 20240517003652.png]]

> So, I will show you realize to use a pre-loaded setup file and I'll just click through this and show you how this works. So, If you already have run the tool, You may already have a setup file with a GUI created for you.

![[support/img/Pasted image 20240517003734.png]]

> [!question] weight layer?

- Setup file是为了好移植, Lithomodel相当于另起炉灶了.

![[support/img/Pasted image 20240517004133.png]]

> Yeah, here we go. So if we look at a particular source in a regular model, Uh, what winds up happening is After the sources created. The tool goes through analyzes and figures out. Where the energy lies in the pupil. And then, To make things computationally efficient. Uh, the various locations of the, of the, um, or the The the k vectors of these plane waves. Are decided based on this. Basically, the center of mass of the, of the illumination, so it figures out where the bright spots bright spots are and then based on Um, the energy density inside the people. It picks a location. Very close by. Um, It picks a location, very close by that that the plane waves are coming from so that you only have Um a necessary and usually small number of plane waves that get stimulated and this saves on disk space. It saves on computation time and there's Basically don't know limitation and accuracy. This is the standard way that DDM. Libraries are computed and used. that Because pixel can turn on and off any pixel at any time. Through a range of intensities. We need special DDM. Libraries that Sample, the entire pupil. And because we need this as it didn't exist, the the Optics team created, this Universal DDM Library. For us. Smo is the only tool that uses Universal ddm. Libraries directly. All the other tools. Are built on. Approach of having the universal DDM Library and then feeding it into an interpolation tool with a particular source and having the universal DDM Library interpolated to form a ready type DDM Library

 ![[support/img/Pasted image 20240517004748.png]]
 > now have the lithium model, the litho model, defined the fines, um, The different layers and and what they represent. So you can see here after loading the lifo model. It has this list here, which which map? Various litho model layers and tell the tool, what transmission Uh what what phase they might have. So, And with the background and foreground values for these are, So this is this is mainly descriptive unless you have a very Advanced lithium model that has several different math layers but in our case that's that's not typically how we operate
 
 - Target Layer是最通常设置的, 一般只有几个宽度不同的rectangle.
 ![[support/img/Pasted image 20240517005127.png]]
 > Contours are are calculated actually pardon me. The objectives are calculated based on. Depending on the objective, their calculated either based on area. Of the of the **==frame==**.Or they're calculated against the. The shoreline Contour, the the line integral around the target shape.
 > But first, is that the sharp Corners Can cause problems in the calculation of the, of the line integral I believe, because it's not a smooth path. So, But they're really not allowed to to be used. 
 > The second thing That I that I note here is that Nothing ever prints to square corners, anyway? Right? Because you can think of the entire ==*Lithography system as a big Lopez filter*==. So um, anything with sharp corners basically won't have them. So it doesn't make sense.
 
 > [!question] smooth target
 > 问题出在无法制造sharp corner, 但是, 有以下问题
 > 1. smooth target 是一个层吧, 上边的shape长啥样的
 > 2. 设置了这一层, 能够smooth谁, 好处在哪?
 > 3. 需要和哪一层搭配使用??
 
 ![[support/img/Pasted image 20240517120015.png]]
 > we pick the smooth Target, and you can see that. In this case, there is no layout layer that we choose. It's automatic meaning that the smooth Target recipe runs using this input layer as Target. And then, finally, it outputs the, the smooth Target, but it's all based, it's done automatically
 
 ![[support/img/Pasted image 20240517120210.png]]
 - 如果已经知道目标的smooth target, 可以开启左下角这个按钮, 之后选择哪一层
> [!question] Curve Target

 ![[support/img/Pasted image 20240517120402.png]]
> So, when you think about, you think about how the objective is calculated Uh the the um objectives are calculus, some objectives are calculated over the area of the of the clip. Of the frame, others are calculated over the line integral of Contours that exist. Inside that.  a lot more of the line integral calculated over over a length portion of the, of the, of the feature versus The small line end and so Uh, as I think about it, there's this. Uh, basically you can think of it almost like an automatic waiting of the various features. So If you did want some provide greater performance or increase the performance, Add a line end. The best way to do it. Is to put a weight layer over the line end and then assign some weight to it. Like 5 10, we use sometimes, 20, sometimes even 50.

 - 相当于是一个标记, 位于weight layer中的目标被着重考虑.
 - 处理的问题是: 长线会占更多的比重来影响结果, 而出现问题的地方, 或者说是更着重关注的地方是linend这种短线的地方.
 > [!question] objective如何理解?
 
 ![[support/img/Pasted image 20240517121438.png]]
 ![[support/img/Pasted image 20240517121123.png]]
 ![[support/img/Pasted image 20240517121137.png]]![[support/img/Pasted image 20240517121151.png]]
  - 意思是: 当你在这个界面选择layer的时候, 你忘了原始layout每一层长啥样, 或者代表什么, 你就可以回去看看
![[support/img/Pasted image 20240517121558.png]]

> all the target stuff is on layers, six, like we input. All right. And one of the final steps that we come to is, um, it will ask us to For a path for the setup file. Okay. So, the sub file is the same one that I pointed to earlier during the layout setup. Um, It's a blank file at this point, but we Define it here because all the different settings that you and I have put together over the last minute or so, Mine up getting written out into that file, so Like, I mentioned earlier if you needed to set up a new run, if you didn't want to go through all these steps and risk

![[support/img/Pasted image 20240517121656.png]]

- 可以改最下面的名字
![[support/img/Pasted image 20240517122001.png]]
- 会执行在database的double check, 可以帮助你检查是不是弄了重复的东西, 并且你要是只想Session不同, 可以在原来的LGP上建立Session, 这样可以更加便捷的比较.