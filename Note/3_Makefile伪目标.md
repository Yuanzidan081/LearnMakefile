在本章中，我们将学习使用Makefile的伪目标。回顾上一小节的Makefile：


```makefile
SHELL = cmd.exe
objects = main.o func.o

final : $(objects)
	gcc -o final $(objects)
main.o : main.c func.h
	gcc -c main.c
func.o : func.c func.h
	gcc -c func.c
clean :
	del /F /Q final.exe main.o func.o
```

我们可以用隐晦规则，将clean定义为一个伪目标文件，如下所示：

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
	del /F /Q final.exe $(objects)
```

上面文件内容中，`.PHONY`表示，`clean` 是个伪目标文件。

相关代码在**Code**文件夹下的**3_Makefile_PHONY**文件夹中。

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
Makefile PHONY!
```


+ 运行命令
```bash
make clean
```
清空.o和.exe文件。
