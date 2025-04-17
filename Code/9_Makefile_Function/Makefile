SHELL = cmd.exe
export LC_ALL := C.UTF - 8
export LANG := C.UTF - 8

# 定义一些变量用于函数测试
comma := ,
empty :=
space := $(empty) $(empty)
text := a b c
words := foo bar baz
file_names := src/foo.c src-1.0/bar.c hacks
names := a b c d
condition := true
expression = $(1) $(2)
variable := names
dir := test_dir
# 字符串替换函数subst示例
Echo_subst:
	@echo "subst函数示例:"
	@echo "原字符串: $(text)"
	@echo "替换后: $(subst $(space),$(comma),$(text))"
# 模式字符串替换函数patsubst示例
Echo_patsubst:
	@echo "patsubst函数示例:"
	@echo "原字符串: $(text).c"
	@echo "替换后: $(patsubst %.c,%.o,$(text).c)"
# 去空格函数strip示例
Echo_strip:
	@echo "strip函数示例:"
	@echo "原字符串: $(space)$(text)$(space)"
	@echo "处理后: $(strip $(space)$(text)$(space))"
# 查找字符串函数findstring示例
Echo_findstring:
	@echo "findstring函数示例:"
	@echo "在$(words)中查找foo: $(findstring foo,$(words))"
	@echo "在$(words)中查找xyz: $(findstring xyz,$(words))"
# 过滤函数filter示例
Echo_filter:
	@echo "filter函数示例:"
	@echo "原字符串: $(words)"
	@echo "过滤后: $(filter f%,$(words))"
# 反过滤函数filter-out示例
Echo_filter-out:
	@echo "filter-out函数示例:"
	@echo "原字符串: $(words)"
	@echo "反过滤后: $(filter-out f%,$(words))"
# 排序函数sort示例
Echo_sort:
	@echo "sort函数示例:"
	@echo "原字符串: $(words)"
	@echo "排序后: $(sort $(words))"
# 取单词函数word示例
Echo_word:
	@echo "word函数示例:"
	@echo "原字符串: $(words)"
	@echo "第2个单词: $(word 2,$(words))"
# 取单词串函数wordlist示例
Echo_wordlist:
	@echo "wordlist函数示例:"
	@echo "原字符串: $(words)"
	@echo "从第2个到第3个单词: $(wordlist 2, 3,$(words))"
# 单词个数统计函数words示例
Echo_words:
	@echo "words函数示例:"
	@echo "原字符串: $(words)"
	@echo "单词个数: $(words $(words))"
# 首单词函数firstword示例
Echo_firstword:
	@echo "firstword函数示例:"
	@echo "原字符串: $(words)"
	@echo "首单词: $(firstword $(words))"
# 取目录函数dir示例
Echo_dir:
	@echo "dir函数示例:"
	@echo "文件名序列: $(file_names)"
	@echo "目录部分: $(dir $(file_names))"
# 取文件函数notdir示例
Echo_notdir:
	@echo "notdir函数示例:"
	@echo "文件名序列: $(file_names)"
	@echo "非目录部分: $(notdir $(file_names))"
# 取后缀函数suffix示例
Echo_suffix:
	@echo "suffix函数示例:"
	@echo "文件名序列: $(file_names)"
	@echo "后缀部分: $(suffix $(file_names))"
# 取前缀函数basename示例
Echo_basename:
	@echo "basename函数示例:"
	@echo "文件名序列: $(file_names)"
	@echo "前缀部分: $(basename $(file_names))"
# 加后缀函数addsuffix示例
Echo_addsuffix:
	@echo "addsuffix函数示例:"
	@echo "原单词: $(words)"
	@echo "加后缀后: $(addsuffix .txt,$(words))"
# 加前缀函数addprefix示例
Echo_addprefix:
	@echo "addprefix函数示例:"
	@echo "原单词: $(words)"
	@echo "加前缀后: $(addprefix pre_,$(words))"
# 连接函数join示例
Echo_join:
	@echo "join函数示例:"
	@echo "列表1: $(words)"
	@echo "列表2: 1 2 3"
	@echo "连接后: $(join $(words), 1 2 3)"
# foreach函数示例
Echo_foreach:
	@echo "foreach函数示例:"
	@echo "原列表: $(names)"
	@echo "处理后: $(foreach n,$(names),$(n).txt)"
# if函数示例
Echo_if:
	@echo "if函数示例:"
	@echo "条件为$(condition)时的结果: $(if $(condition),满足条件,不满足条件)"
# call函数示例
Echo_call:
	@echo "call函数示例:"
	@echo "调用结果: $(call expression,a,b)"
# origin函数示例
Echo_origin:
	@echo "origin函数示例:"
	@echo "$(variable)变量来源: $(origin $(variable))"
# shell函数示例
Echo_shell:
	@echo "shell函数示例:"
	@echo "当前目录文件: $(shell dir /b)"
# error函数示例
Echo_error:
	@echo "error函数示例:"
	$(error This is an error triggered by the error function.)
Echo_warning:
	@echo "warning函数示例:"
	$(warning This is an warning triggered by the warning function.)