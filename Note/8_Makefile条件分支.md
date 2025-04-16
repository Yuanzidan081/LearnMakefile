# 1. `ifeq`和`ifneq`

使用条件判断，可以让 make 根据运行时的不同情况选择不同的执行分支。条件表达式可以是比较变量的值，或是变量和常量的值。判断条件相等可以用`ifeq`，其语法格式是：

```makefile
ifeq (<arg1>, <arg2>)
ifeq '<arg1>' '<arg2>'
ifeq "<arg1>" "<arg2>"
ifeq "<arg1>" '<arg2>'
ifeq '<arg1>' "<arg2>"
```

条件表达式的语法为：

```makefile
<conditional-directive>
<text-if-true>
endif
```

以及：

```makefile
<conditional-directive>
<text-if-true>
else
<text-if-false>
endif
```

例如：

```makefile
x = 10
y = 20

# 条件判断
ifeq ($(x), $(y))
Echo1:
	@echo equal
else
Echo1:
	@echo not equal
endif
```

在上面的例子中，`ifeq` 是一个条件判断语句，它会比较变量 `x` 和 `y` 的值是否相等。如果相等，则执行 `Echo1` 目标的命令；否则执行 `else` 分支的命令。相关代码在**Code**文件夹下的**8_Makefile_Condition**文件夹中。运行命令：

```bash
make Echo1 -f Makefile_1 
```

便可以打印出

```bash
not equal
```

和`ifeq`对应的是`ifneq`。其语法格式是：

```makefile
ifneq (<arg1>, <arg2>)
ifneq '<arg1>' '<arg2>'
ifneq "<arg1>" "<arg2>"
ifneq "<arg1>" '<arg2>'
ifneq '<arg1>' "<arg2>"
```
# 2. `ifdef`和`ifndef`

另一个条件关键字是`ifdef`，其语法格式是：

```makefile
ifdef <variable-name>
```

如果变量`<variable-name>`的值非空，那到表达式为真。否则，表达式为假。当然，`<variable-name>`同样可以是一个函数的返回值。注意，`ifdef` 只是测试一个变量是否有值，其并不会把变量扩展到当前位置。给出示例：

```makefile
# 写法1
bar1 =
foo1 = $(bar1)
ifdef foo1
frobozz1 = yes
else
frobozz1 = no
endif

# 写法2
foo2 =
ifdef foo2
frobozz2 = yes
else
frobozz2 = no
endif

Echo2:
	@echo $(frobozz1) $(frobozz2)
```

在上面的例子中，`ifdef` 是一个条件判断语句，它会比较变量 `foo1` 和 `foo2` 的值是否为空。如果为空，则执行 `else` 分支的命令。相关代码在**Code**文件夹下的**8_Makefile_Condition**文件夹中。运行命令：

```bash
make Echo2 -f Makefile_1
```

便可以打印出

```bash
yes no
```

和`ifdef`对应的是`ifndef`。其语法格式是：

```makefile
ifndef <variable-name>
```


在`<conditional-directive>`这一行上，多余的空格是被允许的，但是不能以`[Tab]`键做为开始（不然就被认为是命令）。而注释符“#”同样也是安全的。`else`和`endif`也一样，只要不是以`[Tab]`键开始就行了。

特别注意的是，make 是在读取 Makefile 时就计算条件表达式的值，并根据条件表达式的值来选择语句，所以，你最好不要把自动化变量（如`$@`等）放入条件表达式中，因为自动化变量是在运行时才有的。

而且，为了避免混乱，make 不允许把整个条件语句分成两部分放在不同的文件中。


