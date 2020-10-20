Introducción a Webpack
Webpack es un empaquetador para Javascript y sus amigos. Convierte módulos con dependencias en archivos estáticos que los navegadores entienden.

Nos permite empaquetar, optimizar los diferentes módulos Javascript y sus dependencia en nuestro proyecto. Es usado en proyectos basados en Javascript como: React, Vue, Angular entre otros.

### User Experience
Se logra con una aplicación que:

* Funcione
* Sea rápida
* Cumpla sus necesidades
* Se actualice
* Responda a sus interacciones
* Producto de calidad

### Developer Experience
* Escribir aplicaciones de manera eficiente.
* Tener un código limpio.
* Aplicar tecnología para resolver sus problemas.
* Tener un conjunto de reglas y convenciones.
* Entorno de desarrollo optimizado en productividad.

## Instalación de entorno
[Instalar node con Ubuntu server](https://www.digitalocean.com/community/tutorials/how-to-install-node-js-on-ubuntu-16-04)
Los programas necesarios para instalar el entorno son los siguientes:
 * Node.js
 * npm
   * webpack
### Manejados de paquetes
Se puede instalar desde el manejador de paquetes, pero tienes sus inconvenientes y son que se descarga por separados node y npm y no tienes la posibilidad de descargar la version mas reciente.
`sudo apt-get install nodejs npm`
### Por medio de code source de node
Solo es necesario instalar node y viene con npm incluido.
`curl -sL https://deb.nodesource.com/setup_8.x -o nodesource_setup.sh`
`sudo bash ./nodesource_setup.sh`
`sudo apt-get install nodejs`

### Instalar webpack
Primero que todo hay dos formas de instalar web pack, global o local (en la carpeta del proyecto) aca vamos a ver de forma local. primero tenermos que empezar un proyecto con npm.
`npm init`
despues instalar webpack como dependencia de desarrollo con la bandera ` --save-dev`.
`npm install webpack --save-dev`
O si se quiere instalar una version en especifico
`npm install webpack@3.8.1`

### Verificar si todo esta instalado
`node -v`
`npm -v`
`webpack -v` si ha sido instalado de forma global
`npm list webpack` para ver la version del modulo en la carpeta

## Nuestro hola mundo
creamos un archivo de entrada para compilar con webpack en este caso un ` console.log()` 

~~~javascript
// Archivo index.js
console.log("Hola mundo!!!")
~~~

si tuviéramos webpack de manera global podríamos usar el siguiente comando en la terminal.
`webpack index.js [--output <nombre_archivo>] --mode <development|production|none>`
pero como esta de forma local podemos complementarnos de los scripts de npm y dentro del archivo `package.json` configuramos el objeto script.
~~~json
{
	"script": {
		"test": "echo no hay ningún comando",
		"build": "webpack index.js --output bundel.js --mode development"
	}
}
~~~
Después con npm podemos ejecutar los scripts con el comando `run` de esta manera `npm run build`. Esto crea un archivo `bundel.js` con todo compilado.

## webpack.config
La manera mas correcta es configurar el archivo que por defecto webpack lee para ver su configuración `webpack.config.js`.
~~~javascript
module.export = {
	mode: "development",
	entry: "./index.js"
	output: {
		filename: "./bundel.js" 
/* El output sin path te creará por defecto una carpeta “dist”
 * en la cual estará el archivo bundle.js*/
	}
}
~~~
Después es solo crear un script que mande `webpack`
~~~json
{
	"script": {
		"test": "echo no hay ningún comando",
		"build": "webpack index.js --output bundel.js --mode development",
		"build:local": "webpack"
	}
}
~~~
Después con npm podemos ejecutar el scripts `npm run build`.

### Rutas relativas
`"build:external": "webpack --config ./external/webpack.config.js"`

~~~javascript
// ./external/webpack.config.js
const path = require('path');
module.exports = {
  mode: 'development',
  entry: path.resolve(__dirname, 'index.js'),
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'bundle.js'
  }
};
~~~

