# 1. `subst`函数

`subst` 函数用于进行字符串替换，它会将字符串中指定的旧字符串替换为新字符串。其语法格式是：

```makefile
$(subst <from>,<to>,<text>)
```

其中，`<from>` 是要被替换的旧字符串，`<to>` 是用来替换的新字符串，`<text>` 是要进行替换操作的原始字符串。

例如：

```makefile
# 定义一些变量用于函数测试
comma := ,
empty :=
space := $(empty) $(empty)
text := a b c

# 字符串替换函数subst示例
Echo_subst:
    @echo "subst函数示例："
    @echo "原字符串: $(text)"
    @echo "替换后: $(subst $(space),$(comma),$(text))"
```

在上面的例子中，`subst` 函数将 `text` 变量中的空格替换为逗号。运行命令：

```bash
make Echo_subst
```

便可以打印出：

```bash
subst函数示例：
原字符串: a b c
替换后: a,b,c
```

# 2. `patsubst`函数

`patsubst` 函数是模式字符串替换函数，它会根据指定的模式进行字符串替换。其语法格式是：

```makefile
$(patsubst <pattern>,<replacement>,<text>)
```

其中，`<pattern>` 是要匹配的模式，`<replacement>` 是匹配成功后要替换成的字符串，`<text>` 是要进行替换操作的原始字符串。

例如：

```makefile
text := a b c

# 模式字符串替换函数patsubst示例
Echo_patsubst:
    @echo "patsubst函数示例："
    @echo "原字符串: $(text).c"
    @echo "替换后: $(patsubst %.c,%.o,$(text).c)"
```

在上面的例子中，`patsubst` 函数将 `text` 变量后面加上 `.c` 后的字符串中的 `.c` 替换为 `.o`。运行命令：

```bash
make Echo_patsubst
```

便可以打印出：

```bash
patsubst函数示例：
原字符串: a b c.c
替换后: a b c.o
```

# 3. `strip`函数

`strip` 函数用于去除字符串首尾的空格。其语法格式是：

```makefile
$(strip <string>)
```

其中，`<string>` 是要进行处理的字符串。

例如：

```makefile
empty :=
space := $(empty) $(empty)
text := a b c

# 去空格函数strip示例
Echo_strip:
    @echo "strip函数示例："
    @echo "原字符串: $(space)$(text)$(space)"
    @echo "处理后: $(strip $(space)$(text)$(space))"
```

在上面的例子中，`strip` 函数去除了 `text` 变量首尾的空格。运行命令：

```bash
make Echo_strip
```

便可以打印出：

```bash
strip函数示例：
原字符串:  a b c 
处理后: a b c
```

# 4. `findstring`函数

`findstring` 函数用于在字符串中查找指定的子字符串。如果找到，则返回该子字符串；否则返回空字符串。其语法格式是：

```makefile
$(findstring <find>,<in>)
```

其中，`<find>` 是要查找的子字符串，`<in>` 是要在其中查找的字符串。

例如：

```makefile
words := foo bar baz

# 查找字符串函数findstring示例
Echo_findstring:
    @echo "findstring函数示例："
    @echo "在$(words)中查找foo: $(findstring foo,$(words))"
    @echo "在$(words)中查找xyz: $(findstring xyz,$(words))"
```

在上面的例子中，`findstring` 函数在 `words` 变量中查找 `foo` 和 `xyz`。运行命令：

```bash
make Echo_findstring
```

便可以打印出：

```bash
findstring函数示例：
在foo bar baz中查找foo: foo
在foo bar baz中查找xyz: 
```

# 5. `filter`函数

`filter` 函数用于过滤字符串，只保留符合指定模式的单词。其语法格式是：

```makefile
$(filter <pattern...>,<text>)
```

其中，`<pattern...>` 是一个或多个模式，`<text>` 是要进行过滤的字符串。

例如：

