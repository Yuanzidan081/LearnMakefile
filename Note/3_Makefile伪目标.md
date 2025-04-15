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
	-del /F /Q final.exe $(objects)
```

上面文件内容中，`.PHONY`表示，`clean` 是个伪目标文件。`del`命令前面加了一个小减号的意思就是，也许某些文件出现问题，但不要管，继续做后面的事。

我们并不生成`clean`这个文件。“伪目标”并不是一个文件，只是一个标签，由于“伪目标”不是文件，所以 `make` 无法生成它的依赖关系和决定它是否要执行。我们只有通过显示地指明这个“目标”才能让其生效。当然，“伪目标”的取名不能和文件名重名，不然其就失去了“伪目标”的意义了。

当然，为了避免和文件重名的这种情况，我们可以使用一个特殊的标记`.PHONY`来显示地指明一个目标是“伪目标”，向 `make` 说明，不管是否有这个文件，这个目标就是“伪目标”。

```makefile
.PHONY : clean
clean :
	-del /F /Q final.exe $(objects)
```


只要有`.PHONY : clean`这个声明，不管是否有“clean”文件，要运行“clean”这个目标，只有`make clean`。


也可以这样写

```makefile
.PHONY : clean cleanObj
clean : cleanObj
	-del /F /Q final.exe 
cleanObj :
	-del /F /Q *.o
```

这样可以输入`make clean`和`make cleanObj`命令来达到清除不同种类文件的目的。

相关代码在**Code**文件夹下的**3_Makefile_PHONY**文件夹中。

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
Makefile PHONY!
```


+ 运行命令
```bash
make clean
```
清空`.o`和`.exe`文件。
