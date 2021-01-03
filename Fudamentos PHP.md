## Definición de sintaxis
Un lenguaje de programación como cualquier idioma mantiene su propia regla, estas reglas se llaman sintaxis.

## Ciclo de vida
Tener en cuenta que PHP funciona a base de una 
1. Petición
    * Es cuando le das click a un vinculo, el cual hace la petición al servidor.
1. Procesamiento
    * Recibe la petición y procesa el codigo basado en la petición
1. Respuesta
    * Cuando tiene un resultado envía un respuesta de nuevo al que hizo la petición.

## Herraminetas
Es muy importante comprender que PHP es un lenguaje del lado del servidor y funciona con las estructura cliente-servidor, por el cual debemos tener una configuración mínima que interprete las peticiones de la web (HTTP) y genere respuestas adecuadas para el cliente. Para esta tarea los mas conocidos son Apache y NGNIX. En nuestro caso vamos a usar Apache.

Además del interprete, también necesitamos el procesador del lenguaje. En nuestro caso PHP. 

Otro temas que también va muy vinculado con una aplicación web son las bases de datos. Que es donde va estar la información dinámica. No es de mas decir que las bases de datos es otro mundo entero, en el cual pueden haber bases de datos relacionales y no relacionables. Las cuales cumplen la misma función, pero con una estructura diferente. En nuestro caso estaremos usando una base de datos relacional MySql.

En resumen estas son las herramientas:
* Apache
* PHP
* MySql

Es bueno saber que ahí programas que nos solucionan estos problemas y nos instala todos las 3 herramientas.
* XAMP
* MAMP
* Laragon

## Composer
Composer es el manejador de paquetes de php. Donde podemos instalar librerías para nuestro proyecto.

## Sintaxis

### Reglas generales
PHP tiene ciertas reglas:
* Necesita una etiqueta de apertura `<?php`
* Es necesario terminar cualquier instrucción con un punto y coma `;`

* Muy importante, pero no obligatorio.
  * Los comentarios
    * `# text` comentario de una linea
    * `// text` comentario de una linea
    * `/* text */` Comentario de varias lineas donde el `*/` indica el final del comentario.
  * Organización del codigo
    * No usar mas de una linea por sentecia
    * Espacios entres bloques de instrucciones
    * Comentar todo, pero sin reutilizar lo ya programado. Si no dar una explicación de lo que hace.

~~~php
<?php

/* Comentario de varias
 * lineas para
 * palabras
 */
class Answer
{
  /* Comentario de varias
  * lineas para
  * palabras
  */
  protected $client = []; // Mi comentario

  /* Comentario de varias
  * lineas para
  * palabras
  */
  protected $insurers = []; # Mi comentario
}
~~~

Como puede ver tenermos una jerarquia en los comenratrios de varias lineas, cada sentencia termino con `;` y tambien con un salto de linea para su mejor legibilidad.

### Sintaxis aritméticas
Como en muchos lenguaje con el signo `=` se asigna un valor a una variable, aunque en php tenemos unos mas que es de asignacion a una llave de un arreglo. `=>`

~~~php
<?php

// Asignacion
$num = 9;
$lang = [
  'es' => 'español',
  'en' => 'ingles'
]

// Aritmetica
echo "Suma de 2 + 2 = " . ((int) 2 + (int) 2);
echo "Resta de 2 - 2 = " . ((int) 2 - (int) 2);
echo "Multiplicacion de 2 * 2 = " . 2 * 2;
echo "Division de 2 / 2 = " . 2 / 2;
echo "Modulo (residuo) de 2 % 2 = " . 2 % 2;
echo "Exponente de 2 ** 2 = " . 2 ** 2;

// Comparación

// Igual ==, valor '9' == 9 --> Comparación de valores
// Igual ===, valor - tipo 9 === 9 --> Comparacion de valores y tipo de dato

// Diferencias !=, valor
// Diferencias !==, valor - tipo

// <, >, <=, >= Comparacion numericas

