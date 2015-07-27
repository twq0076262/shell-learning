# shell 学习四十七天---文件比较 cmp，diff，patch

文件比较  
所谓的文件比较,一般设计四个领域  
1. 检查两个文件是否相同,如果不同,找不哪里不同  
2. 应用两个文件的不同之处,使从其中一个回复另外一个  
3. 使用校验和找出相同一致的文件  
4. 使用数字签名以验证文件  
 
 
 
cmp 和 diff  
在文字处理上,最常出现的问题应该是比较两个或两个以上的文件,看看他们的内容是否相同----即便它们的名称不同.  
案例:  

```
$cp /bin/ls /tmp/ls #制作/bin/ls的私用副本  
$cmp /bin/ls /tmp/ls #拿原始文本与副本比较  
$cmp /bin/cp /tmp/ls #输出结构指出第一个不同处的位置  
/bin/cp /tmp/ls differ: byte 25, line 1  
```
 
分析:cmp 发现两个参数文件一致时,会采用默认的方式.如果你只对他的离开状态有兴趣,可以使用-s 选项,抑制警告信息:  

```
$cmp -s /bin/cp /tmp/ls       #默认的比较两文件的不同   
$echo $? #显示离开码  
1 #非0,表示两个文件不同  
```
 
cmp 命令详解:  
语法:  
cmp [选项] [文件]  
主要选项:  
<table>
<tbody>
<tr>
<td valign="top">
<p>-c</p>
</td>
<td valign="top">
<p>除了标明差异处的十进制字码之外<span style="font-family:Times New Roman">,</span><span style="font-family:宋体">一并显示该字符所对应字符</span><span style="font-family:Times New Roman">.</span></p>
</td>
</tr>
<tr>
<td valign="top">
<p>-i&lt;<span style="font-family:宋体">字符数目</span><span style="font-family:Times New Roman">&gt;&nbsp;</span></p>
</td>
<td valign="top">
<p>制定一个数目</p>
</td>
</tr>
<tr>
<td valign="top">
<p>-l</p>
</td>
<td valign="top">
<p>显示出所有不一样的地方</p>
</td>
</tr>
<tr>
<td valign="top">
<p>-s</p>
</td>
<td valign="top">
<p>不显示错误的信息</p>
</td>
</tr>
<tr>
<td valign="top">
<p>-v</p>
</td>
<td valign="top">
<p>显示版本信息</p>
</td>
</tr>
</tbody>
</table>

注意:在比较结果中,只能够西显示第一个不同的比较结果.  
 
 
 
