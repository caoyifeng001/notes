#### 
    编译时加 -g  告诉编译器你以后可能会调试这个程序。

#### help
    List of classes of commands:

    aliases -- Aliases of other commands
    breakpoints -- Making program stop at certain points
    data -- Examining data
    files -- Specifying and examining files
    internals -- Maintenance commands
    obscure -- Obscure features
    running -- Running the program
    stack -- Examining the stack
    status -- Status inquiries
    support -- Support facilities
    tracepoints -- Tracing of program execution without stopping the program
    user-defined -- User-defined commands

####  list 查看源码
  list <linenum>
        显示程序第linenum行的周围的源程序。
   
    list <function>
        显示函数名为function的函数周围源程序。
       
    list
        显示当前行后面的源程序。
   
    list -
        显示当前行前面的源程序。
        
    一般是打印当前行的上5行和下5行，如果显示函数是是上2行下8行，默认是10行 
    
    set listsize <count>
        设置一次显示源代码的行数。
       
    show listsize
        查看当前listsize的设置。

  list命令还有下面的用法：

    list <first>, <last>
        显示从first行到last行之间的源代码。
   
    list , <last>
        显示从当前行到last行之间的源代码。
       
    list +
        往后显示源代码。
        
    一般来说在list后面可以跟以下这们的参数：

    <linenum>   行号。
    <+offset>   当前行号的正偏移量。
    <-offset>   当前行号的负偏移量。
    <filename:linenum> 哪个文件的哪一行。
    <function> 函数名。
    <filename:function> 哪个文件中的哪个函数。
    <*address> 程序运行时的语句在内存中的地址。
    
#### b BreakPoint 设置断点
   
    我们用break命令来设置断点。正面有几点设置断点的方法：
   
    break <function>
        在进入指定函数时停住。C++中可以使用class::function或function(type,type)格式来指定函数名。

    break <linenum>
        在指定行号停住。

    break +offset
    break -offset
        在当前行号的前面或后面的offset行停住。offiset为自然数。

    break filename:linenum
        在源文件filename的linenum行处停住。

    break filename:function
        在源文件filename的function函数的入口处停住。

    break *address
        在程序运行的内存地址处停住。

    break
        break命令没有参数时，表示在下一条指令处停住。

   break ... if <condition>
        ...可以是上述的参数，condition表示条件，在条件成立时停住。比如在循环境体中，可以设置break if i=100，表示当i为100时停住程序。

    查看断点时，可使用info命令，如下所示：（注：n表示断点号）
    info breakpoints [n]
    info break [n] 
    
#### 设置捕捉点（CatchPoint）

    你可设置捕捉点来补捉程序运行时的一些事件。如：载入共享库（动态链接库）或是C++的异常。设置捕捉点的格式为：
   
    catch <event>
        当event发生时，停住程序。event可以是下面的内容：
        1、throw 一个C++抛出的异常。（throw为关键字）
        2、catch 一个C++捕捉到的异常。（catch为关键字）
        3、exec 调用系统调用exec时。（exec为关键字，目前此功能只在HP-UX下有用）
        4、fork 调用系统调用fork时。（fork为关键字，目前此功能只在HP-UX下有用）
        5、vfork 调用系统调用vfork时。（vfork为关键字，目前此功能只在HP-UX下有用）
        6、load 或 load <libname> 载入共享库（动态链接库）时。（load为关键字，目前此功能只在HP-UX下有用）
        7、unload 或 unload <libname> 卸载共享库（动态链接库）时。（unload为关键字，目前此功能只在HP-UX下有用）

    tcatch <event>
        只设置一次捕捉点，当程序停住以后，应点被自动删除。
        
#### 维护停止点

上面说了如何设置程序的停止点，GDB中的停止点也就是上述的三类。在GDB中，如果你觉得已定义好的停止点没有用了，你可以使用delete、clear、disable、enable这几个命令来进行维护。

    clear
        清除所有的已定义的停止点。

    clear <function>
    clear <filename:function>
        清除所有设置在函数上的停止点。

    clear <linenum>
    clear <filename:linenum>
        清除所有设置在指定行上的停止点。

    delete [breakpoints] [range...]
        删除指定的断点，breakpoints为断点号。如果不指定断点号，则表示删除所有的断点。range 表示断点号的范围（如：3-7）。其简写命令为d。

