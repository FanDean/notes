﻿GNU Make 项目管理：第 3 版
===================
标签：开发工具

---

<font face=楷体> 
Managing Projects with GNU Make,Third Edition		
[美] Robert Mecklenburg 著 	2004            
O Reilly Taiwan 公司编译 , 2006.7		    
翻译的很不符合大陆风格
</font>


## Make常用命令

命令
--------

|   命令				| 描述						|
|------------------------| ------------------------|
| make					| 将查找makefle 或 Makefile |
|-f fileName			 | 指定文件文件作为makefile文件		|
|-k						| 发现错误仍然继续执行，而不是停止 |
|-n						| 让make输出要执行的操作		|
|-s						|执行时不打印命令名，(执行而不显示执行状况) |
|-w						| 如果make在执行时改变目录，打印当前目录名|
|-r						| 禁止使用所有的内置规则  |
|-Wfile					| 如果file已经修改，则使用-n来显示将要执行的命令 |
|-d						| 打印调试信息 ，（Debug模式，输出有关文件和检测时间的详细信息      |
|-I dirname				|指定被包含的makefile所在目录  |
|-i						| 忽略makefile规则中的Linux命令返回的非零错误码，make继续执行|
|-k						|如果某个目标编译失败，继续编译其他目标|
|-jN					|每次运行N条命令|
|-p						|  打印出Makefile中所有宏定义和描述内部规则的相关行  |
| -c dir				| 在读取Makefile之前改变到指定的目录   |





规则
-------

Makefile规则通用形式：

```makefile
target: dependency [dependency [...]]
	command						
	command			#每一个命令前必须有一个制表符
	[...]
```



`\`  用处与shell中的类似

`#` 注释




用于程序名和标志的预定义变量：

| 变量  |  说明      |
|-------|--------------|
|AR		|归档维护程序，默认 = ar |
|AS		|汇编程序，默认 = as	|
|CC		| C编译程序，默认 = cc |
|CPP    |C预处理程序，默认 = cpp |
|RM		|文件删除程序，默认 = "rm -f" |
|ARFLAGS|传给归档维护程序的标志 默认 =rv |
|ASFLAGS | 传给汇编程序的标志 |
|CFLAGS	|传给C编译器的标志   |
|CPPFLAGS| 传给C预处理器的标志 |
|LDFAGS	|传给连接器程序 (ld)的标志|


变量的展开:需使用`$()` `${}`

```makefile
#几个变量使用示例：  
CFLAGS	:= -g -W -std=c99 -c
LDFAGS	:= -lusrese
```








### 伪目标

除了一般的文件目标体，make也允许指定伪目标。   
称其为伪目标是因为它们并不对应于实际的文件。  

如常见的 clean 

伪目标体规定了make执行的命令，但是没有依赖体，
make就认为目标体是最新的而不执行任何操作，**所以它的命令不会被自动执行**，除非明确指定。

|  目标   |    功能     |
|-------------| ------------|
|all		| 执行编译应用程序的所有工作|
|install	| 从已编译的二进制文件进行应用程序的安装|
|clean		| 将产生自源代码的二进制文件删除|
|distclean	| 删除编译过程中产生的任何文件  |
|TAGS		| 建立可供编辑器使用的标记表    |
|info		| 从Texinfo	源代码来创建GUN info 文件 |
|check		| 执行与应用程序相关的任何测试  |






#### 自动变量

make会设定自动变量，下面是重要的几个自动变量：

|   自动变量	|   作用		|
|---------------|---------------------------------------|
|$@	| 目标的文件名								|
|$% | 归档文件中的文件名							|
|$< | 第一个必要条件的文件名						|
|$? | 当前目标所依赖的文件列表中比当前目标文件还要新的必要条件|
|$^ | 所有必要条件的文件名列表（并且会把重复的文件名删除） |
|$+ | 与$^的区别是，包含重复的文件名 |
|$(@D) | 目标文件的目录部分(如果目标在子目录中)  |
|$(@F) | 目标文件的文件名部分(如果目标在子目录中)  |


