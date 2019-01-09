# SQL y MySQL
Vamos a manejarnos desde la consola.
**NOTA:** *Es importante tener instalado MySQL y si es usuario windows declararlo como variable global.*

## Agregar variables (globales) del sistema.
Propiedades del sistema->Opciones avanzadas->Variables de entorno->(Editamos Path) Editar->"Agregan la ruta de sql al final de la cadena, separado por un punto y coma"

# MySQL
Para iniciar session en la base de datos en la terminar se usa el el comando `mysql`.
## Comando MySQL
`mysql -u root -h localhost -p` **#** Vamos a desglosar el siguiente comando
 * `-u <user>` **#** El usuario de la base de dato.
 * `-h <host>` **#** La ip o dirección de la base de dato si es localhost se puede dejar abierto.
 * `-p <password>` **#** Aquí se puede ingresar el password, pero no es recomendado para conexiones remotas ya que este comando es abierto (Al enviar el comando se pedirá el password que va encryptado).

`show databases;` **#** Muestra todas las bases de datos.
`use <DB-name>;` **#** Selecciona un DB
`select database();` **#** Muestra en que DB estas.
`create database <nombre_DB>;` **#** crea una base de datos. |**Nota:** *tira error si el DB existe.*
`create database if not exist <nombre_DB>;` **#** crea una base de datos. |**Nota:** *tira un warning si el DB existe.*
`show warnings;` **#** Muestra los warnings que han pasado en la ultima query.

## Opciones en la estructura de una tabla
 * `integer [unsigned]` **#** Tipo de dato entero, que puede ser con signos negativos por defecto o `unsigned` para hacelos números positivos.
 * `varchar(`**`n`**`)` **#** Tipo de dato texto, donde **`n`** es el numero de caracteres máximos que admite esa estructura.
 * `not null` **#** 
 * `auto_increment` **#** 
 * `primary key` **#** 
 * `default <valor|texto>` **#** 
 * `comment <valor|texto>` **#** 
## Tablas
Hay dos tipos de tablas, MyISAM y InnoDB.

### Convenciones.

**MyISAM**
Es más rápido para las lecturas(consultas) debido a que no se preocupa tanto por la integridad referencial. 
**InnoDB**
Es algo lenta ya que procura cuidar la integridad (ACID).
**Nombre de tabla**
Un estándar que no es obligatorio, pero por convenciones internacionales se usa los nombres de las tablas en plural y en ingles. Esto es por que es mas fácil de adaptarnos a otros sistemas u otros sistemas adaptarse al nuestro.

### Comandos y palabras reservadas.
`show tables;` **#** Muestra las tablas de el DB seleccionado.
`drop table <nombre_tabla>;` **#** Elimina una tabla |**Cuidado:** *Estas tablas no se pueden recuperar.*
`description|desc <nombre_tabla>;` **#** Muestra la estructura de la table deseada.
`show full columns from <nombre_tabla>;` **#** Muestra la estructura y extra información de la tabla.
### CREATE TABLE.
`create table <nombre_tabla> ( <estructura> );` **#** Crea una tabla en el DB que estas usando. |**Nota:** *tira error si la tabla existe.*
`create table if not exists <nombre_tabla> ( <estructura> );` **#** Crea una tabla en el DB que estas usando. |**Nota:** *tira un warning si la tabla existe.*
**Ejemplos:**
~~~sql
CREATE TABLE IF NOT EXISTS books ( 
	book_id INTEGER UNSIGNED PRIMARY KEY AUTO_INCREMENT,
	author INTEGER UNSIGNED,
	title VARCHAR(100) NOT NULL,
	`year` INTEGER NOT NULL DEFAULT 1900,
	`language` VARCHAR(2) NOT NULL DEFAULT 2 COMMENT 'ISO 639-1 Languages',
	cover_url VARCHAR(500),
	price DOUBLE(6,2) NOT NULL DEFAULT 10,
	sellable TINYINT(1) NOT NULL DEFAULT 1,
	copies INTEGER NOT NULL DEFAULT 1,
	description TEXT
);
~~~
**Nota:** *Cuando se asigna un nombre a una columna con una palabra reservada es recomendable usar las tildes invertidas. En el ejemplo no hace falta poner las tildes.*
 * `NULL` **#** No es lo mismo que una cadena vaciá.
 * `integer [unsigned]` **#** Tipo de dato entero, que puede ser con signos negativos por defecto o `unsigned` para hacelos números positivos.
 * `varchar(`**`n`**`)` **#** Tipo de dato texto, donde **`n`** es el numero de caracteres máximos que admite esa estructura.
 * `default <valor|texto>` **#** Se le da un dato de default por si no fue enviado o erróneo.
 * `not null` **#** Se le declara a la columna que no sea dato nullo, por lo general si se envía un dato nulo este llenara el dato por default.
 * `auto_increment` **#** Se declara una columna auto incrementar para evitar datos iguales y hace una columna como id.
 * `primary key` **#** Hablaran mas tarde de ello
 * `comment <valor|texto>` **#** Para insertar un comentario en la columna.

~~~sql
CREATE TABLE IF NOT EXISTS authors ( 
	author_id INTEGER UNSIGNED PRIMARY KEY AUTO_INCREMENT,
	name VARCHAR(100) NOT NULL,
	nationality VARCHAR(3)
);
~~~