//Variables variables, basicamente apartir de un valor se llama a
//una variable con el mismo nombre, en este caso usando $$ se 
//llama a la variable name haciendo uso del valor 'name'.

$app = 'name';
$name = 'Platzi';

echo $app; // output: name
echo $$app; // output: Platzi
~~~

## Temas que se tocaron que ya tengo conocimiento

* if, else, elseif (shorthand)
* switch
* foreach
* while, do while
* composer.json
* psr-4
* composer dump
* composer install
* comillas simples y dobles y escapar caracteres
* comillas dobles mostrar un array con llaves `{xxx[]}`
* comillas dobles mostrar un variable variable con llaves `${xxx}`
* extraccion
  * substr()
  * explode()
  * implode()
  * trim()
* alterar
  * strtolower()
  * strtoupper()
  * ucfirst()
  * lcfirst()
* remplazar
  * str_replace()
  * str_pad()
* regexp
  * preg_match()
* funciones
  * parametros
  * referencia "&"
  * default values ($name = "name")
  * return
  * closure
* arrays
  * key => value
  * matrix
  * array_walk(array $array, string $funcion)
  * array_key_exists()
  * in_array()
  * array_keys()
  * array_values()
  * sort()
  * rsort()
  * ksort()
  * krsort()
  * array_slice()
  * array_chunk()
  * array_shift()
  * array_shift()
  * array_unshift()
  * array_pop()
  * array_push()
  * array_flip()
  * array_diff()
  * array_diff_assoc()
  * $array1 + $array2
  * array_merge()
  * array_merge_recursive() // group by key
  * array_combine()
* foreach
* include, include_once
* require, require_once

## Extracion de datos
~~~php
$data = "Hello World";
$data[0] // H
$data{2} // l
~~~

## PHPunit
Para hacer test requiere el documento xml con la configuracion del phpunit

phpunit.xml
~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<phpunit bootstrap="vendor/autoload.php" colors="true">
  <testsuite name="Test directory">
    <directory>tests</directory>
  </testsuite>
</phpunit>
~~~

`Muy importante que termine con la palabra Test`
test/ValidateTest.php
~~~php
<?php

use PHPUnit\Framework\TestCase;
use App\Validate;

class ValidateTest extends TestCase{

  // muy importante que el metodo comience con la palabra test
  public function test_email(){ 
    $email = Validate::email('test@asd.com');
    $this->assertTrue($email);

    $email = Validate::email('test@@asd.com');
    $this->assertFalse($email);
  }

}
~~~

`Este es el archivo que le estamos haciendo el test`
app/validate.php
~~~php
<?php

namespace App;

class Validate{
  public static function email($value){
    return (bool) filter_var($value, FILTER_VALIDATE_EMAIL);
  }
}
~~~

## Closure

~~~php
<?php

//Función Anonima

$greet = function ($name){ //Variable que requiere lógica
	return "<h1>Hola, $name</h1>"; 
};

echo greet('Danny');

function greet(Closure $lang, $name){
	return $lang($name);
}

$es = function ($name){
	return "Hola, $name";
};

$en = function ($name){
	return "Hello, $name";
};

echo greet($es, 'Danny');
echo greet($en, 'Danny');
~~~

¿Qué es PHP Avanzado?
¿Qué signos puedo escapar entre comillas simples?

# Buenas practicas
## temas a tocar
Los siguientes elementos dotan de calidad al código:
**Legibilidad:** qué tan fácil es interpretar lo que el código dice.
**Mantenibilidad:** cuánto esfuerzo supondrá adaptar el código a nuevos requerimientos.
**Testeabilidad:** cuánto esfuerzo supondrá realizar pruebas sobre este código.

## codigo prolijo
### indentar
### llave de apertura pegado al if
~~~php
if (true) { // ejemplo correcto
  // code
}

if (true)
{ // ejemplo incorrecto
  // code
}
~~~

