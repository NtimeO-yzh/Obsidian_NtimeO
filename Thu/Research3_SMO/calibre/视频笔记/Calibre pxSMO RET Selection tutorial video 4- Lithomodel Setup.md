   ![[support/img/Pasted image 20240520163855.png]]
1. 在下方选择Session Browser
2. 左侧选择一个Session
3. 上边有一个mode, 本次training针对的是Free-Form pxSMO 
4. 上边的小太阳标志, 好像是可以看是否编辑的一个标志,是不是active session
5. Clone 
- 配置环节
1. mdf已经编辑过了, 但是仍然可以从左边的setup file中详细看, 并且有链接的形式
> [!question] nominal optical model

> has a dark background. And the mask layer, the drawn geometries have a clear foreground. And actually the specifics of the foreground and background. Are defined in this ddm library dark underscore omog.m. This is a pretty Simple. But um, Classic. Definition of how the DDM libraries can be used.

- nominal Optical model. So, what if? So this has happened sometimes in the past, what if I have a session, everything's set up. The gauges of the way I want it. Successful pics. OBC OPC verify your set up the way that I want. *==What if I want to use a different model?==* Different Optical model, a different aerial threshold. How would I do that? 
方法1:   go through here. So The. Optical models. The resist models. These are all read, only so, One way that you would input a new Optical model would be to edit the MDF setup. So, If you create a new Optical model or a new resist model or a new DDM Library, You can create a new little model. And then you can edit this setup, right? So, So, um, Assuming you created a new lithium model, you could call it whatever you wanted, as long as this with the model. 
- MDF窗口的第一个作用就是能够让你去看到model使用的是哪一个
- 第二个作用就是: 能够详细查看模型的参数, 百分之九十九的参数是不变的 
变得是什么?
What becomes different is? Inside, SMO and post SML. The illumination type will change from this default. Then it will change to point back to The output Source map that comes out of SMIL.![[support/img/Pasted image 20240520171643.png|339]]