~~~sql
CREATE TABLE clients (
	client_id INTEGER UNSIGNED PRIMARY KEY AUTO_INCREMENT,
	name VARCHAR(50) NOT NULL,
	email VARCHAR(100) NOT NULL UNIQUE,
	birthdate DATETIME,
	gender ENUM('M', 'F', 'ND') NOT NULL,
	active TINYINT(1) NOT NULL DEFAULT 1,
	created_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
	updated_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
);
~~~
**Nota:** *Diferencia entre `timestamp` y `datetime`. `timestamp` es la cantidad de segundos transcurridos desde 1970 (El numero epoch "comienzo de las computadoras") lo que lo hace mas flexible para cálculos y el `datetime` guarda una fecha en el siguiente formato `yyyy-mm-dd hh-mm-ss` po lo cual puede guardar cualquier fecha*
 * `timestamp` **#** Time stamp tipo de dato para guardar el numero epoch.
 * `datetime` **#** Guarda una fecha con el formato `yyyy-mm-dd hh-mm-ss`.
 * `current_timestamp` **#** Es una función que devuelve el timestamp actual (fecha que se ejecuta).

### INSERT
`insert into <nombre_tabla> [( <columnas, ...> )] values ( <datos, ...> );` **#** Inserta datos en una tabla en la columnas asignadas.
~~~sql
INSERT INTO authors (author_id, name, nationality) VALUES ('', 'Juan Rulfo', 'MEX');
~~~
**Nota:** *En este ejemplo se puede ver como se haria una insercion en la tabla `authors`. Este ejemplo nos va a tirar un warning por que la columna `author_id` es una llave primaria y no deveria estar vacio, pero como es una `auto_increment` el dato es insertado sin problema.*
~~~sql
INSERT INTO authors (name, nationality) VALUES ('Juan Rulfo', 'MEX');
~~~
**Nota:** *En el ejemplo anterior enviamos el dato `author_id` y nos a tirado un warning, el cual en este ejemplo no hemos enviado y no tendremos error ya que tomara su valor `default` que en este caso es `auto_increment`.*
~~~sql
INSERT INTO authors VALUES ('', 'Juan Rulfo', 'MEX');
~~~
**Nota:** *En este otro ejemplo podemos ver que hemos dejados la parte de las columnas vació, pero tenemos que enviar los datos en el mismo orden que esta la estructuras y no va dar el mismo warning del primer ejemplo.*
~~~sql
INSERT INTO authors (name, nationality) VALUES ('Juan Rulfo', 'MEX'),
	('Gabriel García Márquez', 'COL'),
	('Juan Gabriel Vasquez','COL');
~~~
**Nota:** *Esta es la manera en la que se enviá multiples paquetes.*

#### Enviar STD_IN a mysql (command line)
Con el siguiente comando enviaremos un archivo de texto a mysql para que procese.
 * `mysql -p -u <usuario> -D <DB> < <./archivo>` **#** Se enviá un archivo con comandos MySQL los cuales van a ser exportados a el DB seleccionado. 

### SELECT INTO
La primera manera que vemos para ver datos en una tabla

 * `select <columnas, ...> from <tabla>;` **#** Esta es la sintaxis del select
 * `select <columnas, ...> from <tabla>\G` -> para mostrar una fila con los datos en tablas.
~~~sql
SELECT * FROM authors;
~~~
**Nota:** *Con este ejemplo trae todas las columnas y datos de la tabla `authors`. Donde `*` significa todos las columnas*
~~~sql
SELECT name, nationality FROM authors LIMIT 5;
~~~
**Nota:** *Con este ejemplo trae solo 5 filas con los `name` y `nationality` de la tabla `authors`. Donde el 5 puede ser cualquier otro entero*

### WHERE
El `where` es la manera en la que detallaremos las busqueda de un elemto
~~~sql
SELECT `name`, nationality FROM authors WHERE nationality = 'COL' AND `name` LIKE '%<text>%';
~~~
 * `=` **#** en esta ocasión el símbolo `=` es de comparación, donde solo se traerá los datos que sea igual a `COL`.
    * `=`
    * `<`
    * `>`
    * `<=`
    * `>=`
 * `AND` **#** El `and` es para seguir concatenando mas condiciones donde la condición de la derecha e izquierda deben cumplirse. 
    * `AND`
    * `OR`
 * `LIKE` **#** esta es otra función de comparación como el `=`, pero es mas tanto para cadenas de texto. Donde el símbolo `%` significa que puede haber cualquier cosa.
    * `LIKE <búsqueda>`
    * `BETWEEN <desde>,<hasta>`
    * `[NOT] IN(<búsqueda1>,<búsqueda2>,...)`

### JOIN
Es para unir 2 tablas con relaciones 
 * `INNER JOIN`
 * `LEFT JOIN`
 * `RIGHT JOIN`
~~~sql
SELECT c.name, b.title, t.type FROM transactions AS t
JOIN books AS b ON t.book_id = b.book_id
JOIN clients AS c ON t.client_id = c.client_id
JOIN authors AS a ON a.author_id = b.author_id;
WHERE c.gender = 'F' AND t.type IN ('sell','lend')
~~~

### UPDATE
### DELETE    
### FUNCIONES MySQL
 * `now()` **#** Trae la fecha actual del sistema.
 * `year(<fecha>)` **#** Devuelve el año de la fecha dada y lo retorna como entero con el cual se puede hacer operaciones.
 * `count()` **#** 
 * `avg()` **#** 
 * `max()` **#** 
 * `min()` **#** 
 * `staddev()` **#** 
 * `to_days()` **#** 
 * `()` **#** 
 * `()` **#** 
 * `()` **#** 



Pendiente
`ON DUPLICATE KEY UPDATE VALUES(active = 0)`
`ORDER BY <columna>, ...`
`GROUP BY <columna>, ...`

