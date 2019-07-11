The general job control commands in Linux are:

 * `jobs` - list the current jobs
 * `fg` - resume the job that's next in the queue
 * `fg %[number]` - resume job [number]
 * `bg` - Push the next job in the queue into the background
 * `bg %[number]` - Push the job [number] into the background
 * `kill %[number]` - Kill the job numbered [number]
 * `kill -[signal] %[number]` - Send the signal [signal] to job number [number]
 * `disown %[number]` - disown the process(no more terminal will be owner), so command will be alive even after closing the terminal.

**ADD env. varible**
` export PATH="$PATH: $HOME/.composer/vendor/bin"`

# vim
**eliminar linea**
<kbd>d</kbd> + <kbd>d</kbd>
<kbd>`<numero de lineas>`</kbd> + <kbd>d</kbd> + <kbd>d</kbd>
ir a modo visual, seleccionar lo que quiere eliminar y <kbd>d</kbd>

# Introducion linea de comando
## Manejadores de paquetes (APT)
```bash
apt-get install <nombre del paquete> # Instalar un paquete
```
```bash
apt-get remove <nombre del paquete> # Eliminar paquete
```
```bash
apt-get --only-upgrade install <nombre del paquete> # Para actualizar un paquete en especifico
```
```bash
apt-get upgrade # Para actualizar todo el sistema
```
## DNS
### Comando dig

`TTL` "time to live" : es el tiempo que se define para refresque las direcciones ip.

Si un usuario hace una consulta a th3cod3.com y tiene el TTL en 500, después de 500 segundos se volverá hacer una petición al servicio DNS.

Permite hacer consultas a DNS de cualquier dominio

`dig th3cod3.com`
 * Ver dirección asociadas a un dominio

`dig th3cod3.com NS`
 * Ver quien esta administrando el dominio

`dig th3cod3.com MX`
 * Ver registro MX configurado

`dig th3cod3.com MX @DNS`
 * Ver registro A en un otro dns

### Tipos de Subdominio

 * **A:** Apunta a una dirección IP, debe ser una dirección valida a nivel mundial, puede ser privada o pública
 * **CNAME:** Alias a cualquier otro dominio que se encuentre en Internet
 * **MX:** Permite saber hacia donde va ir el trafico del correo
 * **TXT:** Permite configurar texto plano que soporte los dominos, es comúnmente usado para evitar el spam en los correos
 * **AAA:** Usado para IPv6

 ## Sistema de archivos

