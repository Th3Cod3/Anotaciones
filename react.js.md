## React
Es una libreria con diferentes configuraciones ya pre-configuradas como por ejemplo WebPack, Babel, Gulp, entre otros.

##Herramientas
* Navegador (Chrome es recomendado)
* React Developer Tools (extention para chorme o firefox)
* Prettier (Organiza el codigo)
* Create-react-app (para eso se necesita npm)

## Instalar Create-React-app
`npm install -g create-=react-app`

ahora puede crear el init de react con `create-react-app <nombre_proyecto>` este comando instala `webpack`, `babel`, ...  y crea diferentes archivos.
* `./src` es la carpeta que tiene todo el código fuente y configuración.

## Empecemos con React
Cuando clonas un repositorio se debe usar el comando `npm install` para asi instalar todas las dependencias.
Es importante entender que React tiene su forma de escribir `JSX`, Esta es la forma de escribir `HTML` dentro de `React` para esto se necesita `import React from 'react'`.

Después para importar el elemento se usa `ReactDOM.render(que, donde)`

## JSX vs createElement
Se usa mas tanto JSX por motivos que es mas lejible, vemos un mismo ejemplo con ambos.

~~~javascript
import React from 'react'
import ReactDOM from 'react-dom'

const name = "Richard"

const jsx = (
	<div>
		<h1>Hola, soy {name}</h1>
		<p>Soy ingeniero frontend</p>
	</div>
);

// React.createElement(tag, attr, ...children)
const element = React.createElement(
	'div',
	{},
	React.createElement('h1', {}, `Hola, soy ${name}`),
	React.createElement('p', {}, 'Soy ingeniero frontend')
)
~~~

## Componentes o Elementos
Un componente puede ser un cojunto de elementos o se puede retornar un elemento, el componente tiene como distinción que toma etiquetas, logica, calculos, variables, etc. Como resultado tiene un elemento.

## Componente
Los componentes es una buena practica para crear una carpeta en `src/components/` ahí tener todos los componestes en este ejemplo vamos a crear el componente badge

`src/components/Badge.js`
~~~javascript
// estructura de un componente
import React from 'react'
import confLogo from "../images/badge-header.svg"
import './style/Badge.css' // webpack con babel detectan e importan el CSS

class Badge extends React.Component{
	render(){
		return (
			// Aca viene el JSX

			// react usa className para llamar clases para que no haga conflicto con la palabra reservada de JS class
			<div className="Badge__image-container">
				<img src={confLogo} alt="Logo conference" />
				<h1>{this.props.nombre}</h1>
				<img className="Badge__image" src={this.props.imageUrl} alt={this.props.imageAlt} />
			</div>
			// los atributos con jsx se le llaman props y no se le pueden dar strings sino dentro de etiquetas.
		)
	}
}
~~~

`src/index.js`
~~~javascript
// como llamar un componente
import React from "react";
import ReactDOM from "react-dom";

import 'bootstrap/dist/css/bootstrap.css'; // también se puede instalar bootstrap por medio de npm y después llamar el archivo que necesitas.

import './global.css' // También se puede agregar estilos globales

import Badge from "./components/Badge.js"; // llamar al componente

const container = document.getElementById("app");

// los atributos que se le pasan al componentes se le llaman props (properties)
ReactDOM.render(<Badge 
	imageUrl="http://image.com/img.jpg" 
	imageAlt="Imagen del autor"
	nombre="Yefri Gonzalez"
/>, container);
~~~


## Concatenación de componentes (pages)
Después que tengas un componente lo puede rehusar dentro de un mismo componente. También se tiene por separado lo que se considera una pagina donde viene un conjuntos de componentes. veamos en el código.

`src/page/BadgeNew.js`
~~~javascript
import React from 'react'
import Badge from './Badge'

class Badge extends React.Component{
	render(){
		return (
			<div className="container-fluid">
				<h1>Welcome<h1>
				<Badge 
					imageUrl="http://image.com/img.jpg" 
					imageAlt="Imagen del autor"
					nombre="Yefri Gonzalez"
				/>
			</div>
		)
	}
}

export default BadgeNew
~~~

## Crear eventos
En react los event trigger se dan como props a el elemento. es recomendado usar `handle<Evento>` por ejemplo si es un evento de click a handle se le puede llamar `handleClick`
veamos un ejemplo en un formulario.

`src/component/BadgeForm.js`
~~~javascript
import React from 'react'
import Badge from './Badge'

class Badge extends React.Component{
	state={};
	handleClick = e =>{
		console.log({value: e.target.value};)
	}

	handleClick = e => {
		console.log("Button was clicked");
	};

	handleSubmit = e => {
		e.preventDefault(); // esto es para prevenir que la pagina se recargue
		console.log("Form was submitted");
	};

	render(){
		return (
			<div className="container-fluid">
				<h1>Form<h1>
					<form onSubmit={this.handleSubmit}> {/*Esta es el callback al momento que se active el evento Submit y asi sucesivamente para todos los demás */}
					<div className="form-group">
						<label>First Name</label>
						<input
							onChange={this.handleChange}
							className="form-control"
							type="text"
							name="firstName"
						/>
					</div>

					<button
						onClick={this.handleClick}
						type="submit"
						className="btn btn-primary"
					>
						Save
					</button>
				</form>
			</div>
		)
	}
}

export default BadgeNew
~~~

## States
Se usa para guardar los estados de un input, es importante hacer que el input obtenga el valor del estado ya que sino lo va estar guardando en 2 lugares. Veamos un ejemplo.

`src/component/BadgeForm.js`
~~~javascript
import React from "react";

class BadgeForm extends React.Component {
	// state es requerido aunque sea vació, ya que re requiere para el value.
	state = {
		firstName: "Yefri"
	};// asi se inicializa un input con react

	handleChange = e => {
		console.log(e.target.value);
		this.setState({
			[e.target.name]: e.target.value
		});
	};

	handleSubmit = e => {
		e.preventDefault();
		console.log(this.state);// se pueden usar los estado en cualquier momento
	};

	render() {
		return (
			<React.Fragment>
				<h1>NEW ATTENDANT</h1>
				<form onSubmit={this.handleSubmit}>
					<div className="form-group">
						<label>First Name</label>
						<input
							onChange={this.handleChange}
							className="form-control"
							type="text"
							name="firstName"
							value={this.state.firstName} // aquí se obtiene lo que este en el estado.
						/>
					</div>

					<button
						onClick={this.handleClick}
						type="submit"
						className="btn btn-primary"
					>
						Save
					</button>
				</form>
			</React.Fragment>
		);
	}
}
~~~

## Levantar states
Levantar states es la manera que se comparte información entre componentes. Para compartir componentes el state debe estar un nivel mas alto del componente que lo utiliza. Veamos un ejemplo.

`src/page/BadgeNew.js`
~~~javascript
//...
class BadgeNew extends React.Component {
	// aca suele dar un warning cuando no se inicializa un state
	state = {
		form: {
			firstName: "",
			lastName: "",
			email: ""
		}
	};

	handleChange = e => {
		this.setState({
			form: {
				...this.state.form,
				[e.target.name]: e.target.value
			}
		});
	};

	render() {
		return (
			//...
			//Dos maneras de como se pueden usar los states
			<div className="col-6">
				<Badge
					lastName={this.state.form.lastName}
					firstName={this.state.form.firstName}
					email={this.state.form.email}
					/>
			</div>
			<div className="col-6">
				<BadgeForm onChange={this.handleChange} formValues={this.state.form}/>
			</div>
			// para acceder al ultimo se usa "this.props.formValues.<key>"
			//...
		);
	}
}
~~~
