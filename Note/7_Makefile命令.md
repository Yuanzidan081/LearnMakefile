# 1. 显示命令
有时候为了调试，我们可以打印一些信息：

```makefile
@echo 正在编译 XXX 模块......
```

每次生成`.o`文件以后，都打印生成成功：

```makefile
# 生成 .o 文件的规则
$(objects) : %.o: %.c
	gcc -c $(CFLAGS) $< -o $@
	@echo generated $@
```

完整的代码是：

```makefile
SHELL = cmd.exe
objects = main.o func.o
depends = $(objects:.o=.d)

# 最终目标
final : $(objects)
	gcc -o final $(objects)

# 生成 .d 文件的规则
%.d: %.c
	@del /F /Q $@ 2>nul || rem
	gcc -MM $(CPPFLAGS) $< > $@.tmp
	sed "s,\($*\)\.o[ :]*,\1.o $@ : ," < $@.tmp > $@
	del /F /Q $@.tmp

# 包含依赖文件
-include $(depends)


# 生成 .o 文件的规则
$(objects) : %.o: %.c
	gcc -c $(CFLAGS) $< -o $@
	@echo generated $@

# 清理规则
.PHONY : clean
clean :
	del /F /Q final.exe *.o *.d
```

**其他相关**：

+ `make -n`或`make --just-print`，那么其只是显示命令，但不会执行命令，这个功能很有利于我们调试我们的 Makefile，看看我们书写的命令是执行起来是什么样子的或是什么顺序的。
+ `make -s`或`--slient`全面禁止命令的显示。


在Makefile_1里加上显示命令的语句：

```makefile
# 生成 .o 文件的规则
$(objects) : %.o: %.c
	gcc -c $(CFLAGS) $< -o $@
	@echo generated $@
```

运行

```bash
make -f Makefile_1
```

即可看到

```bash
...
gcc -c  main.c -o main.o
generated main.o
gcc -c  func.c -o func.o
generated func.o
gcc -o final main.o func.o
```

# 2. 命令执行

当依赖目标新于目标时，也就是当规则的目标需要被更新时，make 会一条一条的执行其后的命令。需要注意的是，如果要让上一条命令的结果应用在下一条命令时，应该使用分号分隔这两条命令。比如第一条命令是 cd 命令，希望第二条命令得在 cd 之后的基础上运行，那么就不能把这两条命令写在两行上，而应该把这两条命令写在一行上，用分号分隔。如：

```makefile
exec:
    cd build
    pwd
```

上面的代码是错误的，因为第二条命令 pwd 是在 cd 命令之后运行的，而不是在 cd 命令的基础上运行的。正确的写法是：

```makefile
exec:
    cd build; pwd
```

上面的代码是正确的，因为第二条命令 pwd 是在 cd 命令的基础上运行的。

然而上述代码只能在 Linux 下运行，在 Windows 下是无法运行的。为此我们进行改写，参见Makefile_2，用`&&`替代分号：

```makefile
final : $(objects)
	gcc -o final $(objects)
	-mkdir build
	@echo [1] exec command in one line
	cd build && dir
	cd ..
	@echo [2] exec command in two lines
	cd build 
	dir
```

完整的代码是：

```makefile
SHELL = cmd.exe
objects = main.o func.o
depends = $(objects:.o=.d)

# 最终目标
final : $(objects)
	gcc -o final $(objects)
	-mkdir build
	@echo [1] exec command in one line
	cd build && dir
	cd ..
	@echo [2] exec command in two lines
	cd build 
	dir

# 生成 .d 文件的规则
%.d: %.c
	@del /F /Q $@ 2>nul || rem
	gcc -MM $(CPPFLAGS) $< > $@.tmp
	sed "s,\($*\)\.o[ :]*,\1.o $@ : ," < $@.tmp > $@
	del /F /Q $@.tmp

# 包含依赖文件
-include $(depends)


# 生成 .o 文件的规则
$(objects) : %.o: %.c
	gcc -c $(CFLAGS) $< -o $@

# 清理规则
.PHONY : clean
clean :
	del /F /Q final.exe *.o *.d
```

运行

```bash
make -f Makefile_2
```

即可看到

```bash
...
mkdir build
子目录或文件 build 已经存在。
make: [final] 错误 1 (忽略)
[1] exec command in one line
cd build && dir
 驱动器 E 中的卷是 新加卷
 卷的序列号是 A417-F7B1

 E:\CS\LearnMakefile\Code\7_Makefile_Command\build 的目录

2025/04/15  23:40    <DIR>          .
2025/04/15  23:40    <DIR>          ..
               0 个文件              0 字节
               2 个目录 543,786,074,112 可用字节
cd ..
[2] exec command in two lines
cd build 
dir
 驱动器 E 中的卷是 新加卷
 卷的序列号是 A417-F7B1

 E:\CS\LearnMakefile\Code\7_Makefile_Command 的目录

2025/04/15  23:48    <DIR>          .
2025/04/15  23:48    <DIR>          ..
2025/04/15  23:40    <DIR>          build
2025/04/15  23:48            54,310 final.exe
2025/04/15  23:03                86 func.c
2025/04/15  23:48                31 func.d
2025/04/14  23:49                82 func.h
2025/04/15  23:48               870 func.o
2025/04/14  23:50                65 main.c
2025/04/15  23:48                31 main.d
2025/04/15  23:48               780 main.o
2025/04/15  23:26               529 Makefile_1
2025/04/15  23:46               663 Makefile_2
              10 个文件         57,447 字节
               3 个目录 543,786,074,112 可用字节
```


# 3. 命令出错

+ Makefile 的命令行前加一个减号`-`（在Tab 键之后），标记为不管命令出不出错都认为是成功的。

```makefile
clean:
	-del /F /Q final.exe *.o *.d
```

注意到Makefile_2文件里的`-mkdir build`命令，是不管命令出不出错都可以继续往下执行。

**其他相关**：

+ `make -k`或`--keep-going`，如果某规则中的命令出错了，那么就终止该规则的执行，但继续执行其它规则。
+ `make -i`或`--ignore-errors`，那么所有命令都会忽略错误。

相关代码在**Code**文件夹下的**7_Makefile_Command**文件夹中。
