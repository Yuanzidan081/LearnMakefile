在5_Makefile静态模式.md文件中，我们看到了静态模式的使用，下面是原来的代码：


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

在上面的makefile中，我们还是要知道目标文件有哪些依赖文件。如果是一个比较大型的工程，就必需清楚哪些 C 文件包含了哪些头文件，并且，在加入或删除头文件时，也需要小心地修改 Makefile，这是一个很没有维护性的工作。为了避免这种繁重而又容易出错的事情，我们可以使用 C/C++编译的一个功能。大多数的 C/C++编译器都支持一个“-M”的选项，即自动找寻源文件中包含的头文件，并生成一个依赖关系。

如果我们执行下面的命令：

```bash
cc -M main.c
```

其输出是：

```bash
main.o : main.c defs.h
```

于是由编译器自动生成的依赖关系，这样一来，你就不必再手动书写若干文件的依赖关系，而由编译器自动生成了。本教程里使用GNU的C/C++编译器，必须得用`-MM`参数，不然，`-M`参数会把一些标准库的头文件也包含进来。

```bash
gcc -M main.c
```

其输出是：

```bash
main.o: main.c func.h \
 E:/Qt/Tools/mingw730_64/x86_64-w64-mingw32/include/stdio.h \
 E:/Qt/Tools/mingw730_64/x86_64-w64-mingw32/include/crtdefs.h \
 E:/Qt/Tools/mingw730_64/x86_64-w64-mingw32/include/_mingw.h \
 E:/Qt/Tools/mingw730_64/x86_64-w64-mingw32/include/_mingw_mac.h \
 E:/Qt/Tools/mingw730_64/x86_64-w64-mingw32/include/_mingw_secapi.h \
 E:/Qt/Tools/mingw730_64/x86_64-w64-mingw32/include/vadefs.h \
 E:/Qt/Tools/mingw730_64/x86_64-w64-mingw32/include/sdks/_mingw_directx.h \
 E:/Qt/Tools/mingw730_64/x86_64-w64-mingw32/include/sdks/_mingw_ddk.h \
 E:/Qt/Tools/mingw730_64/x86_64-w64-mingw32/include/_mingw_print_push.h \
 E:/Qt/Tools/mingw730_64/x86_64-w64-mingw32/include/_mingw_off_t.h \
 E:/Qt/Tools/mingw730_64/x86_64-w64-mingw32/include/swprintf.inl \
 E:/Qt/Tools/mingw730_64/x86_64-w64-mingw32/include/sec_api/stdio_s.h \
 E:/Qt/Tools/mingw730_64/x86_64-w64-mingw32/include/_mingw_print_pop.h
```

改为

```bash
gcc -MM main.c
```

其输出就正常了：

```bash
main.o: main.c func.h
```

编译器的这个功能如何与我们的 Makefile 联系在一起呢。因为这样一来，我们的Makefile 也要根据这些源文件重新生成，让 Makefile 自已依赖于源文件？这个功能并不现实，不过我们可以有其它手段来迂回地实现这一功能。GNU 组织建议把编译器为每一个源文件的自动生成的依赖关系放到一个文件中，为每一个`name.c`的文件都生成一个`name.d`的 Makefile 文件，`[.d]`文件中就存放对应`[.c]`文件的依赖关系。

于是，我们可以写出`[.c]`文件和`[.d]`文件的依赖关系，并让 make 自动更新或自成`[.d]`文件，并把其包含在我们的主Makefile 中，这样，我们就可以自动化地生成每个文件的依赖关系了。

原书给出了一个模式规则来产生`[.d]`文件：

```makefile
%.d: %.c
    @set -e; rm -f $@; \
    $(CC) -M $(CPPFLAGS) $< > $@.$$$$; \
    sed 's,\($*\)\.o[ :]*,\1.o $@ : ,g' < $@.$$$$ > $@; \
    rm -f $@.$$$$
```

这段Makefile代码定义了一个模式规则，其主要目的是为每个 `.c` 文件自动生成对应的依赖文件（`.d` 文件）。依赖文件记录了 `.c` 文件所依赖的所有头文件，这样在头文件发生更改时，Make 工具能够正确地重新编译受影响的 `.c` 文件。下面我们逐行来详细解释这个规则：

## 1. 规则格式

