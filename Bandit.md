# BANDIT - LAB_1

Bandit Level 0
~~~
ssh bandit0@bandit.labs.overthewire.org -p 2220
~~~

Bandit Level 0 → Level 1

~~~
ls
cat readme

ZjLjTmM6FvvyRnrb2rfNWOZOTa6ip5If
~~~

Bandit Level 1 → Level 2

~~~
ls -a
cat ./-

263JGJPfgU6LtdEvgfWU1XP5yac29mFx
~~~

Bandit Level 2 → Level 3

~~~
ls
cat spaces\ in\ this\ filename

MNk8KNH3Usiio41PRUEoDFPqfxLPlSmx
~~~

Bandit Level 3 → Level 4

~~~
ls
cd inhere
ls -a
cat .hidden

2WmrDFRmJIq3IPxneAaMGhap0pFhF3NJ
~~~

Bandit Level 4 → Level 5

~~~
ls -a
cd inhere
ls -a
file ./-*
cat ./-file07

4oQYVPkxZOOEOO5pTW81FB8j8lxXGUQw
~~~

Bandit Level 5 → Level 6

~~~
find / -user bandit7 -group bandit6 -size 33c 2>/dev/null
cat /var/lib/dpkg/info/bandit7.password

morbNTDkSW6jIlUc0ymOdMaLnOlFVAaj
~~~

Bandit Level 6 → Level 7
~~~
find / -user bandit7 -group bandit6 -size 33c 2>/dev/null
cat /var/lib/dpkg/info/bandit7.password

morbNTDkSW6jIlUc0ymOdMaLnOlFVAaj
~~~

Bandit Level 7 → Level 8
~~~
ls -a
awk '/^millionth/ {print $2;}' data.txt

dfwvzFQi4mU0wfNbFOe9RoWskMLg7eEc
~~~

Bandit Level 8 → Level 9
~~~
ls
cat data.txt | sort | uniq -u

4CKMh1JI91bUIZZPXDqGanal4xvAg0JM
~~~

Bandit Level 9 → Level 10
~~~
ls -a
strings data.txt | grep "="

FGUW5ilLVJrxX9kMYMmlN4MgbpfMiqey
~~~

Bandit Level 10 → Level 11
~~~
ls -a
echo VGhlIHBhc3N3b3JkIGlzIGR0UjE3M2ZaS2IwUlJzREZTR3NnMlJXbnBOVmozcVJyCg== | base64 --decode

dtR173fZKb0RRsDFSGsg2RWnpNVj3qRr
~~~

Bandit Level 11 → Level 12
~~~
ls -a
cat data.txt
echo Gur cnffjbeq vf 7k16JArUVv5LxVuJfsSVdbbtaHGlw9D4 | tr [a-zA-Z] [n-za-mN-ZA-M]

7x16WNeHIi5YkIhWsfFIqoognUTyj9Q4
~~~

Bandit Level 12 → Level 13
~~~
ls -a
mkdir /tmp/peter
xxd -r data.txt > /tmp/peter/file.bin
cd /tmp/peter
ls -a
file file.bin
zcat file.bin | file -
zcat file.bin | bzcat | file -
zcat file.bin | bzcat | zcat | file -
zcat file.bin | bzcat | zcat | tar xO | file -
zcat file.bin | bzcat | zcat | tar xO | tar xO | file -
zcat file.bin | bzcat | zcat | tar xO | tar xO | bzcat | file -
zcat file.bin | bzcat | zcat | tar xO | tar xO | bzcat | tar xO | file -
zcat file.bin | bzcat | zcat | tar xO | tar xO | bzcat | tar xO | zcat | file -
zcat file.bin | bzcat | zcat | tar xO | tar xO | bzcat | tar xO | zcat

FO5dwFsc0cbaIiH0h8J2eUks2vdTDwAn
~~~

Bandit Level 13 → Level 14
~~~
ls -a
ssh -i sshkey.private bandit14@localhost -p 2220
cat /etc/bandit_pass/bandit14

MU4VWeTyJk8ROof1qqmcBPaLh7lDCPvS
~~~

Bandit Level 14 → Level 15
~~~
telnet localhost 30000
MU4VWeTyJk8ROof1qqmcBPaLh7lDCPvS

8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo
~~~

Bandit Level 15 → Level 16
~~~
openssl s_client -ign_eof -connect localhost:30001
8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo

kSkvUpMQ7lBYyCM4GBPvCvT1BfWRy0Dx
~~~

Bandit Level 16 → Level 17
~~~
nmap -sV localhost -p 31000-32000
echo kSkvUpMQ7lBYyCM4GBPvCvT1BfWRy0Dx | openssl s_client -quiet -connect localhost:31790
~~~
Copiando Key
~~~
nano keylevel17.private
chmod 600 keylevel17.private
ssh -i keylevel17.private bandit17@bandit.labs.overthewire.org -p 2220
cat /etc/bandit_pass/bandit17

EReVavePLFHtFlFsjn3hyzMlvSuSAcRD
~~~

Bandit Level 17 → Level 18
~~~
diff passwords.new passwords.old

x2gLTTjFwMOhQ8oWNbMN362QKxfRqGlO
~~~

Bandit Level 18 → Level 19
~~~
ssh bandit18@bandit.labs.overthewire.org -p 2220 'ls -al'
ssh bandit18@bandit.labs.overthewire.org -p 2220 'cat readme'

cGWpMaKXVwDUNgPAVJbWYuGHVn9zl3j8
~~~

