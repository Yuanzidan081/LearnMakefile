SHELL = cmd.exe
objects = main.o func.o
# objects := $(wildcard *.o)

final : $(objects)
	gcc -o final $(objects)
main.o : main.c func.h
	gcc -c main.c
func.o : func.c func.h
	gcc -c func.c
clean :
	del /F /Q final.exe main.o func.o