### Criterio comun
se recomienda que todos los if lleven llaves aunque sea un if de una linea.
~~~php
if (true)
  $test = 0;
if (true)
  $test = 0;

if (true)
  $test = 0;

if (true){
  $test = 0;
}
~~~

si vas a usar if de una linea sin llaves no uses llaves en los demas casos

### Debemos seguir un estándar de codificación, el cual nos ayuda a:
* Generar código claro y consistente.
* Evitar perder tiempo en decisiones triviales.

### Tips para mejorar la legibilidad de nuestro código:
* Define un estándar: Piénsalo una vez y déjalo por escrito. recomiendo usar uno que ya existe.
* Respétalo: Haz un esfuerzo por adherir al estándar durante tu día a día.
* Apóyate en algún linter: Esta sencilla herramienta te ayudará a incorporar buenas prácticas.

## Identificadores mnemotécnicos, específicos y precisos
Los identificadores son variables, funciones, clases, módulos, componentes, etc. Elementos a los que nosotros debamos crearles un nombre propio.

Ejemplo sin un identificador mnemotécnico una función se vería así:
~~~php
function f( int $b, int $a ) : float {
  return ( $b * $a ) / 2;
}
~~~
Al leer este código no sabemos para qué funciona y hasta podríamos borrarlo por equivocación.

Ahora utilizando un identificador mnemotécnico se vería así:
~~~php
function areaRectangulo( int $base, int $altura ) : float {
  return ( $base * $altura ) / 2;
}
~~~
Ahora gracias a que el código es más legible sabemos para qué funciona esta función.

Atención a los identificadores que estableces.

## Codigo modular
El código modular son pedazos de códigos divididos que pueden ser utilizados en cualquier lugar para evitar tener un solo archivo con un bloque de código gigante.


### sin modular
~~~php
<?php
for ($i = 0; $i < $total; $i++) {
  $rows = oci_fetch_array($rss, OCI_NUM);
  $empl_num = $rows[0];
  $nombre = $rows[1];
  $apaterno = $rows[2];
  $amaterno = $rows[3];
  $fecha = $rows[4];
  $hora = $rows[5];
  $horario = $rows[6];

  $empl_num = str_replace("?", 'Ñ', $empl_num);
  $nombre = str_replace("?", 'Ñ', $nombre);
  $apaterno = str_replace("?", 'Ñ', $apaterno);
  $amaterno = str_replace("?", 'Ñ', $amaterno);
  $fecha = str_replace("?", 'Ñ', $fecha);

  $trabajo = '14:00:00';

  if ($hora < $trabajo) {
      $horaentrada = $hora;
      $horae = $horaentrada;
      $horas = '00:00:00';
  } else {
      $horasalida = $hora;
      $horae = '00:00:00';
      $horas = $horasalida;
  }
  $horae = str_replace("?", 'Ñ', $horae);
  $horas = str_replace("?", 'Ñ', $horas);
  $horario = str_replace("?", 'Ñ', $horario);

  echo "$empl_num, $nombre, $apaterno, $amaterno, $fecha, $horae, $horas, $horario".PHP_EOL;
}
~~~

### modulado
~~~php
<?php
for ($i = 0; $i < $total; $i++) {
  imprimirRegistro();
}

function imprimirRegistro() {
  $rows = oci_fetch_array($rss, OCI_NUM);
  $empl_num = $rows[0];
  $nombre = $rows[1];
  $apaterno = $rows[2];
  $amaterno = $rows[3];
  $fecha = $rows[4];
  $hora = $rows[5];
  $horario = $rows[6];

  $empl_num = str_replace("?", 'Ñ', $empl_num);
  $nombre = str_replace("?", 'Ñ', $nombre);
  $apaterno = str_replace("?", 'Ñ', $apaterno);
  $amaterno = str_replace("?", 'Ñ', $amaterno);
  $fecha = str_replace("?", 'Ñ', $fecha);

  $trabajo = '14:00:00';

  list($horae, $horas) = calcularHorario($hora, $trabajo)
  $horae = str_replace("?", 'Ñ', $horae);
  $horas = str_replace("?", 'Ñ', $horas);
  $horario = str_replace("?", 'Ñ', $horario);

  echo "$empl_num, $nombre, $apaterno, $amaterno, $fecha, $horae, $horas, $horario".PHP_EOL;
}

