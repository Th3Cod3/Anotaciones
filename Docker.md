# Docker

## Crear un contenedor
Es importante tener en cuenta que docker debe estar instalado.

`docker run [opciones] <nombre_de_imagen> [comando] ` **Ejemplo:** `docker run hello-world`

`<nombre_de_imagen>` Si el contener no existe, docker lo busca en su repositorio de contenedores en linea.

Docker trabaja de la siguiente manera, esta el cliente que se encarga de comunicarse con el deamon (servicio) de docker. lo interesante es que el deamon puede estar remoto o local.
## Estado de control basico de los contenedores
para ver todos los procesos de docker usamos `docker ps` pero este solo mostrara los procesos se estan ejecutando con `docker ps -a` mostrara todos los estados de los contenedores 

todos los contenedores tiene un ID único o un alias con ellos podemos ver o acceder a el contenedor. 

Por ejemplo para ver la informacion de un contenedor se usa `docker inspect <id_contenedor|alias_contenedor>` y si aparte quieres hacer un filtro se usa el flag `-f '<filtro>'` que es importante comprender que se hace con la sintaxis de GO ya que **docker esta escrito en GO** veamos un ejemplo `docker inspect -f '{{ json .Config.Env }}' <id_contenedor|alias_contenedor>` 

También podemos renombrar un contenedor con el comando `docker rename <container_name> <container_new_name>` o a la hora de crear el contenedor se le puede dar el nombre `docker run --name <nombre> <nombre_de_imagen>`

para ver el log de un contenedor "el output que se a registrado" se usa el comando `docker logs <nombre_contenedor>` y veras todo el output que ha generado.

para eliminar contenedores se puede hacer de la siguiente manera `docker rm <nombre_contenedor>` y si quiere eliminar todos los contenedores puedes hacer un poco de shell `docker rm $(docker ps -aq)` elimina todos los contenedores. ya que la bandera `-q` solo devuelve los ids de los contenedores.
## Mantener contenedores arriba
Si quiero que al crear un contenedor se ejecute usamos el comando `docker run -it <nombre_de_imagen>` esto creara y ejecutara el contenedor.

pero si queremos iniciar un contenedor ya existente podemos usar `docker container start [opciones] <id_contenedor|alias_contenedor> <comando>`

las banderas`-it` lo que hace es que hace que el std_output del contenerdor llega a tu linea de comando y lo hace interactivo. (se comunica el contenedor y tu terminal). *Asi fue lo que comprendí no fue muy claro*

Ahora si quiero ejecutar un comando en un contaneder puedesusar `docker exec -it <id_contenedor|alias_contenedor> <comando>` por ejemplo si tenemos un contenedor con nombre ubuntu-1 y queremos entrar a el bash usamos de la siguiente manera `docker exec -it ubuntu-con bash`  es importante que el contenedos debe estar corriendo.

Ahora si queremos que Docker cree un contenedor y no perder la CLI usamos la bandera `-d|--detach` que ignora el std_out del contenedor y queda mi Terminal libre. De este modo el contenedor queda trabajando y podemos hacer otras cosas con `docker exec`

## Conexión y volúmenes
Como ya sabemos un contenedor esta insolado de todo lo demás de mi computadora asi que cuando usamos un contenedor que hace uso de puertos estos solo serán accesibles por medio de su ip (dependiendo del la configuración de tu contenedor) o los podemos vincular directamente a un puerto de la maquina host (mi maquina) con la bandera `-p <puerto_maquina_host>:<puerto_contenedor>` asi que por ejemplo queremos un container con Apache

Docker también soporta vincular (bind) carpetas del host con las del contenedor. En caso que se elimine el contenedor este no ser afectado, pero no son recomendados ya que pueden ser cambiados desde el host (o un programa del host) `-v <host_path>:<docker_bind_path>` este es el primer método que integro docker y es el mas usado en las documentaciones, pero hay otra forma de poner volúmenes y es con los volúmenes de docker.

