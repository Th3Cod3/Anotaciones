# Introducción a Webpack
Webpack es un empaquetador para Javascript y sus amigos. Convierte módulos con dependencias en archivos estáticos que los navegadores entienden.

Nos permite empaquetar, optimizar los diferentes módulos Javascript y sus dependencia en nuestro proyecto. Es usado en proyectos basados en Javascript como: React, Vue, Angular entre otros.

## User Experience
Se logra con una aplicación que:
* Funcione
* Sea rápida
* Cumpla sus necesidades
* Se actualice
* Responda a sus interacciones
* Producto de calidad

## Developer Experience
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
Se puede instalar desde el manejador de paquetes, pero tienes sus inconvenientes y son que se descarga por separados node y npm. No tienes la posibilidad de descargar la version mas reciente.
`sudo apt-get install nodejs npm`

### Por medio de code source de node
Solo es necesario instalar node y viene con npm incluido.
`curl -sL https://deb.nodesource.com/setup_8.x -o nodesource_setup.sh`
`sudo bash ./nodesource_setup.sh`
`sudo apt-get install nodejs`

### Instalar webpack
Primero que todo hay dos formas de instalar web pack, global o local (en la carpeta del proyecto). Acá vamos a ver de forma local. primero tenemos que empezar un proyecto con npm.
`npm init`
después instalar webpack como dependencia de desarrollo con la bandera `--save-dev` o `-D`.
`npm install webpack --save-dev`
O si se quiere instalar una version en especifico
`npm install -D webpack@3.8.1`

### Verificar si todo esta instalado
`node -v`
`npm -v`
`webpack -v` si ha sido instalado de forma global
`npm list webpack` para ver la version del modulo en la carpeta

## Nuestro hola mundo
creamos un archivo de entrada para compilar con webpack en este caso un ` console.log()` 

**index.js**
~~~javascript
console.log("Hola mundo!!!")
~~~

Si tuviéramos webpack de manera global podríamos usar el siguiente comando en la terminal.
`webpack index.js [--output <nombre_archivo>] --mode <development|production|none>`
pero como esta de forma local podemos complementarnos de los scripts de npm y dentro del archivo `package.json` configuramos el objeto script.

**package.json**
~~~json
{
  "script": {
    "build": "webpack index.js --output bundel.js --mode development"
  }
}
~~~

Después con npm podemos ejecutar los scripts con el comando `run` de esta manera `npm run build`. Esto crea un archivo `bundel.js` con todo compilado.

## webpack.config
La manera mas correcta es configurar el archivo que por defecto webpack lee para ver su configuración `webpack.config.js`.

**webpack.config.js**
~~~javascript
module.export = {
  mode: "development",
  entry: "./index.js"
  output: {
    filename: "./bundel.js" 
    /* El output sin path te creará por defecto una carpeta “dist”
     * en la cual estará el archivo bundle.js
     * path: "./dist/"
     */
  }
}
~~~

Después es solo crear un script que mande `webpack`

**package.json**
~~~json
{
  "script": {
    "build": "webpack" // por defecto webpack busca el archivo webpack.config.js
  }
}
~~~
Después con npm podemos ejecutar el scripts `npm run build`.

### Rutas relativas
Si el archivo `webpack.config.js` esta en otra ruta se tiene que definir en el `package.json`.

**package.json**
~~~json
{
  "script": {
    "build": "webpack --config ./external/webpack.config.js"
  }
}
~~~

**./external/webpack.config.js**
~~~javascript
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
Los loader son complementos para webpack que hay que instalar con `npm` en este ejemplo vamos a usar `style-loader` y `css-loader`. Los cuales se encargan de cargar de permitir importar archivos CSS. `css-loader` se encarga de cargar un archivo CSS dentro del javascript y el `style-loader` inyecta el css cargado con `ccs-loader` al HTML.
Para instalar los dos loader podemos hacerlo de la siguiente manera `npm install --save-dev style-loader css-loader` o `npm i -D style-loader css-loader` y en el webpack hay que definir los módulos que vamos a usar en el array `module.rules`.

para importar un archivo css se usa el `require(<archivo>)`
Para este ejemplo se necesito un archivo `index.html`, `style.css` y `index.js`. donde el javascript es el entry point y el css contendrá los estilos que importaremos.

el archivo html debe hacer un link con el `bundle.js`

**style.css**
~~~css
body {
  background: red;
}
~~~

**index.js**
~~~javascript
require('./style.css'); // También se puede usar import "./style.css"

document.write("Hola mundo!!!");
~~~

**webpack.config.js**
~~~javascript
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
          { loader: "style-loader" }, // Agrega el CSS al DOM en un <style>
          { loader: "css-loader" }, // interpreta los archivos css en js via import
        ]
      }
    ]
  }
}
~~~

