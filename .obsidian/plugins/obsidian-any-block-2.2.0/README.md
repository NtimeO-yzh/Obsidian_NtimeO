[中文](README_zh.md) | English

# obsidian-any-block

A Obsidian Plugin. You can flexibility to create a 'Block' by some means.
 
## Usage

Function：Block conversion、list to table or other tree graph，See the `demo` sample folder for details

[Usage Tutorial & Sample Library](./demo)

## If bug

~~try to close `strict line wrapping`~~

## Suport command

Look the setup panel or [Usage Tutorial & Sample Library](./demo)

Here are some of the more common processors:
- list2table  (2datatable)
- list2listtable
- list2mermaid  (graph LR)
- list2mindmad  (mermaid v9.3.0 mindmap)
- list2tab
- list2timeline
- title2list + list2somthing

![](demo/png/list2table.png)

![](demo/png/list2mdtable.png)

![](demo/png/list2tableT.png)

![](demo/png/list2ut.gif)

![](demo/png/list2tab.gif)

![](demo/png/list2mermaid.png)

![](demo/png/list2mindmap.png)

![](demo/png/titleSelector.png)

![](demo/png/addTitle.png)

![](demo/png/scroll.gif)

![](demo/png/overfold.png)

![](demo/png/flod.gif)

![](demo/png/heimu.gif)

![](demo/png/userProcessor.png)

## Update log

- v1.3.0
	- 用户层面的修改不多，但对代码处理器部分**重构了一遍**，以便后续的开发
		- 使用了SVELTE框架辅助开发
		- 扩展了处理器接口，能够接受更多种类的处理器、且处理器的组合会更加多样
	- 提供了处理器的简单自定义（通过设置面板设置 `别名` 的方式）
	- 提供了众多新的处理器，与新的处理器类型——装饰处理器
		- 标题& 黑幕& 折叠& 滚动& 动态增加class
- v1.3.0 之前
	- 不记录，详见commits
	- 增加了标题选择器

## support

开发不易，赞助入口（可备注：OB插件support）

![](demo/png/support_zfb.png)![](demo/png/support_wechat.png)


![Obsidian Downloads](https://img.shields.io/badge/dynamic/json?logo=obsidian&color=%23483699&label=downloads&query=%24%5B%22obsidian-any-block%22%5D.downloads&url=https%3A%2F%2Fraw.githubusercontent.com%2Fobsidianmd%2Fobsidian-releases%2Fmaster%2Fcommunity-plugin-stats.json)

## Todo

**(Don't repet it in issue)**

- 首要TODO
	- 第一优先级：解决嵌套问题（顺便有助于解决复选框的bug、有助于增加段落选择器和负级选择器）
	  （实现思路：以list选择器为例。
	  md选择器策略：例如开始正则选择时列表，然后通过`-`的前面得知该块的所属 `环境`，要求在上一行找到同`环境`下的header。
	  缺点：无法启用无header搜索，否则一但嵌套就会发生选择器范围交叉。
	  html选择器策略：直接在query find list元素，然后找到该选择器的父亲，如果不是根div说明该列表被嵌套，就在嵌套容器找header。如果是根div，那就要到上一个根div里找header）
	- 第二优先级：
	  （这三个点要依次渐进实现，实现前一个才能实现下一个）
		1. `| `增加下级项=>`\ `或`/ `或`& ` 增加同级项（能更好地压缩高度，也有主于ul表格的生成。开发难度：`|`和`\`混杂在一起不好处理）
		2. `表格项` 的接口需要扩展，加多一个接口项：来表明这个项是通过换行生成还是`|`或`\`，否则难以做到下面的问题
		3. 可视化编辑表格（实现难度：必须前解决上面的问题，否则反向编辑会有问题（会将内联块拆除掉了））
	- 第三优先级
		- 增加处理器或选择器。例如：转置表格、QA处理器
- reinforce
	- 选择器
		- **嵌套选择器**
		  没有嵌套的程序是没有灵魂的 !!!
		  （但问题在于，例如说第一层是tree，可能会破坏结构，有歧义。因为现在的tree格式是number-str的，那需要number-dom才行）
		  （或者说：列表选择器不能嵌套列表选择器有歧义，需要嵌套引用选择器，在此基础上你解除引用选择器间接嵌套）
	- 处理器
		- 优化2ultable，在这个模式中让内联换行变成同级换行而非下级换行的意思
	- 层级
		- 负级列表开关
		- 根据层级关系，推荐合理的处理器（如检测到树相关的就推荐树类处理器）
	- 样式
		- 树表格的间隔着色样式获取可以优化
		  方案1：例如多行的格可以视情况使用渐变（单数不用，复数需要，但会不会有不统一的问题）？
		  方案2：仿mindmap的着色，后面的列就不要隔行着色了
	- 转化
		- 右键选择转化为：md原生(表格)/html格式/图片
- fixing bug
	- **引用块内的列表/列表内的引用块 无法识别**
	- **表格转置与表头符号冲突、转置模式目前是纯css实现的 如果大家的行高不相同，会出现不匹配的情况。**
	  后续会将css实现改进为转化table元素实现

Reference、import

- [html-to-md](https://github.com/stonehank/html-to-md)
- [mermaid](https://github.com/mermaid-js/mermaid)