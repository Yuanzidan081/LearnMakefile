SHELL = cmd.exe

final : main.o func.o
	gcc -o final main.o func.o
main.o : main.c func.h
	gcc -c main.c 
func.o : func.c func.h
	gcc -c func.c 
clean :
	del /F /Q final.exe main.o func.o
