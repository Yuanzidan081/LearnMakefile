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


---------------

变量的相关知识：
### 1. 变量的命名
变量的命名字可以包含字符、数字，下划线（可以是数字开头），但不应该含有`:`、`#`、`=`或是空字符（空格、回车等）。变量是大小写敏感的，`foo`、`Foo`和`FOO`是三个不同的变量名。传统的 Makefile 的变量名是全大写的命名方式，但我推荐使用大小写搭配的变量名，如：`MakeFlags`。这样可以避免和系统的变量冲突，而发生意外的事情。
### 2. 变量的声明
+ 变量在声明时需要给予初值，而在使用时，需要给在变量名前加上`$`符号，但最好用小括号`()`或是大括号`{}`把变量给包括起来。如果你要使用真实的`$`字符，那么你需要用`$$`来表示。
### 3. 变量的定义
+ 定义变量的值时，我们可以使用其它变量来构造变量的值，在 Makefile 中有两种方式来在用变量定义变量的值：

#### 3.1 使用`=`定义
第一种方式，也就是简单的使用`=`号，在`=`左侧是变量，右侧是变量的值，右侧变量的值可以定义在文件的任何一处，也就是说，右侧中的变量不一定非要是已定义好的值，其也可以使用后面定义的值。如：

```makefile
SHELL = cmd.exe
foo = $(bar)
bar = $(ugh)
ugh = Huh?

all :
	@echo $(foo)
```

上述代码定义在了**Code**文件夹下的**2_Makefile变量**文件夹的**Makefile_2**中。运行下面指令：

```makefile
make all -f Makefile_2
```

会打印出`Huh?`（`$(foo)`的值是`$(bar)`，`$(bar)`的值是`$(ugh)`，`$(ugh)`的值是`Huh?`）可见，变量是可以使用后面的变量来定义的。使用这个特点可以把变量的真实值推到后面来定义，如：

```makefile
CFLAGS = $(include_dirs) -O
include_dirs = -Ifoo -Ibar
```

但是谨防递归定义，如：

```makefile
CFLAGS = $(CFLAGS) -O
```

或循环引用：

```makefile
A = $(B)
B = $(A)
```

这会让 make 陷入无限的变量展开过程中去，当然，我们的 make 是有能力检测这样的定义，并会报错。还有就是如果在变量中使用函数，那么，这种方式会让我们的 make 运行时非常慢，更糟糕的是，它会使用得两个 make 的函数`wildcard`和`shell`发生不可预知的错误。因为你不会知道这两个函数会被调用多少次。所以就有了第二种调用方式。

#### 3.2 使用`:=`定义

第二种方式，使用`:=`号，如：

```makefile
x := foo
y := $(x) bar
x := later
```

这等价于：

```makefile
y := foo bar
x := later
```

这种方法，前面的变量不能使用后面的变量，只能使用前面已定义好了的变
量。如果是这样：

```makefile
y := $(x) bar
x := foo
```

y 的值是`bar`，而不是`foo bar`。

我们利用上面的规则可以定义一个变量，其值是一个空格：

```makefile
nullstring :=  
space := $(nullstring) # end-of-line
comma := ,
str := a$(space)b$(comma)c$(space)d

Echo2:
	@echo $(str)
```

上述代码定义在了**Code**文件夹下的**2_Makefile变量**文件夹的**Makefile_2**中。运行下面指令：

```makefile
make Echo2 -f Makefile_2
```

会打印出`a b,c d`。

变量后面用`#`注释符来表示变量定义的终止，请注意这里关于`#`的使用，如果我们这样写：

```makefile
dir := /foo/bar     # directory to put the frobs in
filepath := $(dir)/file
Echo3:
	@echo $(filepath)
```

`dir` 这个变量的值是`/foo/bar`，后面还跟了 4 个空格。上述代码定义在了**Code**文件夹下的**2_Makefile变量**文件夹的**Makefile_2**中。运行下面指令：

```makefile
make Echo3 -f Makefile_2
```

我们会看到：

```bash
/foo/bar     /file
```

四个空格也在变量里，所以空格的使用要格外小心。

#### 3.3 使用`?=`定义

第三种方式，使用`?=`号，如：

```makefile
FOO ?= bar
```

如果`FOO`没有定义，那么`FOO`的值就是`bar`，如果`FOO`定义了，那么`FOO`的值就是`FOO`的值。如：

```makefile
ifeq ($(origin FOO), undefined)
    FOO = bar
endif
```

我们可以写出示例代码：

```makefile
foo2 := brd
foo2 ?= bar
foo3 ?= bar
Echo4:
	@echo $(foo2) $(foo3)
```

上述代码定义在了**Code**文件夹下的**2_Makefile变量**文件夹的**Makefile_2**中。运行下面指令：

```makefile
make Echo4 -f Makefile_2
```

会打印出`brd bar`。


注意变量是可以替换的，如下面的`all`变量为`hello`。

```makefile
first_second = Hello
a = first
b = second
all = $($a_$b)
```

### 4. 变量的追加

追加变量有两种写法，第一种写法是：

