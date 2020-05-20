## Data binding
~~~javascript
let app = new Vue({
	el: "#querySelector",
	data(){
		return {
			title: "hello!!!",
			src: "http://facebook.com"
		}
	}
});
~~~

La funcion `data` siempre debe retornar un objeto. Estos haran bind dentro del elemento seleccionado. Para insetar la variable dentro del html se usan las llaves *ejemplo* `<a v-bind:href="src">{{ title }}</a>` quedaria en `<a href="http://facebook.com">{{ title }}</a>` como puedes ver hay dos maneras de hacer bind de la variable dentro de las etiquetas se usa el `v-bind:attributo="variable"` o en en cualquier parte del html con las llaves `{{ variable }}`. Dentro de las llaves se pueden hacer operaciones y uso de funciones, casi de todo. Lo que no se puede hacer son `loops`, `if`, `switch`, etc.

## Condicionales
Para usar un condicional para que se muestre un elemento se usa el `if` o `show`

~~~html
<span v-if="condicion > 0">text1</span>
<span v-else-if="condicion < 0">text2</span>
<span v-else>text3</span>

<span v-show="condicion > 0">text1</span>
<span v-show="condicion < 0">text2</span>
<span v-show="condicion === 0">text3</span>
~~~

`v-if` no renderea el elemento y `v-show` no muestra el elemento, aplica un estilo `display: none;`
Se recomienda usar `v-if` cuando no se va estar chequeando esta condición asi evitamos estar muntando el DOM.

## Loops
También es posible hacer loops de un array.

~~~html
<table>
	<tr v-for="person in persons">
		<td>{{ person.name }}</td>
	</tr>
</table>
~~~

## Eventos
Para ejecutar eventos se usa la directiva `v-on:<eventTrigger>` veamoslo en un ejemplo.

**HTML**
~~~html
<div id="querySelector">
	<button v-on:click="toggleView">{{ showText ? "hide" : "show" }}</button>
	<p v-show=showText>I'm here!</p>
</div>
~~~

**JS**
~~~javascript
let app = new Vue({
	el: "#querySelector",
	data(){
		return {
			showText: false
		}
	},
	methods: {
		toggleView: function () {
			showText = !showText;
		}
	}
});
~~~

## Cambiar estilos
Veamos unos ejemplos de como poner estilos a un elementos ya sea por medio de clases o style.

**CSS**
~~~css
.blue{
	color: blue
}
.red{
	color: red
}
.green{
	color: green
}
~~~

**HTML**
~~~html
<ul v-bind:style="{ background-color: '#' + color }">
	<li v-for="price in prices">
		<span v-bind:class="{blue: var === price, red: var > price, green: var < price}"></span>
	</li>
</ul>
~~~

## Datos computadas y watchers
Los datos computados o `computed` es un objeto de funciones con datos que pueden ser compuestos por diferentes critereios que siempre son retornados. Veamos un ejemplo.
**JS**
~~~javascript
let app = new Vue({
	el: "#querySelector",
	data(){
		return {
			showText: false,
			firstName: "yefri",
			lastName: "gonzalez"
		}
	},
	computed: {
		fullName: function () {
			if(this.showText){
				return `${this.firstName.toUpperCase()} ${this.lastName.toUpperCase()}`
			}else{
				return "Anonymous"
			}
		}
	}
});
~~~

Los watchers son un caso similar, pero en vez de retornar un valor, ejecutan una funcion si uno de los datos que se han pasado tienen un cambio y no tiene la necesidad de retornar algo.
**JS**
~~~javascript
let app = new Vue({
	el: "#querySelector",
	data(){
		return {
			showText: false,
			firstName: "yefri",
			lastName: "gonzalez"
		}
	},
	watch: {
		firstName: function (newVal, oldVal) {
			alert(`You changed your name from ${oldVal} to ${newVal}`);
		}
	}
});
~~~

## Two way data binding
Con la directiva `v-model` se puede hacer un `two way data binding` lo que hace que cuando cambien el value de un input se actualice en la variable dentro del componente
**HTML**
~~~html
<input type="text" v-model="firstName">
~~~