比删除更好的一种方法是disable停止点，disable了的停止点，GDB不会删除，当你还需要时，enable即可，就好像回收站一样。

    disable [breakpoints] [range...]
        disable所指定的停止点，breakpoints为停止点号。如果什么都不指定，表示disable所有的停止点。简写命令是dis.

    enable [breakpoints] [range...]
        enable所指定的停止点，breakpoints为停止点号。

    enable [breakpoints] once range...
        enable所指定的停止点一次，当程序停止后，该停止点马上被GDB自动disable。

    enable [breakpoints] delete range...
        enable所指定的停止点一次，当程序停止后，该停止点马上被GDB自动删除。

五、停止条件维护

前面在说到设置断点时，我们提到过可以设置一个条件，当条件成立时，程序自动停止，这是一个非常强大的功能，这里，我想专门说说这个条件的相关维护命令。一般来说，为断点设置一个条件，我们使用if关键词，后面跟其断点条件。并且，条件设置好后，我们可以用condition命令来修改断点的条件。（只有break和watch命令支持if，catch目前暂不支持if）

    condition <bnum> <expression>
        修改断点号为bnum的停止条件为expression。

    condition <bnum>
        清除断点号为bnum的停止条件。


还有一个比较特殊的维护命令ignore，你可以指定程序运行时，忽略停止条件几次。

    ignore <bnum> <count>
        表示忽略断点号为bnum的停止条件count次。
        
        
#### 恢复程序运行和单步调试


    你可以用continue命令恢复程序的运行直到程序结束，或下一个断点到来。也可以使用step或next命令单步跟踪程序。

    continue [ignore-count]
    c [ignore-count]
    fg [ignore-count]
    恢复程序运行，直到程序结束，或是下一个断点到来。ignore-count表示忽略其后的断点次数。continue，c，fg三个命令都是一样的意思。
    step <count>
        单步跟踪，如果有函数调用，他会进入该函数。进入函数的前提是，此函数被编译有debug信息。很像VC等工具中的step in。后面可以加count也可以不加，不加表示一条条地执行，加表示执行后面的count条指令，然后再停住。

    next <count>
        同样单步跟踪，如果有函数调用，他不会进入该函数。很像VC等工具中的step over。后面可以加count也可以不加，不加表示一条条地执行，加表示执行后面的count条指令，然后再停住。

    set step-mode
    set step-mode on
        打开step-mode模式，于是，在进行单步跟踪时，程序不会因为没有debug信息而不停住。这个参数有很利于查看机器码。

    set step-mod off
        关闭step-mode模式。

    finish
        运行程序，直到当前函数完成返回。并打印函数返回时的堆栈地址和返回值及参数值等信息。

   until 或 u
        当你厌倦了在一个循环体内单步跟踪时，这个命令可以运行程序直到退出循环体。

    stepi 或 si
    nexti 或 ni
        单步跟踪一条机器指令！一条程序代码有可能由数条机器指令完成，stepi和nexti可以单步执行机器指令。与之一样有相同功能的命令是“display/i $pc” ，当运行完这个命令后，单步跟踪会在打出程序代码的同时打出机器指令（也就是汇编代码）


#### 信号（Signals）
信号是一种软中断，是一种处理异步事件的方法。一般来说，操作系统都支持许多信号。尤其是UNIX，比较重要应用程序一般都会处理信号。UNIX定义了许多信号，比如SIGINT表示中断字符信号，也就是Ctrl+C的信号，SIGBUS表示硬件故障的信号；SIGCHLD表示子进程状态改变信号；SIGKILL表示终止程序运行的信号，等等。信号量编程是UNIX下非常重要的一种技术。

