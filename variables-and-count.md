# shell 学习二十六天---变量与算数
 
变量与算数  
shell 脚本与函数还有位置参数的功能;传统的说法应该是”命令行参数”;  
shell 为内嵌算数提供了一种标记法,称为算数展开.shell 回对$((...))里的算符表达式进行计算,再将计算后的结构放回到命令的文本内.  
有两个相似的命令提供变量的管理,一个是 readonly,它可以使变量称为只读模式;而赋值给它们是被禁止的.在 shell 程序中,这是创建符号常量的一个好方法:  
```days_per_week=7```     赋值  
```readonly days_per_week```    设为只读模式  
 
export,readonly  
语法:  
```export name[=word]...```  
```export -p```  
```readonly name[=word]...```  
```readonly =p```  
用途:  
export 用于修改或打印环境变量,readonly 则使得变量不得修改.  
  
主要选项:  
-p:打印命令的名称以及所有被到处(只读)变量的名称与值,这种方式可使得 shell 重新读取输出以便重新建立环境(只读设置).  
行为模式:  
使用-p 选项,这两条命令都会分别打印他们的名称以及被到处的或只读的所有变量.  
较常见的命令是 export,用法是将变量放进环境变量里.环境是一个名称与值的简单列表,可供所有执行中的程序使用.新的进程会从父进程继承环境,也可以在建立新的紫禁城之前修改它.export 命令可将新变量添加到环境中:  
```PATH=$PATH:/usr/local/bin``` 更新 PATH  
```export PATH``` 导出它  
使用 export -p 命令可以显示当前环境  
变量可以添加到程序环境中,但是对 shell 或接下来的命令不会一直有效:将该变量赋值,置于命令名称与参数前即可:  
```PATH=/bin:/usr/bin awk ‘...’ file1 file2```  
这个 PATH 值只对后面 awk 起作用,其他命令将使用系统 PATH.  
使用 env 命令显示所有环境变量.  
unset 命令从执行中的 shell 中删除变量与函数.   
案例:  
清除环境变量的值使用 unset 命令.如果未定义指定值,则该变量值将被设为 NULL.  
首先使用命令```export TEST=”test”```来增加一个环境变量  
接着使用命令```env | grep TEST```,得到结果 TEST=test   
然后使用命令```unset $TEST``` 删除环境变量 TEST  
最后使用命令```env | grep TEST命令```,该命令不会有输出,说明成功的删除了.  
其中 unset 还可以通过添加-f选项删除指定的函数.  
unset 的行为模式  
如果没有提供选项,则参数将视为变量名称,并告知变量已删  
除,如果使用-f 选项,参数则被视为函数名称,并删除函数.  
 
注意:myvar=赋值并不会将 myvar 删除,只不过试讲其设为 null 字符串.相对的:unset myvar 完全删除它.这一差异在于”是变量设置”以为”是变量设置,但非 null”展开.  
 
参数展开  

```
var =”hello,world”  
echo ${var}  
hello,world  
```

其实这里说的参数(parameter)不就是我们通常说的变量(variable)么? 嗯。。其实大部分时候这俩名词的意思基本等同。只不过在 Shell 中 parameter(参数)是 variable(变量)的超集: 变量名不能以数字开头，而参数名可以。比如说$1 就表示命令行传入的第一个参数。  
参数展开是 shell 提供变量值在程序中使用的过程:例如,作为新变量的值,或是作为命令行的部分或全部参数.最简单的形式如下所示:  
```reminder =”Time to go to the dentist”```  将值存储在 reminder 中  
```sleep 120``` 等待两分钟  
```echo $reminder``` 显示信息  
在 shell 下,有更复杂的形式可用于更特殊的情况.这些形式都是将变量名称括在花括号里(${variable}),然后再增加额外的语法以告诉 shell 该做些什么.花括号本身也是很好用的,当你需要在变量名称之后马上跟着一个可能会解释为名称一部分的字符时,他就派上用场了:
 
reminder =”Time to go to the dentist”    将值存储在reminder中
sleep 120 等待两分钟
echo ${reminder} 显示信息
 
警告:默认情况下,未定义的变量会展开为 null(空的)字符串.程序随便乱写,就可能会导致灾难发生:  
rm -rf /$MYPROGRAM   如果未设置 MYPROGRAM,就会有大灾难发生了,所以在写 程序时一定要小心.  
 