与普通变量不同的是，要展开变量的值需要使用`$()`或者`${}`,自动变量则不需要这样。 
make直会在目标与条件能够进行匹配的时候，才会设置自动变量，所以自动变量只能应用在命令中。   




#### 特殊目标  

| 	特殊目标	 	 | 		描述			 |
|-------			-|--------			|	
|   .PHONY          |   它的所有依赖被作为伪目标      |
| .SUFFIXES			| 它的所有依赖指出了一系列在后缀规则中需要检查的后缀名|
|.DEFAULT			| ... |
| .PRECIOUS		| 它的依赖文件，当命令在执行过程中被中断时，make不会删除它们，而且如果目标的依赖文件是中间过程文件，这些文件同样不会删除|
|.INTERMEDIATE | 它的依赖文件被当作中间过程文件 |
|.SECONDARY   | 也被当作中间过程文件对待，但这些文件不会被自动删除 |
|.DELETE_ON_ERROR | 如果规则的命令执行错误，将删除已经被修改的目标文件 |
|.IGNORE  |		...  |
|更多详情见 《GNU make中文手册》  |   p42     |




### 其他摘抄

下面的命令将git安装在 /usr/local/bin 中 

```shell
make prefix=/usr/local all
sudo make prefix=/usr/local install
```





第一部分 基本概念 
============


看完第一部分后，你将获得相当完整的GNU make操作知识，并且掌握许多高级的用法。



第一章 如何编写一个简单的 makefile 
-------------------------------------------



make程序可让"将源代码转换成可执行文件"之类的例行工作自动化，相较于脚本，make的
优点是：make会根据关系和时间戳判断应该重新进行哪些步骤。    

make定义了一种语言，可以用来描述源文件、中间文件、以及可执行文件之间的关系。    

make还提供了一些功能，可以用来管理各种候选配置、实现可重用程序库的细节以及让用户以自定义
宏将过程参数化。

**可将makefile中的某个目标（target）指定成命令行参数，make就会只针对该目标进行更新的动作**

make默认以第一个目标为默认目标。  

在makefile 中，默认的目标一般就是编译程序。





###目标与必要条件



makefile中包含的规则可分成三个部分：    

1. 目标（target）: 一个必须建造的文件或进行的事情。   
2. 必要条件（prerequisite）：或称依存对象，是目标得以被成功创建之前，必须存在的文件。 
3. 所要执行的命令（command）: 必要条件成立时，将会创建目标的那些shell命令。

```make
target1 target2 : prereq1 prereq2
	command1
	command2
```


makefile 一般都是以“从上而下”的风格的方式编写。




###检查依存关系 

示例：   

```make
count_words: count_words.o lexer.o -lfl
	gcc count_words.o lexer.o -lfl -o count_words

count_words.o:count_words.c
	gcc -c count_words.c

lexer.o:lexer.c
	gcc -c lexer.c

lexer.c:lexer.l
	flex -t lexer.l > lexer.c       #对与程序flex不做介绍，它是用来生成lexer.c文件的
```


关于示例中的 `-lfl` 其中`-l` 是选项，`fl`参数代表库libfl.a 。   

GUN make 对这个语法提供了特别的支持，当`-l<NAME>`形式的必要条件被发现时，
make会搜索`libNAME.so`形式的文件，如果没有接着会搜索`libNAME.a`形式的文件。   




###尽量减少重新编译的工作量

由于前一个示例介绍了目前无法理解的程序 故略




###调用 make  





###Makefile 的基本语法


```make
target1 target2 : prereq1 prereq2
	command1
	command2
```

1. 可以有一个或多个目标，

2. 可以有零个或多个必要条件。
	如果没有必要条件，那么只有在目标所代表的文件不存在时才会进行更新动作（应该还有例外）。

3. 每个命令必须以制表符开头，这个（隐含的）语法用来要求make将紧跟在tab后的内容
	传给 subshell 来执行。   

`#`  为注释符，但**注意**当`#`出现在命令文本后，它也会传给shell。

