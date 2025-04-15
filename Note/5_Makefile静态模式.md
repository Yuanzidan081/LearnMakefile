在4_Makefile通配符.md文件中，我们看到了通配符的使用，下面是原来的代码：


```makefile
SHELL = cmd.exe
objects = main.o func.o

final : $(objects)
	gcc -o final $(objects)
main.o : main.c func.h
	gcc -c main.c
func.o : func.c func.h
	gcc -c func.c
.PHONY : clean
clean :
	-del /F /Q final.exe *.o
```

我们发现main.o和func.o生成的方式是类似的，在执行make命令以后，其实会执行：

```bash
gcc -c  main.c -o main.o
gcc -c  func.c -o func.o
```

如果有很多像上面一样的命令，我们可以进行简化嘛？答案是可以的，可以使用静态模式，静态模式可以更加容易地定义多目标的规则，它的语法规则是：

```makefile
<targets ...>: <target-pattern>: <prereq-patterns ...>
    <commands>
    ...
```
`targets` 是目标文件，`target-pattern` 是目标文件的模式，`prereq-patterns` 是依赖文件的模式，`commands` 是执行的命令。

如果我们的`<target-parrtern>`定义成`%.o`，意思是我们的`<target>`集合中都是以`.o`结尾的，而如果我们的`<prereq-parrterns>`定义成“%.c”，意思是对`<target-parrtern>`所形成的目标集进行二次定义，其计算方法是，取`<target-parrtern>`模式中的`%`（也
就是去掉了`[.o]`这个结尾），并为其加上`[.c]`这个结尾，形成的新集合。对于上面的例子我们就可以写成：

```makefile
SHELL = cmd.exe
objects = main.o func.o

final : $(objects)
	gcc -o final $(objects)
${objects} : %.o: %.c
	gcc -c ${CFLAGS} $< -o $@
.PHONY : clean
clean :
	-del /F /Q final.exe *.o
```

上面的例子中，静态模式的语法为：

```makefile
${objects} : %.o: %.c
	gcc -c ${CFLAGS} $< -o $@
```

它指明了我们的目标从`$object` 中获取，`%.o`表明要所有以`.o`结尾的目标，也就是`func.o main.o`，即变量`$object` 集合的模式，而依赖模式`%.c`则取模式`%.o`的`%`，也就是`func main`，并为其加下`.c`的后缀，于是，我们的依赖目标就是`func.c main.c`。而命令中的`$<`和`$@`则是自动化变量，`$<`表示所有的依赖目标集（也就是`func.c main.c`），`$@`表示目标集（也就是`func.o main.o`）。于是，上面的规则展开后等价于下面的规则，这等价于：

```makefile
main.o : main.c
	gcc -c ${CFLAGS} main.c -o main.o
func.o : func.c
	gcc -c ${CFLAGS} func.c -o func.o
```

如果我们的`%.o`有几百个，那种我们只要用这种很简单的“静态模式规则”就可以写完一堆规则，实在是太有效率了。

相关代码在**Code**文件夹下的**5_Makefile_StaticMode**文件夹中。

+ 运行命令

```bash
make
```
可以生成上述的.o和.exe文件。

+ 运行命令

```bash
final.exe
```

便可以打印出

```
Makefile Static Mode!
```


+ 运行命令
```bash
make clean
```
清空.o和.exe文件。