# Porque Laravel?
Laravel cada dia su comunidad es mas grande. po lo cual lo es fácil de buscar una solución en el.

# Como instalar Laravel
con composer se usa `composer global require laravel/installer`
esto hace que laravel se instale globalmente. eso hace que podamos usar laravel desde cualquier parte.

Los comando que se usa en laravel son: 
* help | Muestra una ayuda
* list | Muestra todos los comandos
* new | Crea una aplicación con laravel

Vamos a usar el comando new para eso escribimos `laravel new <nombre>` eso se encarga de crear el archivo composer y instalar todas las dependencias y crear todos los archivos básicos de laravel.

después de que todo este listo con `php artisan serve` posemos levantar un servidor

# rutas
el archivo donde están las rutas esta en `laravel/routes/web.php` por defecto laravel tiene la ruta `/` ya definida con un get. el código ejemplo es el siguiente 

~~~php
Route::get('/', function () {
	return view('welcome');
});
~~~
* El primer parametro es la ruta, las rutas no se puede repetir. 
* el segundo parametro es un clausure. 
  * donde tenemos la method view que es del template manager `Blade` los templetes se encuentran en `Laravel/resources/views/<filename>.blade.php`