## Componentes
Un componente es una seccion del codigo que puede ser reusado y consta con las mis opciones que cuando se inicializa el VueJS.

**HTML**
~~~html
<div id="querySelector">
	<h1>{{ title }}</h1>
	<counter></counter>
</div>
~~~

**JS**
~~~javascript
Vue.component("counter", {
	data(){
		return {
			count: 0,
		}
	},
	methods: {
		count: function(){
			this.count++;
		}
	},
	template: `
		<div>
			<button v-on:click="count">Click Here!</button>
			<span>{{ count }}</span>
		</div>
	`
});

let app = new Vue({
	el: "#querySelector",
	data(){
		return {
			title: "Counter"
		}
	}
});
~~~

### Propiedades
Por medio de `props` se pueden pasar valores, pero estos valores no pueden ser modificados por el componente. mas tarde veremos como se pueden hacer cambios por medio de eventos.

**HTML**
~~~html
<div id="querySelector">
	<h1 v-bind:style="{background-color: '#' + color}">{{ title }}</h1>
	<div v-for="coin in coins">
		<coin-detail v-bind:coin="coin" v-on:change-color="function"></coin-detail>
	</div>
</div>
~~~

**JS**
~~~javascript
Vue.component("coinDetail", {
	props: ["coin"],
	data(){
		return {
			count: 0,
			showPrices: false
		}
	},
	methods: {
		count: function(){
			this.count++;
		},
		toggleShowPrices: function () {
			this.showPrices = !this.showPrices
			this.emit("changeColor", "0F0F0F");
		}
	},
	computed: {
		title: function (){
			return `${this.coin.name} - ${this.coin.symbol}`
		}
	},
	template: `
		<div>
			<h5>{{ title }}</h5>
			<button v-on:click="toggleShowPrices">{{ showPrices ? "Hide" : "Show"}}</button>
			<ul v-for="priceDay in coin.prices" v-show="showPrices">
				<li>{{ priceDay.day }}: {{ priceDay.value }}</li>
			</ul>
		</div>
	`
});

let app = new Vue({
	el: "#querySelector",
	data(){
		return {
			title: "Coin Detail",
			color: "FFFFFF"
			coins: [
				{
					name: "Bitcoin",
					symbol: "BTC",
					prices: [
						{day: "Lunes", value: 8400},
						{day: "Martes", value: 8300},
						{day: "Miércoles", value: 8500},
						{day: "Jueves", value: 8450},
						{day: "Viernes", value: 8100},
						{day: "Sábado", value: 8200},
						{day: "Domingo", value: 8000},
					],
					price: 8400,
				}
			]
		}
	},
	methods: {
		updateColor: function (color) {
			this.color = color
		}
	}
});
~~~

## Slots
Los slots son etiquetas donde se le podrá insertar contenido HTML a un componente hijo. hay dos tipos de slot con nombres o anónimo. Los slots con nombres deben de ser insertados en el template del componente como `<slot name="nombre"></slot>` y dentro de la llamada del componente con `<template v-slot:nombre><p>Contenido<p></template>`. el animo no hay necesidad de meterlo dentro de la etiqueta `template` y al `slot` no hay que darle un nombre. Veamoslo en un ejemplo


**HTML**
~~~html
<div id="querySelector">
	<h1 v-bind:style="{background-color: '#' + color}">{{ title }}</h1>
	<child-comp-one v-bind:coin="coin">
		<p>HTML que se va injectar en el componente</p>
	</child-comp-one>
	<child-comp-two v-bind:coin="coin">
		<template v-slot:text>
			<p>HTML que se va injectar en el componente</p>
		</template>
		<template v-slot:link>
			<a href="#">Click aquí</p>
		</template>
	</child-comp-two>
</div>
~~~


