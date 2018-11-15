# TERMINAL

## Comandos

`man [cmd]`  >muestra el manual de el comando.
 * <kbd>space</kbd> >siguiente página.
 * <kbd>enter</kbd> >siguiente línea.
 * <kbd>b</kbd> >retrocede una página.
 
`pwc` >muestra el directorio actual.

`cd [directorio]` >te lleva al directorio.

## Extra
 *	`~` \* directorio home
 *	`/` \* directorio root
 *	`..` >directorio padre
 *	`.` >directorio actual
 *	`\` >caracteres especiales (ej. `cd "mis fotos"` sino `cd mis\ fotos`)
`ls [options] [file|dir]` \* muestra una lista de archivos/carpetas que hay en el directorio. 
Banderas|Flag
•	-a >muestra todos los archivos/carpetas, hasta las escondidas
•	-d >lista los directorios
•	-l >muestra los permisos
•	-lh >legible por humanos (cambia el peso en potencias MB, GB, etc)
•	-R >lista recursivamente (muestra los subdirectorios)
•	-X >hace un sort por extensión
•	-t >hace un sort por tiempo
•	-S >hace un sort por peso
mkdir <nombre del directorio> >crea una carpeta.
touch <nombre archivo> >crea un archivo vacío o si existe cambia la fecha de edición.
mv <archivo> <destino/[nombre a guardar]> >mueve o renombra un archivo
rm [option][directorio] >elimina un archivo o carpeta.
Banderas|Flag
•	-r, -R >elimina recursivamente (elimina los subdirectorios y su contenido)
•	-f >elimina forzoso sin preguntar si está seguro.
•	-i >pregunta antes de eliminar cualquier cosa
cp [archivo] [destino/[nombre a guardar]] >copia el archivo en el destino deseado
pushd >agrega un directorio a la pila 
popd >vuelve al directorio de la pila
tail [option][file] >muestra las últimas líneas de un archivo.
•	-<numero> >muestra la cantidad de líneas deseadas.
•	-f >muestra las últimas líneas y actualiza cada vez que haya un cambio.

more [file] >imprime en pantalla el contenido del archivo, con paginación.
•	<space> >siguiente página.
•	<enter> >siguiente línea.
•	<b> >retrocede una página.
•	<q> >sale del archivo
cat [option] [file] >imprime en pantalla el contenido del archivo, sin paginación.
alias [aliasname=’comando’] >crea un atajo para un comando.
top >muestra los procesos
kill [option] [PID] >mata el proceso con el PID enviado.
grep [option] pattern [file] >Busca patrones dentro de los archivos, si invoca atrás de un pipe | lo busca en el resultado del comando que viene su tras.
Ejemplo: grep -r . -e .php >este comando me devuelve todos las líneas que contienen .php desde la carpeta raíz hacia abajo.
•	-r >recursivamente.
•	-e >patrón den regexp.
•	-n >numero de la línea que encontró el patrón
wc [option] [file] >cuenta cantidad de líneas, palabras, espacios entre otros.
•	-l >cuenta cantidad de líneas.
•	-w >cuenta cantidad de palabras
•	-c >cuenta cantidad de bytes
•	-m >cuenta cantidad de letras

Shortcuts
[CTRL] + [L] = Limpia pantalla.
[CTRL] + [R] = Busca un comando ejecutado.
Streams
Hay dos salidas, estándar output y standard error
2>&1 >STD_ERROR se envía al pointer de 1
; >concateno procesos.
Standard Input
Es lo que enviamos a un programa y se puede enviar por la terminal con el Signo menor que “<”
Standard Output
Las respuestas del input sin errores con la cual se puede concatenar o recibir por medio de un uno “1” y el símbolo mayor que “<comando/programa>  1> <nombre archivo>” con el cual creamos un archivo y ingresamos el STD_OUT, El número uno está de más ponerlo ya que por default envía solamente el STD_OUT. Y si en vez de introducir un signo mayor que ponemos 2 “<comando/programa> 1>> <nombre archivo>” concatena al archivo mencionado.
Standard Error
En este caso es similar al STD_OUT con diferencia que en este es necesario llamarlo con el número 2 “<comando/programa> 2> <nombre archivo>” y “<comando/programa> 2>> <nombre archivo>”



	
https://www.rapidtables.com/code/linux/index.html Interesante link de los comando y banderas