```makefile
words := foo bar baz

# 过滤函数filter示例
Echo_filter:
    @echo "filter函数示例："
    @echo "原字符串: $(words)"
    @echo "过滤后: $(filter f%,$(words))"
```

在上面的例子中，`filter` 函数只保留了 `words` 变量中以 `f` 开头的单词。运行命令：

```bash
make Echo_filter
```

便可以打印出：

```bash
filter函数示例：
原字符串: foo bar baz
过滤后: foo
```

# 6. `filter-out`函数

`filter-out` 函数与 `filter` 函数相反，它会过滤掉符合指定模式的单词，只保留不符合模式的单词。其语法格式是：

```makefile
$(filter-out <pattern...>,<text>)
```

其中，`<pattern...>` 是一个或多个模式，`<text>` 是要进行过滤的字符串。

例如：

```makefile
words := foo bar baz

# 反过滤函数filter-out示例
Echo_filter-out:
    @echo "filter-out函数示例："
    @echo "原字符串: $(words)"
    @echo "反过滤后: $(filter-out f%,$(words))"
```

在上面的例子中，`filter-out` 函数过滤掉了 `words` 变量中以 `f` 开头的单词。运行命令：

```bash
make Echo_filter-out
```

便可以打印出：

```bash
filter-out函数示例：
原字符串: foo bar baz
反过滤后: bar baz
```

# 7. `sort`函数

`sort` 函数用于对字符串中的单词进行排序，排序结果会去除重复的单词。其语法格式是：

```makefile
$(sort <list>)
```

其中，`<list>` 是要进行排序的字符串。

例如：

```makefile
words := foo bar baz

# 排序函数sort示例
Echo_sort:
    @echo "sort函数示例："
    @echo "原字符串: $(words)"
    @echo "排序后: $(sort $(words))"
```

在上面的例子中，`sort` 函数对 `words` 变量中的单词进行了排序。运行命令：

```bash
make Echo_sort
```

便可以打印出：

```bash
sort函数示例：
原字符串: foo bar baz
排序后: bar baz foo
```

# 8. `word`函数

`word` 函数用于从字符串中取出指定位置的单词。其语法格式是：

```makefile
$(word <n>,<text>)
```

其中，`<n>` 是要取出的单词的位置（从 1 开始计数），`<text>` 是要从中取单词的字符串。

例如：

```makefile
words := foo bar baz

# 取单词函数word示例
Echo_word:
    @echo "word函数示例："
    @echo "原字符串: $(words)"
    @echo "第2个单词: $(word 2,$(words))"
```

在上面的例子中，`word` 函数从 `words` 变量中取出了第 2 个单词。运行命令：

```bash
make Echo_word
```

便可以打印出：

```bash
word函数示例：
原字符串: foo bar baz
第2个单词: bar
```

# 9. `wordlist`函数

`wordlist` 函数用于从字符串中取出指定范围的单词。其语法格式是：

```makefile
$(wordlist <start>,<end>,<text>)
```

其中，`<start>` 是起始位置（从 1 开始计数），`<end>` 是结束位置，`<text>` 是要从中取单词的字符串。

例如：

```makefile
words := foo bar baz

# 取单词串函数wordlist示例
Echo_wordlist:
    @echo "wordlist函数示例："
    @echo "原字符串: $(words)"
    @echo "从第2个到第3个单词: $(wordlist 2, 3,$(words))"
```

在上面的例子中，`wordlist` 函数从 `words` 变量中取出了第 2 个到第 3 个单词。运行命令：

```bash
make Echo_wordlist
```

便可以打印出：

```bash
wordlist函数示例：
原字符串: foo bar baz
从第2个到第3个单词: bar baz
```

# 10. `words`函数

`words` 函数用于统计字符串中单词的个数。其语法格式是：

```makefile
$(words <text>)
```

其中，`<text>` 是要统计单词个数的字符串。

例如：