**JS**
~~~javascript
Vue.component("childCompOne", {
	props: ["coin"],
	data(){
		return {
		}
	}
	computed: {
		title: function (){
			return `${this.coin.name} - ${this.coin.symbol}`
		}
	},
	template: `
		<div>
			<h5>{{ title }}</h5>
			<slot></slot>
		</div>
	`
});

Vue.component("childCompTwo", {
	props: ["coin"],
	data(){
		return {
		}
	}
	computed: {
		title: function (){
			return `${this.coin.name} - ${this.coin.symbol}`
		}
	},
	template: `
		<div>
			<h5>{{ title }}</h5>
			<slot name="text"></slot>
			<slot name="link"></slot>
		</div>
	`
});

let app = new Vue({
	el: "#app",
	data(){
		return {
			coin: {
				name: "Bitcoin",
				symbol: "BTC"
			}
		}
	}
});
~~~

## Ciclo de Vida y Hooks
Dentro de vue hay diferentes trigger (hooks) o tiempos de ejecución que son los siguientes:
* beforeCreate = tener en cuenta que los methods y muchas otras cosas no an sido definidas.
* created = ya ha sido inicializado todos sus metodos y datos   
* beforeMount
* mounted = Ya esta el DOM disponible
* beforeUpdate
* updated
* beforeDestroy
* destroyed
Hay 2 hooks que se repiten constantemente en los componentes y son:
* beforeUpdate
* updated
Se ejecutan cada vez que hay un cambio que se tiene que actualizar en el componente.

Los nombres son muy claros, estos se usan de la siguiente manera.

**JS**
~~~javascript
let app = new Vue({
	el: "#app",
	data(){
		return {
			coin: {
				name: "Bitcoin",
				symbol: "BTC"
			}
		}
	},
	created() {
		console.log('created...');
	}
	mounted() {
		console.log('created...');
	}
});
~~~

## Vue CLI
para instalarlo necesitamos `npm` vamos a user el siguiente comando para instalarlo de manera global. `npm install -g @vue/cli` ahora mismo tenemos el comando `vue` por el momento vamos a usar la opción `vue create <nombre-app>`. este comando te va dar unos preset en caso que ya lo hayas usado caso contrario estaras creando uno y te preguntara que dependencia vas a estar usando. en este caso vamos a usar `Babel` y `Linter / Formatter` despues nos hará preguntas para configurar las opciones que hemos seleccionado. Al finalizar te preguntara si lo quiere guardar como un preset.

Ahora se crea la carpeta con el nombre de la aplicación con los archivos básicos.

~~~
node_modules/ # here come all the dependencies
public/ # 
src/
>	assets/
>	components/
>	main.js
<configuration_files>
.gitignore
babel.config.js
package.json
package-lock.json
~~~

## Estructura basica de un archivo .vue

**App.vue**
~~~vue
<template>
	<div id="app">
		<p>{{ greeting }} World!</p>
	</div>
</template>

<script>
module.export = {
	data: () => ({
		greeting: 'Hello'
	})
}
</script>

<style scoped>
p {
	font-size: 2em;
	text-align: center;
}
</style>
~~~

dentro de la etiqueta `template` construimos lo que es el componente, el `script` es lo que llevamos haciendo en el curso dentro de `new Vue()`. Por ultimo la etiqueta `style` donde van todos los estilos y con el attributo `scoped` solo afectara a el componente en cuestion.

**main.js**[1]
~~~javascript
import Vue from "vue"; 
import App from "./App.vue";

Vue.config.productionTip = false;

new Vue({
  render: h => h(App)
}).$mount("#app");
~~~
Main va a ser el documento ha compilar en el la primera linea importamos vue, nos damos cuenta por que no tiene una ruta relativa por lo tanto nos damos cuenta que esta integrando una dependencia. La segunda linea se le indica que vamos a usar el archivo App.vue y lo importa en nuestro main file.
Despues de eso creamos el objeto vue y ahora con un method `$mount(<elementQuerySelector>)` remplaza al elemento `el`