如果你想知道两个文件有何不同,可使用 diff,diff 命令是 linux 上非常重要的命令,用于比较文件的内容,特别是比较两个版本不同的文件以找到改动的地方.diff 在命令行中打印每一行的改动.最新版本的 diff 还支持二进制文件.diff 程序的输出被称为补丁(patch),因为 linux 系统中海油一个 patch 程序,可以根据 diff 的输出将 a.c 的文件内容更新为 b.c . diff 是 svn,cvs,git 等版本控制工具不可获取的一部分.  
 
 
diff 命令详解  
语法:  
diff [选项] [变动前文件或目录] [变动后文件或目录]  
功能:  
比较单个文件或目录的内容.如果指定比较的是文件,则只有当输入为文本文件时才有效.以逐行的方式,比较文本文件的异同处.如果指定比较的是目录,diff 命令会比较两个目录下名字相同的文本文件.列出不同的二进制文件,公共子目录和只在一个目录出现的文本.  
主要选项:  
<table>
<tbody>
<tr>
<td valign="top">
<p>-b</p>
</td>
<td valign="top">
<p>不检查空格字符的不同</p>
</td>
</tr>
<tr>
<td valign="top">
<p>-B</p>
</td>
<td valign="top">
<p>不检查空白行</p>
</td>
</tr>
<tr>
<td valign="top">
<p>-w</p>
</td>
<td valign="top">
<p>忽略全部的空格字符</p>
</td>
</tr>
<tr>
<td valign="top">
<p>-i</p>
</td>
<td valign="top">
<p>不检查大小写的不同</p>
</td>
</tr>
<tr>
<td valign="top">
<p>-q</p>
</td>
<td valign="top">
<p>仅显示有无差异<span style="font-family:Times New Roman">,</span><span style="font-family:宋体">不显示详细的信息</span></p>
</td>
</tr>
</tbody>
</table>
 
 
在 diff 目录时常用的参数如下:  
<table>
<tbody>
<tr>
<td valign="top">
<p>-r</p>
</td>
<td valign="top">
<p>比较子目录中的文件</p>
</td>
</tr>
<tr>
<td valign="top">
<p>-N</p>
</td>
<td valign="top">
<p>文件<span style="font-family:Times New Roman">A</span><span style="font-family:宋体">仅出现在某个目录中</span><span style="font-family:Times New Roman">,</span><span style="font-family:宋体">预设会显示</span><span style="font-family:Times New Roman">:only&nbsp;in</span><span style="font-family:宋体">目录</span><span style="font-family:Times New Roman">,</span><span style="font-family:宋体">文件</span><span style="font-family:Times New Roman">A</span><span style="font-family:宋体">若使用</span><span style="font-family:Times New Roman">-N</span><span style="font-family:宋体">参数</span><span style="font-family:Times New Roman">,</span><span style="font-family:宋体">则</span><span style="font-family:Times New Roman">diff</span><span style="font-family:宋体">会将文件</span><span style="font-family:Times New Roman">A</span><span style="font-family:宋体">与一个空白的文件比较</span></p>
</td>
</tr>
<tr>
<td valign="top">
<p>-P</p>
</td>
<td valign="top">
<p>与<span style="font-family:Times New Roman">-N</span><span style="font-family:宋体">类似</span><span style="font-family:Times New Roman">,</span><span style="font-family:宋体">只有当第二个目录包含了一个第一个目录所没有的文件时</span><span style="font-family:Times New Roman">,</span><span style="font-family:宋体">才会将这个文件与空白的文件作比较</span><span style="font-family:Times New Roman">.</span></p>
</td>
</tr>
</tbody>
</table>

diff 有四种格式:  
1. 正常格式  
2. 并列格式  
3. 上下文格式  
4. 合并格式  
 
正常格式案例:不添加任何参数  


```$cat f1```  
```a b```  
```b```  
```c```  
```d```  
```e```  
```$cat f2```  
```a b c```   
```b```  
```d```  
```e```  
```f```  
```1c1```    
```< a b```  
```---```  
```> a b c```  
```3d2```  
```< c```  
```5a5```  
```> f```  


分析:1c1,3d2,5a5 是说明变动的位置,前边数字代表 f1 中国所变化的行,后面的则代表 f2 中变化的行,中间的字母分别代表”改变(c)”,”删除(d)”和”增加(a)”  
 
<表示 f1 指定行的内容,---分割两个文件的,然后>表示 f2 指定行的内容.删除或增加时,则分别 f2,f1 中指定行无内容  
 
 
并列格式 diff
其他格式diff都实现后显式两个文件的内容变化,并列格式可以并排显式两个文件的内容变化,更形象的看出文件的变化,和vimdiff显式的有些类似.
 
使用方法为加入-y 选项,即可并列显示,-W(大写) num 参数可设定并列的宽度,可以不使用.  
$ diff -y -W 50 f1 f2  
a b                   | a b c  
b                       b  
c                     <  
d                       d  
e                       e  
                      > f  
| 说明此行有变化,<说明此行被删除了,>说明此行是后增加的.   
 
上下文格式 diff  
标准格式 diff 显式的内容不够直观,上下文格式则通过显示变化的上下文,而更加的利与理解.  
使用方法是使用参数-c.  
案例  
  
```$diff -c f1 f2```   
```*** f1  2015-07-13 18:42:50.996380933 +0800```  
```--- f2  2015-07-13 18:43:06.846375746 +0800```  
```***************```  
```*** 1,5 ****```  
```! a b```  
```  b```  
```- c```  
```  d```  
```  e```  
```--- 1,5 ----```  
```! a b c```  
```  b```  
```  d```  
```  e```  
```+ f```  

