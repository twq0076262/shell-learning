# shell 学习二十八天---case 语句

case 语句  

```
case $1 in  
-f)  
... 针对-f玄子昂的程序代码  
;;  
-d | --directory) #允许长选项  
... 针对-d选项的程序代码  
;;  
*)  
echo $1:unkonw option >$2  
exit 1  
#在esac之前的;;形式是一个好习惯,不过并非必要  
esac
```  
 
这里我们看到,要测试的值出现在 case 和 in 之间.将值以双引号括起来并非必要,但也无妨.要测试的值,根据 shell 模式的列别一次测试,返现匹配的时候,便执行相对应的程序代码,直至;;为止.可以使用多个模式,只要| 字符加以分割即可.这种情况称为”or(或)”.模式里会包含任何的 shell 统配字符,且变量,命令与算数替换会在它用作模式匹配之前在此值上被终止.  
可能会觉得每个模式列表之后的部队称的右圆括号有点奇怪,不过这也是 shell 于艳丽部队称定界符的位移实例.
 
最后的*模式视窗通用发,但是非必须的,他作为一个默认的情况.这通常实在你要现实诊断信息并退出时使用.最后一个情况不再需要结尾的;;,不过加上他,会是比较好的形式  
案例:提示输入 1 到 4，与每一种模式进行匹配  
bash 代码:  

```
echo 'input your a number 1 to4'  
echo 'your number is : \n'  
read aNum  
case $aNum in  
    1)echo 'number 1'  
    ;;  
    2)echo 'number 2'  
    ;;  
    3)echo 'number 3'  
    ;;  
    4)echo 'number 4'  
    ;;  
    *)echo 'number default'  
    ;;  
esac  
```
 
案例二:判断输入文件是文件还是目录  

```
option="${1}"  
case ${option} in   
    -f) file="${2}"  
    echo "file name is $file"  
    ;;  
    -d)     dir="${2}"  
    echo "dir name is $dir"  
    ;;  
    *)echo "basename ${0} :usage:[-f file ]| [-d  directory]"  
    exit 1  
    ;;  
esac  
```
 
案例三:

```
bash代码;  
 #!/bin/bash  
name='basename $0 .sh'  
case $1 in  
    s|start) echo "start..."
    ;;
    stop) echo "stop ..."
    ;;
    reload)echo "reload..."
    ;;
    *)echo "Usage: $name [start|stop|reload]"
    exit 1
    ;;
esac
``` 

注意：1、 *) 相当于其他语言中的 default。  
   2、除了*)模式，各个分支中;;是必须的，;;相当于其他语言中的 break  
  3、 | 分割多个模式，相当于 or  
 
复习一下变量说明:
 <table>
<tbody>
<tr>
<td valign="top">
<p>变量</p>
</td>
<td valign="top">
<p>作用</p>
</td>
</tr>
<tr>
<td valign="top">
<p>$$</p>
</td>
<td valign="top">
<p>shell<span style="font-family:宋体">本身的</span><span style="font-family:Times New Roman">PID(ProcessID)</span></p>
</td>
</tr>
<tr>
<td valign="top">
<p>$!</p>
</td>
<td valign="top">
<p>sehll<span style="font-family:宋体">最后运行运行的后台</span><span style="font-family:Times New Roman">Process</span><span style="font-family:宋体">的</span><span style="font-family:Times New Roman">PID</span></p>
</td>
</tr>
<tr>
<td valign="top">
<p>$?</p>
</td>
<td valign="top">
<p>最后运行的命令的结束代码<span style="font-family:Times New Roman">(</span><span style="font-family:宋体">返回值</span><span style="font-family:Times New Roman">)</span></p>
</td>
</tr>
<tr>
<td valign="top">
<p>$-</p>
</td>
<td valign="top">
<p>使用<span style="font-family:Times New Roman">set</span><span style="font-family:宋体">命令设定的</span><span style="font-family:Times New Roman">Flag</span><span style="font-family:宋体">一览</span></p>
</td>
</tr>
<tr>
<td valign="top">
<p>$*</p>
</td>
<td valign="top">
<p>所有参数列表<span style="font-family:Times New Roman">.</span><span style="font-family:宋体">如</span>”$*”用圆括号括起来<span style="font-family:Times New Roman">,</span><span style="font-family:宋体">以</span>”$1&nbsp;$2&nbsp;...$n”的行为输出所有参数</p>
</td>
</tr>
<tr>
<td valign="top">
<p>$@</p>
</td>
<td valign="top">
<p>所有参数列表<span style="font-family:Times New Roman">,</span><span style="font-family:宋体">如果</span>”$@”用圆括号括起来<span style="font-family:Times New Roman">,</span><span style="font-family:宋体">以</span>”$1”&nbsp;“$2”&nbsp;“$n”的形式输出所有参数</p>
</td>
</tr>
<tr>
<td valign="top">
<p>$#</p>
</td>
<td valign="top">
<p>添加到<span style="font-family:Times New Roman">shell</span><span style="font-family:宋体">的参数个数</span></p>
</td>
</tr>
<tr>
<td valign="top">
<p>$0</p>
</td>
<td valign="top">
<p>shell<span style="font-family:宋体">本身的文件名</span></p>
</td>
</tr>
<tr>
<td valign="top">
<p>$1~$n</p>
</td>
<td valign="top">
<p>添加到<span style="font-family:Times New Roman">sehll</span><span style="font-family:宋体">的各参数值</span><span style="font-family:Times New Roman">.$1</span><span style="font-family:宋体">是第一个参数</span><span style="font-family:Times New Roman">,$2</span><span style="font-family:宋体">是第二个参数</span><span style="font-family:Times New Roman">,</span><span style="font-family:宋体">以此类推</span></p>
</td>
</tr>
</tbody>
</table>

案例:  
```printf "The complete list is %s\n" "$$"```  
结果:The complete list is 1567