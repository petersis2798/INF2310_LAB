# HACKERRANK
# BASH

## Let's Echo
![alt text](01.PNG)
~~~
echo "HELLO"
~~~

## Looping and Skipping
![alt text](02.PNG)
~~~
for ITERATOR in {1..99}; do
    if [[ $((ITERATOR % 2)) -eq 1 ]]; then
        echo $ITERATOR
    fi
done
~~~

## A Personalized Echo
![alt text](03.PNG)
~~~
read name
echo "Welcome $name"
~~~

## Looping with Numbers
![alt text](04.PNG)
~~~
for i in {1..50}
do 
   echo $i
done
~~~

## The World of Numbers
![alt text](05.PNG)
~~~
read X
read Y

echo "$(( $X + $Y ))"
echo "$(( $X - $Y ))"
echo "$(( $X * $Y ))"
echo "$(( $X / $Y ))"

~~~

## Comparing Numbers
![alt text](06.PNG)
~~~
read X
read Y

if (( $X < $Y )); then
    echo 'X is less than Y'
elif (( $X > $Y )); then
    echo 'X is greater than Y'
else
    echo 'X is equal to Y'
fi
~~~

## Getting started with conditionals
![alt text](07.PNG)
~~~
read char

if [[("$char" == 'y' || "$char" == 'Y')]]
then
echo "YES"
elif [[("$char" == 'n' || "$char" == 'N')]]
then
echo "NO"
fi
~~~

## More on Conditionals
![alt text](08.PNG)
~~~
read X; read Y; read Z;
if [ $X -eq $Y ] && [ $Y -eq $Z ]; then echo "EQUILATERAL";
elif [ $X -ne $Y ] && [ $X -ne $Z ] && [ $Y -ne $Z ]; then echo "SCALENE";
else echo "ISOSCELES"; fi
~~~

## Arithmetic Operations
![alt text](09.PNG)
~~~
read line;
printf "%.3f" $(echo "scale = 4; $line" | bc);
~~~

## Compute the Average
![alt text](10.PNG)
~~~
sum=0
read N

for i in $(seq 1 $N); do
    read number
    sum=$(( $sum + $number ))
done

printf "%.3f\n" `echo "$sum / $N" | bc -l`
~~~

## Functions and Fractals - Recursive Trees - Bash!
![alt text](11.PNG)
~~~
declare -A a

f() {
    local d=$1 l=$2 r=$3 c=$4
    [[ $d -eq 0 ]] && return
    for ((i=l; i; i--)); do
        a[$((r-i)).$c]=1
    done
    ((r -= l))
    for ((i=l; i; i--)); do
        a[$((r-i)).$((c-i))]=1
        a[$((r-i)).$((c+i))]=1
    done
    f $((d-1)) $((l/2)) $((r-l)) $((c-l))
    f $((d-1)) $((l/2)) $((r-l)) $((c+l))
}
read n
f $n 16 63 49
for ((i=0; i<63; i++)); do
    for ((j=0; j<100; j++)); do
        if [[ ${a[$i.$j]} ]]; then
            printf 1
        else
            printf _
        fi
    done
    echo
done
~~~

## Cut #1
![alt text](12.PNG)
~~~
cut -c 3
~~~

## Cut #2
![alt text](13.PNG)
~~~
cut -c 2,7
~~~

## Cut #3
![alt text](14.PNG)
~~~
cut -c 2-7
~~~

## Cut #4
![alt text](15.PNG)
~~~
cut -c -4
~~~

## Cut #5
![alt text](16.PNG)
~~~
cut -f 1-3
~~~

## Cut #6
![alt text](17.PNG)
~~~
cut -c 13-
~~~

## Cut #7
![alt text](18.PNG)
~~~
cut -d " " -f 4
~~~

## Cut #8
![alt text](19.PNG)
~~~
cut -d " " -f 1-3
~~~

## Cut #9
![alt text](20.PNG)
~~~
cut -f 2-
~~~

## Head of a Text File #1
![alt text](21.PNG)
~~~
head -20
~~~


## Head of a Text File #2
![alt text](22.PNG)
~~~
head -c 20
~~~

## Middle of a Text File
![alt text](23.PNG)
~~~
head -22 | tail -11
~~~

## Tail of a Text File #1
![alt text](24.PNG)
~~~
tail -20
~~~

## Tail of a Text File #2
![alt text](25.PNG)
~~~
tail -c 20
~~~

## 'Tr' Command #1
![alt text](26.PNG)
~~~
tr "()" "[]"
~~~

## 'Tr' Command #2
![alt text](27.PNG)
~~~
tr -d "a-z"
~~~

## 'Tr' Command #3
![alt text](28.PNG)
~~~
tr -s ' '
~~~

## Sort Command #1
![alt text](29.PNG)
~~~
sort
~~~

## Sort Command #2
![alt text](30.PNG)
~~~
sort -r
~~~

## Sort Command #3
![alt text](31.PNG)
~~~
sort -n
~~~

## Sort Command #4
![alt text](32.PNG)
~~~
sort -n -r
~~~

