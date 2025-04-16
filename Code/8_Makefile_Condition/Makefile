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
