# shell 学习三十五天---波浪号展开与通配符

波浪号展开与通配符  
shell 中两种与文件名相关的展开.第一种是波浪号展开,第二种是通配符展开式.  
波浪号展开  
如果命令行字符串的第一个字符为波浪号(~),或者变量指定(例如 PATH 或 CDPATH 变量)的值里任何未被引号括起来的冒号之后的第一个字符为波浪号(~)时,shell 变回执行波浪号展开.  
波浪号展开的目的,将用户根目录的符号型表示方式,改为实际的目录路径.可以采用直接或间接的方式指定执行此程序的用户,如未明白指定,则为当前的用户:  
命令:```vi ~/.profile```       与```vi $HOME/.profile```相同  
命令:```vi ~root/.profile``` 编辑用户 root 的.profile 文件  
 
案例分析:第一个命令,shell 将~换成$HOME,也就是当前用户的根目录.第二个命令,则是 shell 在系统的密码库里,需找用户 root,再将~root 置换为 root 的根目录.  
 
使用波浪号的好处:  
1. 这是一种简介的概念表示方式  
2. 这可以避免在程序里把路径名称直接编码,例如:  
有一段 bash 脚本:  

```
printf “enter username : ”  
read user  
vi /home/$user/.profile 编辑该用户的.profile 文件  
```
这段程序假设所有用户的根目录都在/home 之下.如果这又任何变动(例如,用户子目录根据部门存放在部门目录的子目录下),那么这个脚本就得重写.但如果使用波浪号展开,就能避免重写的情况:  

```
printf “enter username : ”  
read user  
vi /home/$user/.profile 编辑该用户的.profile文件  
```
这样一来,无论用户的根目录在哪里,程序都能正常运行了.  
 
使用通配符  
寻找文件名里的特殊字符,也是shell提供的服务之一.  
<table>
<tbody>
<tr>
<td width="568" valign="top" colspan="2">
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;基本的通配符</p>
</td>
</tr>
<tr>
<td valign="top">
<p>通配符</p>
</td>
<td valign="top">
<p>匹配</p>
</td>
</tr>
<tr>
<td valign="top">
<p>*</p>
</td>
<td valign="top">
<p>任何的字符串字符</p>
</td>
</tr>
<tr>
<td valign="top">
<p>[set]</p>
</td>
<td valign="top">
<p>任何在<span style="font-family:Times New Roman">set</span><span style="font-family:宋体">里的字符</span></p>
</td>
</tr>
<tr>
<td valign="top">
<p>[!set]</p>
</td>
<td valign="top">
<p>任何不在<span style="font-family:Times New Roman">set</span><span style="font-family:宋体">里的字符</span></p>
</td>
</tr>
<tr>
<td valign="top">
<p>?</p>
</td>
<td valign="top">
<p>任何的单一字符</p>
</td>
</tr>
</tbody>
</table>
 