`\` 可用来延长过长的文本行。   






第二章  规则
----------------

弄清楚什么是规则？   


make有多种规则：

* 具体规则(explicit rule)：包含工作目录、必要条件、更新的动作。

* 模式规则(pattern rule)： 使用的是通配符(wildcard)而不是明确的文件名，
	这让make得以对与模式相符的工作目录应用该规则，进行必要的更新。  

* 隐含规则(implicit rule)：可以是模式规则，也可以是内置于make的后缀规则(suffix rule)

* 静态模式规则(static pattern rule): 它就像正规模式规则，只是它们只能应用在一串
	特定的目标文件中。

GUN make 可作为其他版本的make替代品，它特别针对兼容性提供了若干功能。   
GUN make 考虑以模式规则来替换后缀规则。  





###具体规则


你不必将规则一次定义完全。每当make看到一个目标，就会将该目标与其必要条件加入依存图。
如果make看到的目标已经存在与依存图中，则任何额外的必要条件都会被附加到已有的项目中。

(目标和必要条件形成依存图 dependency graph)  



####通配符

当你有一长串文件要指定时，为了简化过程，可使用make提供的通配符，此功能
也被称为文件名模式匹配(globbing)。

make的通配符类似 Bourne shell 的`~  *  ?  [...]   [^....]`   

[...] 代表一个字符集，[^...] 该字符集的补集。

`~` 代表用户主目录。  

注意：当通配符出现在工作目录或必要条件中时，是由make在读取makefile文件时立即进行扩展通配符。
     但是当它出现在命令中时是在执行命令时有shell扩展。  





####假想目标

任何不代表文件的目标就称为假想目标（不生成该以该目标为名的文件）

通常，make总是会执行(在通过命令行指定时)假想目标，因为对应与该规则的命令并不会创建以该目标为名称的文件。 

例如：

```makefile
clean:
	rm -f *.o lexer.c
```

切记：make无法区分文件形式的目标与假想目标。如果当前目录中刚好出现与
假想目标同名的文件，make会将在它的依存图中建立该文件与假想目标的关系。 
举例来说：如果你运行 make clean时，工作目录中有 clean 这个文件,那么会出现下面的信息：  

	$ make clean
	make: 'clean' is up to date.

因为大多数的假想目标并未指定必要条件，clean目标被视为已经更新。   

为了避免这个问题，GUN make 提供一个特殊的目标 `.PHONY`，用来告诉make该
目标不是一个真正的文件。

声明假想目标： 只要将该目标指定成 .PHONY 的必要条件即可：

```makefile
.PHONY: clean
clean:
	rm -f *.o lexer.c
```



另一种用途：  

例如：all目标常会被用来指定所要编译的一串程序：

```makefile
.PHONY: all
all: bash bashbug
```
其中all目标将会创建bash、以及bashbug程序。


标准的假想目标：  

|  目标   |    功能     |
|-------------| ------------|
|all		| 执行编译应用程序的所有工作|
|install	| 从已编译的二进制文件进行应用程序的安装|
|clean		| 将产生自源代码的二进制文件删除|
|distclean	| 删除编译过程中产生的任何文件  |
|TAGS		| 建立可供编辑器使用的标记表    |
|info		| 从Texinfo	源代码来创建GUN info 文件 |
|check		| 执行与应用程序相关的任何测试  |




####空目标

假想目标总是尚未更新，所以它们总是被执行（在通过命令行参数指定时）。

暂时看不懂








###变量

变量的使用：  `$()` `${}` 将变量名括起，但当变量名若为单一字符则不需要为它加上括号。  



####自动变量

make会设定自动变量，下面是重要的7个自动变量：

|   自动变量	|   作用		|
|---------------|---------------------------------------|
|`$@`	| 目标的文件名								|
|`$%` | 归档文件中的文件名							|
|`$<` | 第一个必要条件的文件名						|
|`$?` | 当前目标所依赖的文件列表中比当前目标文件还要新的必要条件|
|`$^` | 所有必要条件的文件名列表（并且会把重复的文件名删除） |
|`$+` | 与$^的区别是，包含重复的文件名 |
|`$(@D)` | 目标文件的目录部分(如果目标在子目录中)  |
|`$(@F)` | 目标文件的文件名部分(如果目标在子目录中)  |





###以VPATH和vpath来查找文件  

一般按照源代码树(source tree)的布局惯例，头文件会放在 include 目录中，而源文件放在 src 目录里。
我们也会这样将 makefile 文件会放在它们的上层目录(parent directory) 。


如：  

```
test/
├── include
│   ├── counter.h
│   └── lexer.h
├── makefile
└── src
    ├── counter.c
    ├── count_words.c
    └── lexer.l
