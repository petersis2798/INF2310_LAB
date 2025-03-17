# LEVIATHAN - LAB_3

Leviathan Level 0
~~~
ssh leviathan0@leviathan.labs.overthewire.org -p 2223
Username: leviathan0
Password: leviathan0
~~~

Leviathan Level 0 → Level 1
~~~
ssh leviathan0@leviathan.labs.overthewire.org -p 2223
ls -la
cd .backup/
ls -a
head bookmarks.html
cat bookmarks.html | grep leviathan

3QJ3TgzHDq
~~~

Leviathan Level 1 → Level 2
~~~
ssh leviathan1@leviathan.labs.overthewire.org -p 2223
ls -la
./check
strings check
ltrace ./check 

strcmp("tes", "sex")

./check
sex
whoami
cat /etc/leviathan_pass/leviathan2

NsN1HwFoyN
~~~

Leviathan Level 2 → Level 3
~~~
ssh leviathan2@leviathan.labs.overthewire.org -p 2223

ls -la
./printfile /etc/leviathan_pass/leviathan3
./printfile .bash_logout
ltrace ./printfile .bash_logout
ltrace ./printfile .bash_logout .profile 

mktemp -d
touch /tmp/tmp.gTflTrPsyR/"test file.txt"
ls -la /tmp/tmp.gTflTrPsyR
ln -s /etc/leviathan_pass/leviathan3 /tmp/tmp.gTflTrPsyR/test
ls -la /tmp/tmp.gTflTrPsyR
chmod 777 /tmp/tmp.gTflTrPsyR
./printfile /tmp/tmp.gTflTrPsyR/"test file.txt"

f0n8h2iWLP
~~~

Leviathan Level 3 → Level 4
~~~
ssh leviathan3@leviathan.labs.overthewire.org -p 2223

ls -la
ltrace ./level3 

strcmp("test\n", "snlprintf\n")

./level3 
snlprintf
whoami
cat /etc/leviathan_pass/leviathan4

WG1egElCvO
~~~

Leviathan Level 4 → Level 5
~~~
ssh leviathan4@leviathan.labs.overthewire.org -p 2223

ls -la
cd .trash/
ls -la
./bin 
echo 0011000001100100011110010111100001010100001101110100011000110100010100010100010000001010 | perl -lpe '$_=pack"B*",$_'

0dyxT7F4QD
~~~

Leviathan Level 5 → Level 6
~~~
ssh leviathan5@leviathan.labs.overthewire.org -p 2223

ls -la
./leviathan5 
ltrace ./leviathan5 
touch /tmp/file.log
./leviathan5
ln -s /etc/leviathan_pass/leviathan6 /tmp/file.log
./leviathan5 

szo7HDB88w
~~~

Leviathan Level 6 → Level 7
~~~
ssh leviathan6@leviathan.labs.overthewire.org -p 2223
ls -la
./leviathan6 
./leviathan6 0000

gdb --args leviathan6 0000
disassemble main

break *0x0804921a
run
info registers
%eax,-0xc(%ebp)
print $ebp-0xc
x 0xffffd36c
print/d 0x00001bd3

$3 = 7123

./leviathan6 7123
whoami
cat /etc/leviathan_pass/leviathan7

qEs5Io5yM8
~~~