function calcularHorario( $hora, $trabajo ) {
  if ($hora < $trabajo) {
    $horaentrada = $hora;
    $horae = $horaentrada;
    $horas = '00:00:00';
  } else {
    $horasalida = $hora;
    $horae = '00:00:00';
    $horas = $horasalida;
  }
}
~~~

## Código reutilizable
Escribir código reutilizable nos va a ayudar a que en lugar de copiar y pegar una misma línea de código pero con diferentes parámetros lo hagamos a través de una función que retorne los valores que necesitamos y luego la podremos llamar en cualquier lugar del código que necesitemos pasándole los parámetros que deseamos.

### modulado
~~~php
<?php
for ($i = 0; $i < $total; $i++) {
  imprimirRegistro();
}

function imprimirRegistro() {
  $rows = oci_fetch_array($rss, OCI_NUM);
  $empl_num = $rows[0];
  $nombre = $rows[1];
  $apaterno = $rows[2];
  $amaterno = $rows[3];
  $fecha = $rows[4];
  $hora = $rows[5];
  $horario = $rows[6];

  $empl_num = normalizar($empl_num);
  $nombre = normalizar($nombre);
  $apaterno = normalizar($apaterno);
  $amaterno = normalizar($amaterno);
  $fecha = normalizar($fecha);

  $trabajo = '14:00:00';

  list($horae, $horas) = calcularHorario($hora, $trabajo)
  $horae = normalizar($horae);
  $horas = normalizar($horas);
  $horario = normalizar($horario);

  echo "$empl_num, $nombre, $apaterno, $amaterno, $fecha, $horae, $horas, $horario".PHP_EOL;
}

function normalizar($campo) {
  return str_replace("?", 'Ñ', $campo);
}

function calcularHorario( $hora, $trabajo ) {
  if ($hora < $trabajo) {
    $horaentrada = $hora;
    $horae = $horaentrada;
    $horas = '00:00:00';
  } else {
    $horasalida = $hora;
    $horae = '00:00:00';
    $horas = $horasalida;
  }
}
~~~

## Código organizado
El código organizado se refiere a cómo tenemos distribuido nuestros archivos en la raíz (root) del proyecto. A mayor organización, mayor entendimiento del código.

Un repositorio moderno tiene un estardar de carpetan que se ven en mucho proyectos.
`/public`
`/src`
`/test`
`/vendor`

## Evitar el hardcoding
El hardcoding es la práctica de escribir valores literales en lugar de identificadores. No debe de usarse, ya que si el día de mañana debemos cambiar los valores eso significa que debemos cambiar el código en los lugares que esté ese valor estático por completo y luego mandar a producción, cuándo podríamos hacer el cambio más orgánico en una variable que afecte a todos los lugares que es llamada.

### Codigo con hardcoding
~~~php
<?php

$precioInicial = $argv[1];
$precioConIVA = $precioInicial * 1.21;

echo "Valor del IVA: 21%".PHP_EOL;
echo "Sin IVA: \$$precioInicial, con IVA: \$$precioConIVA".PHP_EOL;
~~~

### Codigo corregido
~~~php
<?php

$configs = require_once __DIR__.'/config.inc.php';
$precioInicial = $argv[1];
$precioConIVA = $precioInicial * ( 1 + $configs['valor_iva'] / 100 );

echo "Valor del IVA: {$configs['valor_iva']}%".PHP_EOL;
echo "Sin IVA: \$$precioInicial, con IVA: \$$precioConIVA".PHP_EOL;
~~~

### Magic numbers
~~~php
<?php

if ( $cantidad > 30 ) { // no es legible el codigo
  $test = 0;
}
~~~