```makefile
words := foo bar baz

# 单词个数统计函数words示例
Echo_words:
    @echo "words函数示例："
    @echo "原字符串: $(words)"
    @echo "单词个数: $(words $(words))"
```

在上面的例子中，`words` 函数统计了 `words` 变量中单词的个数。运行命令：

```bash
make Echo_words
```

便可以打印出：

```bash
words函数示例：
原字符串: foo bar baz
单词个数: 3
```

# 11. `firstword`函数

`firstword` 函数用于取出字符串中的第一个单词。其语法格式是：

```makefile
$(firstword <text>)
```

其中，`<text>` 是要从中取第一个单词的字符串。

例如：

```makefile
words := foo bar baz

# 首单词函数firstword示例
Echo_firstword:
    @echo "firstword函数示例："
    @echo "原字符串: $(words)"
    @echo "首单词: $(firstword $(words))"
```

在上面的例子中，`firstword` 函数从 `words` 变量中取出了第一个单词。运行命令：

```bash
make Echo_firstword
```

便可以打印出：

```bash
firstword函数示例：
原字符串: foo bar baz
首单词: foo
```

# 12. `dir`函数

`dir` 函数用于从文件名序列中提取目录部分。其语法格式是：

```makefile
$(dir <names...>)
```

其中，`<names...>` 是一个或多个文件名。

例如：

```makefile
file_names := src/foo.c src-1.0/bar.c hacks

# 取目录函数dir示例
Echo_dir:
    @echo "dir函数示例："
    @echo "文件名序列: $(file_names)"
    @echo "目录部分: $(dir $(file_names))"
```

在上面的例子中，`dir` 函数从 `file_names` 变量中提取了目录部分。运行命令：

```bash
make Echo_dir
```

便可以打印出：

```bash
dir函数示例：
文件名序列: src/foo.c src-1.0/bar.c hacks
目录部分: src/ src-1.0/ 
```

# 13. `notdir`函数

`notdir` 函数用于从文件名序列中提取非目录部分（即文件名）。其语法格式是：

```makefile
$(notdir <names...>)
```

其中，`<names...>` 是一个或多个文件名。

例如：

```makefile
file_names := src/foo.c src-1.0/bar.c hacks

# 取文件函数notdir示例
Echo_notdir:
    @echo "notdir函数示例："
    @echo "文件名序列: $(file_names)"
    @echo "非目录部分: $(notdir $(file_names))"
```

在上面的例子中，`notdir` 函数从 `file_names` 变量中提取了非目录部分。运行命令：

```bash
make Echo_notdir
```

便可以打印出：

```bash
notdir函数示例：
文件名序列: src/foo.c src-1.0/bar.c hacks
非目录部分: foo.c bar.c hacks
```

# 14. `suffix`函数

`suffix` 函数用于从文件名序列中提取文件的后缀部分。其语法格式是：

```makefile
$(suffix <names...>)
```

其中，`<names...>` 是一个或多个文件名。

例如：

```makefile
file_names := src/foo.c src-1.0/bar.c hacks

# 取后缀函数suffix示例
Echo_suffix:
    @echo "suffix函数示例："
    @echo "文件名序列: $(file_names)"
    @echo "后缀部分: $(suffix $(file_names))"
```

在上面的例子中，`suffix` 函数从 `file_names` 变量中提取了文件的后缀部分。运行命令：

```bash
make Echo_suffix
```

便可以打印出：

```bash
suffix函数示例：
文件名序列: src/foo.c src-1.0/bar.c hacks
后缀部分: .c .c 
```

# 15. `basename`函数

`basename` 函数用于从文件名序列中提取文件的前缀部分（即去掉后缀的文件名）。其语法格式是：

```makefile
$(basename <names...>)
```

其中，`<names...>` 是一个或多个文件名。

例如：

