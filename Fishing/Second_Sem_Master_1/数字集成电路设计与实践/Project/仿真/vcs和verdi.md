# question
1. 想的是使用一个*filelist.f*文件, 文件之中是*.v* 文件的绝对位置.(其中有一个问题是, 能够tb之中自动例化对应的源代码吗). 之后直接在==*任何一个地方*==使用`make vcs`直接索引*filelist*的绝对位置, 在==**指定的位置**==产生fsdb文件.
# Note
## 1
> 使用vcs仿真时，需要给一个filelist，里面会列出所有需要编译的文件，在命令行中使用 vcs -f filelist.f 来告诉仿真器需要编译哪些文件，在这个filelist.f 中， 的格式类似于：

[//filelist.f](https://link.zhihu.com/?target=https%3A//filelist.f/)

/root/my_test/rtl/a.sv

/root/my_test/verify/uvm/tb/tb.sv

//// all -v, -y, +incdir, +define

+incdir+/root/my_test/uvm/env/drv.sv

+incdir+/root/my_test/uvm/tb

-y /root/tools/axi

-v /root/my_test/lib/lib.sv

+define+MY_MACRO

/////filelist.f

解释：

==*-v file //从文件file中寻找前面文件中instance了但没有define的 module的definition；==*

*==-y dir //从文件夹dir中寻找前面没找到的module definition；*==

+libext+lib_ext //在库目录dir中搜索文件时使用文件扩展名lib_ext

+indir+dir //从文件夹 dir中搜索\`include所包含的文件
## 2
[【工具使用】VCS & Verdi使用Makefile联合仿真 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/651156338)
[VCS+VERDI+Makefile 初体验 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/563041890)