## Loaders
Los loader son complementos para webpack que hay que instalar con `npm` en este ejemplo vamos a usar `style-loader` y `css-loader`.
`npm install --save-dev style-loader css-loader` o `npm i -D style-loader css-loader` y en el webpack hay que definir los módulos que vamos a usar en el array `module.rules`.

para importar un archivo css se usa el `require(<archivo>)`
Para este ejemplo se necesito un archivo `index.html`, `style.css` y `index.js`. donde el javascript es el entry point y el css contendrá los estilos que importaremos.

el archivo html debe hacer un link con el `./dist/bundle.js`

~~~javascript
// ./loaders/index.js
require('./style.css');
document.write("Hola mundo!!!")
~~~

~~~javascript
// ./loaders/webpack.config.js
const path = require('path');

module.exports = {
  entry: path.resolve(__dirname, 'index.js'),
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'bundle.js',
  },
  module: {
    rules: [
      {
        test: /\.css$/,
        use: [
          { loader: "style-loader" }, // Agrega el css al DOM en un <style>
          { loader: "css-loader" }, // interpreta los archivos css en js via import
        ]
      }
    ]
  }
}
~~~

## Plugins
Los plugin son como los loaders pero con una diferencia que se tiene que llamar y crear un objeto.

~~~javascript
const path = require("path");
// Requerir el plugin para extraer contenido CSS importado en un JS a un archivo independiente CSS
const MiniCssExtractPlugin = require("mini-css-extract-plugin");

module.exports = {
  mode: "development",
  entry: path.resolve(__dirname, "index.js"),
  output: {
    filename: "build.js",
    path: path.resolve(__dirname, "dist")
  },
  module: {
    // Aqui van los loaders,
    // Primero se interpreta todo el css importado en el archivoJS, posteriormente se le pasa al plugin para que genere un archivo CSS independiente con todo ese contenido
    rules: [
      {
        test: /\.css$/,
        use: [{loader: MiniCssExtractPlugin.loader}, "css-loader"]
      }
    ]
  },
  plugins: [
    // Aqui van los plugins, pero antes de utulizarlos es necesario requerirlos dentro de este archivo
    // Le indicamos el nombre del archivo final. Por defecto coloca dentro de la caperta dist, por tanto tenemos dist/css/[name_del_entry_point_por_defect_es_main].css
    new MiniCssExtractPlugin({
      filename: "css/[name].css"
    })
  ]
};
~~~

#Multiples Entry Points
~~~javascript
const path = require("path");
// Requerir el plugin para extraer contenido CSS importado en un JS a un archivo independiente CSS
const MiniCssExtractPlugin = require("mini-css-extract-plugin");

module.exports = {
  mode: "development",
  entry: {
    home: path.resolve(__dirname, "index.js"),
    name2: path.resolve(__dirname, "index2.js"),
    name3: [
      path.resolve(__dirname, "index3.js"),
      path.resolve(__dirname, "index4.js"),
    ]

  }
  output: {
    filename: "js/[name].js",
    path: path.resolve(__dirname, "dist")
  },
  module: {
    // Aqui van los loaders,
    // Primero se interpreta todo el css importado en el archivoJS, posteriormente se le pasa al plugin para que genere un archivo CSS independiente con todo ese contenido
    rules: [
      {
        test: /\.css$/,
        use: [{loader: MiniCssExtractPlugin.loader}, "css-loader"]
      }
    ]
  },
  plugins: [
    // Aqui van los plugins, pero antes de utulizarlos es necesario requerirlos dentro de este archivo
    // Le indicamos el nombre del archivo final. Por defecto coloca dentro de la caperta dist, por tanto tenemos dist/css/[name_del_entry_point_por_defect_es_main].css
    new MiniCssExtractPlugin({
      filename: "css/[name].css"
    })
  ]
};
~~~