?通配符匹配于任何的单一字符,所以如果你的目录里含有demo.a,demo.b,demo.txt 这三个文件,与表达式 demo.?匹配为 demo.a,demo.b,但是 demo.txt 则不匹配.  
星号(*)是一个功能强大的且广为使用的通配符;它匹配于任何字符组成的字符串.使用表达式 demo.*会匹配前面说的三个文件;网页设计人员也可以用*.html 表达式匹配他们的输入文件.  
set 结构是一组组字符列表(例如 abc),一段内含的范围(如 a-z),或者是两者的结合.如果希望破折号也是列表的一部分,只要把它放在第一个或最后一个就可以了.  
<table>
<tbody>
<tr>
<td width="568" valign="top" colspan="2">
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;使用<span style="font-family:Times New Roman">set</span><span style="font-family:宋体">结构的通配符</span></p>
</td>
</tr>
<tr>
<td valign="top">
<p>表达式</p>
</td>
<td valign="top">
<p>匹配的单一字符</p>
</td>
</tr>
<tr>
<td valign="top">
<p>[abc]</p>
</td>
<td valign="top">
<p>a,b<span style="font-family:宋体">或</span><span style="font-family:Times New Roman">c</span></p>
</td>
</tr>
<tr>
<td valign="top">
<p>[.,;]</p>
</td>
<td valign="top">
<p>句号<span style="font-family:Times New Roman">,</span><span style="font-family:宋体">逗号</span><span style="font-family:Times New Roman">,</span><span style="font-family:宋体">或分号</span></p>
</td>
</tr>
<tr>
<td valign="top">
<p>[-_]</p>
</td>
<td valign="top">
<p>破折号或下划线</p>
</td>
</tr>
<tr>
<td valign="top">
<p>[a-c]</p>
</td>
<td valign="top">
<p>a,b<span style="font-family:宋体">或</span><span style="font-family:Times New Roman">c</span></p>
</td>
</tr>
<tr>
<td valign="top">
<p>[a-z]</p>
</td>
<td valign="top">
<p>任意一个小写字母</p>
</td>
</tr>
<tr>
<td valign="top">
<p>[!0-9]</p>
</td>
<td valign="top">
<p>任意一个非数字字符</p>
</td>
</tr>
<tr>
<td valign="top">
<p>[0-9!]</p>
</td>
<td valign="top">
<p>任意一个数字会感叹号</p>
</td>
</tr>
<tr>
<td valign="top">
<p>[a-zA-Z]</p>
</td>
<td valign="top">
<p>任意一个大写或小写字母</p>
</td>
</tr>
<tr>
<td valign="top">
<p>[a-zA-Z0_9_-]</p>
</td>
<td valign="top">
<p>任何一个字母<span style="font-family:Times New Roman">,</span><span style="font-family:宋体">任何一个数字</span><span style="font-family:Times New Roman">,</span><span style="font-family:宋体">下划线或破折号</span></p>
</td>
</tr>
</tbody>
</table>
 
在原来的通配符返利中,demo.[ab]与 demo.[a-z]两者都匹配demo.a 和 demo.b,但是 demo.txt 则不匹配.  
在左方括号之后的感叹号用来”否定”一个 set.例如[!.;]符合句号和分号以外的任何一个字符;[!a-zA-Z]符合任何一个非字母的字符.  
 
范围表示法固然方便,但不应该对包含在范围内的字符有太多的假设.比较安全的方式是:分别指定所有大写字母,小写字母,数字,或任意的子范围(例如[f-q].[2-6]).不要想在标点符号字符上指定范围,或是在混用字母大小写上使用,像[a-Z]与[A-z]这样的用法,都不能保证一定能确切的匹配出包括所有想要的字母,而没有其他不想要的字符.更大的问题是在于:这样的范围在不同的类型之间的计算机之间无法提供完全的可移植性.  
 
另一个问题是:很多国家默认的系统语言环境与纯粹的 ASCII 的字符集是不同的.为了解决这个问题,POSIX 标准提出了方括号表达式,用来表示字母,数字,标点符号以及其他类型的字符,并且具有可移植性.在正则表达式下的方括号表达式里也出现相同的元素,它们可被用在兼容 POSIX 的 shell 内的 shell 通配符模式中,不过应该尽量避免将其应用在需可移植的 shell 脚本里.
 
习惯上,当执行通配符展开时,linux shell 会忽略文件名开头为一个点号的文件.像这样的”点号文件”通常用做程序配置文件或启动文件(一般都隐藏起来了,需要使用 ls -a 来查看).像是 shell 的$HOME/.profile,ex/vi 编辑器的$HOME/.exrc,以及 bash 与 gdb 使用的 GNU readline 程序库的$HOME/.inputrc.
 
要看到这类文件,需要在模式前面明确的提供一个点号.例如:  
echo .*                  显示隐藏文件  
 
注意:隐藏文件只是一个习惯用法.在用户层面的软件上他是这样的,但核心程序(kernel)并不认为开头带有一个点号的文件与其他文件有不同.