GDB有能力在你调试程序的时候处理任何一种信号，你可以告诉GDB需要处理哪一种信号。你可以要求GDB收到你所指定的信号时，马上停住正在运行的程序，以供你进行调试。你可以用GDB的handle命令来完成这一功能。

    handle <signal> <keywords...>
        在GDB中定义一个信号处理。信号<signal>可以以SIG开头或不以SIG开头，可以用定义一个要处理信号的范围（如：SIGIO-SIGKILL，表示处理从SIGIO信号到SIGKILL的信号，其中包括SIGIO，SIGIOT，SIGKILL三个信号），也可以使用关键字all来标明要处理所有的信号。一旦被调试的程序接收到信号，运行程序马上会被GDB停住，以供调试。其<keywords>可以是以下几种关键字的一个或多个。

        nostop
            当被调试的程序收到信号时，GDB不会停住程序的运行，但会打出消息告诉你收到这种信号。
        stop
            当被调试的程序收到信号时，GDB会停住你的程序。
        print
            当被调试的程序收到信号时，GDB会显示出一条信息。
        noprint
            当被调试的程序收到信号时，GDB不会告诉你收到信号的信息。
        pass
        noignore
            当被调试的程序收到信号时，GDB不处理信号。这表示，GDB会把这个信号交给被调试程序会处理。
        nopass
        ignore
            当被调试的程序收到信号时，GDB不会让被调试程序来处理这个信号。


    info signals
    info handle
        查看有哪些信号在被GDB检测中。


#### 线程（Thread Stops）

    如果你程序是多线程的话，你可以定义你的断点是否在所有的线程上，或是在某个特定的线程。GDB很容易帮你完成这一工作。

    break <linespec> thread <threadno>
    break <linespec> thread <threadno> if ...
        linespec指定了断点设置在的源程序的行号。threadno指定了线程的ID，注意，这个ID是GDB分配的，你可以通过“info threads”命令来查看正在运行程序中的线程信息。如果你不指定thread <threadno>则表示你的断点设在所有线程上面。你还可以为某线程指定断点条件。如：
   
        (gdb) break frik.c:13 thread 28 if bartab > lim

    当你的程序被GDB停住时，所有的运行线程都会被停住。这方便你你查看运行程序的总体情况。而在你恢复程序运行时，所有的线程也会被恢复运行。那怕是主进程在被单步调试时。



#### 查看栈信息

当程序被停住了，你需要做的第一件事就是查看程序是在哪里停住的。当你的程序调用了一个函数，函数的地址，函数参数，函数内的局部变量都会被压入“栈”（Stack）中。你可以用GDB命令来查看当前的栈中的信息。

下面是一些查看函数调用栈信息的GDB命令：

    backtrace
    bt
        打印当前的函数调用栈的所有信息。如：
       
        (gdb) bt
        #0 func (n=250) at tst.c:6
        #1 0x08048524 in main (argc=1, argv=0xbffff674) at tst.c:30
        #2 0x400409ed in __libc_start_main () from /lib/libc.so.6
       
        从上可以看出函数的调用栈信息：__libc_start_main --> main() --> func()
       
   
    backtrace <n>
    bt <n>
        n是一个正整数，表示只打印栈顶上n层的栈信息。

    backtrace <-n>
    bt <-n>
        -n表一个负整数，表示只打印栈底下n层的栈信息。
       
如果你要查看某一层的信息，你需要在切换当前的栈，一般来说，程序停止时，最顶层的栈就是当前栈，如果你要查看栈下面层的详细信息，首先要做的是切换当前栈。

    frame <n>
    f <n>
        n是一个从0开始的整数，是栈中的层编号。比如：frame 0，表示栈顶，frame 1，表示栈的第二层。
   
    up <n>
        表示向栈的上面移动n层，可以不打n，表示向上移动一层。
       
    down <n>
        表示向栈的下面移动n层，可以不打n，表示向下移动一层。

    上面的命令，都会打印出移动到的栈层的信息。如果你不想让其打出信息。你可以使用这三个命令：
   
            select-frame <n> 对应于 frame 命令。
            up-silently <n> 对应于 up 命令。
            down-silently <n> 对应于 down 命令。

   
