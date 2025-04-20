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

Makefile常见的目标编写规则：

+ `all`
这个伪目标是所有目标的目标，其功能一般是编译所有的目标。
+ `clean`
这个伪目标功能是删除所有被 make 创建的文件。
+ `install`
这个伪目标功能是安装已编译好的程序，其实就是把目标执行文件拷贝到指定的目
标中去。
+ `print`
这个伪目标的功能是例出改变过的源文件。
+ `tar`
这个伪目标功能是把源程序打包备份。也就是一个 tar 文件。
+ `dist`
这个伪目标功能是创建一个压缩文件，一般是把 tar 文件压成 Z 文件。或是 gz 文
件。
+ `TAGS`
这个伪目标功能是更新所有的目标，以备完整地重编译使用。
+ `check`和`test`
这两个伪目标一般用来测试 makefile 的流程。

