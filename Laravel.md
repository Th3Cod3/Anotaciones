# Porque Laravel?
Laravel cada dia su comunidad es mas grande. por lo cual lo es fácil de buscar una solución en el.

# Como instalar Laravel
con composer se usa `composer global require laravel/installer`
esto hace que laravel se instale globalmente. eso hace que podamos usar laravel desde cualquier parte.

Los comando que se usa en laravel son: 
* help | Muestra una ayuda
* list | Muestra todos los comandos
* new | Crea una aplicación con laravel

Vamos a usar el comando new para eso escribimos `laravel new <nombre>` eso se encarga de crear el archivo composer y instalar todas las dependencias y crear todos los archivos básicos de laravel.

después de que todo este listo con `php artisan serve` posemos levantar un servidor o si se hace uso de apache podemos mover el `DOCUMENT_ROOT` a la carpeta `public` o la carpeta `src`, pero dentro necesitaríamos un `.htaccess`.

~~~.htaccess
RewriteEngine On
RewriteCond %{THE_REQUEST} /test/([^\s?]*) [NC]
RewriteRule ^ %1 [L,NE,R=302]
RewriteRule ^((?!test/).*)$ test/$1 [L,NC]
~~~

## Permissions
Algo muy importante de laravel es que las carpeta `store` y `bootstrap/cache` necesitan permiso de escritura.
~~~shell
chown -R root:www-data /var/www/html/storage
chown -R root:www-data /var/www/html/bootstrap/cache

chmod -R 776 /var/www/html/storage
chmod -R 776 /var/www/html/bootstrap/cache
~~~
# Ciclo de vida una solicitud
Laravel tiene su FrontController que es el que se encarga a poner el Framework a trabajar.
primero ejecuta `require __DIR__.'/../vendor/autoload.php';` que se encarga de la carga automatica de archivos.
Después enciende el framework `$app = require_once __DIR__.'/../bootstrap/app.php';`

Ahora el framework enciende el router para enlazar las consultas, el archivo donde están las rutas esta en `laravel/routes/web.php` por defecto laravel tiene la ruta `/` ya definida con un get. el código ejemplo es el siguiente 

~~~php
Route::get('/', function () {
	return view('welcome');
});
~~~
* El primer parametro es la ruta, las rutas no se puede repetir. 
* el segundo parametro es un clausure. 
  * donde tenemos la función view que es del template manager `Blade` los templetes se encuentran en `Laravel/resources/views/<filename>.blade.php` en este caso `Laravel/resources/views/welcome.blade.php`.


# CRUD implementacion
Laravel por defecto tiene un sistema de bases de datos para usuarios. el cual puedes ejecutar con `php artisan migrate:install` o `php artisan migrate:refresh` que elimina las tablas y la vuelve a crear.

Ahora en el router Web vamos a agregar unas rutas para crear y eliminar un elemento. el method `get()` puede tomar un clausure o un string con el nombre del controlador seguido del method `Route::get("/", "UserController@index")` donde `UserController` es el nombre del controlador y el `index` el method a ejecutar.

Con el comando `php artisan make:controller <name>` creas la estructura basica de un controlador. en el cual podemos crear el method `index` para poder responder a la llamada del router.

Artisan viene con una consola que se llama `tinker`  para ejecutar la consola usamos `php artisan `

**router/web.php**
~~~php
<?php

use App\Http\Controllers\UserController;
use Illuminate\Support\Facades\Route;

Route::get('/', [UserController::class, 'index']);
Route::post('user/store', [UserController::class, 'store'])->name('user.store');
Route::delete('user/destroy/{user}', [UserController::class, 'destroy'])->name('user.destroy');
~~~

**app/Http/Controllers/UserController.php**
~~~php
namespace App\Http\Controllers;

use App\Models\User;
use Illuminate\Http\Request;

class UserController extends Controller
{
    public function index()
    {
        $users = User::latest()->get();
        return view("users.index", [
            "users" => $users
        ]);
    }

    public function store(Request $request)
    {
        $request->validate([
            'name' => 'required',
            'email' => ['required', 'email', 'unique:users'],
            'password' => ['required', 'min:8']
        ]); // si falla setea un objeto $errors y no guarda el usuario.

        User::created([
            "name" => $request->name,
            "email" => $request->email,
            "password" => bcrypt($request->password),
        ]);
        return back();
    }

    public function destroy(User $user) // importante que user sea el nombre del modelo
    {
        $user->delete();
        return back();
    }
}
~~~

**resource/view/user/index.blade.php**
~~~php
<div class="card border-0 shadow">
    <div class="card-body">
        @if ($errors->any()) // verify if get any error
            <div class="alert alert-danger">
                @foreach ($errors->all() as $error) // return un array de todos los errores
                    - {{ $error }} <br>
                @endforeach
            </div>
        @endif
        <form action="{{ route('user.store') }}" method="post"> // route() crea la ruta
            <div class="form-row">
                <div class="form-group col-sm-3">
                    <input type="text" value="{{ old('name') }}" name="name" class="form-control"> // old() trae los valores que tenia antiguamente
                </div>
                <div class="form-group col-sm-4">
                    <input type="email" value="{{ old('email') }}" name="email" class="form-control">
                </div>
                <div class="form-group col-sm-3">
                    <input type="password" name="password" class="form-control">
                </div>
                <div class="form-group col-sm-2">
                    @csrf // crea un token para contraatacar "Cross-Site Request Forgery"
                    <button class="btn btn-success btn-block" type="submit">Save</button>
                </div>
            </div>
        </form>
    </div>
</div>
@foreach ($users as $user)
    <tr>
        <td>{{ $user->id }}</td>
        <td>{{ $user->name }}</td>
        <td>{{ $user->email }}</td>
        <td>
            <form action="{{ route('user.destroy', $user) }}" method="post">
                @method('DELETE') // implement a delete request instead of post (overwrite)
                @csrf
                <input
                    value="Delete"
                    class="btn btn-danger btn-sm"
                    type="submit"
                    onclick="return confirm('Do you want to delete this item?')"
                >
            </form>
        </td>
    </tr>
@endforeach
~~~

## Middleware
Son codigo en medio que suelen ser filtros de HTTP. los middleware estan en la carpeta `laravel/app/Http/Middleware/` pero son inicializados dentro del kernel de Http `laravel/app/Http/Kernel.php` ahi se setea el alias de los middleware para poderlos usar dentro de los router podemos concatenar un middleware `Route::get('/', 'Hello')->middleware('auth')` otra manera de usar un middleware es dentro del `__contruct()` de un controlador.

## Rutas y controladores
Todo lo que tiene que ver con rutas se encuentra dentro 

# artisan
Artisan en un programa de linea de comando de laravel

## migrate
Se encarga de migrar las bases de datos del proyecto

## route
Se encarga de manejar las rutas