电脑 MAC, 使用 VMware Fusion 下载 centos7 虚拟机
# 安装的软件
## S
1. DesignCompiler2016
2. Formality2015
3. PrimeTime2016
4. SpyGlass2016
5. Vcs2016
6. Verdi2016
7. hspice
8. cscope
## C
1. ncverilog
2. encounter(innovus)
# 基本问题
1. 查看当前的授权进程:`ps -ef | grep lmgrd`
	使用命令 `ps -ef | grep lmgrd` 可以在 Linux 系统中查找正在运行的名为 "lmgrd" 的进程。该命令会列出所有与 "lmgrd" 相关的进程信息。
2. 
# 安装 Synopsys相关软件
## 参考网站:
- [提供了软件和教程]([https://blog.csdn.net/qiumingT/article/details/131700570](https://blog.csdn.net/qiumingT/article/details/131700570))
- [解决了启动较慢的问题]([https://bbs.eetop.cn/thread-762919-2-1.html](https://bbs.eetop.cn/thread-762919-2-1.html))的 5 楼

## 可能出现的问题
1. 每次重启都需要重新授权: 还未解决, 不太懂 licence 和 crack 的原理
2. 设置 verdi 的启动目录之后还会记录 log 到主文件夹里边: 还未解决
3. 安装过程中可能缺少一些 lib: 使用`yum install 相关 lib` 就可以了
# Hspice
## 参考网站
- [提供教程](https://my.oschina.net/u/4681268/blog/4651724)
- [提供软件](https://bbs.eetop.cn/thread-313905-1-1.html)
## 可能出现的问题
1. 无法保存.dat : 根据教程，不需要替换src文件，查找 src 里边的所有的 2019 和 2020, 改成以后的时间就可以
2. 环境变量如下
```
export PATH=/home/EDA/synopsys/hspice/hspice/bin:$PATH
#export PATH=/home/EDA/synopsys/scl10/amd64/bin$PATH:
export LM_LICENSE_FILE=/home/EDA/synopsys/hspice/synopsys.dat
alias spice='/home/EDA/synopsys/scl10/linux/bin/lmgrd -c /home/EDA/synopsys/hspice/synopsys.dat'
export PATH=$PATH:/home/EDA/synopsys/scl10/linux/bin
```

# Cosmos Scope
## 参考网站
- [安装包来源](https://bbs.eetop.cn/thread-615274-1-1.html)
- 使用方法: 
	- 打开 terminal
	- 进入Cosmosscope F-2011.09 linux .bin所在的文件夹
	- ./Cosmosscope F-2011.09 linux .bin
	- 安装即可
## 可能出现的问题
1. 不需要授权: 需要,使用的是 syno 的 license, 但是不知道为啥
2. 具体安装过程: 忘了

# Nclunch
## 参考网站
- [安装包]([http://pan.baidu.com/s/1geInZ4v](http://pan.baidu.com/s/1geInZ4v)) 提取码:1dhs
- [安装教程]([https://bbs.eetop.cn/thread-315210-1-1.html](https://bbs.eetop.cn/thread-315210-1-1.html))
- [patch文件]([https://bbs.eetop.cn/thread-224081-4-1.html](https://bbs.eetop.cn/thread-224081-4-1.html))
# Innovus(Encounter)
## 参考网站
- [安装包](https://bbs.eetop.cn/thread-921581-1-1.html)
- [安装教程](https://blog.csdn.net/qq_44447544/article/details/122698979)
- [1patch]([https://bbs.eetop.cn/thread-878045-1-1.html](https://bbs.eetop.cn/thread-878045-1-1.html))

# 研一下
这个学期需要使用**服务器**, 不使用那个盗版的东西了, 因此以上的东西就作废了.
服务器使用的是Royal Tsx和XQuartz, 前者能够很方便的直接连上一个服务器, 后者是能够使得图形化界面转发到本地.
优点
1. 能够直接登录
2. 能够把服务器上的文件进行本地编辑
3. 能够很方便的进行拖拽式的上传和下载
缺点
1. 终端不能高亮什么的