`/` Raiz
`/bin` Comandos **`cli`** *"Command Line Interface"*, para todos los usuario.
`/boot` arranque inicio sistema
`/dev` Dispositivos como: disco duro, teclado, mouse, etc. 
 * [Explicaciones acerca de /dev](http://gulix.cl/wiki/Explicaciones_acerca_de_/dev)
 * `/dev/zero` ceros (Se puede hacer formateo de disco llenándolo de ceros)
 * `dev/null`tuberías

`/etc` configuraciones del sistema
 * `/etc/passwd` Donde están todos los usuarios del sistema.

`/home` Donde se guardan todos los usuarios y sus archivos tambien accesible por `~`
`/lib, lib32, lib64` librerías
`/media` usb´s
`/mnt` discos duros externos
`/opt` archivos de terceros
`/proc` sistema archivo virtual dinámico, procesos corriendo
`/root` usuario especifico de root
`/sbin` comandos de super usuario, solo root
`/sys` interactuar con el kernel o el sistema, interactuar con teclado
`/etc` Configuración de todos los programas o kernel 
`/usr` programas instalar con manejadores de paquetes
`/var` información variable, cache, logs.

## Usuarios

`sudo su - [usuario]` 
 * Ingresar como root si no ingresa el usuario, el guion es para cargar un **ENV** *(Environment)* nuevo.

`adduser <nombre_usuario>`
 * Crea un usuario.

`logout`, `exit` o <kbd>ctrl</kbd> + <kbd>d</kbd>
 * permiten salir de la sesión de la consola por la que se ingresó.

`deluser <nombre_usuario>`
 * Elimina usuario, no archivos del usuario

`fdisk -l`
 * Como está configurado el sistema de archivos del disco

`less /etc/passwd`
 * Como root permite conocer los usuarios que id tienen asignados y otra info adicional

`passwd <nombre_usuario>`
 * El root puede forzar la contraseña.

## SSH

`ssh <usuario>@<ip> -i <llave_ssh>`
 * Crea una conexión SSH con el servidor SHH 

## SCP
`scp -i <llave_ssh> <archivo> <usuario>@<ip>:[save_location]`
 * Enviá un archivo a el servidor por medio de SFTP
`scp -i <llave_ssh> <usuario>@<ip>:<archivo> <save_location>`
 * Descargar un archivo del servidor por medio de SFTP
## Repositorios/Paquetes

`apt-get update`
 * Carga la lista de paquetes que esta en los repositorios que tienes en el archivo `/etc/apt/sources.list`

`apt-get upgrade`
 * Actualiza todos los paquetes que están instalados.

`apt-cache search <búsqueda>`
 * Busca en las listas de paquetes que tengo.

`apt-get install <paquete[ paquete ...]>`
 * Busca en las listas de paquetes que tengo.

### Paquetes recomendados
 * `htop` es un upgrade del paquete top, para monitorear el sistema (procesos).
	* Muestra la cantidad de uso de cada core.
	* La cantidad de RAM y uso.
	* La cantidad de SWAP (*Es la cantidad de memoria en disco para cuando se llena la RAM*) y uso.
	* La cantidad de tareas que se estan ejecutando
	* La carga. (Hay tres numero para ver la cantidad de procesos que se estan ejecutando. El primero es por 1 minuto, el segundo por 5 minutos y el tercero el por 15 minutos)
	* Uptime el tiempo que lleva el equipo encendido
 * `screen` *emulador de terminal con sesiones*
	* `screen -S <nombre_ventana>` Crea una venta con el nombre.
	* `screen -r [nombre_ventana|PID]` Entra a una ventana.
	* `screen -ls` Muestra las ventanas disponibles.
	* `screen -d -m <comando>` crea una ventana en segundo plano y la cierra cuando termine de ejecutar el comando/ejecución.
**Los shortcuts solo funcionan cuando estas en un screen.**
	* <kbd>CTRL</kbd>+<kbd>a</kbd> <kbd>c</kbd> crea una ventana nueva.
	* <kbd>CTRL</kbd>+<kbd>a</kbd> <kbd>p</kbd> Muestra la ventana anterior.
	* <kbd>CTRL</kbd>+<kbd>a</kbd> <kbd>`<number>`</kbd> Salta a la venta con ese numero.
	* <kbd>CTRL</kbd>+<kbd>a</kbd> <kbd>n</kbd> Muestra la siguiente ventana.
	* <kbd>CTRL</kbd>+<kbd>a</kbd> <kbd>k</kbd> Elimina la ventana actual.
	* <kbd>CTRL</kbd>+<kbd>a</kbd> <kbd>\\</kbd> Elimina todas las ventanas.
	* <kbd>CTRL</kbd>+<kbd>a</kbd> <kbd>"</kbd> Para cambiar de venta pero en forma de lista. muestra una lista de las ventanas y sus nombre para seleccionar.
	* <kbd>CTRL</kbd>+<kbd>a</kbd> <kbd>A</kbd> Cambia el nombre de la ventana.
	* <kbd>CTRL</kbd>+<kbd>a</kbd> <kbd>|</kbd> Divide la pantalla verticalmente.
	* <kbd>CTRL</kbd>+<kbd>a</kbd> <kbd>S</kbd> Divide la pantalla horizontal.
	* <kbd>CTRL</kbd>+<kbd>a</kbd> <kbd>TAB</kbd> Cambia de division.


## Compresión y empaquetamiento de archivos

Deferencia entre empaquetar y comprimir son:
 * **Empaquetar** es reunir archivos y ponerlos juntos.
 * **Comprimir** es codificar estos archivos para que baje su peso.

 `zip [nombre.zip] [files|path]`

 **Ejemplo zip:**

`zip archivoscsv.zip *csvEste comando comprime todos los archivos que terminen con la palabra csv.

`unzip [option] <file>[.zip]` # Descomprime un archivo .zip
**Banderas unzip** 
 * `-v` # Muestra todos los archivos dentro, sin descomprimir.

 `tar [option] [file]` #agrupa archivos juntos y también puede comprimir y descomprimir.

 **Ejemplo tar:**

 `tar cfz csv.tar.gz *csv` # Agrupa todos los archivos que terminan en csv y lo guarda como `csv.tar.gz`, y los guarda en un `.tar.gz`.

 **Banderas tar**
  * `cfz` #"create file zip" Crea un archivo con compresión zip extension `.gz`.
  * `xfz` #"extract file zip" descomprime un archivo con compresión zip.
`gzip -d <file>` # Descomprime un .gz

## Navegar por internet
 * `wget` para descargar contenido de internet
 * `w3m` para hacer consultas HTTP (No soporta JS)


## Firewall
[Tutorial DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-set-up-a-firewall-with-ufw-on-ubuntu-16-04)
`sudo ufw default deny incoming`
 * Denegar todas las peticiones que entren

`sudo ufw default allow outgoing`
 * Permitir todas las peticiones de salida

`sudo ufw allow ssh`
 * Permitir conexiones ssh
`sudo ufw allow 22`
 * Permitir conexiones por el puerto 22 (ssh)

`sudo ufw enable`
`sudo ufw disable`
 * Activar/desactivar el firewall

`sudo ufw allow 6000:6007/tcp`
`sudo ufw allow 6000:6007/udp`
 * Permitir un rango de puertos y con un protocolo especifico 

`sudo ufw allow from 15.15.15.51`
`sudo ufw allow from 15.15.15.51 to any port 22`
 * Permitir conexiones a un ip o a un rango en especifico o a un puerto en especifico


`sudo ufw status numbered`
 * Ver estado del firewall y permisos

`sudo ufw reset`
 * elimina toda la configuración del firewall

## Administrar disco y particiones
Un disco duro se puede ver como una torta donde se puede dividir en diferentes sectores.
En amazon para crear discos duros se agrega un volumen.
**NOTA:** es muy importante que la zona sea la misma que la instancia.

### Particiones
Existen dos tipos de tablas de particiones, la que siempre se usa o viene por defector es `DOS` que puede tener 4 particiones primarias y muchas lógicas y GPT me permite tener infinidad de particiones

El primer comanddo que vamos a usar es `fdisk` que sirve para manejar nuestros discos.
* `fdisk -l` muestra todas las unidades que estan conectadas.
* `fdisk <nombre_del_disco>`

Ahora vamos a manejar una partición en `DOS` la que nos deja tener 4 particiones primarias y dentro de una partición lógica se puede crear cuantas particiones que ud desé. 
Si no tiene ninguna particion logica no podra crear particiones extendidas

Despues de haber entrado al disco podremos craer particiones y confiugurar el typo de particiones etc. Con e comando `m` se podra ver un meno de todos los comandos. `n` para crear una particion, `p` para ver las particiones, `t` para cambiar el tipo de particion, etc.

**Nota** es muy importante que se guarde la configuracion con `w` si no ningu de los cambios sera efectuado.

* `fdisk -l <nombre_del_disco>`De esta manera se puede ver las particiones de un disco, sin entrar en el disco.

### Formatear una partición
El sistema de archivos es la forma en la que se va guardar los datos. No hay un sistema archivo mejor cada uno tiene su beneficios y deficiencias.

para formartear un particiopn