### Codigo corregido
~~~php
<?php

class Deposito {
  public const CAPACIDAD_MAXIMA = 30;
}

if ( $cantidad > Deposito::CAPACIDAD_MAXIMA ) {
  $test = 0;
}
~~~

## Evitar efectos colaterales
Debemos analizar muy bien nuestro código para evitar efectos colaterales y evitar que nuestro código deje de funcionar. Un consejo de nuestro profesor en esta clase: No uses variables globales.

## Principios SOLID
SOLID son cinco principios básicos de la programación orientada a objetos que ayudan a crear software mantenible en el tiempo.

SOLID significa:

* S: Single Reponsibility Principle
* O: Open/Closed Principle
* L: Liskov Substitution Principle
* I: Interface Segregation Principle
* D: Dependency Inversion Principle

### S: Single Responsibility Principle
La S se trata de una clase que debe tener sólo una razón para cambiar.

#### Principio de responsabilidad única:
* Una clase debería tener sólo una razón para cambiar
* Cada responsabilidad es el eje del cambio
* Para contener la propagación del cambio, debemos separar las responsabilidades.
* Si una clase asume más de una responsabilidad, será más sensible al cambio.
* Si una clase asume más de una responsabilidad, las responsabilidades se acoplan.





# PHP OOP
## Deuda tecnica.
La deuda técnica es el coste y los intereses a pagar por hacer mal las cosas. El sobre esfuerzo a pagar para mantener un producto software mal hecho, y lo que conlleva, como el coste de la mala imagen frente a los clientes, etc

## Code smell
Hace referencia al mal olor del código. Este concepto no se refiere a errores técnicos, sino a errores de orden y diseño. Esto sucede mucho cuando intentamos crear soluciones a partir de otras soluciones.
La solución a estos casos es crear una abstracción.

### Cómo evitarlo
Para esto debemos hacer una programación más limpia, y reusable. Tenemos que evitar crear grandes métodos, o sea, programación estructura dentro de clases. También evitar crear grandes clases o superclases.

Y sin duda, nosotros debemos evitar a toda costa copiar y pegar código.
Recuerda: el sistema va a funcionar pero a futuro va a ser horrible de mantener, hasta imposible.

## Código espagueti
Un código espagueti es código que está estructurado mediante if, while, for netamente, todo en un mismo archivo donde solamente buscamos resolver el problema. Cuando creamos código estructurado corremos peligro de crear código espagueti. La OOP nos ayuda evitarlo.

El dinero en esta profesión está en el mantenimiento del código.

### Cómo evitar el código espagueti
1. Resolver el problema
1. Crea de forma lógica y coherente diferentes métodos que reemplacen tus estructuras de control.
1. Crea una o varias clases dependiendo el caso.

## OOP
La programación orientada a objetos es una forma de programar, un paradigma o una técnica. Los conceptos que aquí aprendiste te servirán en PHP y en otros lenguajes de programación. Recordemos que para programar de esta forma en realidad debemos crear objetos, y un objeto es una instancia de una clase y una clase es el molde. Ejemplo:

**Programación orientada a objetos:** es la técnica.
**PHP:** es el lenguaje de programación (donde implementamos la técnica).
Podemos resumir los diferentes conceptos de la siguiente manera:

**Herencia:** compartir métodos entre clases padres y clases hijas.
Abstracción: significa aislar, separar y sacar.

**Polimorfismo:** capacidad o virtud que tienen los métodos donde, por ejemplo, un mismo método puede tener diferentes comportamientos y dar diferentes resultados.

**Modularidad:** este principio básicamente nos ayuda a tener cada vez piezas de código más pequeñas y entendibles, donde cada pieza es un módulo y muchos módulos forman el sistema entero.

**Encapsulamiento:** un objeto debe estar aislado y ser un módulo natural. Esto se cumple aplicando la protección a las propiedades impidiendo su modificación y básicamente se refiere a controlar el acceso.