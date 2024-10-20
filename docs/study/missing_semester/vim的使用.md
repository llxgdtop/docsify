[The Missing Semester - 第三讲 学习笔记 - GrapefruitCat - 博客园 (cnblogs.com)](https://www.cnblogs.com/grapefruit-cat/p/17071087.html)

如果粘贴文本进vim后缩进不对，解决办法：在粘贴文本内容之前，在vim中执行如下命令

```
1  :set paste   // 设置不自动换行
2   
3  然后按“shift+i”进入编辑模式，然后粘贴文本
4   
5  :set nopaste  // 恢复默认设置
```



![img](assets\clip_image002-1729176852908-1.jpg)



Vim本身就是编程语言！

在vim的命令模式下，可以输入：help ：w，这句话的意思是在命令行下w的帮助文档，如果是：help w，那就是在normal模式下w的帮助文档。

输入：sp或者:vsp，可以打开同一个缓冲区（一个缓冲区可以在0个或多个窗口同时打开），这对于同时查看一个文件不同的两个部分有用。同时按下C-W,在结合移动键hjkl，即可移动光标到新的窗口。:sp filename，这会在一个新的水平窗口中打开 **filename** 文件，并自动将光标置于新窗口。

输入：tabnew，可以新开一个标签页。

有几个标签页，每个标签页有一些窗口，每个窗口对应一个或多个缓冲区。所以输入:q其实是关闭window，:qa，即quit all。

普通模式下：（区分大小写）

hjkl对应了左，下，上，右。可以搭配数字使用，比如4j，向下移动4行。

w前进一个单词（word），b后退一个单词（beginning of word），e（end）将光标移到单词末尾，0移动到行的开头，$移动到行的末尾，^移到行的第一个非空字符上，C-U向上滚动，C-D向下滚动，G（大写）移到文本最底部，gg移到文本顶部。L移到当前屏幕上显示的最底行，M移到中间，H移到最上面。:{行数}<CR回车>，跳转的指定的行；{行数}G ，跳转到下 n 行。（不用：）

在当前行中输入fo，光标就会移动到第一个”o”,输入Fo，就移动到光标前最近的o，如oos，当前光标在s，输入Fo，那光标会跳到第二个o。

i向光标前插入，a向光标后插入，I向行头插入，A向行尾插入。s:删除光标所在的字符并开始插入。

输入to，会移动到光标后第一个o的前面一个字符；输入To，会移动到光标前第一个o的后面一个字符。

输入o，会新开一行并进入insert模式，O就是在这一行的上面新开一行并进入insert模式。

删除按d，撤销按u。其中d要配合上面的移动命令使用，如dw，de（删除光标到单词的末尾这段距离）。有dd和cc，就是删除整行。（之后还能按p复制，复制在光标下面的一行）

c（change）与d很像，唯一区别它会进入insert模式。

与撤销相反为C-R。

x就是删除光标所在字符。

r用于替换光标所在的字符。放好光标，输入r，在输入想替换的字符即可。R也可以，它是一直替换光标所在字符直到esc为止。

y是yank（复制），p是粘贴，y也要配合移动命令使用，yy即复制整行。但要复制文本块怎么办呢？可以输入v进入到visual模式，结合上面的移动命令去选中文本块，之后输入y，p即可复制粘贴了。输入S-V可选中行文本块，C-V选择矩形文本块。除此之外，在visual模式下，输入~即可反转大小写。

输入%可以在匹配的括号之间来回跳转，在括号（还可以引号反正就是一系列配对的符号）内，输入修饰符a（around）或者i（inside）搭配不同的命令就会有不同的效果，比如说（sssaaa），现在光标停留在第一个s，输入 ci( ，就能把括号内容删除并且进入insert模式,输入 ca( ，就能把包括括号的内容给删除掉。

输入/，即可开始搜索。按下enter即可到第一个，n就是下一个。N上一个。

·(点，即右括号那个)这个命令表示重复执行前一个编辑命令。

确定要搜索光标后面的词，就输入?而不是/，C-O回到原来位置

**现已安装ctags插件，cd到项目的目录里面，之后使用    ctags -R .    这个.别忘，就可以实现代码跳转，C-]跳转，g+Ctrl-]用于重名的代码跳转，C-T回到上一个（可能只可以在文件夹内的文件跳转，不能跨文件夹跳转）。**

还安装了CtrlP插件，在vim中使用C-P即可进行搜索文件名，而不用退出vim然后再用fd命令，搜到对应的文件名并进去后，可以使用Ctrl-^来实现在最近使用的两个文件内快速切换。如果打开了多个文件，用:ls，之后用

:b <buffer_number_or_name>来切换文件名。如果你在同一个窗口中更换了文件，可以使用缓冲区命令来关闭当前文件，而不是整个窗口:bd（不保存，若想保存并退出:w | bd，不想保存并强制退出:bd!）命令可以关闭当前的缓冲区（文件），但如果这是你唯一打开的缓冲区，Vim 窗口仍然会关闭。如果有其他缓冲区打开，Vim 会切换到下一个缓冲区。





Some skills learned from vimtutor：



Type dw to delete a word.

Type d$ to delete to the end of the line.

（中间还可以插入数字）

To undo previous actions, type:      u (lowercase u)

To undo all the changes on a line, type: U (capital U)

To change until the end of a word, type ce

Type CTRL-G to show your location in the file and the file status.

Type G to move to a line in the file.

Type :s/old/new/g to substitute 'new' for 'old'.（substitute替代）

（eg：thee best time to see thee flowers is in thee spring.

Type :s/thee/the `<ENTER>` . Note that this command only changes the first occurrence of "thee" in the line. type :s/thee/the/g . Adding the g flag means to substitute globally in the line, change all occurrences of "thee" in the line.

Type  :%s/old/new/g   to change every occurrence in the whole file.

Type  :%s/old/new/gc   to find every occurrence in the whole file,with a prompt whether to substitute or not.

type  :#,#s/old/new/g  where #,# are the line numbers of the range of lines where the substitution is to be done.）

如果碰到了如`</name>`想改成`<name>`，那么得加个转义符号:`%s/<\/name>/<name>`

Type :! followed by an external command to execute that command.

To save the changes made to the text, type :w FILENAME

To save part of the file, type v motion :w FILENAME（选中要保存的东西，然后:之后w再然后filename）

To insert the contents of a file, type :r FILENAME

yw yanks one word

搜索时要高亮， :set hls取消高亮就:nohlsearch

If you want to ignore case for just one search command, use \c,

eg: /需要搜索的东西\c `<ENTER>`

Set the 'ic' (Ignore case) option by entering:  :set ic区分大小写

**Vim中还可以录制宏，输入qp即开始录入名为p的宏（当然可以把p改成别的），之后做一些操作，回到正常模式后，输入q即可停止录制。想要调用宏，在normal模式下用@p，如果想调用10次宏就10@p。宏中还可以嵌套宏，比如先录制好宏p了，现在又qe，之后做一些操作（其中夹杂着999@p），最后q，然后在normal mode下输入199@e，那么它的效果为执行199次宏e，其中每一次宏e中包含了999次宏p。这么做就可以做成全文快速修改等批量处理操作。**