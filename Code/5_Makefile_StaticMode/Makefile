SHELL = cmd.exe
objects = main.o func.o

final : $(objects)
	gcc -o final $(objects)
	
# 写法1
main.o : main.c
	gcc -c main.c
func.o : func.c
	gcc -c func.c

# 写法2
# ${objects} : %.o: %.c
# 	gcc -c ${CFLAGS} $< -o $@
.PHONY : clean
clean :
	-del /F /Q final.exe *.o
