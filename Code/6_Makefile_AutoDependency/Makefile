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