```


可以使用 VPATH 和 vpath 来告诉make到不同的目录去查找源文件。  

	VPATH = src 

表示make会在当前目录和src目录中查找文件。

但是现在gcc还无法找到 头文件,此时我们在编译时使用gcc的 `-I`选项来指定头文件的路径,并且在VPATH 中添加 
include 路径：

我们可以使用一个名为`CFLAGS`的环境变量，用它来指定一个默认的编译选项，在g++ 中只能用`CPPFLAGS`。我们就可以在makefile中直接使用这个变量来对源码进行编译。      


此时之前的makefile需改成下面的形式：

```makefile
VPATH = src include
CFLAGS = -I include			 #也可用 CPPFLAGS = -I include 

count_words: count_words.o lexer.o -lfl
	gcc $^ -o $@

count_words.o:count_words.c  include/counter.h
	gcc -c $< -o $@

counter.o: counter.c include/counter.h include/lexer.h
	gcc -c $< -o $@

lexer.o:lexer.c include/lexer.h
	gcc -c $< -o $@

lexer.c:lexer.l
	flex -t $<  > $@
```

VPATH变量的内容是一份目录列表，该目录列表可以用来搜索工作目录以及必要条件，但不包含脚本中提及的文件。  
该目录列表可用空格` ` 或冒号`:`来表示，而在windows上可以是空格和分号。  

make会为文件填入正确的相对路径，前提是使用自动变量；当使用的是具体的文件名时，make将无法填上
正确的路径。   



虽然VPATH变量可以解决以上的搜索问题，但也有限制。  
如果在多个目录中出现同名的文件，则make只会使用第一个被找到的文件，这可能会造成问题。  

此时可以使用 vpath 指令(directive)；该指令语法如下：

	vpath pattern directory-list

所以之前使用的VPATH变量可以改成：  

	vpath %.l %.c src
	vpath %.h include

现在我们告诉make应该在 src 目录中搜索 `.c .l`文件，在 include 下搜索 `.h` 文件。 










###模式规则


许多程序在读取以及输出文件时都会依照惯例。

这些程序惯例让 make 可以通过文件名模式的匹配来简化规则的建立，以及提供内置规则来
处理它们。

通过这些内置规则我们可以把之前17行的makefile缩减成7行：

```makefile
VPATH = src include
CFLAGS = -I include 

count_words: count_words.o lexer.o -lfl
count_words.o: counter.h
counter.o:counter.h lexer.h
lexer.o: lexer.h
```

通过命令 `make --print-data-base`  或选项 -p ,列出make具有哪些默认规则(和变量)。

所有内置规则(built-in rule) 都是模式规则(pattern rule)的实例。  

上面的makefile之所以可行是因为可以通过下面的三项内置规则来处理：

```
%.o: %.c		
	$(COMPILE.c) $(OUTPUT_OPTION) $<
该规则描述了如何从一个.c 文件编译出一个.o 文件    

%.c: %.l
	@$(RM) $@
	$(LEX.l) $< > $@
该规则描述了如何从 .l 文件产生一个 .c 文件

%: %.c
	$(LINK.c) $^ $(LOADLIBES) $(LDLIBS) -o $@