```makefile
foo := a b c d
foo += e f g h
```

第二种写法是：

```makefile
foo := a b c d
foo := $(foo) e f g h
```

如果变量之前没有定义过，那么，`+=`会自动变成`=`，如果前面有变量定义，那么`+=`会继承于前次操作的赋值符。如果前一次的是`:=`，那么`+=`会以`:=`作为其赋值符。如：

```makefile
variable := value
variable += more
```

等价于：

```makefile
variable := value
variable := $(variable) more
```

如果前一次的是`=`，那么`+=`会以`=`作为其赋值符。如：

```makefile
variable = value
variable += more
```

等价于：

```makefile
variable = value
variable = $(variable) more
```

由于前次的赋值符是“=”，所以“+=”也会以“=”来做为赋值，那么可能会发生变量的递归定义，这是很不好的，所以 make 会自动为我们解决这个问题，我们不必担心这个问题。

### 5. override指示符

`override` 关键字用于强制覆盖通过命令行传递进来的同名变量的值。在 Makefile 中，当你在命令行中为某个变量赋值，同时这个变量也在 Makefile 里被定义时，默认情况下命令行的赋值会覆盖 Makefile 中的定义。而使用 `override` 关键字定义的变量，能让 Makefile 中的赋值覆盖命令行的赋值。


`override` 关键字的语法格式如下：
```makefile
override 变量名 = 值
```
或者
```makefile
override 变量名 := 值
```
- `=` 是递归赋值运算符，在使用变量时才会解析其值。
- `:=` 是立即赋值运算符，会立即计算并确定变量的值。

我们也可以使用追加赋值和条件赋值：

(1) 追加赋值
```makefile
override CFLAGS += -g
```
这会在 `CFLAGS` 变量原有的值后面追加 `-g` 选项，并且会覆盖命令行中对 `CFLAGS` 的赋值。

(2) 条件赋值
```makefile
override CFLAGS ?= -Wall
```
如果 `CFLAGS` 变量之前未被赋值，那么会将其赋值为 `-Wall`，并且会覆盖命令行的赋值。


不使用 `override` 的情况示例如下：

```makefile
# Makefile
CFLAGS = -Wall -Werror

all:
    gcc $(CFLAGS) main.c -o main
```

如果在命令行执行 `make CFLAGS="-O2"`，那么 `CFLAGS` 变量的值就会是 `-O2`，而不是 Makefile 里定义的 `-Wall -Werror`。这是因为命令行的赋值默认会覆盖 Makefile 中的定义。

而使用 `override` 的情况示例如下：

```makefile
# Makefile
override CFLAGS = -Wall -Werror

all:
    gcc $(CFLAGS) main.c -o main
```
在命令行执行 `make CFLAGS="-O2"` 时，`CFLAGS` 变量的值依然是 `-Wall -Werror`，因为 `override` 关键字让 Makefile 中的赋值覆盖了命令行的赋值。

### 6. 多行变量

使用 `define` 关键字设置变量的值可以有换行，这有利于定义一系列的命令。注意，`define` 关键字后面的变量名后面不能有空格。下面的这个示例展示了 `define` 的用法：

```makefile
define two-lines
@echo foo
@echo $(bar)
endef

Echo5:
	$(two-lines)
```
在这个示例中，`two-lines` 是变量名，`echo foo` 和 `echo $(bar)` 是变量的值。`foo`没有加`$`，所以打印的是原始的`foo`，而`bar`在前面表示的是`Huh?`，所以打印的是`Huh?`。上述代码定义在了**Code**文件夹下的**2_Makefile变量**文件夹的**Makefile_2**中。运行下面指令：

```makefile
make Echo5 -f Makefile_2
```

会打印出

```bash
foo
Huh?
```

### 7. 目标变量

目标变量是一种特殊的变量，它的值是一个或多个目标文件。目标变量的语法格式如下：

```makefile
<target> : <variable-assignment>
<target> : override <variable-assignment>
```

例如：

```makefile
prog : CFLAGS = -g
prog : prog.o foo.o bar.o
    $(CC) $(CFLAGS) prog.o foo.o bar.o
prog.o : prog.c
    $(CC) $(CFLAGS) prog.c
foo.o : foo.c
    $(CC) $(CFLAGS) foo.c
bar.o : bar.c
    $(CC) $(CFLAGS) bar.c
```

不管全局的`$(CFLAGS)`的值是什么，在 `prog` 目标，以及其所引发的所有规则中（`prog.o foo.o bar.o` 的规则），`$(CFLAGS)`的值都是`-g`

### 8. 模式变量

模式变量的好处就是，我们可以给定一种“模式”，可以把变量定义在符合这种模式的所有目标上。make 的“模式”一般是至少含有一个“%”的，所以，我们可以以如下方式给所有以`[.o]`结尾的目标定义目标变量。
语法格式是：

```makefile
<pattern ...> : <variable-assignment>
<pattern ...> : override <variable-assignment>
```

例如：

```makefile
%.o : CFLAGS = -O
```

这样，所有以`[.o]`结尾的目标，都会有`CFLAGS`变量，其值都是`-O`。
