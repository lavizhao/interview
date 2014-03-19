makefile文件
============

什么是makefile
--------------

或许很多Winodws的程序员都不知道这个东西，因为那些Windows的IDE都为你做了这个工作，但我觉得要作一个好的和professional的程序员，makefile还是要懂。这就好像现在有这么多的HTML的编辑器，但如果你想成为一个专业人士，你还是要了解HTML的标识的含义。特别在Unix下的软件编译，你就不能不自己写makefile了，会不会写makefile，从一个侧面说明了一个人是否具备完成大型工程的能力。

makefile带来的好处就是——**自动化编译**，一旦写好，只需要一个make命令，整个工程完全自动编译，极大的提高了软件开发的效率。make是一个命令工具，是一个解释makefile中指令的命令工具，一般来说，大多数的IDE都有这个命令，比如：Delphi的make，Visual C++的nmake，Linux下GNU的make。可见，makefile都成为了一种在工程方面的编译方法。

程序的编译与链接
----------------

一般来说，无论是C、C++、还是pas，首先要把源文件编译成中间代码文件，在Windows下也就是 .obj 文件，UNIX下是 .o 文件，即 Object File，这个动作叫做编译（compile）。然后再把大量的Object File合成执行文件，这个动作叫作链接（link）。

即
* 源文件===>中间代码文件=编译
* 中间文件====>执行文件=链接

总结一下

* 源文件首先会生成中间目标文件，再由中间目标文件生成执行文件。
* 在编译时，编译器只检测程序语法，和函数、变量是否被声明。如果函数未被声明，编译器会给出一个警告，但可以生成Object File。
* 在链接程序时，链接器会在所有的Object File中找寻函数的实现，如果找不到，那到就会报链接错误码（Linker Error），在VC下，这种错误一般是：Link 2001错误，意思说是说，链接器未能找到函数的实现。你需要指定函数的Object File。

Makefile介绍
------------

首先，我们用一个示例来说明Makefile的书写规则。以便给大家一个感兴认识。这个示例来源于GNU的make使用手册，在这个示例中，我们的工程有8个C文件，和3个头文件，我们要写一个Makefile来告诉make命令如何编译和链接这几个文件。我们的规则是：
* 如果这个工程没有编译过，那么我们的所有C文件都要编译并被链接。
* 如果这个工程的某几个C文件被修改，那么我们只编译被修改的C文件，并链接目标程序。
* 如果这个工程的头文件被改变了，那么我们需要编译引用了这几个头文件的C文件，并链接目标程序。

Makefile规则
------------

在讲述这个Makefile之前，还是让我们先来粗略地看一看Makefile的规则。

	target ... : prerequisites ...
	command

* target就是一个目标文件，可以是Object File，也可以是执行文件。还可以是一个标签（Label），对于标签这种特性，在后续的“伪目标”章节中会有叙述。
* prerequisites就是，要生成那个target所需要的文件或是目标。
* command也就是make需要执行的命令。（任意的Shell命令）

这是一个文件的依赖关系，也就是说，target这一个或多个的目标文件依赖于prerequisites中的文件，其生成规则定义在command中。说白一点就是说，prerequisites中如果有一个以上的文件比target文件要新的话，command所定义的命令就会被执行。这就是Makefile的规则。也就是Makefile中最核心的内容。

例子
----

正如前面所说的，如果一个工程有3个头文件，和8个C文件，我们为了完成前面所述的那三个规则，我们的Makefile应该是下面的这个样子的。

	edit : main.o kbd.o command.o display.o /
		insert.o search.o files.o utils.o
	cc -o edit main.o kbd.o command.o display.o /
		insert.o search.o files.o utils.o

	main.o : main.c defs.h
	cc -c main.c
	kbd.o : kbd.c defs.h command.h
	cc -c kbd.c
	command.o : command.c defs.h command.h
	cc -c command.c
	display.o : display.c defs.h buffer.h
	cc -c display.c
	insert.o : insert.c defs.h buffer.h
	cc -c insert.c
	search.o : search.c defs.h buffer.h
	cc -c search.c
	files.o : files.c defs.h buffer.h command.h
	cc -c files.c
	utils.o : utils.c defs.h
	cc -c utils.c
	clean :
		rm edit main.o kbd.o command.o display.o /
			insert.o search.o files.o utils.o	










