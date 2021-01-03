The general job control commands in Linux are:
- [vim](#vim)
- [Introducion linea de comando](#introducion-linea-de-comando)
  - [Manejadores de paquetes (APT)](#manejadores-de-paquetes-apt)
  - [DNS](#dns)
    - [Comando dig](#comando-dig)
    - [Tipos de Subdominio](#tipos-de-subdominio)
  - [Sistema de archivos](#sistema-de-archivos)
  - [Usuarios](#usuarios)
  - [SSH](#ssh)
  - [SCP](#scp)
  - [Repositorios/Paquetes](#repositoriospaquetes)
    - [Paquetes recomendados](#paquetes-recomendados)
      - [htop](#htop)
      - [screen](#screen)
      - [tmux](#tmux)
  - [Compresión y empaquetamiento de archivos](#compresión-y-empaquetamiento-de-archivos)
    - [zip](#zip)
    - [tar](#tar)
    - [gzip](#gzip)
  - [Navegar por internet](#navegar-por-internet)
  - [Firewall](#firewall)
  - [Administrar disco y particiones](#administrar-disco-y-particiones)
    - [Particiones](#particiones)
    - [Formatear una partición](#formatear-una-partición)
      - [Sistemas de archivos](#sistemas-de-archivos)
      - [formatear una particion](#formatear-una-particion)
      - [Montar una particion manual](#montar-una-particion-manual)
      - [Montar particion automáticamente con /etc/fstab](#montar-particion-automáticamente-con-etcfstab)
    - [Partición SWAP](#partición-swap)
      - [Como crear una particion SWAP](#como-crear-una-particion-swap)
    - [Generar imágenes de disco](#generar-imágenes-de-disco)
    - [LVM](#lvm)
      - [Physical Volume (Union de discos)](#physical-volume-union-de-discos)
      - [Volume Group (Discos|volumenes)](#volume-group-discosvolumenes)
      - [Logic Volume (Particiones)](#logic-volume-particiones)
    - [Arranque del sistema](#arranque-del-sistema)
      - [MBR:](#mbr)
      - [UEFI:](#uefi)
      - [GRUB](#grub)
  - [System](#system)
    - [Apagar un servidor](#apagar-un-servidor)
    - [runlevels](#runlevels)
    - [Systemd](#systemd)
    - [Variables de entorno](#variables-de-entorno)
    - [Manejo de usuarios y permisos](#manejo-de-usuarios-y-permisos)
    - [Configuraciones del kernel](#configuraciones-del-kernel)
    - [Configuracion de red](#configuracion-de-red)
    - [Firewall](#firewall-1)
  - [Edit file](#edit-file)

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
#### htop
* `htop` es un upgrade del paquete top, para monitorear el sistema (procesos).
* Muestra la cantidad de uso de cada core.
* La cantidad de RAM y uso.
* La cantidad de SWAP (*Es la cantidad de memoria en disco para cuando se llena la RAM*) y uso.
* La cantidad de tareas que se estan ejecutando
* La carga. (Hay tres numero para ver la cantidad de procesos que se estan ejecutando. El primero es por 1 minuto, el segundo por 5 minutos y el tercero el por 15 minutos)
* Uptime el tiempo que lleva el equipo encendido
#### screen
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
#### tmux
* `tmux` *emulador de terminal con sesiones*
* `tmux new -s <session name>` crea una session con un nombre.
* `tmux ls` Muestra todas la sesiones.
* `tmux attach -t <session name>` Conecta a una session.
* `tmux rename-session <current name> <new name>` nombra una session una session.
* `tmux kill-session -t <current name>` Cierra una session.
* <kbd>CTRL</kbd>+<kbd>b</kbd> <kbd>c</kbd> crea una ventana nueva, pero en segundo plano.
* <kbd>CTRL</kbd>+<kbd>b</kbd> <kbd>`<number>`</kbd> Salta a la venta con ese numero.
* <kbd>CTRL</kbd>+<kbd>b</kbd> <kbd>%</kbd> Divide la pantalla en vertical
* <kbd>CTRL</kbd>+<kbd>b</kbd> <kbd>"</kbd> Divide la pantalla en horizontal
* <kbd>CTRL</kbd>+<kbd>b</kbd> <kbd>ARROW</kbd> Cambia de pantalla hacia la dirección que allá oprimido
* <kbd>CTRL</kbd>+<kbd>b</kbd> <kbd>,</kbd> renombrar la ventana
* <kbd>CTRL</kbd>+<kbd>b</kbd> <kbd>d</kbd> Detach the la session y queda en segundo plano


## Compresión y empaquetamiento de archivos

Deferencia entre empaquetar y comprimir son:
* **Empaquetar** es reunir archivos y ponerlos juntos.
* **Comprimir** es codificar estos archivos para que baje su peso.

### zip
`zip [nombre.zip] [files|path]`

**Ejemplo zip:**

`zip archivoscsv.zip *csvEste comando comprime todos los archivos que terminen con la palabra csv.

`unzip [option] <file>[.zip]` # Descomprime un archivo .zip
**Banderas unzip** 
* `-v` # Muestra todos los archivos dentro, sin descomprimir.

### tar
`tar [option] [file]` #agrupa archivos juntos y también puede comprimir y descomprimir.

**Ejemplo tar:**

`tar cfz csv.tar.gz *csv` # Agrupa todos los archivos que terminan en csv y lo guarda como `csv.tar.gz`, y los guarda en un `.tar.gz`.

**Banderas tar**
* `cfz` #"create file zip" Crea un archivo con compresión zip extension `.gz`.
* `xfz` #"extract file zip" descomprime un archivo con compresión zip.

### gzip
`gzip -d <file>` # Descomprime un `.gz`
`gzip <file>` # comprime un `.gz`

## Navegar por internet
* `wget` para descargar contenido de internet
* `curl` para hacer consultas HTTP.
* `w3m` Para navegar por internet y interpretar el resultado (es como un browser, pero no soporta JS)


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
#### Sistemas de archivos
* File systems vinculados con MS-DOS/Windows
  * **FAT** *"File Allocate Table"* Este sistema es de 16 bits y soporta volumenes/particiones de máximo 4GB, es el mas eficiente y rapido para particiones de menos de 200MB. El tamaño del sector es entre 16KB a 64KB y solo soporta un archivo por sector y acepta archivos de máximo 2GB.
  * **FAT32** *"File Allocate Table 32"* Este sistema es de 32 bits y soporta volumenes/particiones de máximo 2TB, es mas eficiente entre particiones entre 512MB hasta 32GB. Puede reducir el tamaño de sus sectores hasta 4 KB y acepta archivos de máximo 4GB
  * **NTFS** *"New Technology File System"* Soporta particiones de hasta 2TB, pero es mas eficiente para particiones de mínimo 512MB hasta 2TB.
* File systems de Linux
  * **EXT** *"Extended File System"* 
  * **EXT2** *"Extended File System 2"* 
  * **EXT3** *"Extended File System 3"* 
  * **EXT4** *"Extended File System 4"* 
  * Son muy similares, las diferencia esta mas tanto en la capacidad del tamaño.
#### formatear una particion
**`mkfs.<file_system> <partition>`** con este comando es para formatear la particion deseada ***Nota:*** la particion sera formateada y perderás todos los archivos que tenga en ella.
***Ejemplo*** `mkfs.ext4 /dev/xvdf1 `

#### Montar una particion manual
Hay que crear la carteta donde va ir montada la particion con el comando: **`mount <partition> <file_location>`**
***Ejemplo*** `mount /dev/xvdf1 /etc/xvdf1`

`df -h` muestra como están montadas las carpetas y cuanto espacio tienen

Si se desea hacer un cambio en una particion hay que desmontar la particion y es importante que no esten dentro de la particion por que le puede salir que ud esta usando la particion `umount <file_location>`

#### Montar particion automáticamente con /etc/fstab
https://wiki.archlinux.org/index.php/Fstab_(Espa%C3%B1ol)
se tiene que editar el archivo `/etc/fstab` con el siguiente orden separado por espacio o tab

~~~Shell
#/ <partition>  <file_location>  <type>  <options>  <dump>  <pass>
/dev/xvdf1  /etc/xvdf1  ext4  default,discard 0  0
~~~

### Partición SWAP
La particion swap es una particion de emergencia para cuando la memoria RAM esta llena, porque de emergencia? Por que la swap es muy lenta comparada con cualquier tipo de dispositivo de almacenamiento como SSD. Esto puede causar que tu sistema se relentice.
#### Como crear una particion SWAP
hay que crear una particion con id 82 que es Linux Swap y con file system.
* Entrar al disco `fdisk /dev/xvdf`
  * Cambiar el id `t` (type)
  * Seleccionar particion `<numero>`
  * Seleccionar id *Nota: **L** (list) para ver la lista de id* `<numero>`
  * Guardar los cambios `w` (write)
* Formatear el File System `mkswap /dev/xvdf<particion>`
* Montar la Swap manualmente
  * Montar la swap manualmente `swapon /dev/xvdf<particion>`
  * Desmontar la swap `swapoff /dev/xvdf<particion>`
* Montar la Swap automáticamente
  * Hay que editar el fstab `vim /etc/fstab`
~~~Shell
#/ <ID>  <type>  <options>  <dump>  <pass>
UUID=<ID>  swap  sw  0  0
~~~

### Generar imágenes de disco
Esto puede ser usado para crear backups de particiones.

Copiamos información en la particion que vamos hacer una imagen del para eso copiamos contenido en la particion.
* `cp -ra <cp_folder|file> <destino>` Copia toda una carpeta recursivamente y guarda todos los permisos y usuarios.
  * `-r` Recursivamente
  * `-a` Todo (Permisos, contraseñas, etc)

con `du -sh` vemos cuanto uso de memoria hace la carpeta actual recursivamente.

Es muy buena practica con la particion desmontada, ya que una particion 
mantienes escribiendo procesos de cache y va estar trabajando y haciendo cambio.

Para hacer la copia de la particion se usa `dd` este comando hace una copia exacta bit a bit. Es muy importante tener en cuenta que dd crear una copia de la particion completa por lo tanto si la particion tiene 10MB de datos y la particion es de 100MB el va a clonar los 100MB.
`dd if=<input_file> of="<output_file> bs=<bit_read_size >` 
**Ejemplo:** `dd if=/dev/xvdf2 of=/disk/xvdf3/backup_xvdf2 bs=1M`

para restaurar es de la misma manera, pero esta vez la particion a la que se va a escribir debe ester desmontada
**Ejemplo:** `dd of=/dev/xvdf3 if=/disk/xvdf3/backup_xvdf2 bs=1M`

**Escribir un archivo de ceros**
`dd if=/dev/zero of=zeros100M bs=1M c=100`

**Leer un archivo para medir la velocidad del disco**
`dd if=zeros100M of=/dev/null`

**Montar una imagen en una USB**
`mount debian.iso /dev/sdb6 -o loop`

### LVM
[Que es LVM](https://www.youtube.com/watch?v=R1lMzb0sKMQ)
LVM *(Logical Volume Manager)* es un sistema de manejo de discos donde puede tener varios discos y agruparlos para que se comporte como un solo volumen.
para instalarlo.
`apt-get install lvm2`

Hay que entender que existe pv *(Physical Volume)* y vg *(Volume Group)*
pv: son las particiones/discos que hacen parte del volumen
vg: es un grupo de discos a los que se crean como una particion

#### Physical Volume (Union de discos)
para agrgar una particion/disco al areglo de lvm usamos el comando `pvcreate <particion|disco>` ejemplo `pvcreate /dev/xvdf1`. Si decea eliminar una particion es `pvremove <particion|disco>` ejemplo `pvremove /dev/xvdf` para ver todos los pv montados se usa `pvs` o con `pvdisplay` se ve mucho mas detallado.

#### Volume Group (Discos|volumenes)
*  Ahora creamos grupos a lvm usamos `vgcreate <nombre> <particion>` ejemplo `vgcreate databases /dev/xvdf1` es bueno practica poner nombre distintivos 
*  Si quieres agregar mas disco a ese grupo se hace con `vgextend <nombre> <particion>` ejemplo `vgextend databases /dev/xvdf2`. 
* Para quitar una particion|disco se usa `vgreduce <particon|disco>` 
*  para ver los grupos `vgs` o con `vgdisplay` se ve mucho mas detallado.

#### Logic Volume (Particiones)
*  Creamos el volumen de la siguiente manera `lvcreate -n <name> -L <length> <volume_group>` ejemplo `lvcreate -n postgres -L 10g databases`. 
*  Esto ya se comportara como una particion a la cual se tiene que formatear y montar después de ser creada. 
*  Si desea agregar mas espacio a una particion `lvextend -L+<length> <logical_volumen>` ejemplo `lvextend -L+10G /dev/databases/postgres` **NOTA:** Hay que modificar el file system, ext4 permite extender en caliente. Se hace de la siguiente manera `resize2fs <particion>`.
* Para acceder a esa particion es `/dev/databases/postgres` 
* Para ver los volumenes `lvs` o con `lvdisplay` se ve mucho mas detallado.

### Arranque del sistema
#### MBR: 
Es una tecnologia viaja, que coloca 512bitys al inicio de la particion de donde unos esta arancando el sistema. este se encarga de decirle al sistema de guardar los links de donde estan los kernel, controladores, dispositivos, entre otros.
#### UEFI:
Salio para tener boots seguros y que esten firmados, por que muchos virus se alojaban ahi para arrancar con ellos. Tambien chequea si todos los componentes esten trabajando como RAM, Controladores de videos, etc.

#### GRUB
Es un bootloader es el que se encarga de cargar el kernel, driver etc.
El grub se encunetra en `/etc/grub.d/` y en `/boot/grub/grub.cfg` es el archivo compilado del grub no se debe modificar, si desea hacer un cambio debe ser en `/etc/default/grub` y debes ejecutar `update-grub2` para compilar.

El GRUB se pone en el MBR, en el disco primario. Si deseas hacer una copia del MBR se puede hacer con `dd` de la siguiente manera `dd if=/dev/xvda of=/root/mbr_backup bs=512 count=1`

## System

### Apagar un servidor

**TIPS:**
* Es bueno verificar que servidor es el que va apagar
* Tener en cuneta que Linux no pregunta si lo quiere apagar. Solamente apaga!

`shutdown -r now` para reiniciar el servidor ahora
`shutdown now` para apagar el servidor ahora
`systemctl reboot` para reiniciar el servidor ahora
`systemctl poweroff` para apagar el servidor ahora

### runlevels
Es la manera en la que se arrancaba un sistema, se derivada de 6 nivels actualmente los puedes ver en `/etc/rc<nivel>d` en estas carpeta se organizaban de tal manera que en cada nivel habia que matar o arrancar ciertos servicios.

El formato de los nombre consistia de una letra 2 digitos y el nombre
Letra podia ser K para matar el servico y S para arrancar el sistema el numero era la prioridad.

### Systemd
Es como un arbol de servicio donde un servicio puede depender de un servicio y si ese servio muere tambine muere sus hijos.

Con `systemctl list-dependencies <servicio>` muestra las dependencias del servio al que se haga la consulta.

Con `syustemctl` salen todos los servicios y su estado

Si quiero ver info de un servicio puede verlo con `systemctl show apache2` muestra todo sobre ese servicio.

tambien con `systemctl status apache2` ves el estado del servicio.

### Variables de entorno
Son variables que se guardan en nuestro usuario, la manera mas facil de agregar una variable de entorno es `VARIBLE_NAME="CONTENIDO DE LA VARIABLE"` de esta manera solo se tendra esa variable por lo que dure la session y si quiere entrar a esa variable debe de ser de esta manera `echo $VARIABLE_NAME`, para guardar una variable de entorno se puede hacer desde el archivo `~/.profile` o `~/.bashrc` esto seria para un usuario en concreto. Si desea crear una variable de entorno para todos los usuarios puede ser esitado el archivo `/etc/bash.bashrc` y poner `export VARIABLE="/home/th3cod3"`

### Manejo de usuarios y permisos
Para crear un usuario se usa el comando `adduser <nombre_usuario>` en si este es un script que ejecuta diferentes comandos, `useradd` que es el comando que en si crea usuarios, y tambien crea un grupo, el directorio home, contraseña y informacion del usuario.

En `/etc/passwd` estan todos los usuarios del sistema con su identificador, home y bash y en el archivo `/etc/shadow` se almacenan todas las contraseñas encryptadas.

En Linux es muy importante los grupos, los usuarios que tiene ese grupo puede acceder a ese archivo. Si quieres ver a que grupos esta vinculado su usuario lo puedes hacer con el comando `groups [usuario]`. Para agregarle un grupo a un usuario es de la siguiente manera `addgroup <usuario> <grupo>` es necesario volver a logearse para que refresque los grupos nuevos. En `/etc/group` se guardan toso los grupos con id.

### Configuraciones del kernel
En el archivo `/etc/security/limits.conf` se encuentra la configuración de límites del kernel

!!!! TEMA A PROFUNDISAR !!!!!

###Permisos
Cuando se hacer un `ls -l` se puede ver todos los archivos y sus permisos, owner y grupo que son muy importantes ya que a base de esto se puede manipular el archivo.

`x xxx xxx xxx user group` el primer caracter representa el tipo del archivo, despues de eso se divide entre 3 caracters que son a quien va dirigido esos permisos, se divide de la siguiente manera `owner group others` y cada grupo se divide con 3 permisos `execute read write`, 
Para cambiar permisos usa el `chmod [permisos] <archivo>`

Se puede dar permisos con numeros enteros que representan un numero binario a la suma de permisos `execute = 4`, `write = 2` y `read = 1`binario con el mismo orden `111 = 7 | 001 = 1 | 010 = 3`


hay diferente formas de dar permisos, hay que tener en cuenta que si se le quiere dar permiso a el owner con `u`, grupo con `g` y otros con `o` seguido de un simbolo que puede ser `-` para quitar el permiso, `+` para agregar permisos y `=` para restaurar los permisos,  
Ejemplo `chmod o+x archivo.sh` agrega la opcion para que other pueda ejecutar este archivo.

Para cambiar el dueño del archivo se usa `chown <usuario[:]> archivo` si se agrega adelante del usuario el doble punto esto tambien cambiara el grupo. 

### Configuracion de red

!!!! TEMA A PROFUNDISAR !!!!!

### Firewall
Hay que tener en cuenta que el firewall trabaja en cascada, es dcecir que si tengo una regla arriba que bloquea todas las conexiones y despues una regl;a que permite una conexion. esta no funcionara ya que el firewall funciona como una cascada.

`iptable -L` ver la tabla de INPUT, FORWARD, OUTPUT
`iptable -L -t nat` ver la  tabla de la nat que tiene la tabla de PREROUTING, INPUT, OUTPUT, POSTROUTING.
`iptable  `


## Edit file
I would use the following approach in the Dockerfile:
`echo "Some line to add to a file" >> /etc/sysctl.conf`

That should do the trick. If you wish to replace some characters or similar you can work this out with sed by using e.g. the following:
`sed -i "s|some-original-string|the-new-string |g" /etc/sysctl.conf`

## Update wsl time
`sudo hwclock --hctosys`