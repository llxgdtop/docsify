课程链接：https://missing-semester-cn.github.io/

## Lab1-The shell

[The Missing Semester - 第一讲 学习笔记 - GrapefruitCat - 博客园 (cnblogs.com)](https://www.cnblogs.com/grapefruit-cat/p/17059665.html)

semester 是一个包含特定命令的脚本文件，其内容是一个 shell 脚本，用于静默模式下获取百度网站首页的 HTTP 头信息。这里的两个命令 ./semester 和 sh semester 用于执行这个脚本，但它们之间有一些细微的区别：

./semester：

这种方式是直接执行名为 semester 的脚本文件。

要使 ./semester 成功执行，脚本文件必须具有执行权限。你可以通过运行 chmod +x semester 命令来添加执行权限。

执行时，会使用脚本第一行的 #!/bin/sh 指定的解释器来运行脚本。这一行被称为 shebang，它告诉系统使用 /bin/sh（即 Bourne shell 或兼容的 shell）来执行脚本中的命令。

sh semester：

这种方式是通过显式调用 shell（在这里是 sh）来运行脚本，不论脚本是否具有执行权限。

通过 sh 命令执行时，脚本中的 shebang（#!/bin/sh）将被忽略，因为已经明确使用 sh 作为解释器来执行脚本。

这种方法对于快速测试脚本或在没有执行权限的情况下运行脚本很有用。

总的来说，如果你确保 semester 脚本有适当的执行权限并且要保证使用脚本内指定的 shell 解释器（在你的案例中是 /bin/sh），使用 ./semester 是较为合适的方法。如果你不关心使用哪个 shell 解释器，或者脚本没有执行权限，使用 sh semester 是一个快捷的替代方式。

 

stat ./semester | grep Modify > ~/last-modified.txt

stat用于列出详细状态信息，用这个作为输出，然后用grep命令在这些信息里面搜索“Modify”,最后输出到主目录~下的lastmodified.txt文件。



## Lab2 -Sheel Tools and Scripting

[The Missing Semester - 第二讲 学习笔记 - GrapefruitCat - 博客园 (cnblogs.com)](https://www.cnblogs.com/grapefruit-cat/p/17068443.html)

tldr命令用来代替man，man工具是常用的文档查阅工具

find .用于表示在当前目录找东西比如：

find . -path ‘**/test/**.py’ -type f

在当前目录下，路径是xx然后包含test文件夹再然后包含xxx.py样式的文件

还可以结合-exec rm {} \;去删除相应的东西。现已经安装了fd命令，使用教程见[Linux 安装 fd ， 替代 find 命令-CSDN博客](https://blog.csdn.net/Chimengmeng/article/details/132683383)

 grep foo mcd.py在mcd.py中查询是否包含foo，现已经安装rg命令，可见[ripgrep: 更快捷的搜索（简介与原理） - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/401086621)

Shebang(#!)通常出现在类Unix系统的脚本中第一行，作为前两个字符。在Shebang之后，可以有一个或数个空白字符，后接解释器的绝对路径，用于指明执行这个脚本文件的解释器。在直接调用脚本时，系统的程序载入器会分析 Shebang 后的内容，将这些内容作为解释器指令，并调用该指令，将载有 Shebang 的文件路径作为该解释器的参数，执行脚本，从而使得脚本文件的调用方式与普通的可执行文件类似。例如，以指令#!/bin/sh开头的文件，在执行时会实际调用 /bin/sh 程序（通常是 Bourne shell 或兼容的 shell，例如 bash、dash 等）来执行。

history 1 | grep convert搜索历史shell输入框内包含convert的语句

用br去代替ls，见[broot - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/383031570)

使用df -h命令来查看磁盘信息

多用#!/usr/bin/env python（将python替换掉即可，而不是用#!/usr/local/bin/python）

使用 Ctrl+R 对命令历史记录进行回溯搜索。敲 Ctrl+R 后可以输入子串来进行匹配，查找历史命令行，查找过程也是敲 Ctrl+R 。按↑退出回溯搜索。

在 Vim 中，你可以通过输入命令模式（按 Esc 键进入），然后输入以下命令来关闭光标行高亮：

:set nocursorline    :set nocursorline  :syntax on

现已实现了一个功能：输入marco保存当前目录，当在别的目录时输入polo回到保存的这个目录

**cat out.log | grep Everything | wc -l**

wc -l：wc (word count) 命令用于计数，其中 -l 选项指定只计算行数。

\# 删除全部扩展名为.tmp 的文件

find . -name '*.tmp' -exec rm {} \;

\# 查找全部的 PNG 文件并将其转换为 JPG

find . -name '*.png' -exec convert {} {}.jpg \;

如果你想要创建从 **filea.html** 到 **fileh.html** 的文件，可以直接在终端使用下面的命令：touch file{a..h}.html

**xargs** 是一个非常强大的 UNIX 和 Linux 命令行工具，它读取来自标准输入的数据，并将这些数据作为参数传递给指定的命令, 之所以能用到这个命令，关键是由于很多命令不支持|管道来传递参数, xargs 可以将管道或标准输入（stdin）数据转换成命令行参数，也能够从文件的输出中读取数据。

find . -type f -printf '%A@ %p\n' | sort -n | tail -3 | cut -d' ' -f2中：

从当前目录（.）开始，查找所有类型为文件（-type f）的项目。对于每个找到的文件，它使用 -printf '%A@ %p\n' 输出文件的最后访问时间（Unix 时间戳形式）和文件的完整路径。输出的每一行都以时间戳开头，后面跟着一个空格，然后是文件路径。Sort将 **find** 的输出按数字顺序进行排序（**-n** 表示按数值排序），这是因为输出的第一列是时间戳（数值），所以按时间戳排序。cut 命令用于按列切分文本。-d' ' 设定了分隔符为单个空格（-d 选项指定分隔符）。-f2 指定输出第二字段，即每行中的第二列。在这里，第二列是 find 命令输出的文件路径。



## Lab3- Editors (Vim) 

[The Missing Semester - 第三讲 学习笔记 - GrapefruitCat - 博客园 (cnblogs.com)](https://www.cnblogs.com/grapefruit-cat/p/17071087.html)

见vim的使用



## Lab4- Data Wrangling

[The Missing Semester - 第四讲 学习笔记 - GrapefruitCat - 博客园 (cnblogs.com)](https://www.cnblogs.com/grapefruit-cat/p/17082541.html)

使用cat命令可以简单地用这些行填充整个终端屏幕。在这里使用cat命令之后，不能执行任何其他操作，比如搜索特定文本。less是“只读”的，所以您没有意外编辑正在查看的文件的风险。用less更好。

假设文件名为filename：less filename，按q退出。还有个more命令，但推荐用less

用tldr去查看less的option。使用管道结合less如：cat aa.log | less;

就可以翻页查看之类的

**Sed是一个流编辑器，用于修改流的内容（可以理解为文本替换），每行替换一次，使用正则表达式** **。（使用不了更现代的语法就加一个** **-E**）**

例子：cat ssh.log | sed ‘s/.*Disconnected from//’ | less

其中s/是替换表达式第一个/和第二个/包含住所有要替换的内容，其中.表示任意单个字符，*表示匹配零次或多次该字符（在这个例子中即.即匹配零次或多次任意单个字符），所以第一个第二个/之间的意思是零个或多个任意字符后面跟着Disconnect from的字符串，然后把它们替换为空(第二个和第三个/之间没有任何东西，所以就是空)，之后用less打开。

想要一次或多次该字符用+。在+和*后面加一个?表示非贪婪匹配，表示不会匹配尽可能多的字符，比如说有两个Disconnect from那么只会匹配第一次出现的。

“或”用[]来表示。如[0-9.]+即匹配“0至9和.”一次或多次,btw:[0-9]也可以表示为\d，[a-z]表示a到z

模式[^abc]将匹配除字母a、b或c之外的任何单个字符。注意^。看习惯用哪个。

例子：echo ‘aba’ | sed ‘s/[ab]//’会输出ba（注意不是 ba即前面有个空格），这个的意思是把a**或**b换成空（记住每行只替换一次，所以是ba而不是输出的全空，如果想全删含有a和b的，那么就得s/[ab]//g）

例子：echo ‘abcaba’ | sed -E ‘s/(ab)*//g’输出ca因为匹配0次或多次含有ab的字符并替换为空

例子：echo ‘abcababc’ | sed -E ‘s/(ab|bc)*//g’输出cc，因为三个ab都被先替换为空了就轮不到bc的替换

^表示行头，$表示行尾，它们也称为锚点。（）中的内容表示一个捕获组。是转义字符，碰上要匹配的字符是特殊字符时(比如[])，就用去表示（  如(\[aaa\])  看清楚\的位置！）

多用正则表达式测试器去检测是否写对，先从简单的正则表达式写起再慢慢写成复杂的。**注意空格。**

例子：只想保留用户名           

![image-20241020222421277](assets\image-20241020222421277.png)

![image-20241020222447585](assets\image-20241020222447585.png)

(invalid )?匹配0或1次含有invalid 的字符串（记得空格），但如此第二行还是去不掉，所以再加上一些

 ![image-20241020222452987](assets\image-20241020222452987.png)

如此只是删除了日志文件的每一行，我们想要保留用户名，所以要把用户名即user 后面的.*给框起来（.*）让用户名成为一个捕获组（这是第二个捕获组，第一个是（invalid ）），btw：head -n显示指定行数默认为10

 ![image-20241020222459804](assets\image-20241020222459804.png)

所以替换内容换成了第二个捕获组（引用第二个捕获组用\2）内的内容，至此即可输出所有的用户名。

 ![image-20241020222505325](assets\image-20241020222505325.png)

如果有用户名（如下图红色部分）是这样子的用上面的表达式就不对了，那么就得s/^.*?Disconnect后面省略让它变成非贪婪，即.*会停止在第一个Disconnect前，不加的话就会停在第二个Disconnect前然后再继续往下匹配，加了之后效果如下

 ![image-20241020222509407](assets\image-20241020222509407.png)

如果想匹配特定的用户名那就再加个管道符再用一次grep或者rg即可。（下面有更好的：见awk）

可以把head -n100换成别的比如说less等等，如果想知道有多少行wc -l即word count计算行数。

Sort是个排序工具，可以把head -n100后面加一个| sort，那么就会把所有用户名有序排列，再加一个| uniq（它也是一个工具），就会去除重复的行，想要知道有多少次就| uniq -c输出后会如

​                          ![image-20241020222527780](assets\image-20241020222527780.png)     

更进一步的：

 ![image-20241020222533355](assets\image-20241020222533355.png)

对第一列上进行数字排序（n与第一个1，-k选择输入中以空格为分隔符的列执行排序，第二个1表示在第一列停止排序，即从第一列开始并在第一列停止数字排序），其实直接写成sort -n也行，不过了解多点肯定更好。

 ![image-20241020222536776](assets\image-20241020222536776.png)

Awk是一种基于列的流处理器（和sed一样但更专注于列），awk默认将输入解析为以空格为分隔符的列，上面的意思是打印第二列。而paste是把多个行合并在一起，-s是分开他们，-d后面接着想要的间隔字符，此处是逗号。所以就会有如上的输出。

![image-20241020222542140](assets\image-20241020222542140.png)

第一列为1（说明只出现过一次，因为uniq -c会计数）并且第二列匹配如下表达式（用/括起来）^c.*e$即以c开头e结尾的用户名，最后是打印整行。

​                               ![image-20241020222602417](assets\image-20241020222602417.png)

Rows初始为0，匹配成功加1，最后打印row，加上的这个和wc -l的效果一样。

比如说我想把出现不止一次的用户的次数加起来，那么可以

 ![image-20241020222609753](assets\image-20241020222609753.png)

之后会输出2+5+1231+...再接上| bc -l即可得到总数。

留意一下lab2中的xargs。

[正则表达式 – 教程 | 菜鸟教程 (runoob.com)](https://www.runoob.com/regexp/regexp-tutorial.html)      

[RegexOne - Learn Regular Expressions - Lesson 1: An Introduction, and the ABCs](https://regexone.com/)正则表达式简单入门教程，忘记的话可以学习一下

使用花括号表示法指定每个字符的重复次数。例如，a {3}将恰好匹配a字符三次。某些正则表达式引擎甚至允许您指定重复的范围，例如，{1，3}将匹配a字符不超过3次，但不少于一次。此量词可以与任何字符或特殊元字符一起使用，例如w {3}（三个w），[wxy]{5}（五个字符，每个字符可以是w，x或y）和.{2，6}（任何字符的2到6之间）。{3，}表示至少重复3次

**ab?c** will match either the strings "abc" or "ac" because the b is considered optional.所以？就是optionality。

任意空白字符\s，tab为\t，换行\n。 if you only wanted to capture the filename without the extension, you could use the pattern **^(IMG\d+)\.png$** which only captures the part before the period.

\D表示任何非数字字符，\S表示任何非空格字符，\W表示任何非字母数字字符，使用\w捕获字母数字字母和数字，**有一个特殊的元字符\B，它匹配单词和非单词字符之间的边界。它在捕获整个单词时最有用（例如使用模式\w+\b）。**

Lab中有一个点：至少包含3个a但不以’s结尾的，共存在多少种词尾两字母组合？

```
cat /usr/share/dict/words | tr "[:upper:]" "[:lower:]" | grep -E "^([^a]*a){3}.*$" | grep -v "'s$" | sed -E "s/.*([a-z]{2})$/\1/" | sort | uniq | wc -l
```

要求哪个组合从未出现过，可以写一个shell脚本

```shell
#!/bin/bash
for i in {a..z};do
 for j in {a..z};do
    echo  "$i$j"
 done
done

之后shell输入
./all.sh > all.txt
```

然后把上面的正则表达式的输出给改一下，不用计数了，直接uniq > aaa.txt

diff --unchanged-group-format**=**'' <**(**cat aaa.txt**)** <**(**cat all.txt**)** | wc -l

--unchanged-group-format=''用于将两个文件中相同的内容设置为空字符串，剩下的内容就是差异的部分。

不能sed s/REGEX/SUBSTITUTION/ input.txt > input.txt，因为shell准备重定向输出时，会打开input.txt以备写入，这个过程中原有数据可能会被清空，然后sed才工作，所以是不行的。若想完成原地替换可以用上一讲的vim也可以稍微改进一下

sed -i 's/REGEX/SUBSTITUTION/' input.txt  -i选项是inplace，可以用tldr查看一些使用教程。

```bash
#!/bin/bash
for i in {0..9}; do
   journalctl -b-$i | grep "Startup finished in"
done
```

journalctl -b-$i：

- journalctl 是用于查询和显示从系统日志管理器（systemd的日志子系统）收集的日志信息的工具。
- -b 选项指定显示某次系统引导的日志，-b-0 代表当前引导，-b-1 是上一次引导，依此类推。
- -b-$i 利用循环变量 $i 动态指定要查询的引导会话。

想求最大最小中位数平均数等等可以看一下lab中的R语言部分

**注意** **uniq** **只能过滤相邻的行，所以必须先排序。**

可以使用sed '0,/STRING/d' 来删除STRING匹配到的字符串前面的全部内容，包括匹配到的这一行。STRING可以写成正则表达式，比如说：

sed '0,/START\s+OF\s+DATA/d' filename.txt 别忘了filename.txt即文件名（如果单单只用sed的话得加文件名，要不然就cat然后加上管道）。

 

sed -n "/table1/,/<\/table>/p" \解释

![image-20241020222904785](assets\image-20241020222904785.png)

![image-20241020222910137](assets\image-20241020222910137.png)

![image-20241020222914728](assets\image-20241020222914728.png)

接上面，一般都不写sed '0,/STRING/d'中的0，因为sed一般是从行号1开始计数的，如果想删除第A行到第B行，就用sed "A,Bd"

curl 是一个非常强大的命令行工具，用于传输数据，支持多种协议，包括 HTTP、HTTPS、FTP、FTPS 等。它常被用于在命令行或脚本中发送请求，获取或发送数据。 ![image-20241020222927926](assets\image-20241020222927926.png)                

可以用tldr去问。

如果您想要获取的是HTML数据，那么[pup](https://github.com/EricChiang/pup)可能会更有帮助。对于JSON类型的数据，可以试试[jq](https://stedolan.github.io/jq/)。

![image-20241020223215333](assets\image-20241020223215333.png)            

**记得sort是按列排序！经常结合awk使用！**

上面也提到过了，sort -k 2,2n可以写成sort -nk2,2（记得中间是没有空格）。

```shell
~$ awk '{print $1,$4,$5}' data | awk '{print $2-$3}' | awk '{s+=$1} END {print s}'
10153001
# 打印145列传给下一个，使用第二列的数据减去第三列的数据后（即原来的第四列减去第五列结果是一列传给下一个），将结果加总（即将上一个传进来的一列的内容全给加起来）
```



## Lab5- Command-line Environment

[The Missing Semester - 第五讲 学习笔记 - GrapefruitCat - 博客园 (cnblogs.com)](https://www.cnblogs.com/grapefruit-cat/p/17178699.html)

[The Missing Semester - 第五讲 学习笔记（二） - GrapefruitCat - 博客园 (cnblogs.com)](https://www.cnblogs.com/grapefruit-cat/p/17691505.html)

- <ctrl + c>：发送SIGINT信号，中断进程；
- <ctrl + z>：发送SIGTSTP信号，暂停进程（挂起）；
- <ctrl + `\`>：发送SIGQUIT信号，退出进程；（和SIGINT差不多）

中断进程的作用是可以记录下某些东西，应尽量避免发送sigkill信号，否则会有很多orphan process。

在命令后加上&就可以在后台运行程序，输入job看到正在运行的程序，job -l可以看到详细详细，pgrep`<name>`来直接获取进程pid

![image-20241020223248122](assets\image-20241020223248122.png)

如果想继续执行线程1，输入bg %1c（bg是后台，前台就是fg），想杀死线程用kill（发送的是sigterm信号），如果还是想挂起线程1，kill -STOP %1

![image-20241020223302999](assets\image-20241020223302999.png)    

Sighup不仅可以通过关闭终端来使它发送，还能通过kill -HUP %ID来使它发送，所以如果是上面的那个nohup例子，那么它如果接受到sighup信号塔就会忽略。例子：

```shell
$ jobs
# output: [1]  + suspended (signal)  sleep 1000
# output: [2]  - running    nohup sleep 2000
$ kill -HUP %1
# output: [1]  + 18653 hangup     sleep 1000
$ jobs
# output: [2]  + running    nohup sleep 2000（SIGHUP 信号给第一个作业后，该作业因为接收到 SIGHUP 而被挂起（hangup）。因为它没有被 nohup 包装，所以它响应了 SIGHUP 信号的默认行为，即终止进程。）
$ kill -SIGHUP %2
$ jobs
# output: [2]  + running    nohup sleep 2000
$ kill %2
# output: [2]  + 18745 terminated  nohup sleep 2000
$ jobs
```



### TMUX

[Linux 终端复用神器 Tmux 使用详解，看完可以回家躺平了～-51CTO.COM](https://www.51cto.com/article/664989.html?u_atoken=bc6a71f3e4e4b308a02fae425f501187&u_asession=01CFxc8WF9fSdUMElbXyq02bNYsdVsScHeW9r_aVWskSF8kISLERJMCHSd0RPEUeGodlmHJsN3PcAI060GRB4YZGyPlBJUEqctiaTooWaXr7I&u_asig=05-6mCjw1Em-jwZLoPcB0i3NO0udsMbaCVhDX1s7psrWWVLMxKxArm3pmWSIw5PG7EXmLf8amvgzD671xecuL0Xta4upc8GvNzGjSYIXV_d0ERn9x7bwHAjODqdeuzWiz1CEf1yRovEtZGTvCYcoGcAJyOjgX8Y_Zyv2dMyQU2NW1g2QMxYs6lyXb1lFWKql56qJ7Su_yP1esVJMnPUDUgi4Z9ij5IoWoUD4Ka-NM-BMbNwOTVmL0G7msauUvsKRy8iVSvSAnsluZfNjWGZoGVcy7bcUBBM-mIwPvGvnAnTIusTpJ-4hEVCCqo-GZeD3WUZHi7af-9T9DT_5BT1SiXZw&u_aref=A2yLV2ya1hxQpxI8eMQhx%2BcPKl0%3D)

**会话** **Session**

每个会话都是一个独立的工作区，其中包含一个或多个窗口。

- tmux 开始一个新的会话；
- tmux new -s     NAME 以指定名称开始一个新的会话；
- tmux ls 列出当前所有会话；

· 在 tmux 中输入 `<C-b>` d ，将当前会话分离；之后输入tmux ls会显示0：2窗口（创建于2015年8月15日星期六17：55：34）[199x44]（分离），之中的0就是tmux会话编号

- tmux a 重新连接最后一个会话。您也可以通过 -t 来指定具体的会话；如tmux attach -t 0连接到会话0
- 重命名现有对话tmux     rename-session -t 0 database
- tmux     kill-session -t `<name>`关闭指定会话 ；

**窗口** **Window**

相当于编辑器或是浏览器中的标签页，从视觉上将一个会话分割为多个部分。

- `<C-b>`     c 创建一个新的窗口，使用 `<C-d>`关闭；
- `<C-b>`     N 跳转到第 *N* 个窗口，注意每个窗口都是有编号的；
- `<C-b>`     p 切换到前一个窗口；
- `<C-b>`    n 切换到下一个窗口；
- `<C-b`>`    ,    重命名当前窗口；
- `<C-b`>`    w 列出当前所有窗口；

**面板** **Pane**

像 vim 中的分屏一样，面板使我们可以在一个屏幕里显示多个 shell。

- `<C-b>`    " 水平分割；
- `<C-b>`    % 垂直分割；
- `<C-b>`    `<方向>` 切换到指定方向的面板，`<方向>` 指的是键盘上的方向键；

· 可以在tmux里面的命令模式（按`<C-b>`: 打开），输入set -g mouse on，就可以启用鼠标点击和滚轮了；

- `<C-b>`    z 切换当前面板的缩放；
- `<C-b>`    `<空格>` 在不同的面板排布间切换（比如说几等分）；
- `<C-b>`    [ 开始往回卷动屏幕。您可以按下空格键来开始选择，回车键复制选中的部分（按q退出）；

- `<C-b>` `<C-方向键>` 按方向键调整窗格大小

- `<C-b>`    x 关闭当前的面板/窗口；C-d退出（等于exit）





alias alias_name="command_to_alias arg1 arg2"

注意， =两边是没有空格的，因为 alias 是一个 shell 命令，它只接受一个参数。比如上一节课就有很长的命令，这个时候就可以通过alias来取别名。还可以重新定义一些命令行的默认行为，就不用输入那么多了。

alias mv="mv -i"      # -i prompts before overwrite

alias mkdir="mkdir -p"   # -p make parent dirs as needed

alias df="df -h"      # -h prints human readable format

\# 或者禁用别名

unalias df

\# 获取别名的定义

alias df

\# 会打印 df="df -h"

为了让别名持续生效，您需要将配置放进 shell 的启动文件里，像是.bashrc 或 .zshrc（记得放在~下，即vim ~/.bashrc然后编辑最后:wq）。

![image-20241020223842437](assets\image-20241020223842437.png)

做一个symlink（类似于Windows中的快捷方式，也称为软链接在操作系统课程中有讲过），那么当系统需要读取或者写入.bashrc的时候，请求就会转到dotfile下的bashrc。

目的是在 /A/C/ 目录下创建一个名为 link.txt 的符号链接，该链接指向 /A/B/original.txt。

这里是命令的详细解析：

ln -s /A/B/original.txt /A/C/link.txt

- ln -s 是创建符号链接的命令。
- /A/B/original.txt 是源文件的绝对路径，即你想要创建链接指向的文件。（pathname）
- /A/C/link.txt 是链接文件的目标路径和名称，即在 /A/C/ 目录下创建一个名为 link.txt 的符号链接。（symlink）

**确保目录存在**

在执行这个命令之前，确保 /A/C/ 目录已经存在。如果目标目录不存在，命令将失败。如果你不确定目录是否存在，可以使用以下命令来创建目录：

mkdir -p /A/C

这个命令中的 -p 参数会确保整个路径的目录都被创建，如果目录已存在，它不会报错。

**验证链接**

创建链接后，你可以通过 ls -l /A/C/link.txt 命令查看链接的详情，确保它正确地指向源文件：

ls -l /A/C/link.txt

如果一切设置正确，输出应该显示 link.txt 指向 /A/B/original.txt。这样，你可以通过 /A/C/link.txt 访问 /A/B/original.txt 的内容。

**这种方式可以让你在不同的目录中方便地访问相同的文件，而无需复制文件本身，节省空间且使文件管理更加高效。**

![image-20241020223947124](assets\image-20241020223947124.png)

所以alacritty是不能在ssh中做的。

很多程序的配置都是通过纯文本格式的被称作*点文件*的配置文件来完成的（之所以称为点文件（dotfile），是因为它们的文件名以 . 开头，例如 ~/.vimrc。也正因为此，它们默认是隐藏文件，ls并不会显示它们）。

如果要在服务器上开启SSH服务，我们应该修改sshd_config配置文件，而不是ssh_config配置文件。

- ssh_config文件是SSH客户端的配置文件，用于配置SSH客户端的行为和选项。
- sshd_config文件是SSH服务器的配置文件，用于配置SSH服务器的行为和选项。

**使用** **ssh** **复制文件，即在主机与远程机之间传输文件有很多方法：**

- ssh+tee, 最简单的方法是执行 ssh 命令，然后通过这样的方法利用标准输入实现 cat localfile | ssh     remote_server tee serverfile。回忆一下，[tee](https://www.man7.org/linux/man-pages/man1/tee.1.html) 命令会将标准输出写入到一个文件；
- [scp](https://www.man7.org/linux/man-pages/man1/scp.1.html) ：当需要拷贝大量的文件或目录时，使用scp 命令则更加方便，因为它可以方便的遍历相关路径。语法如下：scp     path/to/local_file remote_host:path/to/remote_file；
- [rsync](https://www.man7.org/linux/man-pages/man1/rsync.1.html) 对 scp 进行了改进，它可以检测本地和远端的文件以防止重复拷贝。它还可以提供一些诸如符号连接、权限管理等精心打磨的功能。甚至还可以基于 --partial标记实现**断点续传**（课上展示了-avP参数）。rsync 的语法和scp类似。

 

可以使用pgrep -f xxx来列出**包含关键字xxx**的进程（比如说有sleep 10和sleep 20，而xxx是sleep那么这两个都会被列出来，如果不加-f，那么就是得要完全匹配xxx才行）的pid，然后使用pkill -f xxx去结束进程。pgrep 是一个在 Unix 和类 Unix 操作系统中用来搜索符合特定条件的进程并返回它们的进程 ID (PID) 的命令。

$!想成解引用最近使用的进程那就是求这个进程的pid，$想成解引用就行。

wait $(pgrep sleep); ls

$是解引用，解开pgrep sleep返回的pid，等待这个进程完成后，执行ls，如此即可实现某个进程结束后再开始另外一个进程。

![image-20241020224022434](assets\image-20241020224022434.png)

使用 **kill -0 $1** 来检查PID为 $1 的进程是否存在。如果存在，kill -0 将返回0（成功），否则返回非零（失败）。

![image-20241020224051634](assets\image-20241020224051634.png)

一个是未提交一个已提交(git add)

![image-20241020224113975](assets\image-20241020224113975.png)



## Lab6- Version Control (Git)

见git使用教程



## Lab7- Debugging and Profiling

日志较普通的打印语句有如下的一些优势：

- 您可以将日志写入文件、socket     或者甚至是发送到远端服务器而不仅仅是标准输出；
- 日志可以支持严重等级（例如 INFO,     DEBUG, WARN, ERROR等)，这使您可以根据需要过滤日志；
- 对于新发现的问题，很可能您的日志中已经包含了可以帮助您定位问题的足够的信息。

当通过打印已经不能满足您的调试需求时，您应该使用调试器。

调试器是一种可以允许我们和正在执行的程序进行交互的程序，它可以做到：

- 当到达某一行时将程序暂停；
- 一次一条指令地逐步执行程序；
- 程序崩溃后查看变量的值；
- 满足特定条件时暂停程序；
- 其他高级功能。

Gdb的使用见[pwndbg的安装和gdb使用-CSDN博客](https://blog.csdn.net/zino00/article/details/122716412)

当程序需要执行一些只有操作系统内核才能完成的操作时，它需要使用 [系统调用](https://en.wikipedia.org/wiki/System_call)。有一些命令可以**追踪您的程序执行的系统调用**。在 Linux 中可以使用[strace](http://man7.org/linux/man-pages/man1/strace.1.html) 。如sudo strace -e lstat ls -l > /dev/null；

这条命令的整体作用是：

- 使用strace来监视ls -l命令执行过程中涉及的所有lstat系统调用。
- 由于输出被重定向到/dev/null，你不会看到ls -l命令的实际输出内容，但strace会在终端中打印所有捕获到的lstat系统调用及其结果。

![image-20241020224226792](assets\image-20241020224226792.png)

有些情况下，我们需要查看网络数据包才能定位问题。像 [tcpdump](http://man7.org/linux/man-pages/man1/tcpdump.1.html) 和 [Wireshark](https://www.wireshark.org/) 这样的网络数据包分析工具可以帮助您获取网络数据包的内容并基于不同的条件进行过滤。

 

大多数的编辑器和 IDE 都支持在编辑界面显示这些工具的分析结果、高亮有警告和错误的位置。 这个过程通常称为 **code linting** 。风格检查或安全检查的结果同样也可以进行相应的显示。

在 vim 中，有 [ale](https://vimawesome.com/plugin/ale) 或 [syntastic](https://vimawesome.com/plugin/syntastic) 可以做同样的事情。 在 Python 中， [pylint](https://www.pylint.org/) 和 [pep8](https://pypi.org/project/pep8/) 是两种用于进行风格检查的工具，而 [bandit](https://pypi.org/project/bandit/) 工具则用于检查安全相关的问题。

对于风格检查和代码格式化，还有以下一些工具可以作为补充：用于 Python 的 [black](https://github.com/psf/black)、用于 Go 语言的 gofmt、用于 Rust 的 rustfmt 或是用于 JavaScript, HTML 和 CSS 的 [prettier](https://prettier.io/) 。这些工具可以自动格式化代码，这样代码风格就可以与常见的风格保持一致。

 

执行时间（wall clock time）不一定是该程序实际在CPU上运行的时间，（分时系统，时间片到期后需等待调度）

对于工具来说，需要区分真实时间、用户时间和系统时间。通常来说，用户时间+系统时间代表了您的进程所消耗的实际 CPU （更详细的解释可以参照[这篇文章](https://stackoverflow.com/questions/556405/what-do-real-user-and-sys-mean-in-the-output-of-time1)）。

- 真实时间 - 从程序开始到结束流失掉的真实时间，包括其他进程的执行时间以及阻塞消耗的时间（例如等待 I/O或网络）；
- User - CPU 执行用户代码所花费的时间；
- Sys - CPU 执行系统内核代码所花费的时间。

```shell
•	$ time curl https://missing.csail.mit.edu &> /dev/null`
•	real    0m2.561s
•	user    0m0.015s
•	sys     0m0.012s
```

大多数情况下，当人们提及性能分析工具的时候，通常指的是 CPU 性能分析工具。

CPU 性能分析工具有两种： 追踪分析器（*tracing*）及采样分析器（*sampling*）。

追踪分析器会记录程序的每一次函数调用，而采样分析器则只会周期性的监测（通常为每毫秒）您的程序并记录程序堆栈。

在 Python 中，使用 cProfile 模块来分析每次函数调用所消耗的时间。

在C中，可以使用gprof、Valgrind + Callgrind、perf、gperftools、Intel VTune Profiler。

更加符合直觉的显示分析信息的方式是包括每行代码的执行时间，这也是行分析器的工作。C中也有类似的工具，具体可以上网搜。

为了应对内存类的 Bug，我们可以使用类似 [Valgrind](https://valgrind.org/) 这样的工具来检查内存泄漏问题。

 

在使用strace调试代码时，可能会希望忽略一些特殊的代码并希望在分析时将其当作黑盒处理。[perf](http://man7.org/linux/man-pages/man1/perf.1.html) 命令将 CPU 的区别进行了抽象，它不会报告时间和内存的消耗，而是报告与您的程序相关的系统事件。

例如，perf 可以报告不佳的缓存局部性（poor cache locality）、大量的页错误（page faults）或活锁（livelocks）。下面是关于常见命令的简介：

- perf     list - 列出可以被 pref 追踪的事件；
- perf stat     COMMAND ARG1 ARG2 - 收集与某个进程或指令相关的事件；
- perf record     COMMAND ARG1 ARG2 - 记录命令执行的采样信息并将统计数据储存在perf.data中；
- perf     report - 格式化并打印 perf.data 中的数据。

 

**调用图和控制流图**可以显示子程序之间的关系，它将函数作为节点并把函数调用作为边。将它们和分析器的信息（例如调用次数、耗时等）放在一起使用时，调用图会变得非常有用，它可以帮助我们分析程序的流程。 在 Python 中可以使用 [pycallgraph](http://pycallgraph.slowchop.com/en/master/) 来生成这些图片。C语言也有。

 

有时候，分析程序性能的第一步是搞清楚它所消耗的资源。程序变慢通常是因为它所需要的资源不够了。例如，没有足够的内存或者网络连接变慢的时候。

有很多很多的工具可以被用来显示不同的系统资源，例如 CPU 占用、内存使用、网络、磁盘使用等。

- **通用监控** - 最流行的工具要数 [htop](https://htop.dev/)了，它是 [top](http://man7.org/linux/man-pages/man1/top.1.html)的改进版。htop 可以显示当前运行进程的多种统计信息。如果需要合并测量全部的进程， [dstat](http://dag.wiee.rs/home-made/dstat/) 是也是一个非常好用的工具，它可以实时地计算不同子系统资源的度量数据，例如 I/O、网络、 CPU 利用率、上下文切换等等；
- **I/O** **操作** - [iotop](http://man7.org/linux/man-pages/man8/iotop.8.html) 可以显示实时 I/O 占用信息而且可以非常方便地检查某个进程是否正在执行大量的磁盘读写操作；
- **磁盘使用** - [df](http://man7.org/linux/man-pages/man1/df.1.html) 可以显示每个分区的信息，而 [du](http://man7.org/linux/man-pages/man1/du.1.html) 则可以显示当前目录下每个文件的磁盘使用情况（ **d**isk **u**sage）。-h 选项可以使命令以对人类（**h**uman）更加友好的格式显示数据；[ncdu](https://dev.yorhel.nl/ncdu)是一个交互性更好的 du ，它可以让您在不同目录下导航、删除文件和文件夹；
- **内存使用** - [free](http://man7.org/linux/man-pages/man1/free.1.html) 可以显示系统当前空闲的内存。内存也可以使用 htop这样的工具来显示；
- **打开文件** - [lsof](http://man7.org/linux/man-pages/man8/lsof.8.html) 可以列出被进程打开的文件信息。 当我们需要查看某个文件是被哪个进程打开的时候，这个命令非常有用；
- **网络连接和配置** - [ss](http://man7.org/linux/man-pages/man8/ss.8.html) 能帮助我们监控网络包的收发情况以及网络接口的显示信息。ss 常见的一个使用场景是找到端口被进程占用的信息。如果要显示路由、网络设备和接口信息，您可以使用 [ip](http://man7.org/linux/man-pages/man8/ip.8.html) 命令。注意，netstat 和 ifconfig 这两个命令已经被前面那些工具所代替了。
- **网络使用** - [nethogs](https://github.com/raboof/nethogs) 和 [iftop](http://www.ex-parrot.com/pdw/iftop/) 是非常好的用于对网络占用进行监控的交互式命令行工具。

如果希望测试一下这些工具，可以使用 [stress](https://linux.die.net/man/1/stress) 命令来为系统人为地增加负载。（还需安装）

![image-20241020224317033](assets\image-20241020224317033.png)

Linux 上的 journalctl 命令来获取系统日志，如

```shell
journalctl | grep sudo
```

查看最近一天中超级用户的登录信息及其所执行的指令。

在 ~/.vimrc 文件中添加 vim-plug 的初始化代码块和 ALE 插件（这是一个linter插件，这样它就可以自动地显示相关警告信息。）：

```bash
call plug#begin('~/.vim/plugged')
Plug 'dense-analysis/ale'
call plug#end()

" Optional: Enable ALE linting and specify linters
let g:ale_linters = {
\   'python': ['flake8', 'pylint'],
\   'javascript': ['eslint'],
\}

" Optional: Enable ALE fixers and specify fixers
let g:ale_fixers = {
\   '*': ['remove_trailing_lines', 'trim_whitespace'],
\   'python': ['autopep8', 'yapf'],
\   'javascript': ['prettier'],
\}

" Optional: Automatically fix files on save
let g:ale_fix_on_save = 1
```

1. 保存并关闭 ~/.vimrc 文件。
2. 重新打开 Vim，运行以下命令以安装插件：
3. :PlugInstall



**使用** **ALE**

安装完成后，ALE 会自动在你编辑文件时进行语法检查，并在检测到问题时在侧边显示错误标记。如果需要手动触发检查，可以使用以下命令：

:ALELint

**配置示例**

如果你需要为不同的编程语言配置特定的 linter，可以在 ~/.vimrc 中按需添加。例如：

let g:ale_linters = {

\  'python': ['flake8', 'pylint'],

\  'javascript': ['eslint'],

\  'cpp': ['clang', 'gcc'],

\  'go': ['golint'],

\}

你也可以指定自动修复工具（fixers），例如：

let g:ale_fixers = {

\  '*': ['remove_trailing_lines', 'trim_whitespace'],

\  'python': ['autopep8', 'yapf'],

\  'javascript': ['prettier'],

\}

 

rr是一个**反向调试**的工具[rr：轻量级记录和确定性调试 --- rr: lightweight recording & deterministic debugging (rr-project.org)](https://rr-project.org/)

perf内置在linux-tools中，使用rr需要先安装perf，一个小例子可以见第四题[Solution-调试与性能分析 · the missing semester of your cs education (missing-semester-cn.github.io)](https://missing-semester-cn.github.io/missing-notes-and-solutions/2020/solutions/debugging-profiling-solution/)

网站还有对python cProfile 模块的使用例子。

![image-20241020224516747](assets\image-20241020224516747.png)

内存设备是否已经挂载是指系统中的某个内存设备（例如，RAM 盘）是否已经通过挂载操作使其可以被系统访问和使用。挂载是将一个设备或文件系统关联到操作系统的目录结构中的过程，使用户可以通过一个特定的目录来访问该设备的内容。

![image-20241020224527275](assets\image-20241020224527275.png)

![image-20241020224531003](assets\image-20241020224531003.png)

![image-20241020224540767](assets\image-20241020224540767.png)

还有很多内容（暂时用不上）可见课程网址。



## Lab8- Metaprogramming

如果您使用 LaTeX 来编写论文，您需要执行哪些命令才能编译出您想要的论文呢？对于大多数系统来说，不论其是否包含代码，都会包含一个“构建过程”。有时，您需要执行一系列操作。通常，这一过程包含了很多步骤，很多分支。执行一些命令来生成图表，然后执行另外的一些命令生成结果，然后再执行其他的命令来生成最终的论文。

有很多工具可以帮助我们完成这些操作。构建系统：帮助完成构建过程操作的工具。需要定义**依赖、目标 和规则**。我们必须告诉构建系统我们具体的构建目标，系统的任务则是找到构建这些目标所需要的依赖，并根据规则构建所需的中间产物，直到最终目标被构建出来。理想的情况下，如果目标的依赖没有发生改动，并且我们可以从之前的构建中复用这些依赖，那么与其相关的构建规则并不会被执行。

make 是最常用的构建系统之一，您会发现它通常被安装到了几乎所有基于UNIX的系统中。make并不完美，但是对于中小型项目来说，它已经足够好了。当您执行 make 时，它会去参考当前目录下名为 Makefile 的文件。所有构建目标、相关依赖和规则都需要在该文件中定义，它看上去是这样的：

```makefile
paper.pdf: paper.tex plot-data.png
	pdflatex paper.tex

plot-%.png: %.dat plot.py
	./plot.py -i $*.dat -o $@
```

冒号左侧的是构建目标，冒号右侧的是构建它所需的依赖。缩进的部分是从依赖构建目标时需要用到的一段程序。

在 make 中，第一条指令还指明了构建的目的，如果您使用不带参数的 make，这便是我们最终的构建结果。或者，您可以使用这样的命令来构建其他目标：make plot-data.png。

规则中的 % 是一种模式，它会匹配其左右两侧相同的字符串。例如，如果目标是 plot-foo.png， make 会去寻找 foo.dat 和 plot.py 作为依赖。此时如果make了，之后对foo.dat修改再make一次，那么不会对plot.py重新构建，因为没有必要。

在Makefile中，.PHONY 是一个特殊的标志，用来声明“伪目标”（phony targets）。伪目标是一种不会生成实际文件的目标，而是用来执行一些特定操作的，比如清理文件、编译代码等。

```makefile
.PHONY: clean
clean:
	rm *.pdf *.aux *.log *.png
	git ls-files -o | xargs rm -f
```

.PHONY: clean 声明 clean 作为一个伪目标，而 clean 目标中的命令会在每次运行 make clean 时执行。这个命令删除当前目录下所有扩展名为 .pdf、.aux、.log 和 .png 的文件。删除所有未被Git追踪的文件。这在清理工作目录时非常有用，特别是你在编译或生成过程中产生了许多临时文件，而这些文件未被添加到Git版本控制中。

 

大多数被其他项目所依赖的项目都会在每次发布新版本时创建一个*版本号*。通常看上去像 8.1.3 或 64.1.20192004。版本号有很多用途，其中最重要的作用是保证软件能够运行。它的格式是这样的：主版本号.次版本号.补丁号。相关规则有：

- 如果新的版本没有改变 API，请将补丁号递增；
- 如果添加了 API 并且该改动是向后兼容的，请将次版本号递增；
- 如果修改了 API 但是它并不向后兼容，请将主版本号递增。

[货物清单- The Cargo Book --- Specifying Dependencies - The Cargo Book (rust-lang.org)](https://doc.rust-lang.org/cargo/reference/specifying-dependencies.html)

 

持续集成(**Continuous integration systems**)，或者叫做 CI 是一种雨伞术语（umbrella term，涵盖了一组术语的术语），它指的是那些“当您的代码变动时，自动运行的东西”，市场上有很多提供各式各样 CI 工具的公司，这些工具大部分都是免费或开源的。比较大的有 Travis CI、Azure Pipelines 和 GitHub Actions。

工作原理：您需要在代码仓库中添加一个文件，描述当前仓库发生任何修改时，应该如何应对。目前为止，最常见的规则是：如果有人提交代码，执行测试套件。当这个事件被触发时，CI 提供方会启动一个（或多个）虚拟机，执行您制定的规则，并且通常会记录下相关的执行结果。您可以进行某些设置，这样当测试套件失败时您能够收到通知或者当测试全部通过时，您的仓库主页会显示一个徽标。

**Test**

- 测试套件：所有测试的统称。
- 单元测试：一种“微型测试”，用于对某个封装的特性进行测试。
- 集成测试：一种“宏观测试”，针对系统的某一大部分进行，测试其不同的特性或组件是否能*协同*工作。
- 回归测试：一种实现特定模式的测试，用于保证之前引起问题的 bug 不会再次出现。
- 模拟（Mocking）: 使用一个假的实现来替换函数、模块或类型，屏蔽那些和测试不相关的内容。例如，“模拟网络连接” 或 “模拟硬盘”。

 

**构建属于您的** **GitHub action****，对仓库中所有的****.md****文件执行** [**proselint**](http://proselint.com/) **或** [**write-good**](https://github.com/btford/write-good)**，在您的仓库中开启这一功能，提交一个包含错误的文件看看该功能是否生效。**

在 Github marketplace 中，可以找到，[Lint Markdown](https://github.com/marketplace/actions/lint-markdown)



## Lab9- Security and Cryptography

[熵](https://en.wikipedia.org/wiki/Entropy_(information_theory))(Entropy) 度量了不确定性并可以用来决定密码的强度。

熵的单位是 *bits*(*比特*)。对于一个均匀分布的随机离散变量，熵等于 log_2(# of possibilities)。扔一次硬币的熵是1 bits，即log_2(2)。掷一次（六面）骰子的熵大约为2.58 bits，即log_2(6)。

使用多少比特的熵取决于应用的威胁模型。

大约40比特的熵足以对抗在线穷举攻击（受限于网络速度和应用认证机制）。

而对于离线穷举攻击（主要受限于计算速度）, 一般需要更强的密码 (比如80比特或更多)。

[Passwords (rumkin.com)](https://rumkin.com/tools/password/)可以使用这个网站来测试密码的强度

 

[密码散列函数](https://en.wikipedia.org/wiki/Cryptographic_hash_function) (Cryptographic hash function) 可以将**任意大小的数据**映射为一个**固定大小的输出**。

hash(value: array<byte>) -> vector<byte, N> (N对于该函数固定)

[SHA-1](https://en.wikipedia.org/wiki/SHA-1)是Git中使用的一种散列函数， 它可以将任意大小的输入映射为一个160比特（可被40位十六进制数表示）的输出。

```bash
$ printf 'hello' | sha1sum
aaf4c61ddcc5e8a2dabede0f3b482cd9aea9434d
$ printf 'hello' | sha1sum
aaf4c61ddcc5e8a2dabede0f3b482cd9aea9434d
$ printf 'Hello' | sha1sum 
f7ff9e8b7bb2e09b70935a5d785e0cc5d9d0abf0
```

抽象地讲，散列函数可以被认为是一个**不可逆，且看上去随机（但具确定性）**的函数(比如前两个就是相同的值)。 一个散列函数拥有以下特性：

- **确定性**：对于不变的输入永远有相同的输出。
- **不可逆性**：对于hash(m) = h，难以通过已知的输出h来计算出原始输入m。
- 目标碰撞抵抗性/弱无碰撞：对于一个给定输入m_1，难以找到m_2 != m_1且hash(m_1)     = hash(m_2)。
- 碰撞抵抗性/强无碰撞：难以找到一组满足hash(m_1)     = hash(m_2)的输入m_1, m_2（该性质严格强于目标碰撞抵抗性，即Hash函数基本没有碰撞）。

 

**密码散列函数的应用**

- Git中的内容寻址存储(Content addressed storage)：[散列函数](https://en.wikipedia.org/wiki/Hash_function)是一个宽泛的概念（存在非密码学的散列函数），那么Git为什么要特意使用密码散列函数？
  - 普通的散列函数没有无碰撞性，Git 使用密码散列函数，来确保分布式版本控制系统中的两个不同数据不会有相同的摘要信息（例如两个内容不同的 commit 不应该有相同的哈希值）。
- 文件的信息摘要(Message digest)：例如下载文件时，对比下载下来的文件的哈希值和官方公布的哈希值是否相同来判断文件是否损坏或者被篡改。
- [承诺机制](https://en.wikipedia.org/wiki/Commitment_scheme)(Commitment     scheme)： 假设我希望承诺一个值，但之后再透露它—— 比如在没有一个可信的、双方可见的硬币的情况下在我的脑海中公平的“扔一次硬币”。
  - 假定偶数r代表正面，奇数r代表反面。
  - 我可以选择一个值r = random()，并和你分享它的哈希值h = sha256(r)。
  - 这时你可以开始猜硬币的正反。
  - 我告诉你值r的内容，得出胜负。同时你可以使用sha256(r)来检查我分享的哈希值h以确认我没有作弊。

在linux中是shasum -a 256命令

![image-20241020224823512](assets\image-20241020224823512.png)

![image-20241020224827922](assets\image-20241020224827922.png)

SHA256SUMS中有多个版本的哈希值，所以要grep一下，第二个是求出下载的文件的哈希值，之后两个即可做对比

![image-20241020224837575](assets\image-20241020224837575.png)

**密钥生成函数**

[密钥生成函数](https://en.wikipedia.org/wiki/Key_derivation_function) (Key Derivation Functions，KDF) 与密码散列函数类似，用以产生一个固定长度的密钥。但是为了对抗穷举法攻击，密钥生成函数通常较慢。

**密钥生成函数的应用**

- 将其结果作为其他加密算法的密钥，例如对称加密算法
- 数据库中保存的用户密码为密文
  - 针对每个用户随机生成一个[盐](https://link.segmentfault.com/?url=https%3A%2F%2Fen.wikipedia.org%2Fwiki%2FSalt_(cryptography))，并存储盐，以及密钥生成函数对连接了盐的明文密码生成的哈希值 KDF(password + salt)。
  - 在验证登录请求时，使用输入的密码连接存储的盐重新计算哈希值KDF(input + salt)，并与存储的哈希值对比。
  - **盐**（Salt），在[密码学](https://link.segmentfault.com/?url=https%3A%2F%2Fzh.wikipedia.org%2Fwiki%2F密码学)中，是指在[散列](https://link.segmentfault.com/?url=https%3A%2F%2Fzh.wikipedia.org%2Fwiki%2F散列)之前将散列内容（例如：密码）的任意固定位置插入特定的字符串。这个在散列中加入字符串的方式称为“加盐”。
  - 在大部分情况，盐是不需要保密的。
  - 通常情况下，当字段经过散列处理，会生成一段散列值，而散列后的值一般是无法通过特定算法得到原始字段的。但是某些情况，比如一个大型的[彩虹表](https://link.segmentfault.com/?url=https%3A%2F%2Fzh.wikipedia.org%2Fwiki%2F彩虹表)，通过在表中搜索该SHA-1值，很有可能在极短的时间内找到该散列值对应的真实字段内容。
  - 加盐可以避免用户的短密码被彩虹表破解，也可以保护在不同网站使用相同密码的用户。

 

**对称加密**

对称加密使用以下几个方法来实现这个功能：

```bash
keygen() -> key (这是一个随机方法)

encrypt(plaintext: array<byte>, key) -> array<byte> (输出密文)

decrypt(ciphertext: array<byte>, key) -> array<byte> (输出明文)
```

加密方法encrypt()输出的密文ciphertext很难在不知道key的情况下得出明文plaintext。
 解密方法decrypt()有明显的正确性。对于给定密文及其密钥，解密方法必须输出明文：decrypt(encrypt(m, k), k) = m。

**对称加密的应用**

- 加密不信任的云服务上存储的文件。对称加密和密钥生成函数配合起来，就可以使用密码加密文件： 将密码输入密钥生成函数生成密钥 key     = KDF(passphrase)，然后存储encrypt(file, key)。

[AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard) 是现在常用的一种对称加密系统。在 Linux 下可以使用 openssl 工具：

```bash
# 加密
openssl aes-256-cbc -salt -in {源文件名} -out {加密文件名}   此时会让输入一个密码
# 解密
openssl aes-256-cbc -d -in {加密文件名} -out {解密文件名}
加密后即可把加密文件发给别人，输入这个密码即可解密
```

不过这普通的KDF已经过时了。使用 -iter 或 -pbkdf2 选项可以提高安全性。-pbkdf2 采用密码基础密钥派生函数2（PBKDF2），这是一种更安全的密钥派生方法。

![image-20241020224927856](assets\image-20241020224927856.png)

![image-20241020224932085](assets\image-20241020224932085.png)

**非对称加密**

非对称加密的“非对称”代表在其环境中，使用两个具有不同功能的密钥： 一个是**私钥(private key)，不向外公布；另一个是公钥(public key)**，公布公钥不像公布对称加密的共享密钥那样可能影响加密体系的安全性。

```bash
keygen() -> (public key, private key)  (这是一个随机方法)
encrypt(plaintext: array<byte>, public key) -> array<byte>  (输出密文)
decrypt(ciphertext: array<byte>, private key) -> array<byte>  (输出明文)
sign(message: array<byte>, private key) -> array<byte>  (生成签名)
verify(message: array<byte>, signature: array<byte>, public key) -> bool  (验证签名是否是由和这个公钥相关的私钥生成的)
```

非对称的加密/解密方法和对称的加密/解密方法有类似的特征（**公钥加密，私钥解密**）：
 信息在非对称加密中使用 *公钥* 加密， 且输出的密文很难在不知道 *私钥* 的情况下得出明文。
 解密方法decrypt()有明显的正确性。 给定密文及私钥，解密方法一定会输出明文： decrypt(encrypt(m, public key), private key) = m。

签名/验证(sign/verify)：

在不知道 *私钥* 的情况下，不管需要签名的信息为何，很难计算出一个可以使 verify(message, signature, public key) 返回为真的签名。

**非对称加密的应用**

- PGP电子邮件加密](https://en.wikipedia.org/wiki/Pretty_Good_Privacy)：用户可以将所使用的公钥在线发布，比如：PGP密钥服务器或 [Keybase](https://keybase.io/)。任何人都可以向他们发送加密的电子邮件。
- 聊天加密：像 [Signal](https://signal.org/) 和 [Keybase](https://keybase.io/) 使用非对称密钥来建立私密聊天。
- 软件签名：Git 支持用户对提交(commit)和标签(tag)进行GPG签名。任何人都可以使用软件开发者公布的签名公钥验证下载的已签名软件。



不使用下面的GPG而是使用自带的OpenSSL生成密钥对的方法：

1.生成私钥

```bash
openssl genpkey -algorithm RSA -out private_key.pem
```

- genpkey：生成私钥。
- -algorithm RSA：指定使用 RSA 算法生成密钥。
- -out private_key.pem：指定输出文件，将生成的私钥保存到 private_key.pem 文件中。

2.从私钥生成公钥

```bash
openssl rsa -pubout -in private_key.pem -out public_key.pem
```

- -pubout：输出公钥。
- **-in private_key.pem**：输入文件，指定私钥文件 private_key.pem。

- out public_key.pem：指定输出文件，将生成的公钥保存到 public_key.pem 文件中

3.加密

```bash
openssl rsautl -encrypt -inkey public_key.pem -pubin -in file.txt -out file.enc
```

- rsautl：使用 RSA 的 utl 工具（util 是 utility 的缩写），用于加密和解密。
- **-encrypt**：指定加密操作。
- **-inkey public_key.pem**：输入文件，指定用于加密的公钥文件 public_key.pem。
- **-pubin**：表明输入的是公钥。
- **-in file.txt**：指定要加密的输入文件 file.txt。
- **-out file.enc**：指定输出文件，将加密后的内容保存到 file.enc 文件中。

4.解密

```bash
openssl rsautl -decrypt -inkey private_key.pem -in file.enc -out file.dec
```

- -decrypt：指定解密操作。
- **-inkey private_key.pem**：输入文件，指定用于解密的私钥文件 private_key.pem。
- **-in file.enc**：指定要解密的输入文件 file.enc。
- **-out file.dec**：指定输出文件，将解密后的内容保存到 file.dec 文件中。

OpenSSL 更适合快速和灵活的加密任务，尤其是在现有系统上无需额外安装的情况下。

GPG 则是一个更完整的加密解决方案，适合需要密钥管理和非对称加密的场景，但需要额外安装和配置。

一个具体的应用可以见solution的第四点[Solution-安全与密码学 · the missing semester of your cs education (missing-semester-cn.github.io)](https://missing-semester-cn.github.io/missing-notes-and-solutions/2020/solutions/security-solution/)

对其中一些的解释见下面图片：

![image-20241020225213173](assets\image-20241020225213173.png)

![image-20241020225218064](assets\image-20241020225218064.png)

GPG（GnuPG 或 Gnu Privacy Guard） 是一种用于加密和签名数据及通信的免费开源软件。它是 PGP（Pretty Good Privacy）标准的一种实现，广泛用于保护电子邮件、文件和其他数据的隐私和完整性。

 

 

GPG 的主要功能

1. 加密数据：
   - 使用对称或非对称加密算法加密数据，确保只有拥有正确解密密钥的人才能访问内容。
2. 数字签名：
   - 使用私钥对数据进行签名，接收者可以用相应的公钥验证数据的来源和完整性，确保数据未被篡改。
3. 密钥管理：
   - 生成、管理和分发加密和签名所需的密钥对（包括公钥和私钥）。



**其中的一些示例命令**

**生成密钥对**

**gpg --full-generate-key（运行后系统会提示输入一系列信息，其中含有电子邮件地址，那么就可以为不同的邮箱去设置一个专门的GPG密钥对）**

**加密文件**

**gpg --output encrypted_file.gpg --encrypt –recipient recipient@example.com file.txt**

**解密文件**

**gpg --output decrypted_file.txt --decrypt encrypted_file.gpg**

**签名文件**

**gpg --output signed_file.gpg --sign file.txt**

**验证签名**

**gpg --verify signed_file.gpg**

![image-20241020225358451](assets\image-20241020225358451.png)

![image-20241020225403768](assets\image-20241020225403768.png)

![image-20241020225413645](assets\image-20241020225413645.png)

![image-20241020225421747](assets\image-20241020225421747.png)

![image-20241020225429454](assets\image-20241020225429454.png)

那anish收到我发的邮件后（里面包含了这个message.txt.asc文件)，他该使用什么命令去解开这个文件呢？

![image-20241020225447332](assets\image-20241020225447332.png)

![image-20241020225452069](assets\image-20241020225452069.png)

那我怎么知道我的公钥并且分享给别人呢？

![image-20241020225504951](assets\image-20241020225504951.png)

![image-20241020225511204](assets\image-20241020225511204.png)

![image-20241020225517926](assets\image-20241020225517926.png)

**密钥分发**

非对称加密面对的主要挑战是，如何分发公钥并对应现实世界中存在的人或组织。

- Signal的信任模型：信任用户第一次使用时给出的身份(trust on first use)，支持线下(out-of-band)面对面交换公钥（Signal里的safety number）。
- PGP使用的是[信任网络](https://link.segmentfault.com/?url=https%3A%2F%2Fen.wikipedia.org%2Fwiki%2FWeb_of_trust)。
- Keybase主要使用[社交网络证明 (social proof)](https://link.segmentfault.com/?url=https%3A%2F%2Fkeybase.io%2Fblog%2Fchat-apps-softer-than-tofu)。(倾向于用这个)

 

**密码管理器**

每个人都应该尝试使用密码管理器，比如[KeePassXC](https://keepassxc.org/)、[pass](https://www.passwordstore.org/) 和 [1Password](https://1password.com/))。

密码管理器会帮助你对每个网站生成随机且复杂（表现为高熵）的密码，并使用你指定的主密码配合密钥生成函数来对称加密它们。

你只需要记住一个复杂的主密码，密码管理器就可以生成很多复杂度高且不会重复使用的密码。密码管理器通过这种方式降低密码被猜出的可能，并减少网站信息泄露后对其他网站密码的威胁。

**两步验证（双因子验证）**

[两步验证](https://en.wikipedia.org/wiki/Multi-factor_authentication)(2FA)要求用户同时使用密码（“你知道的信息”）和一个身份验证器（“你拥有的物品”，比如[YubiKey](https://www.yubico.com/)）来消除密码泄露或者[钓鱼攻击](https://en.wikipedia.org/wiki/Phishing)的威胁。

**全盘加密**

对笔记本电脑的硬盘进行全盘加密是防止因设备丢失而信息泄露的简单且有效方法。 Linux的[cryptsetup + LUKS](https://wiki.archlinux.org/index.php/Dm-crypt/Encrypting_a_non-root_file_system)， Windows的[BitLocker](https://fossbytes.com/enable-full-disk-encryption-windows-10/)，或者macOS的[FileVault](https://support.apple.com/en-us/HT204837)都使用一个由密码保护的对称密钥来加密盘上的所有信息。

 

在git commit时可以加上一个-S(前提：已配置了GPG密钥)，GPG 签名用于验证提交者的身份，从而确保提交的真实性和完整性。这是一个提高代码提交安全性和可信度的功能。具体教程可以问GPT或者见solution中的末尾。



## Lab10- Potpourri

**修改键位映射**

修改键位映射通常由在计算机上运行的软件实现。当某一个按键被按下，软件截获键盘发出的按键事件（keypress event）并使用另外一个事件取代。比如：

- **将** **Caps Lock** **映射为** **Ctrl** **或者** **Escape**：Caps     Lock 使用了键盘上一个非常方便的位置而它的功能却很少被用到；

下面是一些修改键位映射的软件：

- macOS - [karabiner-elements](https://pqrs.org/osx/karabiner/), [skhd](https://github.com/koekeishiya/skhd) 或者 [BetterTouchTool](https://folivora.ai/)
- Linux - [xmodmap](https://wiki.archlinux.org/index.php/Xmodmap) 或者 [Autokey](https://github.com/autokey/autokey)
- Windows - 控制面板，[AutoHotkey](https://www.autohotkey.com/) 或者 [SharpKeys](https://www.randyrants.com/category/sharpkeys/)
- QMK - 如果你的键盘支持定制固件，[QMK](https://docs.qmk.fm/) 可以直接在键盘的硬件上修改键位映射。保留在键盘里的映射免除了在别的机器上的重复配置。

 

**守护进程**

守护进程是一种在后台保持运行，不需要用户手动运行或者交互的进程。

以守护进程运行的程序名一般以 d 结尾，比如 SSH 服务端 sshd，用来监听传入的 SSH 连接请求并对用户进行鉴权，MySQL服务端 mysqld。

Linux 中的 systemd（the system daemon）是最常用的配置和运行守护进程的方法。systemd 配置文件的详细指南可参见 [freedesktop.org](https://www.freedesktop.org/software/systemd/man/systemd.service.html)。

- 运行 systemctl     status 命令可以看到正在运行的所有守护进程。
- 用户使用 systemctl 命令和 systemd 交互来enable（启用）、disable（禁用）、start（启动）、stop（停止）、restart（重启）、或者status（检查）配置好的守护进程及系统服务。

举例

![image-20241020225549378](assets\image-20241020225549378.png)

如果想定期运行一些程序，可以直接使用 [cron](https://www.man7.org/linux/man-pages/man8/cron.8.html)。它是一个系统内置的，用来执行定期任务的守护进程。(即使用 crontab 指令)

![image-20241020225557824](assets\image-20241020225557824.png)

使用crontab -e命令编辑**当前用户**的 crontab 文件

![image-20241020225606457](assets\image-20241020225606457.png)

![image-20241020225610857](assets\image-20241020225610857.png)

**环境变量**：cron 任务的环境变量可能与用户的登录环境变量不同。如果你的命令依赖于特定的环境变量，可以在 crontab 文件顶部设置这些变量，例如：

PATH=/usr/local/bin:/usr/bin:/bin

​     

[FUSE](https://en.wikipedia.org/wiki/Filesystem_in_Userspace)（**F**ilesystem in **Use**rspace，用户空间文件系统）允许运行在用户空间上的程序实现文件系统调用，并将这些调用与内核接口联系起来。在实践中，这意味着用户可以在文件系统调用中实现任意功能。

FUSE 可以用于实现如：一个将所有文件系统操作都使用 SSH 转发到远程主机，由远程主机处理后返回结果到本地计算机的虚拟文件系统。这个文件系统里的文件虽然存储在远程主机，对于本地计算机上的软件而言和存储在本地别无二致。sshfs就是一个实现了这种功能的 FUSE 文件系统。

一些有趣的 FUSE 文件系统包括：

- [sshfs](https://github.com/libfuse/sshfs)：使用 SSH 连接在本地打开远程主机上的文件
- [rclone](https://rclone.org/commands/rclone_mount/)：将 Dropbox、Google Drive、Amazon S3、或者 Google Cloud Storage 一类的云存储服务挂载为本地文件系统
- [gocryptfs](https://nuetzlich.net/gocryptfs/)：覆盖在加密文件上的文件系统。文件以加密形式保存在磁盘里，但该文件系统挂载后用户可以直接从挂载点访问文件的明文
- [kbfs](https://keybase.io/docs/kbfs)：分布式端到端加密文件系统。在这个文件系统里有私密（private），共享（shared），以及公开（public）三种类型的文件夹
- [borgbackup](https://borgbackup.readthedocs.io/en/stable/usage/mount.html)：方便用户浏览删除重复数据后的压缩加密备份

 

**API****（应用程序接口）**

大多数线上服务提供的 API（应用程序接口）让你可以通过编程方式来访问这些服务的数据。

- 这些 API 大多具有类似的格式。它们的结构化 URL 通常使用 api.service.com 作为根路径，用户可以访问不同的子路径来访问需要调用的操作，以及添加查询参数使 API 返回符合查询参数条件的结果。
- 通常这些返回都是JSON格式，你可以使用[jq](https://stedolan.github.io/jq/)等工具来选取需要的部分。
- 有些需要认证的 API 通常要求用户在请求中加入某种私密令牌（secret token）来完成认证。大多数 API 都会使用 [OAuth](https://www.oauth.com/)。

 

**常见命令行标志参数及模式**

- --help 或 -h 或者类似的标志参数（flag）来显示简略用法
- 会造成不可撤回操作的工具一般会提供“空运行”（dry run）标志参数和“交互式”（interactive）标志参数
- 会造成破坏性结果的工具一般默认进行非递归的操作，但是支持使用“递归”（recursive）标志函数（通常是 -r）
- --version 或者 -V 标志参数可以让工具显示它的版本信息
- --verbose 或者 -v 标志参数来输出详细的运行信息。多次使用这个标志参数，比如 -vvv，可以让工具输出更详细的信息（经常用于调试）
- --quiet 标志参数来抑制除错误提示之外的其他输出。
- 使用 - 代替输入或者输出文件名意味着工具将从标准输入（standard input）获取所需内容，或者向标准输出（standard output）输出结果。如echo "Hello, World!" | cat -  又如cat file1.txt - | grep "pattern"（cat 会先输出 file1.txt 的内容，然后等待从标准输入读取数据比如手动输入一些文本。grep 会在所有输入数据中搜索包含     "pattern" 的行。）
- 有的时候你可能需要向工具传入一个 *看上去* 像标志参数的普通参数，这时候你可以使用特殊参数 -- 让某个程序 *停止处理* -- 后面出现的标志参数以及选项（以 - 开头的内容）：

- rm -- -r 会让 rm 将 -r 当作文件名；

- ssh machine --for-ssh -- foo --for-foo 的 -- 会让 ssh 知道 --for-foo 不是 ssh 的标志参数。



大部分操作系统默认的窗口管理方式都是“拖拽”式的，这被称作堆叠式（floating/stacking）管理器。另外一种管理器是平铺式（tiling）管理器，其使用逻辑和 [tmux](https://link.segmentfault.com/?url=https%3A%2F%2Fgithub.com%2Ftmux%2Ftmux) 管理终端窗口的方式类似，可以让我们在完全不使用鼠标的情况下使用键盘切换、缩放、以及移动窗口。

- Linux
  - awesome
  - i3

- macOS
  - yabai
  - Divvy

- Windows
  - FancyZones



**Docker, Vagrant, VMs, Cloud, OpenStack**

[虚拟机](https://en.wikipedia.org/wiki/Virtual_machine)（Virtual Machine）以及容器化（containerization）等工具可以帮助你模拟一个包括操作系统的完整计算机系统。虚拟机可以用于创建独立的测试或者开发环境，以及用作安全测试的沙盒。

[Vagrant](https://www.vagrantup.com/)：一个构建和配置虚拟开发环境的工具。它支持用户在配置文件中写入比如操作系统、系统服务、需要安装的软件包等描述，然后使用 vagrant up 命令在各种环境（VirtualBox，KVM，Hyper-V等）中启动一个虚拟机。

[Docker](https://www.docker.com/)：一个使用容器化概念的与 Vagrant 类似的工具，在后端服务的部署中应用广泛。

VPS（虚拟专用服务器）:将一台服务器分割成多个虚拟专用服务器的服务

- 实现VPS的技术分为容器技术和虚拟机技术

- 受欢迎的 VPS 服务商有 [Amazon AWS](https://aws.amazon.com/)，[Google Cloud](https://cloud.google.com/)、[ Microsoft Azure](https://azure.microsoft.com/)以及[DigitalOcean](https://www.digitalocean.com/)。



**交互式记事本编程**

[交互式记事本](https://en.wikipedia.org/wiki/Notebook_interface)可以帮助开发者进行与运行结果交互等探索性的编程。

现在最受欢迎的交互式记事本环境大概是 [Jupyter](https://jupyter.org/)。它的名字来源于所支持的三种核心语言：Julia、Python、R。

[Wolfram Mathematica](https://www.wolfram.com/mathematica/) 是另外一个常用于科学计算的优秀环境。



**GitHub**

[GitHub](https://github.com/) 是最受欢迎的开源软件开发平台之一。常用方法：

- 创建一个[issue](https://help.github.com/en/github/managing-your-work-on-github/creating-an-issue)。 issue可以用来反映软件运行的问题或者请求新的功能。创建issue并不需要创建者阅读或者编写代码，所以它是一个轻量化的贡献方式。

- 使用[pull request](https://help.github.com/en/github/collaborating-with-issues-and-pull-requests/about-pull-requests)提交代码更改。pull request（拉取请求）是请求别人把你自己的代码拉取（且合并）到他们的仓库里。很多开源项目仅允许认证的管理者管理项目代码，所以一般需要[fork](https://help.github.com/en/github/getting-started-with-github/fork-a-repo)这些项目的上游仓库（upstream repository），在你的 Github 账号下创建一个内容完全相同但是由你控制的fork仓库。这样你就可以在这个fork仓库自由创建新的分支并推送修复问题或者实现新功能的代码。完成修改以后再回到开源项目的 Github 页面[创建一个pull request](https://help.github.com/en/github/collaborating-with-issues-and-pull-requests/creating-a-pull-request)。
  - [fork](https://help.github.com/en/github/getting-started-with-github/fork-a-repo)这些项目的上游仓库（upstream repository），在你的 Github 账号下创建一个内容完全相同但是由你控制的fork仓库。
  - 在自己的fork仓库自由创建新的分支并推送修复问题或者实现新功能的代码。
  - 完成修改以后再回到开源项目的 Github 页面[创建一个pull request](https://help.github.com/en/github/collaborating-with-issues-and-pull-requests/creating-a-pull-request)。
  - 提交请求后，项目管理者会和你交流pull request里的代码并给出反馈。如果没有问题，你的代码会和上游仓库中的代码合并。