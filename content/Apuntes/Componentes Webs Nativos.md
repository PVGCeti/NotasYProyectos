
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

Debemos de pensar que estamos introduciendo el contenido como si fuera un archivo **.html**, por lo que podremos utilizar la etiqueta `<style>` para añadir los estilos en caso de que no querramos añadir estilos en linea.

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

### 5. Como utilizar atributos

Algo muy habitual a la hora de crear paginas/componentes es que estos puedan recibir atributos y modificar el contenido en función de ellos, haciendo que sea más versatil y especifico. Para añadir atributos simplemente debremos de crear una variable utilizando **this.getAttribute()**.

```javascript
const theme = this.getAttribute('theme') ?? 'dark'
```

Esta variable deberemos de situarla dentro del **Render()** pero encima del **this.shadowRoot.innerHTML**.. Tendremos que usar el operador ternario **??**, para que en caso de que no reciba ningún valor de dicho atributo utilice el que le hemos puesto
### 6. Ciclo de Vida: Conexión con el DOM 🔄

Para que el componente se muestre en la página, el método `render()` debe ser invocado cuando el elemento es **insertado en el DOM**. Esto se logra usando el _callback_ del ciclo de vida `connectedCallback()`.

```javascript
connectedCallback(){
	this.render()
}
```

## Componente Final 🧑‍💻

El código final del componente `SimpleHeader` combinando todos los pasos anteriores:

A la hora de importar el componente simplemente deberemos de tenerlo en el archivo de JavaScript, importar dicho archivo y usar el nombre que le hemos puesto entre comillas en el **customElements.define()**, como si fuera una etiqueta normal y corriente:

```html
<body>
    <simple-header></simple-header>
    <script src="script.js"></script>
</body>
```

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