```makefile
%.d: %.c
    命令
```
这里的 `%.d` 是目标文件的模式，代表所有以 `.d` 结尾的文件；`%.c` 是依赖文件的模式，代表所有以 `.c` 结尾的文件。此规则表明，每个 `.d` 文件依赖于同名的 `.c` 文件。

## 2. 命令部分

```makefile
@set -e; rm -f $@; \
 $(CC) -M $(CPPFLAGS) $< > $@.$$$$; \
 sed 's,\($*\)\.o[ :]*,\1.o $@ : ,g' < $@.$$$$ > $@; \
 rm -f $@.$$$$
```

### 2.1 `@set -e; rm -f $@;`
- `@`：在 Makefile 中，`@` 符号用于抑制命令的回显，即执行命令时不会在终端显示该命令本身。
- `set -e`：在 shell 脚本中，`set -e` 表示如果后续命令执行失败（返回非零退出状态），则立即终止脚本执行。
- `rm -f $@`：`$@` 是 Makefile 中的自动变量，代表当前目标文件。这行命令的作用是强制删除当前的 `.d` 文件，确保后续生成的是最新的依赖信息。

### 2.2 `$(CC) -M $(CPPFLAGS) $< > $@.$$$$;`
- `$(CC)`：通常是 C 编译器的名称，例如 `gcc`。
- `-M`：是编译器的选项，用于生成依赖信息。它会分析 `.c` 文件中包含的所有头文件，并输出一个包含这些依赖关系的列表。
- `$(CPPFLAGS)`：是预处理器的选项，例如包含头文件的路径等。
- `$<`：是 Makefile 中的自动变量，代表第一个依赖文件，即当前的 `.c` 文件。
- `$@.$$$$`：`$@` 代表当前目标文件（`.d` 文件），`$$$$` 是 shell 进程的进程 ID，用于生成一个唯一的临时文件名。这行命令的作用是将编译器生成的依赖信息输出到一个临时文件中。

### 2.3 `sed 's,\($*\)\.o[ :]*,\1.o $@ : ,g' < $@.$$$$ > $@;`
- `sed`：是一个流编辑器，用于对文本进行替换、删除、插入等操作。
- `s,\($*\)\.o[ :]*,\1.o $@ : ,g`：这是 `sed` 的替换命令，其中 `$*` 是 Makefile 中的自动变量，代表模式匹配中的通配部分（即 `.c` 和 `.d` 文件的文件名部分）。该命令的作用是将临时文件中的依赖信息进行修改，在 `.o` 文件的依赖项中添加对应的 `.d` 文件。
- `< $@.$$$$`：表示从临时文件中读取输入。
- `> $@`：表示将处理后的结果输出到最终的 `.d` 文件中。
`s,\($*\)\.o[ :]*,\1.o $@ : ,g` 是 `sed` 命令中的替换表达式，下面详细解释其各个部分的含义：

#### 2.3.1 `sed` 命令概述
`sed` 是一个强大的流编辑器，常用于对文本进行替换、删除、插入等操作。基本的替换命令格式为 `s/旧文本/新文本/修饰符`，其中 `s` 表示替换操作，`旧文本` 是要被替换的内容，`新文本` 是替换后的内容，`修饰符` 可以控制替换的范围等。在你给出的表达式中，分隔符使用的是逗号 `,` 而非斜杠 `/`，这是因为表达式里包含了斜杠，使用逗号能避免混淆。



##### 2.3.1.1 `s`
它是 `sed` 替换命令的标志，表明接下来要进行文本替换操作。

##### 2.3.1.2 `\($*\)\.o[ :]*`
这是要被替换的旧文本，它由几个部分构成：
 - `\(` 和 `\)`：在正则表达式里，这两个符号用于捕获分组，把括号内的内容作为一个整体，后续可以通过 `\1`、`\2` 等引用这些分组。
 - `$*`：这是 Makefile 中的自动变量，代表模式匹配中的通配部分。例如，对于规则 `%.d: %.c`，如果目标是 `foo.d`，依赖是 `foo.c`，那么 `$*` 就代表 `foo`。
 - `\.o`：`\.` 用于匹配点号 `.`，因为点号在正则表达式中有特殊含义，所以需要转义；`o` 就是匹配字符 `o`。整体 `\.o` 用于匹配 `.o` 字符串。
 - `[ :]*`：`[ :]` 是一个字符类，用于匹配空格或者冒号；`*` 是量词，表示前面的字符类可以出现 0 次或多次。所以 `[ :]*` 用于匹配零个或多个空格或者冒号。

