---
title: vim+cscope+ctags阅读源码
date: 2021-12-22 14:20:21
tags:
---
使用vim + cscope/ctags，就能够实现Source Insight的功能，可以很方便地查看分析源代码。

关键词: vim, cscope, ctags, tags


1.查看vim是否支持cscope
> $ vim --version | grep cscope

2.查看帮助
> $ man cscope
  $ man ctags
  :help cscope (vim command)

3.使用cscope[2]
    当前目录有main.c，其中调用了cstest.c中的print()，此函数在cstest.h中进行了声明。
    使用下面的命令生成代码的符号索引文件：
    $ cscope -Rbkq
    这个命令会生成三个文件：cscope.out, cscope.in.out, cscope.po.out。

    其中cscope.out是基本的符号索引，后两个文件是使用"-q"选项生成的，可以加快cscope的索引速度。上面命令的参数含义如下：
    -R: 在生成索引文件时，搜索子目录树中的代码
    -b: 只生成索引文件，不进入cscope的界面
    -k: 在生成索引文件时，不搜索/usr/include目录
    -q: 生成cscope.in.out和cscope.po.out文件，加快cscope的索引速度
    -i: 如果保存文件列表的文件名不是cscope.files时，需要加此选项告诉cscope到哪儿去找源文件列表。可以使用"-"，表示由标准输入获得文件列表。
    -I dir: 在-I选项指出的目录中查找头文件
    -u: 扫描所有文件，重新生成交叉索引文件
    -C: 在搜索时忽略大小写
    -P path: 在以相对路径表示的文件前加上的path，这样，你不用切换到你数据库文件所在的目录也可以使用它了。

    在缺省情况下，cscope在生成数据库后就会进入它自己的查询界面，一般不用这个界面，所以使用了"-b"选项。如果已经进入了这个界面，按CTRL-D退出。

	查看阅读c++代码[3]
    cscope缺省只解析C文件(.c和.h)、lex文件(.l)和yacc文件(.y)，虽然它也可以支持C++以及Java，但它在扫描目录时会跳过C++及Java后缀的文件。
	如果希望cscope解析C++或Java文件，需要把这些文件的名字和路径保存在一个名为cscope.files的文件。
	当cscope发现在当前目录中存在cscope.files时，就会为cscope.files中列出的所有文件生成索引数据库。
    下面的命令会查找当前目录及子目录中所有后缀名为".h", ".c", "cc"和".cpp"的文件，并把查找结果重定向到文件cscope.files中。
	然后cscope根据cscope.files中的所有文件，生成符号索引文件。最后一条命令使用ctags命令，
	生成一个tags文件，在vim中执行":help tags"命令查询它的用法。它可以和cscope一起使用。

    $ find . -name "*.h" -o -name "*.c" -o -name "*.cc" -o "*.cpp" > cscope.files
    $ cscope -bkq -i cscope.files
    $ ctags -R

    接下来可以在vim里浏览代码了
    $ vim main.c
    在vim里命令状态下添加符号索引库
    : cscope add cscope.out
    然后可以查看相应的函数定义或文件，ctrl+t返回。
    : cscope find g print
    : cscope find f cstest.h

    #注意# 所生成的cscope.out和tags文件要在打开VIM所在的文件夹，否则VIM无法找到相关符号信息。


4.创建相应的快捷键
    将以下内容添加到~/.vimrc中，vim会自动加载当前目录下的符号索引cscope.out，可以使用ctrl+t、ctrl+]等。

    """""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
    function UpdateCtags()
         let curdir=getcwd()
         while !filereadable("./tags")
             cd ..
             if getcwd() == "/"
                 break
             endif
         endwhile
         if filewritable("./tags")
             :!ctags -R
         endif
         execute ":cd " . curdir
     endfunction
    
     function UpdateCStags()
         let curdir=getcwd()
         while !filereadable("./cscope.out")
             cd ..
             if getcwd() == "/"
                 break
             endif
         endwhile
         if filewritable("./cscope.out")
             :!cscope -Rbq
             execute ":cscope kill 0"
             execute ":cscope add cscope.out"
         endif
         execute ":cd " . curdir
     endfunction
    
     nmap <F8> :call UpdateCtags()<CR>
     nmap <F9> :call UpdateCStags()<CR>
    
     " 这样就可以在更新源代码文件后，随时使用<F8>及<F9>更新tags及cscope.out文件，
     " 不必关闭编辑文件，执行ctags -R/cscope -Rbq及重新打开文件。
    
     if has("cscope")
     set cscopetag   " 使支持用 Ctrl+]  和 Ctrl+t 快捷键在代码间跳来跳去
     " check cscope for definition of a symbol before checking ctags:
     " set to 1 if you want the reverse search order.
     set csto=1
    
     " add any cscope database in current directory
     if filereadable("cscope.out")
     cs add cscope.out
     " else add the database pointed to by environment variable
     elseif $CSCOPE_DB !=""
     cs add $CSCOPE_DB
     endif
    
     " show msg when any other cscope db added
     set cscopeverbose
    
     nmap <C-\>s :cs find s <C-R>=expand("<cword>")<CR><CR>
     nmap <C-\>g :cs find g <C-R>=expand("<cword>")<CR><CR>
     nmap <C-\>c :cs find c <C-R>=expand("<cword>")<CR><CR>
     nmap <C-\>t :cs find t <C-R>=expand("<cword>")<CR><CR>
     nmap <C-\>e :cs find e <C-R>=expand("<cword>")<CR><CR>
     nmap <C-\>f :cs find f <C-R>=expand("<cfile>")<CR><CR>
     nmap <C-\>i :cs find i <C-R>=expand("<cfile>")<CR><CR>
     nmap <C-\>d :cs find d <C-R>=expand("<cword>")<CR><CR>
     endif
    
    """""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
	其中<C-/>g是先同时按ctrl+\键，之后再按一个g。功能就是查看当前光标所在符号的定义。

