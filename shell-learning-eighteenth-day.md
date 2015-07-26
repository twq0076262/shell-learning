# shell 学习十八天
## 文本排序

排序文本  
行的排序,使用的命令 sort,该命令的语法是: ```sort [option] [files...].sort```将文件/文本的每一行作为一个单位，相互比较，比较原则是从首字符向后，依次按[ASCII](http://zh.wikipedia.org/zh/ASCII)码值进行比较，最后将他们按升序输出。  
入门案例:有一个文件 temp.txt,内容为:  
aaa:10:1.1ccc:30:3.3ddd:40:4.4bbb:20:2.2eee:50:5.5eee:50:5.5
 
使用 sort temp.txt 输出结果为:  
aaa:10:1.1bbb:20:2.2ccc:30:3.3ddd:40:4.4eee:50:5.5eee:50:5.5  
再来看 sort 的各个选项的使用:  
-b：忽略每行前面开始处的空格字符；  
-c：检查文件是否已经按照顺序排序，排序过为真；  
-d：排序时，处理英文字母、数字和空格字符，以字典顺序排序。忽略其他所有字符；  
-f：排序时，将小写字母视为大写字母；  
-i：排序时，处理 040~176 之间的 ASCII 字符，忽略其他所有字符；    
-m：将几个排序好的文件进行合并；   
-M：将前面3个字母按月份的缩写进行排序；  
-n：按照数值大小进行排序；  
-o outfile.txt：将排序后的结果存入 outfile.txt；  
-r：以相反的顺序进行排序；  
-k：指定需要排序的列数（栏数）；  
-t 分隔符：指定排序时所用到的栏位分隔符；  
+ 起始栏位 - 结束栏位：以指定的栏位来排序，范围从起始栏位到结束栏位的前一栏位。（古老的用法）  
 
案例:使用-u 选项的输出,还是针对文件 temp.txt  
aaa:10:1.1  
bbb:20:2.2  
ccc:30:3.3  
ddd:40:4.4  
eee:50:5.5  
例如:有一个文件 sort.txt,内容为:  
AA:BB:CC  
   aaa:30:1.6  
   ccc:50:3.3  
   ddd:20:4.2  
   bbb:10:2.5  
   eee:40:5.4  
   eee:60:5.1  
使用```sort -nk 2 -t: sort.txt``` 排序后的结果为:  
AA:BB:CC  
bbb:10:2.5  
ddd:20:4.2  
aaa:30:1.6  
eee:40:5.4  
ccc:50:3.3  
eee:60:5.1  
上述命令的意思是说,将第二列按照数字从小到大排列  
 
使用```sort -nrk 3 -t: sort.txt```,该命令的意思是说,将第三列数字从大到小排列,所以输出的结果为:  
eee:40:5.4  
eee:60:5.1  
ddd:20:4.2  
ccc:50:3.3  
bbb:10:2.5  
aaa:30:1.6  
AA:BB:CC  
备注: -n 是按照数字大小排序，-r 是以相反顺序，-k 是指定需要排序的栏位，-t 指定栏位分隔符为冒号.  
问题:有一个文件,内容为:  
banana:30:5.5  
apple:10:2.5  
pear:90:2.3  
orange:20:3.4  
第一列表示水果名称,第二列表示水果数量,第三列表式水果价格.我想按照水果的数量进行排序,怎么办?  
```sort -nk 2 -t : fruit.txt```(从小到大排序)  
```sort -nrk 2 -t : fruit.txt```(从大到小排序,注意选项的位置)  
如果我想给/etc/passwd 按照第三行排序,应该是这样:  
```sort -t':' -k 3 /etc/passwd```,但是细心的你一定会发现其中的问题,貌似不对啊?不应该啊?那是因为-k 选项默认是按照字典数排列,如果想按照数字排列需要指定```sort -t':' -k 3n /etc/passwd```,也就是需要加上-n 选项.  
如果想逆序排列呢?  
```sort -t':' -k 3nr /etc/passwd```  
如果要对/etc/passwd 文件中第六列的第二个字符到第四个字符正序排列,再基于第一列进行反向排序.  
```sort -t':' -k 6.2,6.4 -k 1r /etc/passwd```  
引入一个关于-k 选项的新知识.  
 
-k 选项的语法格式：  
 
```FStart.CStart Modifie,FEnd.CEnd Modifier-------Start--------,-------End-------- FStart.CStart 选项  ,  FEnd.CEnd 选项```
 
 
这个语法格式可以被其中的逗号（“,”）分为两大部分，Start 部分和 End 部分。Start 部分也由三部分组成，其中的 Modifier 部分就是我们之前说过的类似 n 和 r 的选项部分。我们重点说说 Start 部分的 FStart 和 C.Start。C.Start 也是可以省略的，省略的话就表示从本域的开头部分开始。FStart.CStart，其中 FStart 就是表示使用的域，而 CStart 则表示在 FStart 域中从第几个字符开始算“排序首字符”。同理，在 End 部分中，你可以设定 FEnd.CEnd，如果你省略.CEnd，则表示结尾到“域尾”，即本域的最后一个字符。或者，如果你将 CEnd 设定为 0(零)，也是表示结尾到“域尾”。  
那么很显然 -k 6.2,6.4 的意思很明确,第六个字段的第二个字符到第六个字段的第四个字符.按照正序排列.
 
案例:
查看/etc/passwd 有多少个 shell:对/etc/passwd 的第七个域进行排序，然后去掉重复的行:  
```cat /etc/passwd |  sort -t':' -k 7 -u```
 
sort 的行为模式:它会读取指定的文件,如果文件未给出,则读取标准输入,再将排序好的数据写至标准输出.  
 
总结:  
如果按照行排序,使用-k 6;按照字段排序,使用-k 6.2 6.4 这样的书写格式.字段以及字段里的字符是有 1 开始.如果进指定一个字段编号,则排序键值会自该字段的起始处开始,一直继续到记录的结尾(而非字段的结尾).  
使用逗号(或者空格)隔开的字段,是由逗号(或者空格)左边开始,逗号(或者空格)右边结束.例如:-k 6.2, 6.4 表示从第六个字段的第二个字符到第六个字段的第四个字符.  
当出现多个-k 选项的时候,会先从第一个键值字段开始排序,找出匹配该键值的记录后,再进行第二个键值字段的排序,以此列推.  