Makefile的基本规则：

```makefile
target ... : prerequisites ...
    command
    ...
    ...
```

+ `target`：目标文件，可以是对象文件（.o），也可以是执行文件。
+ `prerequisites`：生成目标文件所需的文件或目标。
+ `command`：生成目标文件所需的命令。

下面是一个Makefile的示例，该示例具有一个`main.c`和`func.h`、`func.c`文件，我们最终编译为`final`：

```makefile
final : main.o func.o
    gcc -o final main.o func.o
main.o : main.c func.h
    gcc -c main.c
func.o : func.c func.h
    gcc -c func.c
clean :
    rm -f final main.o func.o
```

在Linux下采用`rm`这个指令，如果我们要在Windows使用`make clean`，我们需要使用命令`del`，并且在开头显式地指定SHELL为`cmd.exe`，如下所示：

```makefile
SHELL = cmd.exe

final : main.o func.o
	gcc -o final main.o func.o
main.o : main.c func.h
	gcc -c main.c 
func.o : func.c func.h
	gcc -c func.c 
clean :
	del /F /Q final.exe main.o func.o
```

代码在**Code**文件夹下的**1_HelloMakefile**文件夹中。

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
Hello Makefile!
```


+ 运行命令
```bash
make clean
```
清空.o和.exe文件。