展开运算符  
第一组字符串处理运算符用来测试变量的存在状态,且为在某种情况下的允许默认值的替换.  
替换运算符  
<table>
<tbody>
<tr>
<td valign="top">
<p>运算符</p>
</td>
<td valign="top">
<p>替换</p>
</td>
</tr>
<tr>
<td valign="top">
<p>${varname:=word}</p>
</td>
<td valign="top">
<p>如果<span style="font-family:Times New Roman">varname</span><span style="font-family:宋体">存在且不是</span><span style="font-family:Times New Roman">null,</span><span style="font-family:宋体">则返回它的值</span><span style="font-family:Times New Roman">;</span><span style="font-family:宋体">否则</span><span style="font-family:Times New Roman">,</span><span style="font-family:宋体">设置它为</span><span style="font-family:Times New Roman">word,</span><span style="font-family:宋体">并返回其值</span></p>
<p>用途<span style="font-family:Times New Roman">:</span><span style="font-family:宋体">如果变量未定义</span><span style="font-family:Times New Roman">,</span><span style="font-family:宋体">则返回默认值</span><span style="font-family:Times New Roman">.</span></p>
<p>范例<span style="font-family:Times New Roman">:</span><span style="font-family:宋体">如果&nbsp;</span><span style="font-family:Times New Roman">count</span><span style="font-family:宋体">未定义</span><span style="font-family:Times New Roman">,</span><span style="font-family:宋体">则&nbsp;</span><span style="font-family:Times New Roman">echo&nbsp;${count:-0}</span><span style="font-family:宋体">的值为</span><span style="font-family:Times New Roman">0</span></p>
</td>
</tr>
<tr>
<td valign="top">
<p>${varname:word}</p>
</td>
<td valign="top">
<p>如果<span style="font-family:Times New Roman">varname</span><span style="font-family:宋体">存在且不是</span><span style="font-family:Times New Roman">null,</span><span style="font-family:宋体">则返回它的值</span><span style="font-family:Times New Roman">;</span><span style="font-family:宋体">否则</span><span style="font-family:Times New Roman">,</span><span style="font-family:宋体">设置它为</span><span style="font-family:Times New Roman">word,</span><span style="font-family:宋体">并返回其值</span><span style="font-family:Times New Roman">.</span></p>
<p>用途<span style="font-family:Times New Roman">:</span><span style="font-family:宋体">如果变量未定义</span><span style="font-family:Times New Roman">,</span><span style="font-family:宋体">则设置变量为默认值</span><span style="font-family:Times New Roman">.</span></p>
<p>范例<span style="font-family:Times New Roman">:</span><span style="font-family:宋体">如果</span><span style="font-family:Times New Roman">count</span><span style="font-family:宋体">未定义</span><span style="font-family:Times New Roman">,echo${count:=0}</span><span style="font-family:宋体">输出为</span><span style="font-family:Times New Roman">0</span></p>
</td>
</tr>
<tr>
<td valign="top">
<p>${varname:?message}</p>
</td>
<td valign="top">
<p>如果<span style="font-family:Times New Roman">varname</span><span style="font-family:宋体">存在且非</span><span style="font-family:Times New Roman">null,</span><span style="font-family:宋体">则返回它的值</span><span style="font-family:Times New Roman">;</span><span style="font-family:宋体">否则</span><span style="font-family:Times New Roman">,</span><span style="font-family:宋体">显示</span><span style="font-family:Times New Roman">varname:message,</span><span style="font-family:宋体">并退出当前的命令或脚本</span><span style="font-family:Times New Roman">.</span><span style="font-family:宋体">省略</span><span style="font-family:Times New Roman">message</span><span style="font-family:宋体">会出现默认信息</span><span style="font-family:Times New Roman">parameter&nbsp;null&nbsp;or&nbsp;not&nbsp;set.</span><span style="font-family:宋体">注意</span><span style="font-family:Times New Roman">,</span><span style="font-family:宋体">在交互式</span><span style="font-family:Times New Roman">shell</span><span style="font-family:宋体">下不需要退出</span><span style="font-family:Times New Roman">(</span><span style="font-family:宋体">在不同的</span><span style="font-family:Times New Roman">shell</span><span style="font-family:宋体">间会有不同的行为</span><span style="font-family:Times New Roman">,</span><span style="font-family:宋体">用户需自行注意</span><span style="font-family:Times New Roman">).</span></p>
<p>用途<span style="font-family:Times New Roman">:</span><span style="font-family:宋体">为了捕捉由于变量未定义所导致的错误</span><span style="font-family:Times New Roman">.</span></p>
<p>范例<span style="font-family:Times New Roman">:${count:?</span>”undefined”}<span style="font-family:宋体">将显示</span><span style="font-family:Times New Roman">:count:undefined!,</span><span style="font-family:宋体">且如果</span><span style="font-family:Times New Roman">count</span><span style="font-family:宋体">未定义</span><span style="font-family:Times New Roman">,</span><span style="font-family:宋体">则退出</span></p>
</td>
</tr>
<tr>
<td valign="top">
<p>${varname:+word}</p>
</td>
<td valign="top">
<p>如果<span style="font-family:Times New Roman">varname</span><span style="font-family:宋体">存在且非</span><span style="font-family:Times New Roman">null,</span><span style="font-family:宋体">则返回</span><span style="font-family:Times New Roman">word;</span><span style="font-family:宋体">否则</span><span style="font-family:Times New Roman">,</span><span style="font-family:宋体">返回</span><span style="font-family:Times New Roman">null.</span></p>
<p>用途<span style="font-family:Times New Roman">:</span><span style="font-family:宋体">未测试变量的存在</span><span style="font-family:Times New Roman">.</span></p>
<p>如果<span style="font-family:Times New Roman">:</span><span style="font-family:宋体">如果</span><span style="font-family:Times New Roman">count</span><span style="font-family:宋体">已定义</span><span style="font-family:Times New Roman">,</span><span style="font-family:宋体">则</span><span style="font-family:Times New Roman">${count:+1}</span><span style="font-family:宋体">返回</span><span style="font-family:Times New Roman">1(</span><span style="font-family:宋体">也就是真</span><span style="font-family:Times New Roman">)</span></p>
</td>
</tr>
</tbody>
</table>
该表中每个运算符内的冒号(:)都是可选的.如果省略冒号,则将每个定义的”存在且非 null”部分改为”存在”,也就是说,运算符仅用于测试变量是否存在.  
 