## Sort Command #5
![alt text](33.PNG)
~~~
sort -k2 -n -r -t$'\t'
~~~

## Sort Command #6
![alt text](34.PNG)
~~~
sort -n -k2 -t$'\t'
~~~

## 'Sort' command #7
![alt text](35.PNG)
~~~
sort -k2 -n -r -t '|'
~~~

## 'Uniq' Command #1
![alt text](36.PNG)
~~~
uniq
~~~

## 'Uniq' Command #2
![alt text](37.PNG)
~~~
uniq -c | cut -c7- 
~~~

## 'Uniq' command #3
![alt text](38.PNG)
~~~
uniq -i -c | cut -c7-
~~~

## 'Uniq' command #4
![alt text](39.PNG)
~~~
uniq -u
~~~

## Paste - 3
![alt text](40.PNG)
~~~
paste -s 
~~~

## Paste - 4
![alt text](41.PNG)
~~~
paste - - -
~~~

## Paste - 1
![alt text](42.PNG)
~~~
paste -d";" -s
~~~


## Paste - 2
![alt text](43.PNG)
~~~
paste -d ";" - - -
~~~

## Read in an Array
![alt text](44.PNG)
~~~
while read line
do
    arr=(${arr[@]} $line)
done

echo ${arr[@]}
~~~

## Slice an Array
![alt text](45.PNG)
~~~
arr=($(cat))
echo ${arr[@]:3:5}
~~~

## Filter an Array with Patterns
![alt text](46.PNG)
~~~
while read word; do
    array=(${array[*]} $word)
done

for var in ${array[*]}; do
    if echo $var | grep 'A' > /dev/null ; then
        continue
    elif echo $var | grep 'a'> /dev/null; then 
        continue
    else
        echo $var
    fi
done
~~~

## Concatenate an array with itself
![alt text](47.PNG)
~~~
array=($(cat))
array3=("${array[@]}" "${array[@]}" "${array[@]}")
echo ${array3[@]}
~~~

## Display an element of an array
![alt text](48.PNG)
~~~
array=($(cat))
echo ${array[3]}
~~~

## Count the number of elements in an Array
![alt text](49.PNG)
~~~
array=($(cat))
echo ${#array[@]}
~~~

## Remove the First Capital Letter from Each Element
![alt text](50.PNG)
~~~
while read line
do 
    first_character=${line:0:1}
    rest_of_characters=${line:1}
    if [[ ${line} =~ [A-Z] ]]
    then
        new_line=$(echo "$first_character" | tr '[:upper:]' '.')
        line=$new_line$rest_of_characters
    fi
    myArray=(${myArray[@]} $line)
done

echo ${myArray[@]}
~~~

## Lonely Integer - Bash!
![alt text](51.PNG)
~~~
read
arr=($(cat)) 
echo "${arr[@]}" | tr ' ' '\n' |sort | uniq -u | tr '\n' ' '
~~~

## 'Awk' - 3
![alt text](52.PNG)
~~~
awk '{ total=$2+$3+$4; avg=total/3; print $0 " : " (avg > 50 ? avg > 60 ? avg > 80 ? "A" : "B" : "C" : "FAIL"); }'
~~~

## 'Awk' - 4
![alt text](53.PNG)
~~~
awk 'END{ if((NR%2)) print p ";" }!(NR%2){ print p ";" $0 }{ p = $0 }'
~~~

## 'Grep' #1
![alt text](54.PNG)
~~~
grep -w "the"
~~~

## 'Grep' #2
![alt text](55.PNG)
~~~
grep -iw "the"
~~~

## 'Grep' #3
![alt text](56.PNG)
~~~
grep -iv "that"
~~~

## 'Grep' - A
![alt text](57.PNG)
~~~
grep -iwe "the\|that\|then\|those"
~~~


## 'Grep' - B
![alt text](58.PNG)
~~~
grep '\([0-9]\) *\1'
~~~

## 'Sed' command #1
![alt text](59.PNG)
~~~
sed -e 's/the /this /1'
~~~

## 'Sed' command #2
![alt text](60.PNG)
~~~
sed -e 's/thy /your /gI'
~~~

## 'Sed' command #3
![alt text](61.PNG)
~~~
sed -e 's/thy/{&}/gI'
~~~

## 'Sed' command #4
![alt text](62.PNG)
~~~
sed -r 's/[0-9]{4}[ ]/**** /g'
~~~


## 'Sed' command #5
![alt text](63.PNG)
~~~
sed -E 's/([0-9]{4}) ([0-9]{4}) ([0-9]{4}) ([0-9]{4})/\4 \3 \2 \1 /g' 
~~~

## 'Awk' - 1
![alt text](64.PNG)
~~~
awk '{ if(length($4) == 0) print "Not all scores are available for " $1 }'
~~~

## 'Awk' - 2
![alt text](65.PNG)
~~~
awk '{ print $1 ($2 < 50 || $3 < 50 || $4 < 50 ? " : Fail" : " : Pass") }'
~~~