综合起来，`\($*\)\.o[ :]*` 用于匹配类似 `foo.o` 后面跟着零个或多个空格或者冒号的字符串。

##### 2.3.1.3 `\1.o $@ :`
这是替换后的新文本，它也由几个部分组成：
 - `\1`：引用前面捕获分组中的内容，也就是 `$*` 的值。比如，若 `$*` 是 `foo`，那么 `\1` 就是 `foo`。
 - `.o`：直接匹配 `.o` 字符串。
 - `$@`：这是 Makefile 中的自动变量，代表当前目标文件。例如，对于目标 `foo.d`，`$@` 就是 `foo.d`。
 - `:`：直接匹配冒号字符。

整体来看，`\1.o $@ :` 的作用是在原来的 `.o` 文件后面添加当前的 `.d` 文件作为依赖。

##### 2.3.1.4 `g`
这是替换修饰符，`g` 代表全局替换，意味着会将文本中所有匹配的内容都进行替换，而不只是替换第一个匹配项。

#### 2.3.2 示例说明
假设 `$*` 是 `foo`，`$@` 是 `foo.d`，并且有如下输入文本：
```
foo.o: bar.h baz.h
```
执行 `sed 's,\($*\)\.o[ :]*,\1.o $@ : ,g'` 后，输出会变为：
```
foo.o foo.d : bar.h baz.h
```
这样就把 `.o` 文件的依赖信息更新为同时依赖对应的 `.d` 文件，保证了 Makefile 能在 `.d` 文件或者头文件发生变化时，正确重新编译 `.c` 文件。 

### 2.4 `rm -f $@.$$$$`
这行命令的作用是删除之前生成的临时文件，以保持工作目录的整洁。

这段代码通过模式规则为每个 `.c` 文件生成对应的 `.d` 文件，记录了 `.c` 文件所依赖的所有头文件。在 Make 工具执行时，会根据这些依赖信息来判断哪些文件需要重新编译，从而提高编译效率。


---------------------------

于是，我们的`[.d]`文件也会自动更新了，并会自动生成了，当然，你还可以在这个`[.d]`文件中加入的不只是依赖关系，包括生成的命令也可一并加入，让每个`[.d]`文件都包含一个完赖的规则。一旦我们完成这个工作，接下来，我们就要把这些自动生成的规则放进我们的主Makefile 中。我们可以使用 Makefile 的`include`命令，来引入别的 Makefile 文件，例如：

```makefile
sources = main.c func.c
include $(sources:.c=.d)
```

上述语句中的`$(sources:.c=.d)`中的`.c=.d`的意思是做一个替换，把变量`$(sources)`所有`[.c]`的字串都替换成`[.d]`，关于这个“替换”的内容，在后面我会有更为详细的讲述。当然，你得注意次序，因为 include 是按次来载入文件，最先载入的`[.d]`文件中的目标会成为默认目标。


然而上面的代码只能在Linux上运行，我的环境是在Windows，所以我更改了makefile的代码配置并沿用前面的代码。注意原代码的sed命令在Windows还需要安装一下，安装地址在[https://gnuwin32.sourceforge.net/packages/sed.htm](https://gnuwin32.sourceforge.net/packages/sed.htm)，下载了Windows版本的sed工具以后，将bin目录添加到环境变量，打开cmd，输入`sed --version`，如果输出了版本信息，说明安装成功。更改的makefile如下：

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

# 清理规则
.PHONY : clean
clean :
	del /F /Q final.exe *.o *.d
```



相关代码在**Code**文件夹下的**6_Makefile_AutoDependency**文件夹中。

+ 运行命令

```bash
make
```
可以生成上述的`.o`、`.d`和`.exe`文件。
```bash
gcc -MM  func.c > func.d.tmp
sed "s,\(func\)\.o[ :]*,\1.o func.d : ," < func.d.tmp > func.d
del /F /Q func.d.tmp
gcc -MM  main.c > main.d.tmp
sed "s,\(main\)\.o[ :]*,\1.o main.d : ," < main.d.tmp > main.d
del /F /Q main.d.tmp
gcc -c  main.c -o main.o
gcc -c  func.c -o func.o
gcc -o final main.o func.o
```

+ 运行命令

```bash
final.exe
```

便可以打印出

```
Makefile Auto Dependency!
```


+ 运行命令
```bash
make clean
```
清空`.o`和`.exe`文件。