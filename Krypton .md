# Krypton - LAB_3

Krypton Level 0 → Level 1
~~~
ssh krypton1@krypton.labs.overthewire.org -p 2231
KRYPTONISGREAT

cd /krypton/
ls -la
cd krypton1
ls -a
cat README
alias rot13="tr 'A-Za-z' 'N-ZA-Mn-za-m'"
alias rot5="tr '0-9' '5-90-4'"
alias rot="tr 'A-Za-z0-9' 'N-ZA-Mn-za-m5-90-4'"
cat krypton2

ROTTEN
~~~

Krypton Level 1 → Level 2
~~~
ssh krypton2@krypton.labs.overthewire.org -p 2231

cd /krypton/krypton2/
mktemp -d
cd /tmp/tmp.pDUUogv4NJ
ln -s /krypton/krypton2/keyfile.dat
chmod 777 .
echo "AAAAA" > encrypt.txt
/krypton/krypton2/encrypt encrypt.txt 
ls -la
cat ciphertext

cd /krypton/krypton2/
cat krypton3 | tr 'A-Za-z' 'O-ZA-No-za-n'

CAESARISEASY
~~~

Krypton Level 2 → Level 3
~~~
ssh krypton3@krypton.labs.overthewire.org -p 2231

cd /krypton/krypton3
ls -la
for i in {A..Z}; do printf $i; cat found1 found2 found3 | tr -cd $i | wc -c; done
for i in {A..Z}; do cat found1 found2 found3 | tr -cd $i | wc -c | tr -d '\n'; printf " $i \n"; done | sort -nr
cat krypton4 | tr 'SQJUBNGCDZVWMYTXKELAFIORHP' 'ETAOINSRHDLUCMFYWGPBVKXQJZ'

BRUTE
~~~

Krypton Level 3 → Level 4
~~~
ssh krypton4@krypton.labs.overthewire.org -p 2231
cd /krypton/krypton4
ls -a
cat krypton5
cat found1
~~~
utilizando la pagina https://www.dcode.fr/vigenere-cipher
~~~
FREKEY
CLEARTEXT
~~~

Krypton Level 4 → Level 5
~~~
ssh krypton5@krypton.labs.overthewire.org -p 2231
cd /krypton/krypton5
ls -a
cat found1
~~~
utilizando la pagina https://www.dcode.fr/vigenere-cipher
~~~
KEYLENGTH
RANDOM
~~~

Krypton Level 5 → Level 6
~~~
ssh krypton6@krypton.labs.overthewire.org -p 2231
cd /krypton/krypton6
ls -a
cat README
cat HINT1
cd /tmp
touch test.txt
python3 -c "print('A'*50)" > test.txt
ln -s /krypton/krypton6/keyfile.dat 
/krypton/krypton6/encrypt6 test.txt output.txt
cat output.txt

python3 -c "print('B'*50)" > test2.txt
/krypton/krypton6/encrypt6 test2.txt output.txt
cat output.txt

LFSRISNOTRANDOM
~~~