## Comandos CLI de la aplicacion
Dentro de `package.json` se declaran los comandos que podemos ejecutar con `npm run <comando>` por defecto vue nos crea los comandos `lint`, `build` y `serve`.
* `lint` busca errores dentro de nustro poryecto
* `build` nos compila el proyecto en la carpeta `/dist` para producion.
* `serve` crea un servidor que hostea nuestro projecto en local.

## Agregar una dependencia a nuestro proyecto
En este caso es el framework de CSS `tailwind`, pero vamos a traerlo del usuario `@ianaya89` que ya esta pre-configurado lo hacemos de la siguiente manera `vue add @ianaya89/tailwind` es importante ejecutar este comando dentro de la carpeta raiz del proyecto. esto no agregara la configuracion de tailwind y mas importante un archivo `src/assets/css/tailwind.css`. Necesitamos agregara el archivo donde va ser usado en este caso en `main.js` ya que lo vamos a usar en todo el proyecto. Dentro de `main.js` importamos de la siguiente manera `import '@/assets/css/tailwind.css'`. 
En este caso usamos el `@/` que se refiere a la carpeta `src` por lo cual podriamos mover el archivo y no afectaria la ruta a diferencia de `./` que es relativa a la ubicacion del archivo.

## Usar componentes dentro de componentes
La manera de usar componentes dentro de otro componente es de la siguiente manera.

parent.vue
~~~vue
<template>
	<form>
		<child />
	</form>
</template>
<script>
	import child from "@/component/customInput.vue";

	export default {
		name: "parent",
		componets: { child }
	}
</script>
~~~

child.vue
~~~vue
<template>
	<input type="text" >
</template>
<script>

	export default {
		name: "child"
	}
</script>
~~~

## Vue Router
Para instalar vue router usamos `npm i -S vue-router` despues de eso creamos un archivo que va ser el entry point que llamaremos `router.js` donde pondremos la configuracion del router y las rutas que vamos a usar.

**router.js**
~~~javascript
// llamamos a todos las dependencia que vamos a usar
import Vue from "vue"
import Router from "vue-router"

// importamos los componentes que tenemos como vistas (son componentes comunes)
import Home from "@/views/Home"
import Error from "@/views/Error"
import About from "@/views/About"

// indicamos que vamos hacer uso del router
Vue.use(Router)

// creamos las diferentes rutas
export default new Router({
  mode: "history",

  routes: [
    {
      path: "/",
      name: "home",
      component: Home
    },

    {
      path: "/about",
      name: "about",
      component: About
    },

    {
      path: "*",
      name: "error",
      component: Error
    },
  ]
})
~~~

dentro del archivo que mostramos anteriormente `main.js[1]` decimos que nuestro elemento de entrada es `App.vue` por lo tanto vamos a insertar la etiqueta/componente `<router-view>` es aqui donde se va estar injectado el componente segun la ruta indicada.
**main.js**[2]
~~~javascript
import Vue from "vue";
import App from "./App.vue";
import "@/assets/css/tailwind.css"

import router from "@/router"

Vue.config.productionTip = false;

new Vue({
  router, // indiacmos a Vue que agregue la configuracion del router
  render: h => h(App)
}).$mount("#app");
~~~

Dentro de un componente para crear rutas se hace con la etiqueta/componente `<router-link to="/ruta">` 

## Fetch y API de Coincap
Primero que todo vamos a a crear un archivo separado para todas las llamadas de api dentro de funciones.

**api.js**
~~~javascript
const url = 'https://api.coincap.io/v2';

function getAssets() {
	return fetch(`${url}/assets?limit=20`)
		.then(response => response.json())
		.then(response => response.data);
}

export default {
	getAssets
};
~~~

Ahora cuando quieras hacer uso de la api lo importa y usas sus funciones como un objeto.
~~~javascript
import api from '@/api'

api.getAssets()
	.then(data => { data })
~~~

## Filtros de vue
Para esto tambien vamos a usar una directiva `numeral.js` que sirve para representar numeras ya sea en binario, moneda, time entre otro. para instalarla con `npm i -S numeral`
craemos un archivo filters donde vamos a usar la libreria y creamos un funcion que va a filtrar un numero y devolverlo en dolares.

