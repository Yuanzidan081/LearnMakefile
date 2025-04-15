在3_Makefile伪目标.md文件中，我们看到了变量的使用，下面是原来的代码：

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
	-del /F /Q final.exe $(objects)
```


如果文件很多，我们可以使用通配符来代替文件名取代之前的`${objects}`，但是下面这样是不起作用的：

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

注意上面的`-del /F /Q final.exe *.o`就使用了`*`通配符，意思是后缀为`.o`的文件都被清除掉。

相关代码在**Code**文件夹下的**4_Makefile_star**文件夹中。

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
Makefile Star!
```


+ 运行命令
```bash
make clean
```
清空.o和.exe文件。