模式匹配运算符  
<table>
<tbody>
<tr>
<td valign="top">
<p>运算符</p>
</td>
<td valign="top">
<p>替换</p>
</td>
</tr>
<tr>
<td valign="top">
<p>例<span style="font-family:Times New Roman">:${path#/*/}</span></p>
</td>
<td valign="top">
<p>结果<span style="font-family:Times New Roman">:tolstoy/mem/long.file.name</span></p>
</td>
</tr>
<tr>
<td valign="top">
<p>例<span style="font-family:Times New Roman">:${variable##pattern}</span></p>
</td>
<td valign="top">
<p>如果模式匹配于变量值的开头处<span style="font-family:Times New Roman">,</span><span style="font-family:宋体">则删除匹配的最长部分</span><span style="font-family:Times New Roman">,</span><span style="font-family:宋体">并返回剩下的部分</span><span style="font-family:Times New Roman">.</span></p>
</td>
</tr>
<tr>
<td valign="top">
<p>例<span style="font-family:Times New Roman">:${path##/*/}</span></p>
</td>
<td valign="top">
<p>结果<span style="font-family:Times New Roman">:long.file.name</span></p>
</td>
</tr>
<tr>
<td valign="top">
<p>例<span style="font-family:Times New Roman">:${path%.*}</span></p>
</td>
<td valign="top">
<p>结果<span style="font-family:Times New Roman">:/home/tolstoy/mem/long.file</span></p>
</td>
</tr>
<tr>
<td valign="top">
<p>例<span style="font-family:Times New Roman">:${variable%pattern}</span></p>
</td>
<td valign="top">
<p>如果模式匹配于变量值的结尾处<span style="font-family:Times New Roman">,</span><span style="font-family:宋体">则删除匹配的最短部分</span><span style="font-family:Times New Roman">,</span><span style="font-family:宋体">并返回剩下的部分</span><span style="font-family:Times New Roman">.</span></p>
</td>
</tr>
<tr>
<td valign="top">
<p>例<span style="font-family:Times New Roman">:${variable%%pattern}</span></p>
</td>
<td valign="top">
<p>如果模式匹配于变量值的结尾处<span style="font-family:Times New Roman">,</span><span style="font-family:宋体">则删除匹配的最长部分</span><span style="font-family:Times New Roman">,</span><span style="font-family:宋体">并返回省下的部分</span></p>
</td>
</tr>
<tr>
<td valign="top">
<p>例<span style="font-family:Times New Roman">:${path%%.*}</span></p>
</td>
<td valign="top">
<p>结果<span style="font-family:Times New Roman">:/home/tolstoy/mem/long</span></p>
</td>
</tr>
</tbody>
</table>
 
 
 
案例分析:${parameter#word}或{parameter##word}  
作用:从 parameter 头部开始匹配 word,并删除成功匹配的部分.在构造 word 时可以使用”*”表示任意长度的字符,”?”表示单位长度字符,并可用形如”[a-c]”的方式制定匹配”abc”中的任意字符.  
另外,”#”和”##”的区别在于前置是匹配最短,而后者是最长匹配;实际上就是正则表达式中的”懒惰”和”贪婪”的概念.  
```var=br1br2ead```  
```echo ${var$$*br}```  
输出:2ead  
```echo ${var#*br}```  
输出:1br2ead  
案例:  
${parameter%word}或${parameter%%word}  
作用:与前例相似,唯一不同的是从$parameter 的为不开始匹配.  
```var="La.Maison.en.Petits.Cubes.avi"```  
```echo ${var%.*}```  
输出:La.Maison.en.Petits.Cubes  
```echo ${var%%.*}```  
输出:La  
分析:匹配案例中的”.”时,shel l会从$var 的尾部开始查找”.”,如果是最短匹配(echo ${var%.*}),则会找到第一个”.”就停止,否则(echo ${var%%.*})会一直找到最后一个”.”才停止.可以看到,这种用法可以分方便的去掉文件后缀,从而得到文件名.  
 
使用${#variable}可以获得 variable 的长度:  
案例:```variable=qwertyuiop```;  
```echo ${#variable}```  
输出:10  
记忆:  
 #匹配的是前面,因为数字正负号总是置于数字之前;%匹配的是后面,因为百分比符号总是更在数字的后面.  
这里用到了两种匹配模式:/*/,匹配任何位于两个斜杠之间的元素;.*,匹配点号之后接着的任何元素.  
 
位置参数  
所谓的位置参数,指的是 shell 脚本的命令行参数;同时也表示 zaishell 函数内的函数参数,他们的名称是以单个的整数来命名.当整数大于9时,就应该以花括号括起来:  
echo frist arg is $1  
echo tenth arg is ${10}  
也可以将其与模式匹配运算符结合,应用到位置参数:  
filename=${1:-/dev/tty} 如果给定参数则使用它,如无参数则使用/dev/tty  
接下来的特殊”变量”提供了对传递的参数的总数的访问,以及一次对所有参数的访问:  
$# : 提供传递到 shell 脚本或函数的参数总是.当你是为了处理选项和参数而建立循环时,它会很有用.举例:  

```
while [ $# !=0 ] 以 shift 逐渐减少$#,循环将会终止  
do  
case $1 in  
... 处理第一个参数  
esac  
shift 已开第一个参数  
done  
```
 
$*,$@ : 以此表示所有的命令行参数.着两个参数可用来把命令行参数传递给脚本或函数所执行的程序.
 
“$*” : 将所有命令行参数视为单个字符串.等同于”$1 $2 ...” $IFS 的第一个字符用来作为分隔符,衣服个不同的值来建立字符串.案例:
printf “他和 arguments were %s\n” “$*”  
 
“$@” : 将所有的命令韩参数视为单独的而个体,就业就单独字符串.等同于”$1” “$2” ....这是将参数传递给其他程序的最佳凡是,因为他会保留所有的内嵌在每个参数里的任何空白.案例:  
lpr “$@” 现实每一个文件  
shift 命令是用来”截去”来自列表的位置参数,由左开始.一旦执行 shift,$1 的初值会永远消失,取而代之的是$2 的旧值.$2 的值变成$3 的旧值,依次类推.$#的值会逐次减一.shift 也可使用一个可选的参数,也就是要位移的参数的计数.单纯的 shift 等同于 shift 1.案例:  
```set -- hello “hi there” greetings``` 结束选项部分,自 hello 开始新的参数  
```echo $#``` 显示计数值  
 
```for i in $*``` 循环处理每一个参数  
```> do echo i is $i```         
```> done```  
输出:  
i is hello  
i is hi  
i is there  
i is greeting  
注意,内嵌的空白已消失  
使用命令:for i in $@    在没有双引号的额情况下,$@和$*得到的结果一样  
```> do echo i is $i```  
```> done```  
加了双引号 for i in “$*”  $*表示一个字符串  
```> do echo i in $i```  
```> done```  
输出:  
i in hello hi there greeting  
加了双引号 for i in “$@”   $@保留真正的参数值  
输出:  
i in hello  
i in hi there  
i in greeting  
使用命令 shift 截去第一个参数  
```echo there are now $# arguments```  
输出:there are now 2arguments 证明第一个参数已经消失  
使用命令:  
```for i in “$@”```  
输出为:  
i in hi there  
i in greeting  
 
特殊变量  
POSIX 中的内置变量  
<table>
<tbody>
<tr>
<td valign="top">
<p>变量</p>
</td>
<td valign="top">
<p>意义</p>
</td>
</tr>
<tr>
<td valign="top">
<p>#</p>
</td>
<td valign="top">
<p>表示变量的个数<span style="font-family:Times New Roman">,</span><span style="font-family:宋体">常用于循环</span></p>
</td>
</tr>
<tr>
<td valign="top">
<p>@</p>
</td>
<td valign="top">
<p>当前命令行所有参数<span style="font-family:Times New Roman">,</span><span style="font-family:宋体">置于双引号中</span><span style="font-family:Times New Roman">,</span><span style="font-family:宋体">表示个别命令</span></p>
</td>
</tr>
<tr>
<td valign="top">
<p>*</p>
</td>
<td valign="top">
<p>当前命令行所有参数<span style="font-family:Times New Roman">.</span><span style="font-family:宋体">置于双引号中</span><span style="font-family:Times New Roman">,</span><span style="font-family:宋体">表示将命令行所有参数当做一个单独参数</span></p>
</td>
</tr>
<tr>
<td valign="top">
<p>-(<span style="font-family:宋体">连字号</span><span style="font-family:Times New Roman">)</span></p>
</td>
<td valign="top">
<p>在引用数给予<span style="font-family:Times New Roman">shell</span><span style="font-family:宋体">的选项</span></p>
</td>
</tr>
<tr>
<td valign="top">
<p>?</p>
</td>
<td valign="top">
<p>表示上一个命令退出的状态</p>
</td>
</tr>
<tr>
<td valign="top">
<p>$</p>
</td>
<td valign="top">
<p>表示当前进程编号</p>
</td>
</tr>
<tr>
<td valign="top">
<p>0</p>
</td>
<td valign="top">
<p>表示当前进程名称</p>
</td>
</tr>
<tr>
<td valign="top">
<p>!</p>
</td>
<td valign="top">
<p>表示最近一个后台命令的进程编号</p>
</td>
</tr>
<tr>
<td valign="top">
<p>HOME</p>
</td>
<td valign="top">
<p>表示当前用户的根目录</p>
</td>
</tr>
<tr>
<td valign="top">
<p>IFS</p>
</td>
<td valign="top">
<p>表示内部的字符分隔符</p>
</td>
</tr>
<tr>
<td valign="top">
<p>LANG</p>
</td>
<td valign="top">
<p>当前<span style="font-family:Times New Roman">locale</span><span style="font-family:宋体">默认名称</span></p>
</td>
</tr>
<tr>
<td valign="top">
<p>PATH</p>
</td>
<td valign="top">
<p>环境变量</p>
</td>
</tr>
<tr>
<td valign="top">
<p>PPID</p>
</td>
<td valign="top">
<p>父进程编号</p>
</td>
</tr>
<tr>
<td valign="top">
<p>PWD</p>
</td>
<td valign="top">
<p>当前工作目录</p>
</td>
</tr>
</tbody>
</table>
特殊变量$$可在编写脚本时用来建立具有唯一性的文件名(多半是临时的),这是根据 shell 的进程编号建立文件名.不过系统中还有一个 mktemp 也能做同样的事情.  
 
算数展开  
shell 的算数运算符与C语言里的差不多,优先级与顺序也相同.  
<table>
<tbody>
<tr>
<td valign="top">
<p>运算符</p>
</td>
<td valign="top">
<p>意义</p>
</td>
<td valign="top">
<p>顺序</p>
</td>
</tr>
<tr>
<td valign="top">
<p>++&nbsp;--</p>
</td>
<td valign="top">
<p>增加以减少<span style="font-family:Times New Roman">,</span><span style="font-family:宋体">可前置也可放在结尾</span></p>
</td>
<td valign="top">
<p>由左至右</p>
</td>
</tr>
<tr>
<td valign="top">
<p>+&nbsp;-&nbsp;!&nbsp;~</p>
</td>
<td valign="top">
<p>一元的正好与符号<span style="font-family:Times New Roman">;</span><span style="font-family:宋体">逻辑与位的取反</span></p>
</td>
<td valign="top">
<p>由右至左</p>
</td>
</tr>
<tr>
<td valign="top">
<p>*&nbsp;/&nbsp;%</p>
</td>
<td valign="top">
<p>乘&nbsp;除&nbsp;取余</p>
</td>
<td valign="top">
<p>由左至右</p>
</td>
</tr>
<tr>
<td valign="top">
<p>+&nbsp;-</p>
</td>
<td valign="top">
<p>加&nbsp;减</p>
</td>
<td valign="top">
<p>由左至右</p>
</td>
</tr>
<tr>
<td valign="top">
<p>&nbsp;&lt;&lt;&nbsp;&gt;&gt;</p>
</td>
<td valign="top">
<p>向左位移<span style="font-family:Times New Roman">,</span><span style="font-family:宋体">向右位移</span></p>
</td>
<td valign="top">
<p>由左至右</p>
</td>
</tr>
<tr>
<td valign="top">
<p>&lt;&nbsp;&lt;=&nbsp;&gt;&nbsp;&gt;=</p>
</td>
<td valign="top">
<p>比较</p>
</td>
<td valign="top">
<p>由左至右</p>
</td>
</tr>
<tr>
<td valign="top">
<p>==&nbsp;!=</p>
</td>
<td valign="top">
<p>相等不相等</p>
</td>
<td valign="top">
<p>由左至右</p>
</td>
</tr>
<tr>
<td valign="top">
<p>&amp;</p>
</td>
<td valign="top">
<p>位的<span style="font-family:Times New Roman">AND</span></p>
</td>
<td valign="top">
<p>由左至右</p>
</td>
</tr>
<tr>
<td valign="top">
<p>^</p>
</td>
<td valign="top">
<p>韦德<span style="font-family:Times New Roman">Exclusive&nbsp;OR</span></p>
</td>
<td valign="top">
<p>由左至右</p>
</td>
</tr>
<tr>
<td valign="top">
<p>|</p>
</td>
<td valign="top">
<p>位的<span style="font-family:Times New Roman">OR</span></p>
</td>
<td valign="top">
<p>由左至右</p>
</td>
</tr>
<tr>
<td valign="top">
<p>&amp;&amp;</p>
</td>
<td valign="top">
<p>逻辑的<span style="font-family:Times New Roman">AND</span></p>
</td>
<td valign="top">
<p>由左至右</p>
</td>
</tr>
<tr>
<td valign="top">
<p>||</p>
</td>
<td valign="top">
<p>逻辑的<span style="font-family:Times New Roman">OR</span></p>
</td>
<td valign="top">
<p>由左至右</p>
</td>
</tr>
<tr>
<td valign="top">
<p>?:</p>
</td>
<td valign="top">
<p>条件表达式</p>
</td>
<td valign="top">
<p>由右至左</p>
</td>
</tr>
<tr>
<td valign="top">
<p>=&nbsp;+=&nbsp;-=&nbsp;*=&nbsp;/=&amp;=&nbsp;^=&nbsp;&lt;&lt;=&nbsp;&gt;&gt;=&nbsp;|=</p>
</td>
<td valign="top">
<p>赋值运算符</p>
</td>
<td valign="top">
<p>由右至左</p>
</td>
</tr>
</tbody>
</table>  
该表的运算符的优先级由高排列至最低.  
可利用圆括号将子表达式语句括起来.像 C 一样:关系运算符(<,<=,>,>=,==与!=)产生数字结果中,1 为真,0 为假.  
例如:```$((3>2))```的值为 1;```echo $(((3>2)||(4<=1)))```也为 1,因为着两个子表达式里有一个为真.  
对逻辑的 AND 与 OR 运算符而言,任何非 0 值函数都为真:  
```echo $((3&&4))``` 3 与 4 都为”真”  
++和--运算符不用说了.++与--运算符是可选的;实际上,所有支持  ${{...}}的 shell,都可以让用户在提供变量名称时,无须前置$符号.  