Bandit Level 19 → Level 20
~~~
ls -al
./bandit20-do
./bandit20-do id
./bandit20-do ls /etc/bandit_pass
./bandit20-do cat /etc/bandit_pass/bandit20
0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO
~~~

Bandit Level 20 → Level 21
~~~
echo -n '0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO' | nc -l -p 1234 &
./suconnect 1234

EeoULMCra2q0dSkYj561DX7s1CpBuOBt
~~~

Bandit Level 21 → Level 22
~~~
ls -la /etc/cron.d
cat /etc/cron.d/cronjob_bandit22
cat /usr/bin/cronjob_bandit22.sh
chmod 644 /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
cat /etc/bandit_pass/bandit22 > /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
cat /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv

tRae0UfB9v0UzbCdn9cY0gQnds9GF58Q
~~~

Bandit Level 22 → Level 23
~~~
ls -la /etc/cron.d
cat /etc/cron.d/cronjob_bandit23
cat /usr/bin/cronjob_bandit23.sh
echo I am user bandit23 | md5sum | cut -d ' ' -f 1
cat /tmp/8ca319486bfbbc3663ea0fbe81326349

0Zf11ioIjMVN551jX3CmStKLYqjk54Ga
~~~

Bandit Level 23 → Level 24
~~~
ls -la /etc/cron.d
cat /etc/cron.d/cronjob_bandit24
cat /usr/bin/cronjob_bandit24.sh
mktemp -d
chmod 777 /tmp/tmp.GyNkA7xX1k
ls -al /tmp/tmp.GyNkA7xX1k
ls -al /var/spool/bandit24
echo '#!/bin/bash' > /var/spool/bandit24/foo/GetPassword24.sh
echo 'cat /etc/bandit_pass/bandit24 > /tmp/tmp.GyNkA7xX1k/pass_24.txt' >> /var/spool/bandit24/foo/GetPassword24.sh
chmod 777 /var/spool/bandit24/foo/GetPassword24.sh
cat /tmp/tmp.GyNkA7xX1k/pass_24.txt

gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8
~~~

Bandit Level 24 → Level 25
~~~
echo abc | nc localhost 30002
(for i in $(seq -f “gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8 %04g” 0 9999); do echo $i; done) | nc localhost 30002
nc -lvnp 30002
(for i in $(seq -f "%04g" 0 9999); do printf "gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8 %04g\n" $i; done) | nc localhost 30002

iCi86ttT4KSNe1armKiwbQNmB3YJP3q4
~~~

Bandit Level 25 → Level 26
~~~
file bandit26.sshkey
ssh -i bandit26.sshkey bandit26@localhost
cat /etc/passwd
cat /etc/passwd | grep bandit26
cat /usr/bin/showtext
ssh -i bandit26.sshkey bandit26@localhost
cat /etc/bandit_pass/bandit26

s0773xxkk0MXfdqOfPRVr9L3jJBUOgCZ
~~~

Bandit Level 26 → Level 27
~~~
ls -la
file bandit27-do
cat text.txt
./bandit27-do id
./bandit27-do whoami
echo 'cat /etc/bandit_pass/bandit27' > /tmp/getpass27.sh
chmod a+x /tmp/getpass27.sh
./bandit27-do /tmp/getpass27.sh

upsNCc7vzaRDx6oZC6GiR6ERwe1MowGB
~~~

Bandit Level 27 → Level 28
~~~
mkdir /tmp/fcch-git
cd /tmp/fcch-git
git clone ssh://bandit27-git@localhost/home/bandit27-git/repo
cat repo/README

Yz9IpL0sBcCeuG7m9uQFt8ZNpS4HZRcN
~~~

Bandit Level 28 → Level 29
~~~
mkdir /tmp/fcch28git
cd /tmp/fcch28git
git clone ssh://bandit28-git@localhost/home/bandit28-git/repo
cd repo
ls -la 
ls -la .git
git log

4pT1t5DENaYuqnqvadYs1oE4QLCdjmJ7
~~~

Bandit Level 29 → Level 30
~~~
mkdir /tmp/fcch29git
cd /tmp/fcch29git
cat README.md
git branch
git branch -a
git checkout dev
git log 
git diff 33ce2e95d9c5d6fb0a40e5ee9a2926903646b4e3 a8af722fccd4206fc3780bd3ede35b2c03886d9b

qp30ex3VLz5MDG1n91YowTv4Q8l7CDZL
~~~

Bandit Level 30 → Level 31
~~~
git clone ssh://bandit30-git@localhost/home/bandit30-git/repo
cd repo
cat README.md
git branch -a
git show-branch --all
git log
cat .git/packed-refs
git show-ref --tags -d
git show --name-only secret

fb5S2xb7bRyFmAvQYQGEqsbhVyJqhnDy
~~~

Bandit Level 31 → Level 32
~~~
mkdir /tmp/fcch31git
cd /tmp/fcch31git
git clone ssh://bandit31-git@localhost/home/bandit31-git/repo
cd repo
ls -la
cat README.md
touch key.txt
vim key.txt
cat .gitignore
git add	-f key.txt
git commit -m 'add key'
git push origin master

3O9RfhqyAlVBEZpVb6LYStshZoqoSx5K
~~~

Bandit Level 32 → Level 33
~~~
ls
clear
pwd
ls -la 
file uppershell
cat uppershell
cat /etc/bandit_pass/bandit33

tQdtbs5D5i2vJwkO8mEyYEyTL8izoeJ0
~~~