分析:首先,显示两个文件的基本情况:   
*** f1  2015-07-13 18:42:50.996380933 +0800  
--- f2  2015-07-13 18:43:06.846375746 +0800  
***表示变动前的文件 f1,---表示变动后的文件 f2   
然后 15 个星号将文件的基本情况和变动内容分隔开  
*** 1,5 ****  表示 f1 文件的 1-5 行  
--- 1,5 ----    表示 f2 文件的 1-5 行  
!代表此行内容有变动,+表示新增加的,- 表示此行被删除了.  
上下文格式默认显示包括修改行前后的三行内容,可以使用-num 来设置前后 num 行,如:  
$diff -C(大写C) -1(数字1) f1 f2  
 
 
合并格式 diff  
两个文件大量内容重复,上下文格式显示很多无用的干扰信息,后来就推出了合并式 diff.  
使用方法为,加入-u 参数,案例:  

```
$diff -u f1 f2  
diff -u f1 f2  
--- f1  2015-07-13 18:42:50.996380933 +0800  
+++ f2  2015-07-13 18:43:06.846375746 +0800  
@@ -1,5 +1,5 @@  
-a b  
+a b c  
 b  
-c  
 d  
 e  
+f  
```

分析:同样前两行表示两个文件的基本情况  
然后@@-1,5 +1,5 @@表示修改的位置,-代表 f1 的 1-5 行,+代表 f21-5 行.  
最后是合并显示的变动具体内容,依旧是-代表 f1,+代表 f2.  
同上下文格式一样,合并格式也是默认显示修改前后三行的内容,可以使用-num 来设置显示前后 num 行:  
```$diff -u -1 f1 f2```   
 
patch 工具程序  
patch 将 diff 的输出重定向到文本文件中,即得到了补丁文件(patch),可以使用 patch 命令对文本文件或目录打补丁,从而进行内容更新.  
patch详解  
语法:  
patch [选项] [补丁文件]  
主要参数:  
<table>
<tbody>
<tr>
<td valign="top">
<p>-p&nbsp;num</p>
</td>
<td valign="top">
<p>忽略几层文件夹</p>
</td>
</tr>
<tr>
<td valign="top">
<p>-E</p>
</td>
<td valign="top">
<p>选项说明如果发生了空文件<span style="font-family:Times New Roman">,</span><span style="font-family:宋体">那么就删除它</span></p>
</td>
</tr>
<tr>
<td valign="top">
<p>-R</p>
</td>
<td valign="top">
<p>取消打过的补丁</p>
</td>
</tr>
</tbody>
</table>
 
如果使用参数-p0,表示从当前目录找打补丁的目标文件夹,再对该目录中的文件执行 patch 操作.  
而是用参数-p1,表示忽略第一层目录,从当前目录需找目标文件夹中的子目录的文件,进行 patch 操作.  
处理单个文件补丁  
产生补丁:  
```$diff -uN f1 f2 > file.patch```  
打补丁:  
```$patch -p0 <file.patch```  
或者  
```$patch f1 file.patch```  
 
取消补丁:  
```$patch -RE -p0 < file.patch```  
或者  
```$patch -RE f1 file.patch```  
 
 
 
处理文件补丁  
产生补丁:  
```$diff -urN d1 d2 > dir.patch```  
打补丁:  
```$cd d1```  
```$patch -p1 < ../dir.patch```  
取消补丁:  
```patch -R -p1 < ../dir.patch```  
应用补丁时的目标代码和生成补丁时的代码未必相同,打补丁的操作可能失败,补丁失败的文件会以.rej 结尾.  
 
patch 工具程序可利用 diff 输出,结合原始文件,以重建另一个文件,因为相异的部分,通常比原始文件小得多,软件开发人员常会通过 email 交换相异处的列表,再使用 patch 应用它.  
 
 
虽然 patch 可使用 diff 的一半输出,但较通用的当时影视使用 diff 的-c 选项,已取得上下文差异处.这么做会产生较详细冗长的报告,rangpatch 知道文件名,允许他验证变更位置,并回复不同之处.如果两个文件自从差异出已被记录下来之后都未有修改,则上下文差异功能是不重要的,但是在软件开发中,时常会有其中之一牵涉其中.  
不难看出 patch 的作用就是为了高效的就留程序源代码产生的,patch 只包含了对源代码修改的部分.  