## Plugins
Los plugin a diferencia que los loader que solo cargan archivos puede agregar extra funcionalidades. Estos debes ser llamados e instanciados.

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
    rules: [
    /*
     * Aqui van los loaders,
     * Primero se interpreta todo el css importado en el archivoJS,
     * posteriormente se le pasa al plugin para que genere un archivo CSS independiente con todo ese contenido
     */
      {
        test: /\.css$/,
        use: [{loader: MiniCssExtractPlugin.loader}, "css-loader"]
      }
    ]
  },
  plugins: [
    /*
     * Aquí van los plugins, pero antes de utilizarlos es necesario requerirlos dentro de este array.
     * Le indicamos el nombre del archivo final.
     * Por defecto coloca dentro de la carpeta dist,
     * por lo tanto tenemos dist/css/[name_del_entry_point_por_defect_es_main].css
     */
    new MiniCssExtractPlugin({
      filename: "css/[name].css"
    })
  ]
};
~~~

## Multiples Entry Points

**webpack.config.js**
~~~javascript
const path = require("path");
const MiniCssExtractPlugin = require("mini-css-extract-plugin");

module.exports = {
  mode: "development",
  // Aquí van todos los entries
  entry: {
    home: path.resolve(__dirname, "index.js"),
    name2: path.resolve(__dirname, "index2.js"),
    name3: [
      path.resolve(__dirname, "index3.js"),
      path.resolve(__dirname, "index4.js"),
    ]
  }
  output: {
    // Re-utiliza el nombre del entry y lo compila con el mismo nombre dentro de dist.
    filename: "js/[name].js",
    path: path.resolve(__dirname, "dist")
  },
  module: {
    rules: [
      {
        test: /\.css$/,
        use: [{loader: MiniCssExtractPlugin.loader}, "css-loader"]
      }
    ]
  },
  plugins: [
    new MiniCssExtractPlugin({
      filename: "css/[name].css"
    })
  ]
};
~~~

## Watcher
Esta es una bandera del comando webpack, que ya viene integrada en webpack que se encarga de compilar cuando se modifica un archivo. La bandera es `--watch` o `-w`, para usarlo es solo agregarla cuando ejecutemos el comando `webpack` o también lo podemos concatenar con `--` algo como esto `npm run build -- -w`.

## Dev server
Este es un modulo/complemento de webpack, el cual no habilita crear un servidor para servir los archivos por medio de un puerto. Este debe ser instalado independientemente `npm i -D webpack-dev-server`, y para que webpack a la hora de compilar lo levante necesitamos la bandera `serve` seria algo como mínimo de esta manera `webpack serve`. Tener en cuenta que el `Dev Server` por defecto ya usa el `watcher`.
También falta aclara que podemos configurar algunas opciones del `dev server` dentro del `webpack.config.js`.
Veamos un par de ejemplos:

**webpack.config.js**
~~~javascript
module.exports = {
  // ...
  devServer: {
    open: true, // abre el browser cuando se arranca
    port: 8080 // despliega el puerto 8080
  }
};
~~~

## Hot Module Replacement (HMR)
Es un sistema de carga, sin la necesidad de recargar el browser. Esto requiere una configuración dentro de su código que por defecto ya esta controlado por muchos framework, pero en este caso nosotros vamos a manejarlo.

**webpack.config.js**
~~~javascript
const webpack = require('webpack');

module.exports = {
  //...
  devServer: {
    hot: true
  }
  plugins: [
    new webpack.HotModuleReplacementPlugin()
  ]
};
~~~

**index.js**
~~~javascript
import '../css/styles.css'
import text from './text';

text();

if(module.hot) {
  module.hot.accept('./text', function(){
    text()
  })
}
~~~

## Configurar babel
Babel es un traductor de JS moderno para los browser que no soportan funciones nuevas de JS. Babel no necesita de webpack, pero aca estaremos viendo como instalarlo con webpack.
Primero que todo debemos de instalar 3 dependencias `@babel/core`, `@babel/preset-env` y `babel-loader`. Después debemos configurar el loader dentro de `webpack.config.js` y también la configuración de babel dentro del archivo `.babelrc`.

**webpack.config.js**
~~~javascript
module.exports = {
  module: {
    rules: [
      {
        test: /\.js$/,
        use: 'babel-loader', // Intercepta los archivos de js y transpila en versiones más antiguas que entiendan la mayoría de los navegadores
        exclude: /node_modules/ // esta linea es muy importante para que no afecte las dependencias.
      },
    ]
  },
};
~~~

**.babelrc**
~~~json
{
  "presets" : [
    "@babel/preset-env"
  ]
}
~~~

Babel es todo un mundo, pero solo vamos hacer los necesario para que funcione con funciones mas modernas para esto debemos usar el `npm i -E @babel/runtime`. y para la parte de compilación se necesita `npm i -D @babel/plugin-transform-runtime`. Si no estoy mal esto habilita el uso de `async/await`