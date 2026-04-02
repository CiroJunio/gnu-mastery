app: main.o auth.o
	gcc main.o auth.o -o app

main.o: main.c
	gcc -c main.c

auth.o: auth.c # auth.h -> It retains the base value, but if changed, nothing happens in auth.h unless it's declared here.
	gcc -c auth.c

clean:
	rm -f *.o app