5.vim阅读代码
    添加cscope符号索引数据库后，可以调用"cscope find"命令进行查找，vim支持8种cscope的查询功能。
	如在代码中查找调用work()函数的函数，可以在vim命令状态下输入":cs find c work"，回车即可。还可以进行字符串查找，
	它会对双引号或单引号括起来的内容查找。还可以输入一个正则表达式，这类似于egrep程序的功能。
    :cs help (vim command下查询)

    s: 查找C语言符号，即查找函数名、宏、枚举值等出现的地方
    g: 查找函数、宏、枚举等定义的位置，类似ctags所提供的功能
    d: 查找本函数调用的函数
    c: 查找调用本函数的函数
    t: 查找指定的字符串
    e: 查找egrep模式，相当于egrep功能，但查找速度快多了
    f: 查找并打开文件，类似vim的find功能
    i: 查找包含本文件的文件


6.在vim中使用tags查找符号
    查看ctags帮助
    $ man ctags
    :help ctags  (vim command)
    :help tags   (vim command)

    在源代码根目录下执行 ctags -R 命令用来为程序源代码生成标签文件，其-R选项表示递归操作，同时为子目录也生成标签文件。
	vim利用生成的标签文件，可以进行相应检索、并在不同的文件C语言元素之间来回切换。
    $ ctags -R

    A) vim中使用":tag xxx"跳到函数或数据结构xxx处。使用tag命令时，可以使用TAB键进行匹配查找，继续按TAB键向下切换。
    某个函数有多个定义时

    D) 运行vim的时候，必须在"tags"文件所在的目录下运行。否则，运行vim的时候还要用":set tags=xxx"命令设定"tags"文件的路径，
	这样vim才能找到"tags"文件（这儿我们已经设置过了"set tags=tags;"，在子目录中也可以使用）。

    E) 在函数中移动光标的快捷键:
    gd 转到当前光标所指的局部变量的定义
    * 转到当前光标所指的单词下一次出现的地方
    # 转到当前光标所指的单词上一次出现的地方

	:ta tagname 跳转到标签tagname定义的地方
	:stag tagname 在分割窗口中查看包含tagname的文件
	:tags 查看到达当前位置所经过的标签路径
	:ts tagname   :tselect	列出匹配tagname的标签，如为空，则使用标签栈中最后的标签
	如果想跳到包含block的标识符":tag /block" 然后用TAB键来选择。这里'/'就是告诉vim 'block'是一个语句块标签。

	:tf 跳转至第一个匹配的标签
	:tlast   :tl 跳转至最后一个匹配的标签
	:tnext			:tn  跳转到下一个匹配的标签
	:[count]tnext
    :[count]tprevious   :[count] tp     跳到前count个

	跳转快捷键：
	ctrl-] ：跳转至光标所在对象定义之处
	ctrl-t：返回跳转前位置


7.taglist插件使用[4]
    该插件可以像Source Insight那样将当前文件中的宏、全局变量、函数等tag显示在Symbol窗口，用鼠标点上述tag，就跳到该tag定义的位置；
	可以按字母序、该tag所属的类或scope，以及该tag在文件中出现的位置进行排序；
	如果切换到另外一个文件，Symbol窗口更新显示这个文件中的tag。taglist依赖于ctags。

    要使用taglist插件，必须满足：
    1).打开VIM的文件类型自动检测功能；
    2).系统中装了Exuberant ctags工具，并且taglist能够找到此工具（因为taglist需要调用它来生成tag文件）；
    3).你的VIM支持system()调用；

    安装taglist插件
    # emerge -av app-vim/taglist

    查看帮助
    :help helptags
    :help taglist
    :help taglist-intro

    打开tag窗口
    :TlistToggle

    .vimrc中配置如下
    """""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
    " ctags setting
    set tags=./tags,./../tags,./*/tags;
    
    " Tag list (ctags)
    
    filetype on                            "文件类型自动检测
    
    if MySys() == "windows"                "设定windows系统中ctags程序的位置
       let Tlist_Ctags_Cmd = 'ctags'
    elseif MySys() == "linux"              "设定linux系统中ctags程序的位置
       let Tlist_Ctags_Cmd = '/usr/bin/ctags'
    endif
    
    let Tlist_Show_One_File = 1            "不同时显示多个文件的tag，只显示当前文件的
    let Tlist_Exit_OnlyWindow = 1          "如果taglist窗口是最后一个窗口，则退出vim
    let Tlist_Use_Right_Window = 1         "在右侧窗口中显示taglist窗口
    
    map <silent> <F8> :TlistToggle<cr>     "在映射F8键打开tags窗口
    """""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

