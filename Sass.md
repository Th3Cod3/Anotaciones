
# Sass pre-compilador de CSS
## Programas que se pueden usar
 * prepros
 * CodeKit (Solo para Mac)
 * Ambiente con gulp
 * entre otros.

## Distribución de archivos
***Sass*** se encarga de generar un archivo css que es el cual que va a producion después de haber sido pre-procesado "Compilado". Se aconseja de tener una carpeta para todo lo que tiene que ver con código de desarrollo, que no son necesarios llevar a producion.

## Importar archivos
En ***Sass*** se puede distribuir todos los archivos en diferentes maneras, para asi poder tener una aplicación mas manejable, organizada y escalable.

Para importar un archivo a ***Sass*** se usa la palabra clave `@import "<archivo[.scss]>"`

Tambien es muy importante tener en cuenta el fluido en cascada. 

Tambien hay diferentes buenas practicas de como distribuir los archivos.

## Variables
En ***Sass*** se usan las variables para diferentes cosas como guardar colores, medidas, textos y entre otros. En ***Sass*** se usa el símbolo de dolar para crear una variable de esta manera `$<nombre-variable>: <valor>;`.

**Ejemplo:**
~~~scss
$color-primary: #f00;
$font-primary: Arial;

body{
	font: $font-primary;
	color: $color-primary;
}
~~~

## Anidaciones
~~~scss
$color-info: #fff;
.btn {
	font-size: 15pt;
	&__icon {
		font-size: .6em;
	}
	&.btn--info {
		background-color: $color-info;
	}
}
~~~
~~~scss
.btn {
	font-size: 15pt;
}
.btn__icon {
	font-size: 0.6em;
}
.btn.btn--info {
	background-color: #fff;
}
~~~
El amperson `&` sirve para nombrar al padre.
## @Mixin
Los mixins agrupan diferentes declaraciones 
~~~scss
$color-info: #fff;
@mixin position-top{
	top: 0;
	right: 0;
	position: absolute;
}
.btn {  
	font-size: 15pt;
	position: relative;
	&__icon {
		@include position-top;
		font-size: .6em;
		background-color: $color-info;
	}
}
~~~
**Resultado**
~~~css
.btn {
	font-size: 15pt;
	position: relative;
}
.btn__icon {
	top: 0;
	right: 0;
	position: absolute;
	font-size: 0.6em;
	background-color: #fff;
}
~~~

Los mixin tambien puede recibir parametros que funciona muy parecido como una función solo que solo sobrescribe variables y tiene du valor de defecto.
~~~scss
$page-max-width: 800px;
@mixin max-width($max-width: $page-max-width) {
	max-width: $max-width;
	margin-left: auto;
	margin-right: auto;
}

.section{
	@include max-width;
}

.container{
	@include max-width(1200px);
}
~~~
**Resultado**
~~~css
.section {
  max-width: 800px;
  margin-left: auto;
  margin-right: auto;
}

.container {
  max-width: 1200px;
  margin-left: auto;
  margin-right: auto;
}
~~~

## @content dentro de @mixin
Es una herramienta muy util como para hacer los breakpoint, veamoslo en action.

~~~scss
section{
	background: blue;
	@include respond-to(320px) {
		background: red;
	}
}
article{
	font-size: 1em;
	@include respond-to(320px){
		font-size: .5em;
	}
}
~~~
**Resultado**
~~~css
section {
	background: blue;
}

@media only screen and (min-width: 320px) {
	section {
		background: red;
	}
}

article {
	font-size: 1em;
}

@media only screen and (min-width: 320px) {
	article {
		font-size: .5em;
	}
}
~~~

## @extend
`@extend` nos sirve para heredar de una clase ya existente o tambien se puede hacer uso de placeholder que empiezan con `%`.
**Con placeholder**
~~~scss
%btn{
  border: solid 12px #fff;
  padding: 3px;
}

.btn--info{
  @extend %btn;
  background: blue;
}

.btn{
	@extend %btn;
}
~~~
**Sin placeholder**
~~~scss
.btn{
  border: solid 12px #fff;
  padding: 3px;
}

.btn--info{
  @extend .btn;
  background: blue;
}
~~~
**Resultado**
~~~css
.btn--info {
  border: solid 12px #fff;
  padding: 3px;
}

.btn--info {
  background: blue;
}
~~~

## Funciones
En lo que es funciones Sass ya tienes unas funciones que estan creadas y tambien podemos crear nuestras funciones.
### Funciones de Sass
Sass tiene diferentes funciones ya hechas, como por ejemplo hacer un color mas oscuro `darken()`, mas claro `lighten()` o inverso `invert()`. Tambien hay funciones matemáticas como coseno `cos()`, seno `sin()`, tangente `tan()`, etc.

### Crear funciones
Las funciones como en muchos lenguajes de programación, se declaran de la misma manera.
~~~scss
@function suma($numero1, $numero2){
	@return $numero1 + $numero2
}
.container{
	max-width: suma(800px,400px);
}
~~~
**Resultado**
~~~css
.container {
  max-width: 1200px;
}
~~~

## Arrays o mapas
~~~scss
$fs: (
	big: 24px,
	normal: 16px,
	small: 14px,
	x-small: 12px
);
p {
	font-size: map-get($fs, normal);
}
small {
	font.size: map-get($fs, x-small);
}
~~~
### Reto
*Respuesta por [@pablojorgeandres](https://platzi.com/@pablojorgeandres/)*
~~~scss
$indices:(
	main: 1000,
	carrousel: 1010,
	tooltip: 1020,
	navmenu: 1030,
	modal: 1040
);

@function setIndex($layer){
	@return map-get($indices,$layer);
}

.message{
  z-index:setIndex(modal)
}
~~~

## Listas y each
**Option 1** 
~~~scss
@each $font in (normal, bold, italic){
	.#{$font} {font.weight: $font;}
}
~~~
**Option 2**
~~~scss
// Lista de elementos, se separa con espacios.
$font-weights: normal bold italic; 

@each $font in ($font-weights){
	.#{$font} {font.weight: $font;}
}
~~~
**Resultado**
~~~css
.normal {
	font-weight: normal;
}

.bold {
	font-weight: bold;
}

.italic {
	font-weight: italic;
}
~~~

## For
~~~scss
@for $i from 1 to 5 {
	.class-#{$i} {
		&:before {
			content: "#{$i}"
		}
	}
}
~~~

### Aporte 
*Aporte por [@eldoctorgonzo](https://platzi.com/@eldoctorgonzo/)*
*un poco modificado a mi gusto*
Un dato adicional que es importante, es que el bucle for, tiene dos formas
En una usamos la palabra clave `to` y en la otra `through`.

`@for $var from <start> to <end>{ ... }`
Si iteramos con valores de 1 `start` a 5 `end` el bucle iterará 4 veces (salvo la última vez cuando la condición no se cumple).

`@for $var from <start> through <end>{ ... }`
¿Diferencia?
En el caso de through si iteramos con valores de 1 `start` a 5 `end` el bucle repetirá 5 veces el ciclo (incluido el último).

## If

~~~scss
$background-color: black;

@if $background-color == black {
  p {
    text-color: white;
  }
}
@else {
  p {
    text-color: black;
  }
}
~~~