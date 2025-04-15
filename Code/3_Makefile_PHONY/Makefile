SHELL = cmd.exe
objects = main.o func.o

final : $(objects)
	gcc -o final $(objects)
main.o : main.c func.h
	gcc -c main.c
func.o : func.c func.h
	gcc -c func.c

# 写法1
# .PHONY : clean
# clean :
# 	-del /F /Q final.exe $(objects)

# 写法2
.PHONY : clean cleanObj
clean : cleanObj
	-del /F /Q final.exe 
cleanObj :
	-del /F /Q *.o