```makefile
file_names := src/foo.c src-1.0/bar.c hacks

# 取前缀函数basename示例
Echo_basename:
    @echo "basename函数示例："
    @echo "文件名序列: $(file_names)"
    @echo "前缀部分: $(basename $(file_names))"
```

在上面的例子中，`basename` 函数从 `file_names` 变量中提取了文件的前缀部分。运行命令：

```bash
make Echo_basename
```

便可以打印出：

```bash
basename函数示例：
文件名序列: src/foo.c src-1.0/bar.c hacks
前缀部分: src/foo src-1.0/bar hacks
```

# 16. `addsuffix`函数

`addsuffix` 函数用于给字符串中的每个单词添加后缀。其语法格式是：

```makefile
$(addsuffix <suffix>,<names...>)
```

其中，`<suffix>` 是要添加的后缀，`<names...>` 是一个或多个单词。

例如：

```makefile
words := foo bar baz

# 加后缀函数addsuffix示例
Echo_addsuffix:
    @echo "addsuffix函数示例："
    @echo "原单词: $(words)"
    @echo "加后缀后: $(addsuffix .txt,$(words))"
```

在上面的例子中，`addsuffix` 函数给 `words` 变量中的每个单词添加了 `.txt` 后缀。运行命令：

```bash
make Echo_addsuffix
```

便可以打印出：

```bash
addsuffix函数示例：
原单词: foo bar baz
加后缀后: foo.txt bar.txt baz.txt
```

# 17. `addprefix`函数

`addprefix` 函数用于给字符串中的每个单词添加前缀。其语法格式是：

```makefile
$(addprefix <prefix>,<names...>)
```

其中，`<prefix>` 是要添加的前缀，`<names...>` 是一个或多个单词。

例如：

```makefile
words := foo bar baz

# 加前缀函数addprefix示例
Echo_addprefix:
    @echo "addprefix函数示例："
    @echo "原单词: $(words)"
    @echo "加前缀后: $(addprefix pre_,$(words))"
```

在上面的例子中，`addprefix` 函数给 `words` 变量中的每个单词添加了 `pre_` 前缀。运行命令：

```bash
make Echo_addprefix
```

便可以打印出：

```bash
addprefix函数示例：
原单词: foo bar baz
加前缀后: pre_foo pre_bar pre_baz
```

# 18. `join`函数

`join` 函数用于将两个字符串列表连接起来。其语法格式是：

```makefile
$(join <list1>,<list2>)
```

其中，`<list1>` 和 `<list2>` 是两个字符串列表。

例如：

```makefile
words := foo bar baz

# 连接函数join示例
Echo_join:
    @echo "join函数示例："
    @echo "列表1: $(words)"
    @echo "列表2: 1 2 3"
    @echo "连接后: $(join $(words), 1 2 3)"
```

在上面的例子中，`join` 函数将 `words` 变量和 `1 2 3` 连接起来。运行命令：

```bash
make Echo_join
```

便可以打印出：

```bash
join函数示例：
列表1: foo bar baz
列表2: 1 2 3
连接后: foo1 bar2 baz3
```

# 19. `foreach`函数

`foreach` 函数用于对列表中的每个单词执行相同的操作。其语法格式是：

```makefile
$(foreach <var>,<list>,<text>)
```

其中，`<var>` 是循环变量，`<list>` 是要遍历的列表，`<text>` 是要对每个单词执行的操作。

例如：

```makefile
names := a b c d

# foreach函数示例
Echo_foreach:
    @echo "foreach函数示例："
    @echo "原列表: $(names)"
    @echo "处理后: $(foreach n,$(names),$(n).txt)"
```

在上面的例子中，`foreach` 函数对 `names` 变量中的每个单词添加了 `.txt` 后缀。运行命令：

```bash
make Echo_foreach
```

便可以打印出：

```bash
foreach函数示例：
原列表: a b c d
处理后: a.txt b.txt c.txt d.txt
```

# 20. `if`函数

