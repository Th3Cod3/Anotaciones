#Curso de shell

el archrivo .vimrc en la carpeta home es para configurar vim


todos los scripts debe tener la extension `.sh`

Un script en shell comienza con `#` para hacer un comentario seguido del ejecutador en este caso bash `# !/bin/bash`

para ejecutar un script se le debe de dar el permiso de ejecución con 
`chmod [`**`NNN`**`] | [[+|-][r|w|x]] <file>`
y para ejecutar un archivo se usa `./<fiile>`

`type`

## Variables

`sudo vim /etc/profile` Variables de entorno
~~~bash
...
#variables de entorno S.O
COURSE_NAME=Programación Bash
export COURSE_NAME
~~~

Estan son variables de programa
`./programa.sh  # 764`
~~~bash
# !/bin/bash
# programa para revisar la declaracion de variables

opcion=0
nombre=yefri

echo "Opcion: $opcion y Nombre: $nombre"

#Exportar la variable parta otro archivo
export opcion

# archivo al cual se le enviá la variable
./programa2.sh
~~~
`./programa2.sh  # 764`
~~~bash
# !/bin/bash

echo "El nombre enviado es: $nombre"
~~~

## Operadores

`$((`**`operadores`**`))`

~~~bash
# !/bin/bash
# filename.sh file

A=10
B=4
echo "BASIC ARITHMETIC OPERATORS"
# use -e to support especial characters
echo -e "inputs: A = $A , B = $B \n"

# $((A [+|-|*|/|%] B))

x=10
y=4
echo -e "BASIC RELATIONAL OPERATORS"
echo -e "inputs: x = $x , y = $y \n"

# $((x [<|>|>=|<=|==|!=] y))

echo -e "\nBASIC ASSIGNATION OPERATORS"
# $((x [+=|-=|/=|*=|%=] y))

~~~

## Argumentos
 * `$0` Nombre del Script
 * `$1` o `${10}` El numero de argumento. Si es mas de un dígito se pone entre llaves.
 * `$#` Contador de argumentos
 * `$*` Refiere a todos los argumentos

~~~bash
# !/bin/bash
# filename.sh file
# Executing scripts with arguments

courseName = $1 #//first argument to courseName
courseTimetable = $2

echo "parameters:"
echo "courseName: $courseName"
echo "courseShip: $courseTimetable"

echo "parameters number: $#"
echo "list parameters: $*"

~~~
**to execute use:** `bash:~$ ./filename.sh "name course" "18:00 to 20:00" "OTHER_I" "OTHER_II"`

## Ejecutar comando en Shell
~~~bash
# !/bin/bash
# filename.sh

echo "Location pwd: `pwd`" #backtick character to get the result of executing a command
echo "kernel uname -a: $(uname -a)" #similar

# you can assign it to a variable and than call it
~~~

## Capturar información
 * `-n` to receive data
 * `-n1` or `-n{10}` to receive one char or more than 10  char
~~~bash
# !/bin/bash
# filename.sh

option=0
name=""

echo -n "Set you option:"
read
option = $REPLY
echo -n "Set your name:"
read
name = $REPLY

echo "option: $option , name: $name"
# The read command store the input into the variable $REPLY
~~~
**Reduce comamant to read function**
 * `-p` prompt the text.
 * `-t` set timeout.
 * `-s` silent to sensitive information (hide input).

~~~bash
# !/bin/bash
# filename.sh

option=0
name=""

read -p "Set you option:" option
read -p "Set you name:" name

echo "option: $option , name: $name"
~~~

## Validacion de entrada
~~~bash
# !/bin/bash
# filename.sh

option = 0
name = ""
password = ""
read -n1 -p "Set you option:" option
read -n10 -p "Set you name:" name # -n<number> is to set the number of characters
read -s -p "password:" password # -s (silent) to insert passwords
echo "option: $option , name: $name, password: $password"

# Keep in mind that n<number> will submit automatically and you can't backspace (delete) it should insert the backspace char.
~~~

## validacion RegExp

The operator `=~` is to check if a string match the pattern of a RegExp,

~~~bash
# !/bin/bash
# filename.sh

identificationRegex = '^[0-9]{10}$'
countryRegex = '^EC|COL|US$'
birthDateRegex = '^19|20[0-8]{2}[1-12][1-31]$'

echo "RegExp"
read -p "Insert your identification:" identification
read -p "Insert the initials of your country ex. [EC, COL, US]:" country
read -p "Insert your birth date [yyyyMMdd]:" birthDate 

#Validation Identification
if [[ $identification =~ $identificationRegex ]]; then
    echo "Identification $identification valid"
else
    echo "Identification $identification invalid"
fi

#Validation Country
if [[ $country =~ $countryRegex ]]; then
    echo "Country $country valid"
