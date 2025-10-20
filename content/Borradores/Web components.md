---
draft: "true"
---


## Introducción a los Web Components ⚛️

Los **Web Components** son un conjunto de estándares web que permiten crear elementos HTML personalizados, reutilizables y encapsulados. A diferencia de las librerías o _frameworks_ como React, que ofrecen soluciones similares, los Web Components son una **funcionalidad nativa** del navegador.

---

## Ventajas de usar Web Components ✨

El uso de Web Components ofrece múltiples beneficios clave:

- **Reutilizables**: Se construyen una vez y se pueden utilizar en cualquier parte de la aplicación o en diferentes proyectos.

- **Encapsulados**: Sus estilos y lógica están aislados del resto de la página, evitando conflictos (_Shadow DOM_).

- **Nativos**: Forman parte de los estándares del navegador, lo que asegura su compatibilidad sin necesidad de librerías externas para su funcionamiento básico.

- **Independientes**: Funcionan de manera autónoma con o sin el uso de _frameworks_ de JavaScript.

---

## Creación y Estructura de un Web Component

La creación de un Web Component implica extender la clase base `HTMLElement` y seguir una serie de pasos para su definición y renderizado.

### 1. Definición de la Clase 🧱

Un Web Component se define como una **clase de JavaScript** que debe **extender** de la clase nativa `HTMLElement`.

```javascript
class SimpleHeader extends HTMLElement {

}
```

### 2. Constructor y Shadow DOM 🛡️

Dentro del `constructor()`, se inicializa la clase y **es crucial invocar a `super()`** para enlazar correctamente con la jerarquía de elementos.

Para lograr el **encapsulamiento** y evitar que los estilos se rompan, se recomienda adjuntar el **Shadow DOM** (un subárbol de elementos oculto e independiente del DOM principal) en modo `'open'`.

```javascript
class SimpleHeader extends HTMLElement {
    constructor(){
        super()
        
        // Activa el Shadow DOM para encapsulamiento
        this.attachShadow({mode: 'open'}) 
    }
}
```

### 3. Registro del Componente 🏷️

Una vez definida la clase, el componente debe **registrarse** en el navegador para que pueda ser reconocido como una etiqueta HTML válida. Esto se hace usando el método `customElements.define()`, que toma el **nombre de la etiqueta** (debe contener un guion, ej: `'devjobs-avatar'`) y la **clase del componente**.

```
customElements.define('simple-header', SimpleHeader)
```

> [!warning] Importante!
> El archivo de JavaScript que contiene la definición y el registro de la clase debe ser **importado** en la página HTML para que el componente pueda ser usado.

### 4. Renderizado del Contenido 🖼️

El contenido (HTML y estilos) del componente se define en un método, habitualmente llamado `render()`.

En este ejemplo, se utiliza el método `getAttribute()` para recuperar los **atributos** que se le pasen a la etiqueta HTML (ej: `<devjobs-avatar username="PezEjecutivo">`). Se establecen valores por defecto (`??`) si los atributos no están presentes.

```javascript
render() {
        this.shadowRoot.innerHTML = `
            <nav style="display: flex; justify-content: space-between; background-color: bisque;">
                <div style="display: flex; align-items: center;">
                    <img src="" alt="Logo">
                    <h3>Hola</h3>
                </div>
                <div style="display: flex; align-items: center;">
                    <input type="text" placeholder="Search" style="padding: 5px;">
                </div>
                <div style="display: flex; align-items: center;">
                    <img src="" alt="Perfil">
                    <!-- <img src="" alt="Settings"> -->
                </div>
            </nav>
        `;
    }
```

> [!important] **Nota**: 
> Al usar **Shadow DOM**, el contenido se inyecta en `this.shadowRoot.innerHTML` en lugar de `this.innerHTML` para mantener el encapsulamiento de estilos y estructura.

### 5. Ciclo de Vida: Conexión con el DOM 🔄

Para que el componente se muestre en la página, el método `render()` debe ser invocado cuando el elemento es **insertado en el DOM**. Esto se logra usando el _callback_ del ciclo de vida `connectedCallback()`.

```javascript
connectedCallback(){
	this.render()
}
```

## Componente Final 🧑‍💻

El código final del componente `DevJobsAvatar` combinando todos los pasos anteriores:

Para evitar que los estilos se rompan, deberemos de encapsular el componente, para ello, en el constructor deberos de activar el modo shadow DOM

```javascript
class SimpleHeader extends HTMLElement {
    constructor() {
        super();
        this.attachShadow({ mode: 'open' });
    }

    render() {
        this.shadowRoot.innerHTML = `
            <nav style="display: flex; justify-content: space-between; background-color: bisque;">
                <div style="display: flex; align-items: center;">
                    <img src="" alt="Logo">
                    <h3>Hola</h3>
                </div>
                <div style="display: flex; align-items: center;">
                    <input type="text" placeholder="Search" style="padding: 5px;">
                </div>
                <div style="display: flex; align-items: center;">
                    <img src="" alt="Perfil">
                    <!-- <img src="" alt="Settings"> -->
                </div>
            </nav>
        `;
    }
    
    connectedCallback() {
        this.render();
    }
}

customElements.define('simple-header', SimpleHeader);
```

Una vez hemos creado la burbuja, deberemos de añadir el shadowRoot antes del innerHTML

```
render(){
	this.shadowRoot.innerHTML = `
		<img src="" alt="" class="avatar" />
	`	
}
```

De esta manera podemos añadir los estilos sin problemas y sin que haya conflictos

```
render(){
	this.shadowRoot.innerHTML = `
	<style>
		img {
		border: 10px solid red;
		}
	</style>
		<img src="" alt="" class="avatar" />
	`	
}
```

Al renderizar el elemento, podemos recuperar los atributos

```
const service = this.getAttribute('service') ?? 'github'
const username = this.getAttribute('username') ?? 'PezEjecutivo'
const size = this.getAttribute('size') ?? '32'
```

Ahora crearemos un metodo por encima del render() para recuperar el icono correctamente

```
createUrl(service, username){
	return `https://unavatar.io/${service}/${username}`
}
```

Una vez tenemos esto solamente tenmos que llamar a la función y poner los valores correctos en el render:

```
render(){

	const service = this.getAttribute('service') ?? 'github'
	const username = this.getAttribute('username') ?? 'PezEjecutivo'
	const size = this.getAttribute('size') ?? '32'
	
	const url = this.createUrl(service, username)

	this.shadowRoot.innerHTML = `
	<style>
		img {
			width: ${size}px;
			height: ${size}px;
			border-radius: 9999px;
		}
	</style>
		<img src={url} alt="" class="avatar" />
	`	
}
```

Quedando un componente final de esta manera:

```
class DevJobsAvatar extends HTMLElement {
	constructor(){
		super()
		
		this.attachShadow({mode: 'open'})
	}
}

createUrl(service, username){
	return `https://unavatar.io/${service}/${username}`
}

render(){

	const service = this.getAttribute('service') ?? 'github'
	const username = this.getAttribute('username') ?? 'PezEjecutivo'
	const size = this.getAttribute('size') ?? '32'
	
	const url = this.createUrl(service, username)

	this.shadowRoot.innerHTML = `
	<style>
		img {
			width: ${size}px;
			height: ${size}px;
			border-radius: 9999px;
		}
	</style>
		<img src={url} alt="" class="avatar" />
	`	
}

connectedCallback(){
	this.render()
}

customElements.define('devjobs-avatar', DevJobsAvatar)
```