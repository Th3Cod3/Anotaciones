#	BÁSICO
##CONSEJO
Para JavaScript JS se recomienda Chrome, ya que es más compatible con JS. En la <u>*Herramienta de Desarrollador->Consola*</u> se puede ejecutar código JS.
## CONCATENAR
Para concatenar en JS se hace uso de símbolo `+` y también dentro de las comillas invertidas \` \` puedes usar el signo de dólar con llaves \``${…}`\`. Un ejemplo mas claro esta en el apartado de CONSOLE
## CONSOLE
Para enviar código a la consola está el objeto console con el método log para hacer uso de él se escribe lo siguiente `console.log(`**`<string|object|variable|function>`**`)`, vamos a demostrar su uso. Vamos a la consola de Chrome y escribimos lo siguiente:

~~~javascript
console.log('La división de 6 entre 2 es: ' + 6 / 2)
// La división de 6 entre 2 es: 3
~~~
Como pueden observar dentro de los paréntesis se introduce el string (*Cadena de caracteres*), aunque la operación está afuera de las comillas no significa que no hacen parte del string. Lo que sucede es lo siguiente. La operación 6 dividido 2 da el resultado 3 y este se concatena (*Mezcla*) con el string por medio de operador + que en este caso tiene rol de concatenado. Al final de todo el string es “La división de 6 entre 2 es: 3” y se envía a la consola.

~~~javascript
console.log(`La división de 6 entre 2 es: ${6/2}`)
// La división de 6 entre 2 es: 3
~~~
En JS existe otra manera de concatenar código dentro de un string, dentro de las comillas invertidas \`...\` puedes usar el signo de dólar con llaves `${`**`<string|object|variable|function>`**`}`. 
## VARIABLES
En JS se declaran las variables con el prefijo `var`, `let` y `const`. Seguido del nombre de la variable que tienes que comenzar con una letra **`(A-Z, a-z)`**, guion al pizo **`_`** o signo de dólar **`$`**.

Haremos el mismo ejemplo, pero agregando las variables.
~~~javascript
var num1 = 6
var num2 = 2
console.log(`La división de ${num1} entre ${num2} es: ${num1/num2}`)
// La división de 6 entre 2 es: 3
~~~

## FUNCIONES
Ahora vamos a agregar una función, para declarar una función se hace uso de la palabra reservada `function` opcionalmente sigue el nombre de la función para poder hacer llamada la función o si se deja sin el nombre es una ***función anónima*** que no puede ser llamada/invocada. Después se abren los paréntesis donde se nombra los argumentos.
La palabra reservada `return` es lo que devuelve a la hora de llamar la función. Vamos a verlo en acción.

~~~javascript
var num1 = 6
var num2 = 2
function division(numero1, numero2){
	return numero1/numero2
}
console.log(`La división de ${num1} entre ${num2} es: ${divicion(num1,num2)}`)
// La división de 6 entre 2 es: 3
~~~
Las funciones también pueden ser declaradas como una variable. Se aprovecha de que la función no cambia y se usar el prefijo const.
~~~javascript
const division = function (numero1, numero2){
	return numero1/numero2
}
~~~

## ARROW FUNCTION
Es otra manera de declarar funciones en JS.
~~~javascript
//Quitamos la palabra function y ponemos la fecha después de los argumentos
const division = (numero1, numero2) => {
	return numero1/numero2
}
//Como lo que retornamos es la operación podemos olvidarnos del return y las llaves {}
const multiplica = (numero1, numero2) => numero1*numero2;
//Si no hubiera argumentos tenemos que poner los paréntesis
const helloWorld = () => 'Hello World!';
//Si es un solo argumentos podemos dejarlos sin paréntesis
const redondear = numero1 => Math.round(numero1);
~~~

Como puede ver hay dos instrucciones, en la primera se abre llaves por el motivo que hay más de una línea de código. Por eso se encierra entre llaves y se pone return. El otro ejemplo de una línea es porque es solo una instrucción y no es necesario la palabra reservada return ya que por defecto devuelve el resultado.

 **NOTA:** *Las arrow function interpredndan this del scope superior en la mayoria de los casos el objeto **window***
## COMPARACIONES
~~~javascript
var x = 4, y = "7"
x == y // true
x === y // false
~~~
## CONDICIONALES
Es como en la mayoría de los lenguajes de programación.
### IF, ELSE IF Y ELSE
~~~javascript
if (condition) {
	// code
}else if(condition){
	// code
}else{
	// code
}
~~~
### SWITCH
~~~javascript
switch(){
	case 1:
		//code
		break
	case 2:
	case 3:
		//code
		break
	default:
		// code
		break
}
~~~

### SHORT TAG
Manera corta de hacer un if:
En condición si es verdadero, ejecuta la línea ahora como true en caso contrario ejecuta false.
~~~javascript
condición ? true : false;
~~~
## BUCLES 
### FOR
Es como en la mayoría de los lenguajes de programación.
~~~javascript
for (let index = 0; index < array.length; index++) {
	const element = array[index];
	// code
}
~~~
### WHILE
Es como en la mayoría de los lenguajes de programación.
~~~javascript
while (condition) {
	// code
}
~~~
### DO WHILE
Es como en la mayoría de los lenguajes de programación.
~~~javascript
do{
	// code
}while (condition)
~~~

# SCOPE
Es la duración de una variable a las que se puede acceder dentro de cierto bloque de código dependiendo donde y como ha sido declarada. Las variables globales son definidas en el objeto `window.nombreVariable`
## VAR
Cuando declaramos una variable con var, JS lo que hace es hoisting (Elevación), ya que var funciona con ámbito de función. Veamos unos ejemplos:
~~~javascript
function counter(){
	// Como declaramos la variable con var, el escoge el scope superior. 
	// Como si fuera sido declarado acá.
	// var i = 0
	for(var i = 0; i < 10; i++){
		console.log(i) // Hace el conteo desde 0 hasta 9
	}
	console.log(i) // Acá imprime el número 10
}
console.log(i) // Error porque la variable no a sido declarada
~~~

## LET
Cuando declaramos una variable con let, JS lo declara en ámbito de bloque y al momento que el bloque de código donde fue declarado termine. Esa variable ya no será accesible. Veamos un ejemplo:
~~~javascript
function counter(){
	for(let i = 0; i < 10; i++){
		console.log(i) // Hace el conteo desde 0 hasta 9
	}//al finalizar este scope se libra el espacio de esa variable
	console.log(i) // Error porque la variable no a sido declarada
}
console.log(i) // Error porque la variable no a sido declarada
~~~
## CONST
Es lo mismo que let, pero con la diferencia que no puede volver a ser reasignada (No puede cambiar su valor).

# OBJETOS
Para declarar un objeto se hace uso de las llaves `{` `}` un estándar que no es obligatorio pero hace mas fácil es escribir todos los objetos Con la primera letra en mayúscula. Vamos a mostrar un ejemplo de un objeto en JS:
~~~javascript
var carlos = {
	nombre: 'Carlos',
	apellido: 'Garrido',
	edad: 38
}
~~~
Enviar un objeto como parametro en una función es igual que mandarlo como referencia (Quiere decir que el objeto se modifica).
~~~javascript
function nombreEnMayuscula(persona){
	persona.edad += 1
}
console.log(persona.edad) // Edad es igual a 39
~~~

~~~javascript
function nombreEnMayuscula(persona){
	return {
		...persona // copia el objeto
		edad: persona.edad += 1 // Pisa el atributo edad.
	}// Esto devuelve un objeto nuevo con el dato modificado.
}
console.log(nombreEnMayuscula(persona)) // El objeto persona, pero con edad = 39
console.log(persona.edad) // Edad es igual a 38
~~~
## OBJETOS PROTOTIPOS
En JS no existen las clases como tal, vamos a ver la base de OOP de JS.
Es importante entender que antes ECMA 2015 no exitian las clases y por ese motivo vamos a profundisar en el ya que las clases en JS no son como en los otros lenguajes ya que no existía la herencia.

**Ejemplo**
~~~javascript
function Punto(x, y){
// Este seria el constructor del objeto
	this.x = x
	this.y = y
}
// Los métodos/prototipos de el objeto Punto
Punto.prototype.moverEnX = function moverEnX(x){
	this.x += x
}
Punto.prototype.moverEnY = function moverEnY(y){
	this.y += y
}
Punto.prototype.distancia = function distancia(p){
	const x = this.x - p.x
	const y = this.y - p.y

	return Math.sqrt(x*x + y*y)
}

// Creamos el objetos y lo asignamos a una variable
// Es muy importante poner la palabra new sino llamara a Punto como un función
const p1 = new Punto(0, 4)
const p2 = new Punto(3, 0)

// Llama el método distancia y como atributo el objeto p1
console.log(p2.distancia(p1)) 
// Llama el método moverEnX y como atributo 10
p1.moverEnX(10)
console.log(p1.distancia(p2))
~~~
La **herencia** se hacia por medio de una función auxiliar
~~~javascript
function herenciaDe(prototipoHijo, prototipoPadre){
	var fn = function () {}
	fn.prototype = prototipoPadre.prototype
	prototipoHijo.prototype = new fn
	prototipoHijo.prototype.contructor = prototipoHijo
}
function PuntoText(){
	this.x = x
	this.y = y
}
herenciaDe(PuntoText, Punto)
PuntoText.prototype.moverEnX = function moverEnX(x){
	this.x += x
	return `El punto se ha movido ${x}, ahora esta en ${this.x}`
}

const p1t = new PuntoText(3,6)
const p1 = new Punto(3,6)
const p1.moverEnX(4)  // Mueve el punto
const p1t.moverEnX(4) // Mueve el punto y devuelve el string
~~~
## "Clases"
**ECMAScript 2015** introdujo las `"clases"` a JS. En el fondo siguen siendo prototipos.
~~~javascript
class Punto = {
	constructor(x,y){
		this.x = x
		this.y = y
	},
	moverEnX(x){
		this.x += x
	}
	moverEnY(y){
		this.y += y
	}
	distancia(p){
		const x = this.x - p.x
		const y = this.y - p.y

		return Math.sqrt(x*x + y*y)
	}
}
class PuntoText extends Punto {
	constructor(x,y){
		super(x,y) // Cuando es una clase hijo debe enviar los atributos que exige su padre.
	},
	moverEnX(x){
		this.x += x
		return `El punto se ha movido ${x}, ahora esta en ${this.x}`
	}
}

//Creamos el objetos y lo asignamos a una variable
const p1 = new Punto(0, 4)
const p2 = new Punto(0, 4)
const p2t = new PuntoText(0, 4)

// Llama el método distancia y como atributo el objeto p1
console.log(p2.distancia(p1)) 
// Llama el método moverEnX y como atributo 10
p1.moverEnX(10) // Mueve el punto
p2t.moverEnX(10) // Mueve el punto y devuelve el string
console.log(p1.distancia(p2))
~~~

### CONSTRUCTOR
El constructor de un objeto es una función, lo que lo hace un objeto es la palabra reservada `new` a la hora de declararse, seguido del nombre del objeto *(Función ‘Constructor’)* cuando no se usa class y cuando se usa una clase por defecto se llama a la función constructor.
### PARÁMETROS
Para declarar los parámetros se usa la palabra/objeto reservado `this` como se puede observar en el código de arriba.
### METODOS
Para declarar un método se usa el nombre del constructor seguido de la palabra reservada prototype y le sigue el nombre del método de la siguiente manera `objecto.prototype.nombreMetodo`. En el ejemplo con las `"clases"` con solo meterno en el cuerpo de la clase.
### __PROTO__
Esto es equivalente a el método `prototype`, todos los objetos tienes este método donde están todos los punteros de sus métodos. Por lo tanto, si se hace un cambio a un método, todos los objetos son afectados con ese cambio.
`return this` en JS es ilícito por motivo que al usar la palabra reservaba new JS retorna `this`.

En el ejemplo de arriba muestro otra forma de declarar objetos para JS y también tenemos las clases de JS que en si no son clases sino objetos, pero con la ayuda de la versión ECMAScript 2015 se cambia la sintaxis. 

## OBJETO DATE()
`new Date()` *Crea un objeto con la fecha y hora actual*
`new Date(YEAR, MONTH, DAY, HOURS, MINUTES, SECONDS, MILLISECONDS)`
`new Date(MILLISECONDS)`
`new Date(DATE STRING)`
# MÉTODOS NATIVOS JS
## STRING
|Método|Descripción|extra|
|------|:----------|:----|
|`toLowerCase()`|Devuelve el string todo en minúsculas|
|`toUpperCase()`|Devuelve el string todo en mayúsculas|
|`endsWith(`**`str`**`)`|Devuelve true cuando el string termina con el string enviado como argumento.|**str:** String que se buscara al final.|
|`strartsWith(`**`str`**`)`|Devuelve true cuando el string comienza con el string enviado como argumento.|**str:** String que se buscara al comienzo.|
|`slice(`**`start`**` [, `**`end`**`])`|Devuelve un string cortado con los indexes enviados como argumento.|**start:** un entero que define la posición en la que va a comenzar a cortar. <br> **end:** un entero que define la posición en la que termina Números negativos van de derecha a izquierda
|`split(`**`str`**`)`|Devuelve un array con particiones por un delimitador designado que este en el string|**str:** delimitador, si se manda vació va a devolver un array con cada carácter|
|`charAt(`**`index`**`)`|Devuelve el carácter en la posición del index enviado como argumento.|**index:** entero de la posición del carácter deseado.|
|`substr(`**`start`**`)`|
## MATH
|Método|Descripción|extra|
|------|:----------|:----|
|`round(`**`float`**`)`|Redondea hacia el número más cerca.|
|`floor(`**`float`**`)`|Redondea hacia abajo (1.9 = 1)|
|`ceil(`**`float`**`)`|Redondea hacia arriba (1.1 = 2)|
|`random()`|Devuelve un numero al azar del 0 al 1.|
## ARRAY
|Método|Descripción|extra|
|------|:----------|:----|
|`filter(`**`callback`**`)`|Devuelve todo un array que el callback devuelva true.| ver ejemplo abajo|
|`map(`**`callback`**`)`|Pasa una función por el array con el cual retorna otro array.| |
|`reduce(`**`callback`**`[,`**`int`**`])`|Devuelve una acumalacion que se hace con el callback.| **int** Es el numero inicial para el acumulador|
|`reverse()`|Devuelve todo un array en orden contrario.|
|`join()`|Convierte un array en un string concatenando todo su contenido.|
~~~javascript
var personas = [diego, andres, pedro] // todas las variables son objetos.
const esMayor = ({ altura }) => altura > 1.8
personasAltas = personas.filter(esAlto)
console.log(personasAltas) // devuelve todas las personas altas en un array
console.log(personas) // devuelve todas las personas en un array
~~~
## NUMBER
|Método|Descripción|extra|
|------|:----------|:----|
|`toFixed(`**`dec`**`)`|Devuelve un numero flotante con las cifras declarado como argumento para número atrás del punto/coma.| **dec:** número definiendo la cantidad de decimales|
# PROPIEDADES
## STRING
|Propiedades|Descripción|
|-----------|:----------|
|length|Devuelve el número de caracteres de un string|