"`tmpfs` Temporary Memory File System, es un sistema de archivos que vive en la memoria ram, donde se puede usar para no escribir nada en el disco, lo que pasa es que cuando termina el proceso estos datos no persisten.

También tenemos los volúmenes de docker que son guardados en el docker area donde host filesystem no tiene acceso a el. solo se puede acceder po medio de docker, aunque en sistemas linux pueden ser localizados `/var/lib/docker/volumens`. para ver todos los volúmenes podemos usar `docker volume ls` con el comando `docker volume prune` eliminado todos los volúmenes que no estan en uso. Para crear un volumen usamos el comando `docker volumen create <volume_name>` también es posible conectar volumes de la nube como s3 de amazon.

Para iniciar un container y montarle un volumen usamos la bandera `--mount src=<volume_name>,dst=<container_bind_path>`

## Imágenes de docker
Las imágenes de docker son platillas de docker, con `docker pull <nombre_de_imagen>` traemos una imagen que no tenemos en nuestra maquina. Las imágenes son diferentes capas, una imagen viene con una imagen/capa base con las que empieza y hace cambios o une imágenes. con `docker image ls` podemos ver todas las imágenes que tenemos en nuestro sistema.

También podemos hacer nuestras propias imágenes es necesario crear un archivo con el nombre `DockerFile` dentro del archivos hay diferentes instrucciones que vamos a ver ahora
~~~DockerFile
#Siempre se require de una imagen base, se puede empezar con la imagen base scratch 
#La imagen scratch es el kernel de linux.
FROM <imagen_base> 

#Todos los comando que ejecutes ban afectar a la imagen y no a su sistema
RUN <comando bash>
~~~

después de eso podemos construir nuestra imagen con el comando `docker build -t <nombre_imagen>:<tag> .` el punto `.` se refiere que el `DockerFile` esta en la posición actual. ahora podemos ver esa imagen en nuestras imágenes con `docker image ls` y podemos usarla a la hora de ejecutar una imagen con `docker run -it <nombre_imagen>:<tag>`. también podemos compartir nuestra imagen en Docker Hub, con el plan gratuito puede solamente tener imágenes publicas.

Para saber que es lo que tiene una imagen se necesita el `DockerFile` si decea puedes ir a `DockerHub` buscar el Repositorio que deseas analizar y abres el `DockerFile` de la version deseada.
También Docker no da una herramienta para ver de que esta compuesta una imagen con el comando `docker history <option> <ubuntu>:<tag>` que da un resumen de la imagen. o con la bandera/option `--no-trunc` solo que es muy feo. pero tenemos una herramienta que se llama `dive` ve a su repositorio en GitHub `wagoodman/dive`.

Con dive puedo navegar entre las capas con las flechas, y con tab puedo cambiar de apartado. con <kbd>CTRL</kbd> + <kbd>U</kbd> puedes ver solo los cambias (no te muestra todo).

### Crear una imagen para una aplicación
file: `src/DockerFile`
~~~Dockerfile
# La capa base del dockerfile ya que vamos a trabajar en una aplicación de node traemos node
FROM node:8 
# Copia todo lo que tenemos en la carpeta actual (en este ejemplo tenemos nuestro codigo fuente)
COPY [".", "/usr/src/"]
# No movemos a un directoria ya que cuando ejecutemos npm debe estar en el directorio que esta el package.js
WORKDIR /usr/src
# Ejecutamos el comando de npm
RUN npm install
# esto es similar cuando un hace un bind del puerto con -p
EXPOSE 3000
# Este es el comando que se va a ejecutar cuando se corre el contenedor
CMD ["node", "index.js"] 
~~~

Después debemos hacer el build con `docker build -t platziapp .`

