# TERMINAL

## Extra (Navegación)
 * `~` #directorio home.
 * `/` #directorio root.
 * `..` #directorio padre.
 * `.` #directorio actual.
 * `\` #caracteres especiales (ej. `cd "mis fotos"` sino `cd mis\ fotos`).

## Comandos

`man [cmd]`  #muestra el manual de el comando.
 * <kbd>space</kbd> #siguiente página.
 * <kbd>enter</kbd> #siguiente línea.
 * <kbd>b</kbd> #retrocede una página.
 
`pwc` #muestra el directorio actual.

`cd [directorio]` #te lleva al directorio.

`ls [options] [file|dir]` #muestra una lista de archivos/carpetas que hay en el directorio.
**Banderas ls**
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

`mv <archivo> <destino/[nombre a guardar]>` #mueve o renombrar un archivo.

`rm [option] <archivo>` #elimina un archivo o carpeta.
**Banderas rm**
 * `-r` | `-R` #elimina recursivamente (elimina los subdirectorios y su contenido).
 * `-f` #elimina forzoso sin preguntar si está seguro.
 * `-i` #pregunta antes de eliminar cualquier cosa.

`cp <archivo> <destino/[nombre a guardar]>` #copia el archivo en el destino deseado.

`pushd` #agrega un directorio a la pila.

`popd` #vuelve al directorio de la pila.

`tail [option] [file]` #muestra las últimas líneas de un archivo.
 Banderas tail
 * `-<numero>` #muestra la cantidad de líneas deseadas.
 * `-f` #muestra las últimas líneas y actualiza cada vez que haya un cambio.

`more [file]` #imprime en pantalla el contenido del archivo, con paginación.
**Teclas more**
 * <kbd>space</kbd> #siguiente página.
 * <kbd>enter</kbd> #siguiente línea.
 * <kbd>b</kbd> #retrocede una página.
 * <kbd>q</kbd> #sale del archivo

`cat [option] <file>` #imprime en pantalla el contenido del archivo, sin paginación.

`alias <aliasname=’comando’>` #crea un atajo para un comando.

`top` #muestra los procesos.

`kill [option] <PID>` #mata el proceso con el PID enviado.

`grep [option] <patrón> [file]` #Busca patrones dentro de los archivos, si invoca atrás de un pipe | lo busca en el resultado del comando que viene su tras.
**Ejemplo grep:**
`grep -r . -e .php` #este comando me devuelve todos las líneas que contienen .php desde la carpeta raíz hacia abajo.
**Banderas grep**
 * `-r` #recursivamente.
 * `-e` #patrón den regexp.
 * `-n` #numero de la línea que encontró el patrón.
  * `-v` #Excluye el string

`wc [option] [file]` #cuenta cantidad de líneas, palabras, espacios entre otros.
**Banderas wc**
 * `-l` #cuenta cantidad de líneas.
 * `-w` #cuenta cantidad de palabras
 * `-c` #cuenta cantidad de bytes
 * `-m` #cuenta cantidad de letras

`time [comando]` #Me da el tiempo del proceso.

`date` #Devuelve la fecha y hora de la que fue llamado el comando.

`find [directorio][option]` #busca en los meta datos recursivamente.
**Banderas find**
 * `-name <patrón>` #busca en el nombre del archivo y file
 * `-type <f|d>` #filtro para archivos `f` o carpetas `d`

 `awk  [option] 'program' <file>`

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

### Extra Streams
 * `2>&1` #<b>STD_ERROR</b> se envía al pointer de 1.
 * `;` #concateno procesos.
## Descargas
`curl [option] <url>`
 ## Compresión de archivos

 `zip [nombre.zip] [files|path]`
 **Ejemplo zip:**
`zip archivoscsv.zip *csv` #Este comando comprime todos los archivos que terminen con la palabra csv.

`unzip [option] <file>[.zip]` #Descomprime el archivo.
**Banderas unzip** 
 * `-v` #Muestra todos los archivos dentro, sin descomprimir.

 `tar [option] [file]` #agrupa archivos juntos y también puede comprimir y descomprimir.
 **Ejemplo tar:**
 `tar cfz csv.tar.gz *csv` #Agrupa todos los archivos que terminan en csv y lo guarda como `csv.tar.gz`, y los guarda en un `.tar.gz`.
 **Banderas tar**
  * `cfz` #"create file zip" Crea un archivo con compresión zip.
  * `xfz` #"extract file zip" descomprime un archivo con compresión zip.

## Crontab
### Orden
 * Minutó `[0-59]`
 * Hora `[0-23]`
 * Dia del mes `[1-31]`
 * Mes `[1-12]`
 * Dia de la semana `[1-12]`
 * comando a ejecutar `[comando]`

### Formas de introducir datos
 * `[n]` > `[3]` #Numero exacto.
 * `[n1,n2,n2]` > `[1,15,30]` #Lista de números.
 * `[*/n]` > `[*/5]` #Cualquier número divisible por 5.
 * `[n1-n2]` > `[1-10]` #Todos los números desde 1 al 10.
 * `[*]` > `[*]` #Todos los números.

**Nota:** Los números en cada columna tienen sus limites que puede ser apreciados en Orden.
 
 ### Ejemplos:
 `*	*	*	*	*	php ~/files/script.php >> ~/result.txt 2>&1` #Ejecuta el script cada minuto y concatena el <b>STD_OUT</b> y el <b>STD_ERROR</b> en el archivo `resultado.txt`