查看当前栈层的信息，你可以用以下GDB命令：

    frame 或 f
        会打印出这些信息：栈的层编号，当前的函数名，函数参数值，函数所在文件及行号，函数执行到的语句。
   
    info frame
    info f
        这个命令会打印出更为详细的当前栈层的信息，只不过，大多数都是运行时的内内地址。比如：函数地址，调用函数的地址，被调用函数的地址，目前的函数是由什么样的程序语言写成的、函数参数地址及值、局部变量的地址等等。如：
            (gdb) info f
            Stack level 0, frame at 0xbffff5d4:
             eip = 0x804845d in func (tst.c:6); saved eip 0x8048524
             called by frame at 0xbffff60c
             source language c.
             Arglist at 0xbffff5d4, args: n=250
             Locals at 0xbffff5d4, Previous frame's sp is 0x0
             Saved registers:
              ebp at 0xbffff5d4, eip at 0xbffff5d8
             
     info args
        打印出当前函数的参数名及其值。
    
     info locals
        打印出当前函数中所有局部变量及其值。
       
     info catch
        打印出当前的函数中的异常处理信息。

#### 搜索源代码

    不仅如此，GDB还提供了源代码搜索的命令：

    forward-search <regexp>
    search <regexp>
        向前面搜索。

    reverse-search <regexp>
        全部搜索。
       
    其中，<regexp>就是正则表达式，也主一个字符串的匹配模式

#### 指定源文件的路径

    某些时候，用-g编译过后的执行程序中只是包括了源文件的名字，没有路径名。GDB提供了可以让你指定源文件的路径的命令，以便GDB进行搜索。

    directory <dirname ... >
    dir <dirname ... >
        加一个源文件路径到当前路径的前面。如果你要指定多个路径，UNIX下你可以使用“:”，Windows下你可以使用“;”。
    directory
        清除所有的自定义的源文件搜索路径信息。
   
    show directories
        显示定义了的源文件搜索路径。

#### 源代码的内存
    info line命令来查看源代码在内存中的地址, +  “行号”，“函数名”，“文件名:行号”，“文件名:函数名”
    disassemble 可以查看源程序的当前执行时的机器码

#### 查看运行时数据
    可以使用print命令（简写命令为p）
    inspect来查看当前程序的运行数据
    print <expr>
    print /<f> <expr>
        <expr>是表达式，是你所调试的程序的语言的表达式（GDB可以调试多种编程语言），
        <f>是输出的格式，比如，如果要把表达式按16进制的格式输出，那么就是/x。



####  输出格式

    x 按十六进制格式显示变量。
    d 按十进制格式显示变量。
    u 按十六进制格式显示无符号整型。
    o 按八进制格式显示变量。
    t 按二进制格式显示变量。
    a 按十六进制格式显示变量。
    c 按字符格式显示变量。
    f 按浮点数格式显示变量。

#### 查看内存
    使用examine命令（简写是x）来查看内存地址中的值
    x/<n/f/u> <addr>
   
    n、f、u是可选的参数。
   
    n 是一个正整数，表示显示内存的长度，也就是说从当前地址向后显示几个地址的内容。
    f 表示显示的格式，参见上面。如果地址所指的是字符串，那么格式可以是s，如果地址是指令地址，那么格式可以是i。
    u 表示从当前地址往后请求的字节数，如果不指定的话，GDB默认是4个bytes。u参数可以用下面的字符来代替，b表示单字节，h表示双字节，w表示四字节，g表示八字节。当我们指定了字节长度后，GDB会从指内存定的内存地址开始，读写指定字节，并把其当作一个值取出来。
   
    <addr>表示一个内存地址。

    n/f/u三个参数可以一起使用。例如：
   
    命令：x/3uh 0x54320 表示，从内存地址0x54320读取内容，h表示以双字节为一个单位，3表示三个单位，u表示按十六进制显示。

#### 