`if` 函数用于进行条件判断。如果条件为真，则返回第一个参数；否则返回第二个参数。其语法格式是：

```makefile
$(if <condition>,<then-part>,<else-part>)
```

其中，`<condition>` 是条件表达式，`<then-part>` 是条件为真时返回的内容，`<else-part>` 是条件为假时返回的内容（可选）。

例如：

```makefile
condition := true

# if函数示例
Echo_if:
    @echo "if函数示例："
    @echo "条件为$(condition)时的结果: $(if $(condition),满足条件,不满足条件)"
```

在上面的例子中，`if` 函数根据 `condition` 变量的值进行判断。运行命令：

```bash
make Echo_if
```

便可以打印出：

```bash
if函数示例：
条件为true时的结果: 满足条件
```

# 21. `call`函数

`call` 函数用于调用一个定义好的表达式，并传递参数。其语法格式是：

```makefile
$(call <expression>,<param1>,<param2>,...)
```

其中，`<expression>` 是要调用的表达式，`<param1>,<param2>,...` 是传递给表达式的参数。

例如：

```makefile
expression = $(1) $(2)

# call函数示例
Echo_call:
    @echo "call函数示例："
    @echo "调用结果: $(call $(expression),a,b)"
```

在上面的例子中，`call` 函数调用了 `expression` 表达式，并传递了 `a` 和 `b` 作为参数。运行命令：

```bash
make Echo_call
```

便可以打印出：

```bash
call函数示例：
调用结果: a b
```

# 22. `origin`函数

`origin` 函数用于返回变量的来源。其语法格式是：

```makefile
$(origin <variable>)
```

其中，`<variable>` 是要查询来源的变量。

例如：

```makefile
names := a b c d
variable := names

# origin函数示例
Echo_origin:
    @echo "origin函数示例："
    @echo "$(variable)变量来源: $(origin $(variable))"
```

在上面的例子中，`origin` 函数查询了 `variable` 变量的来源。运行命令：

```bash
make Echo_origin
```

便可以打印出：

```bash
origin函数示例：
test_variable变量来源: file
```

# 23. `shell`函数

`shell` 函数用于执行 shell 命令，并返回命令的输出结果。其语法格式是：

```makefile
$(shell <command>)
```

其中，`<command>` 是要执行的 shell 命令。

例如：

```makefile
# shell函数示例
Echo_shell:
    @echo "shell函数示例："
	@echo "当前目录文件: $(shell dir /b)"
```

在上面的例子中，`shell` 函数执行了 `ls` 命令，并返回了当前目录的文件列表。运行命令：

```bash
make Echo_shell
```

便可以打印出当前目录的文件列表。

```bash
"shell函数示例:"
"当前目录文件: func.c func.h main.c Makefile"
```

# 24. `error`函数

`error` 函数用于在 Makefile 中触发一个错误，并终止 make 的执行。其语法格式是：

```makefile
$(error <text>)
```

其中，`<text>` 是错误信息。

例如：

```makefile
# error函数示例
Echo_error:
    @echo "error函数示例："
	$(error This is an error triggered by the error function.)
```

在上面的例子中，当执行 `make Echo_error` 时，`error` 函数会触发一个错误，并终止 make 的执行。

```bash
makefile:131: *** This is an error triggered by the error function..  Stop.
```

# 25. `warning`函数

`warning` 函数用于在 Makefile 中发出一个警告信息，但不会终止 make 的执行。其语法格式是：

```makefile
$(warning <text>)
```

其中，`<text>` 是警告信息。

例如：

```makefile
# warning函数示例
Echo_warning:
    @echo "warning函数示例："
	$(warning This is an warning triggered by the warning function.)
```

在上面的例子中，当执行 `make Echo_warning` 时，`warning` 函数会发出一个警告信息，但 make 会继续执行。

```bash
makefile:134: This is an warning triggered by the warning function.
"warning函数示例:
```