该规则描述了如何从 .c 文件产生一个无扩展名的文件(通常是可执行文件)
```

		
介绍： 如第一条规则，适用makefile中的这一句 
```
count_words.o: counter.h
%.o: %.c		
```
我们一一对应下来，count_words.c 对应 %.o 则 % 代表了count_words ,然后make使用
count_words为词干寻找 名为 count_words.c 的文件进行执行命令。counter.h 是附加的必要条件。
（以上介绍为自己领悟，对错与否还不知道）

可以在脚本中更改变量的值来自定义内置规则。




####模式

模式规则中的百分比`%`大体上等效于shell中的星号`*`，
它可以代表任意多个字符，可放在模式中的任何地方，但只能出现一次。

使用示例：

```
%,v
s%.o
wrapper_%
```



####静态模式规则

static pattern rule 只能应用在特定的目标上。   

```
$(OBJECTS): %.o: %.c
	$(CC) -c $(CFLAGS) $< -o $@
```

开头的 `$(OBJECTS):`规范，将使得该规则只能应用在 $(OBJECTS) 变量中所列举的文件上。  

此规则于一般模式规则类似： %.o 模式会匹配 $(OBJECTS)中所列举的每个目标文件并取其词干。   
然后该词干会被替换进 %.c 模式，以产生目标的必要条件。




####后缀规则

suffix rule 是用来定义隐含的最初（即过时的）方法。  

用于旧式的 make ，不支持 GUN make的模式规则语法。  

示例：

```
.c.o:
	$(COMPILE.c) $(OUTPUT_OPTION) $<
```

必要条件的扩展名在前，而目标的扩展名在后，该规则与之前的`%.o: %.c`一样。  

后缀规则：通过除去后缀的目标名，匹配必要条件的文件名。  


当目标无后缀名时则可将其省略：`.o` 此时通常目标是可执行文件。


扩展名列表：  

可使用`.SUFFIXES`这个特别的目标来设定已知的扩展名,make已经有一个相关的列表。
只要在makefile文件中加入`.SUFFIXES`规则，
就可以将自己的扩展名加入此列表。












###隐含规则 


GNU make 3.80 版有90个隐含规则(是以模式规则和后缀规则的形式)


####一个简单的help命令

为了帮助弄清楚大型makefile中包含的目标，可以编写一个简介的help命令作为默认目标。    

示例：这个help目标会把可用的make目标显示成一份经过排序的四个字段的列表。
为了避免手工维护，我们直接从make的规则库中搜集可用的命令。  

```makefile
#help - The default goal
.PHONY: help
help:
	@ $(MAKE) --print-data-base --question |			\
	  $(AWK) '/^[^.%][-A-Za-z0-9_]*:/					\
			{ print substr($$1, 1, length($$1)-1)}' |	\
	  $(SORT) |											\
	  $(PR) --omit-pagination --width=80 --columns=4
```

解释：此规则的命令脚本中的命令使用管道相连。make命令的--print-data-base 选项输出make的规则库，--question选项
可避免make执行其中的任何命令。然后此规则库被送到awk过滤器，以便收集任何并非以百分号和点号开头(分别代表模式
规则和后缀规则)的工作目标以及删掉额外的信息；最后目标列表经过排序，并以四个字段的格式输出。  










###特殊目标

特殊目标是一个内置的假想目标(phony target),用来变更make的默认行为。  
它实际上比较像用来修改make内部算法的指令。  

* .INTERMEDIATE 该目标的必要条件被视为临时文件，执行完make之后会被删除。  

* .SECONDARY 也被视为临时文件，但不会被自动删除。

...




###自动产生依存关系




###管理程序库






第三章 变量与宏
---------------------

变量名区分大小写。  
变量名不允许包括`: 、 # 、 = `  允许在变量名中间的空格但不允许前置空白和尾置空白。  





当变量用来表示用户在命令行上或环境中所自定义的常数时，习惯上会全部用大写来表示，单词之间以下划线隔开；  
只在makefile文件中出现的变量，全部以小写字母来表示。



###变量的用途



###变量的类型



###宏





###何时扩展变量




###目标与模式的专属变量





###变量来自何处





###条件指令与引入指令的处理






###标准的 make 变量





第四章 函数
-------------


###用户自定义函数



###内置函数


###高级的用户自定义函数






第五章  命令
---------------

###解析命令



###使用哪个shell




###空命令




###命令环境




###对命令脚本求值




###命令行的长度限制






第二部分  高级与特别的议题
==================












