# TERMINAL

## Comandos

`man [cmd]`  #muestra el manual de el comando.
 * <kbd>space</kbd> #siguiente página.
 * <kbd>enter</kbd> #siguiente línea.
 * <kbd>b</kbd> #retrocede una página.
 
`pwc` #muestra el directorio actual.\

`cd [directorio]` #te lleva al directorio.\

## Extra
 * `~` #directorio home.
 * `/` #directorio root.
 * `..` #directorio padre.
 * `.` #directorio actual.
 * `\` #caracteres especiales (ej. `cd "mis fotos"` sino `cd mis\ fotos`).

`ls [options] [file|dir]` #muestra una lista de archivos/carpetas que hay en el directorio.
### Banderas|Flag
 * `-a` #muestra todos los archivos/carpetas, hasta las escondidas.
 * `-d` #lista los directorios.
 * `-l` #muestra los permisos.
 * `-lh` #legible por humanos (cambia el peso en potencias MB, GB, etc).
 * `-R` #lista recursivamente (muestra los subdirectorios).
 * `-X` #hace un sort por extensión.
 * `-t` #hace un sort por tiempo.
 * `-S` #hace un sort por peso.

`mkdir <nombre del directorio>` #crea una carpeta.

`touch <nombre archivo>` #crea un archivo vacío o si existe cambia la fecha de edición.

`mv <archivo> <destino/[nombre a guardar]>` #mueve o renombra un archivo.

`rm [option] <archivo>` #elimina un archivo o carpeta.
### Banderas|Flag
 * `-r` | `-R` #elimina recursivamente (elimina los subdirectorios y su contenido).
 * `-f` #elimina forzoso sin preguntar si está seguro.
 * `-i` #pregunta antes de eliminar cualquier cosa.

`cp <archivo> <destino/[nombre a guardar]>` #copia el archivo en el destino deseado.

`pushd` #agrega un directorio a la pila.

`popd` #vuelve al directorio de la pila.

`tail [option] [file]` #muestra las últimas líneas de un archivo.
### Banderas|Flag
 * `-<numero>` #muestra la cantidad de líneas deseadas.
 * `-f` #muestra las últimas líneas y actualiza cada vez que haya un cambio.

`more [file]` #imprime en pantalla el contenido del archivo, con paginación.
### Banderas|Flag
 * <kbd>space</kbd> #siguiente página.
 * <kbd>enter</kbd> #siguiente línea.
 * <kbd>b</kbd> #retrocede una página.
 * <kbd>q</kbd> #sale del archivo

`cat [option] <file>` #imprime en pantalla el contenido del archivo, sin paginación.

`alias <aliasname=’comando’>` #crea un atajo para un comando.

`top` #muestra los procesos.

`kill [option] <PID>` #mata el proceso con el PID enviado.

`grep [option] <pattern> [file]` #Busca patrones dentro de los archivos, si invoca atrás de un pipe | lo busca en el resultado del comando que viene su tras.
### Ejemplo: 
`grep -r . -e .php` #este comando me devuelve todos las líneas que contienen .php desde la carpeta raíz hacia abajo.
### Banderas|Flag
 * `-r` #recursivamente.
 * `-e` #patrón den regexp.
 * `-n` #numero de la línea que encontró el patrón.

`wc [option] [file]` #cuenta cantidad de líneas, palabras, espacios entre otros.
### Banderas|Flag
 * `-l` #cuenta cantidad de líneas.
 * `-w` #cuenta cantidad de palabras
 * `-c` #cuenta cantidad de bytes
 * `-m` #cuenta cantidad de letras

## Shortcuts
 * <kbd>CTRL</kbd> + <kbd>L</kbd> = Limpia pantalla.
 * <kbd>CTRL</kbd> + <kbd>R</kbd> = Busca un comando ejecutado.

## Streams

### Standard Input
Es lo que enviamos a un programa y se puede enviar por la terminal con el signo menor que `<`.

### Standard Output
Las respuestas del input sin errores con la cual se puede concatenar o recibir por medio de un uno `1` y el símbolo mayor que `<comando/programa>  1> <nombre archivo>` con el cual creamos un archivo y ingresamos el <b>STD_OUT</b>, El número uno está de más ponerlo ya que por default envía solamente el <b>STD_OUT</b> y si en vez de introducir un signo mayor que `>` ponemos 2 `<comando/programa> 1>> <nombre archivo>` concatena al archivo mencionado.

### Standard Error
En este caso es similar al <b>STD_OUT</b> con diferencia que en este es necesario llamarlo con el número `2` `<comando/programa> 2> <nombre archivo>` y `<comando/programa> 2>> <nombre archivo>`.

### Extra
 * `2>&1` #<b>STD_ERROR</b> se envía al pointer de 1.
 * `;` #concateno procesos.