es bueno tener en cuenta que el orden de como esta escrito my dockerfile es muy importante para aprovechar la cache de docker en el ejemplo que tenemos arriba podríamos mejorar que el puerto expuesto valla en la segunda linea ya que es un comando que no cambia. y a la hora de copiar en vez de copiar full el directoria actual, copiar solamente el `package.json` y `package-lock.json` y después ejecutar el npm install. asi lo que son las dependencias hasta que no haya un cambio en ellas va a quedar en cache.

file: `src/DockerFile`
~~~Dockerfile
FROM node:8 
EXPOSE 3000
COPY ["package.json", "package-lock.json", "/usr/src/"]
WORKDIR /usr/src
RUN npm install
#Esto evita que cuando hacemos build no tenga que correr npm install cada vez que hacemos un cambio en nuestro código.
COPY [".", "/usr/src/"]
CMD ["node", "index.js"] 
~~~

## Redes
hay que tener en cuenta que un contenedor cuando hace refiere a localhost hace referencia a si misma no a nuestra maquina host. asi que si necesitamos conectar un contenedor con otro contenedor podemos usar redes.

con `docker network ls` muestra todas las redes. por defecto vienen 3 `bridge` que por motivos de retro-compatibilidad no se usan. también esta `host` que lo que hace es que hace que un contenedor funcione como si fuera la maquina host y hay que tener en cuneta que esto puede ser peligroso ya que un contenedor me puede abrir un puerto sin darme cuenta. por ultimo tenemos `null` lo que hace es que se le deshabilita completamente el networking. pero también podemos crear nuestras propias redes.

Para crear una red usamos `docker network create --attachable <network_name>` con la opción/bandera `--attachable` vamos a permitir que futuros contenedores se conecten a esa red.
ahora si queremos conectar un contenedor a una red `docker network connect <network_name> <contenedor>`. también podemos ver que contenedores estan conectados a una red con el comando `docker network inspect <network_name>`
Otro punto que es interesante es que si 2 contenedores estan conectados en la misma red puedes usas el nombre del contenedor para la conexión como nombre de host.

## Docker composer
`Docker-compose` es una herramineta que facilita la utilazacion de `Docker`
Con `docker` compose podemos describir la arquitectura de nuestra aplicacion.

Nosotros cuando hablamos de nuestra aplicacion hablamos de servicios y no de contenedores 

La diferencia entre un servicio y un contanedor es que un servicio puede tener mas de un contenedor.
~~~yml
version: "3"

services:
    app:
        image: platziapp
        enviroment:
            MONGO_URL: "mongodb://db:27017/test"
        # con esto le voy a dicir a docker composer que levanta ese servicio primero que este. pero no asegura cual va inicializar primero. docker composer no puede ver lo que ocurre en un contenedor.
        depends_on:
            - db
        ports:
         - "3000:3000"

    db:
        image: mongo
~~~

Ahora que tenemos el archivo ejecutamos el comando `docker-compose up` que lo que hace es que crea las networks y monta los containers los contenedores se les da un nombre con el siguiente orden `<nombre_carpeta|env_var>_<servicio>_<numero>`.

`docker-compose` tambien tiene muchos comando como docker, si quieres ver los container usamos `docker-compose ps` 
otros comandos
`docker compose down` elimina todas sus contenedores y networks
`docker-compose log <servicio>` muestra el log de un servicio.
`docker-compose exec [options] <servicio> <cmd>` ejecuta un comando en un servicio.

Tambien podemos hacer un build desde docker-compose y montar volumnes veamoslo modificando el file de arriba.
~~~yml
version: "3"

services:
    app:
        build: .
        enviroment:
            MONGO_URL: "mongodb://db:27017/test"
        # con esto le voy a dicir a docker composer que levanta ese servicio primero que este. pero no asegura cual va inicializar primero. docker composer no puede ver lo que ocurre en un contenedor.
        depends_on:
            - db
        ports:
         - "3000:3000"
         volumes:
            # Esto crea un bind mount
            - .:/usr/src
            # Indicas que no sobre escriba este archivo ya que en el build fue ejecutado.
            - /usr/src/node_module

    db:
        image: mongo
~~~