**filters.js**
~~~javascript
import numeral from 'numeral'

const dollarFilter = function(value) {
  if (!value) {
    return '$ 0.00'
  }

  return numeral(value).format('($ 0.00a)')
}

const percentFilter = function(value) {
  if (!value) {
    return '0%'
  }

  return `${Number(value).toFixed(2)}%`
}

export { dollarFilter, percentFilter } // no se hace export default ya que asi los podemos importar de forma atomica quiere decir que sin necesidad de que se traiga todos filtros que existen.
~~~

para importar la funcion/filtro lo hacemos de la siguiente manera, `import { dollarFilter } from '@/filters'`

**main.js**
~~~javascript
// ...
	import { dollarFilter, percentFilter } from '@/filters'
	Vue.filter('dollar', dollarFilter);
	Vue.filter('percent', percentFilter);
// ...
~~~

dentro de vue se usa con un pipe `|` por ejemplo `{{ a.marketCapUsd | dollar }}`

## Rutas dinamicas
Bueno en este caso es muy similar la manera de agregar la ruta, con diferencia que en el path vamos a usar una varible con el carater `:` (doble puntos)


**router.js**
~~~javascript
// ...
import CoinDetail from '@/components/CoinDetail'
Vue.use(Router)

export default new Router({
	mode: "history",

	routes: [
		// ...
		{
			path: "/coin/:id",
			name: "coin-detail",
			component: CoinDetail
		},
		// ...
	]
})
~~~

Para aceder a la variable del router usamos `this.$route.params.id` dentro de un componente.

## Navegacion
Ahora vamos a profundizar un poco en lo que es el componente `router-link` hay 2 formas de inter-actuar con el router ya sea con un elementento en el html (button, links, imagens, etc) o programatica ejecutado desde el codigo por cierta funcion con un objeto que incluye router a los componentes que es `this.$router` (no confundir con `this.$route` ya que son objetos diferentes).

**Ejemplos**
* `<router-link to="/">`
* `<router-link :to="{ name: 'route-name', params: { id: 'value' } }">`
* `this.$router.push({ name: 'route-name', params: { id: 'value' } })`

## Componentes de terceros
Ahora vamos a ver como agregar componentes de terceros en ellos esta `@Saeris/vue-spinners` y `vue-chartkick` para instalarlos usamos el comando `npm i -S @saeris/vue-spinners vue-chartkick`. 
Es importante leer la documentacion de cada libreria/depencecia para ver como integarla a vue. Por ejemplo en el caso de esta 2 vamos a ponerlo en un ejemplo.

~~~javascript
import Vue from 'vue'
import Chart from 'chart.js' // dentro de la documentacion vemos que chartkick hace uso de chart.js por lo cual tambien debemos instalarlo.
import Chartkick from '@searis/chartkick'
import { VueSpinners } from '@saeris/vue-spinners'

Vue.use(VueSpinners)
Vue.use(Chartkick.use(Chart)) // en este caso chartkick es un wrapper de la libreria chart.js

new Vue({
	render: h => h(App)
}).$mount('#app')
~~~

## Problem de Reactividad
Dentro de vue tenes un problema cuando se crea un componente y un array o objeto no tiene un elemento el no hace tracking de los datos que son concatenados para eso Vue no da una herramienta para agregarlo al sistema de reactividad con  `this.$set(objectOfArray, 'param', insertValue)`

## Router cambio de runta dinamica
Dentro de Vue vemos que cuando queremos cambiar de una ruta dinamica no nos carga nada, veamoslo en un ejemplo.


~~~javascript
{
	path: "/coin/:id",
	name: "coin-detail",
	component: CoinDetail
}
~~~

ahora si dentro de la ruta `/coin/bitcoin` queremos cambiar a `/coin/ethereum` esto no va hacer niun cambio ya que el componente solo recive sus parametros cuando es creado para eso devemos crea un watcher a `$route` para cuando el cambie se actualice el data.