else
    echo "Country $country invalid"
fi

#Validation Birth date
if [[ $birthDate =~ $birthDateRegex ]]; then
    echo "Birth date $birthDate valid"
else
    echo "Birth date $birthDate invalid"
fi
~~~

# Arguments and options

~~~bash
# !/bin/bash
# filename.sh

echo "Inputs:"
echo "parameter 1: $1 , parameter 2: $2, parameter 3: $3, parameter 4: $4 all : $*"
echo "Checking values:"
while [[ -n "$1" ]] #while exit options
	do 
		case $1 in #chose a case
			-a) echo "$1 : it is the -a option";;
			-b) echo "$1 : It is the -b option";;
			-c) echo "$1 : It is the -c option";;
			*) echo "$1 : is not an option";;
		esac
	shift # shift the arg to the next. 
done
~~~
`bash lec_9_file.sh -a -b -f -g`

## Download from internet

~~~bash
# !/bin/bash
# filename.sh

echo "download"
wget https://www-us.apache.org/dist/tomcat/tomcat-8/v8.5.35/bin/apache-tomcat-8.5.35.zip
~~~

## Condicionales
Esto es muy similar a cualquier lenguaje con algunas diferencias vamos a verlo.

estos son los símbolos de comparación.
Diferencia entre `[  ]` y `[[  ]]`
* [What is the difference between test, \[ and \[\[ ?](http://mywiki.wooledge.org/BashFAQ/031)

|Type|viejo [ comando ]|nuevo [[ comando ]]|Example|
|----|--------------------|:---------------------|:----|
Cadena |`>` |`>` |`[[ a > b ]]` |
" |`<` |`<` |`[[ a < b ]]` |
" |`!=` |`!=` |`[[ a != b ]]` |
" |`=` |`=` or `==` |`[[ a = b ]]` |
Numero |`-ge` |`-ge` |`[[ a -ge b ]]` |
" |`-le` |`-le` |`[[ a -le b ]]` |
" |`-lt` |`-lt` |`[[ a -lt b ]]` |
" |`-gt` |`-gt` |`[[ a -gt b ]]` |
Concatenar |`-a` |`&&` |`[[ -n $var && -f $var ]]` |
" |`-o` |`||` |`[[ -n $var || -f $var ]]` |
Agrupar |`\(...\)` |`(...)` |`[[ $var = img* && ($var = *.png || $var = *.jpg) ]]` |
Patrones | - |`=` or `==` |`[[ $name = a* ]]` |
" | - |`=~` |`[[ $(date) =~ ^Fri\ ...\ 13 ]]` |

**ejemplos:**
 * `if [ "$answer" = y -o "$answer" = yes ]`
 * `if [[ $answer =~ ^y(es)?$ ]]`
 * `if [[ $answer = y* ]]`

### if
~~~bash
if [[ <condicional> ]]; then
	# coligo si if es verdadero
elif [[ <condicional> ]] && [ <condicional> ]; then
	if [[ <condicional> ]]; then
		# coligo si if es verdadero
	elif [[ <condicional> ]] && [ <condicional> ]; then
		# código si if es verdadero
	fi
fi
~~~

### case "switch"
~~~bash
case $var in 
	-a) 
		if [[ <condicional> ]]; then
			# coligo si if es verdadero
		fi
	;;
	"A") 
		echo "Letra A" ;;
	*) #default
	 echo "Opcion no existe"
esac
~~~

~~~bash
# ! /bin/bash

arregloNumeros=(1 2 3 4 5 6)
arregloCadenas=(Marco, Antonio, Pedro, Susana)
arregloRangos=({A..Z} {10..20})

#Imprimir todos los valores
echo "Arreglo de Números:${arregloNumeros[*]}"
echo "Arreglo de Cadenas:${arregloCadenas[*]}"
echo "Arreglo de Números:${arregloRangos[*]}"

#Imprimir los tamaños de los arreglos
echo "Tamaño Arreglo de Números:${#arregloNumeros[*]}"
echo "Tamaño Arreglo de Cadenas:${#arregloCadenas[*]}"
echo "Tamaño Arreglo de Números:${#arregloRangos[*]}"

#Imprimir la posición 3 del arreglo de números, cadenas de rango
echo "Posición 3 Arreglo de Números:${arregloNumeros[3]}"
echo "Posición 3 Arreglo de Cadenas:${arregloCadenas[3]}"
echo "Posición 3 Arreglo de Rangos:${arregloRangos[3]}"

#Añadir y eliminar valores en un arreglo
arregloNumeros[7]=20
unset arregloNumeros[0]
echo "Arreglo de Números:${arregloNumeros[*]}"
echo "Tamaño arreglo de Números:${#arregloNumeros[*]}"
~~~

