在本章中，我们将学习使用Makefile的变量。回顾上一小节的Makefile：


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

我们可以用变量来代替文件名，如下所示：

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

相关代码在**Code**文件夹下的**2_Makefile变量**文件夹中。

+ 运行命令

```bash
make
```
可以生成上述的`.o`和`.exe`文件。

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
清空`.o`和`.exe`文件。
