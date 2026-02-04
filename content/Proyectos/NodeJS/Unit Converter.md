---
title: Unit Converter
tags:
  - nodejs
  - principiante
  - web
---
## Información del proyecto

### Requisitos
Es una pagina web que tendra diferentes secciones para las diferentes medidas. El usuario puede introducir un valor a convertir, la unidad a convertir de una a otra, y ver la cantidad convertida.

- El usuario puede introducir un valor a convertir
- El usuario puede introducir de que medida a que medida
- El usuario puede ver el resultado de la conversión
- El usuario puede convertir entre diferentes unidades de medida como longitud, peso, temperatura

Puedes incluir las siguiente unidades de medida a convertir

- Longitud: milimetro, centimetro, metro, kilometro, pulgada, pies, yardas y millas
- Peso: miligramos, gramos, kilogramos, onzas y libras
- Temperatura: Celsius, Farenheit y Kelvin

### Ejemplo de uso

![[unitconverterexample.png]]

## Preparación del proyecto

Al ser un proyecto que utiliza paginas web como interacción principal con el usuario, a diferencia de un CLI que utilizaria una terminal, por esto, para este proyecto deberemos de tener 3 paginas webs creada (ya que es como lo recomienda el proyecto, 1 pagina para que tipo de medida).

De esta manera, antes de empezar deberemos de hacer 3 paginas simples, que solamente tendran un formulario y un header con una navbar para cambiar de tipo de medida facilmente.

Ya que esto no es parte del proyecto, utilizaremos `simple.css` para el estilo y que sea más visual de una manera que no nos tome mucho tiempo, aunque hagamos algunos retoques a dicho estilo.

### Estructura del proyecto

Para que nuestro proyecto funcione de manera correcta, deberemos de estructurarlo adecuadamente, al ser simplemente un archivo que hara de servidor y tres paginas webs, la estructura sera simple pero necesaria:

```
Unit Converter/
├── server.js
└── public/
	├── longitud.html
	├── peso.html
    └── temperatura.html
```

Con esta estructura establecida, podemos proceder a realizar nuestro proyecto

## Creación del servidor

Ya que nuestro servidor va a ser "minimamente" complejo, para facilitarnos el proyecto y hacerlo más similar a la realidad utilizaremos `express`, ya que junto a la estructura que hemos preparado para este proyecto nos va a facilitar mucho el manejo de las paginas web.

```javascript
import express from "express";
const app = express();
```

Ademas de esto, deberemos de usar dos `middelwares` especificos para nuestro objetivo, el primer `middelware` sera el que permitira la navegación entre los archivos en la carpeta public sin necesidad de crear rutas especificas para ello, y el segundo `middelware`, sera el que se encargara de parsear correctamente los datos a `json` para utilizarlos correctamente.

```javascript
app.use(express.static('public'));
app.use(express.json());
```

Con estas poquitas lineas de codigo, si levantamos el servidor en cualquier puerto, podremos comprobar que funciona correctamente y nos permite la navegación de manera sencilla.

```javascript
app.listen(3000, () => {
    console.log('Servidor desplegado en http://localhost:3000');
});
```

## Conversión de longitudes

### Formulario

Lo primero que deberemos de tener, sera nuestra pagina web creada, esta pagina debera de tener un `input` de texto, donde el usuario introducira la cantidad y dos selectores, uno para la longitud que quiere transformar y el otro para la longitudad a la que lo quiere transformar, de esta manera obtendremos todos los datos que necesitamos enviar al servidor para su funcionamiento.

Simplemente crearemos un formulario con dichos campos y valores necesarios:

```html
<div style="display: block;">
    <form id="unitForm" style="display: flex; flex-direction: column; margin-top: 50px;">

        <label for="amount">Enter the length to convert</label>
        <input type="number" name="amount" id="amount">

        <label for="unitFrom">Unit to convert from</label>
        <select name="unitFrom" id="unitFrom">
            <option value="millimeter">Millimeter</option>
            <option value="centimeter">Centimeter</option>
            <option value="meter">Meter</option>
            <option value="kilometer">Kilometer</option>
            <option value="inch">Inch</option>
            <option value="foot">Foot</option>
            <option value="yard">Yard</option>
            <option value="mile">Mile</option>
        </select>

        <label for="unitTo">Unit to convert to</label>
        <select name="unitTo" id="unitTo">
            <option value="millimeter">Millimeter</option>
            <option value="centimeter">Centimeter</option>
            <option value="meter">Meter</option>
            <option value="kilometer">Kilometer</option>
            <option value="inch">Inch</option>
            <option value="foot">Foot</option>
            <option value="yard">Yard</option>
            <option value="mile">Mile</option>
        </select>

        <input type="submit" value="Convert">
    </form>
</div>
```

Una vez tenemos el formulario utilizaremos un poco de javascript para evitar el funcionamiento predeterminado del formulario, despues de hacer eso, simplemente le pasaremos los datos en formato JSON a una ruta de nuestra API la cual tendra la logica para hacer las conversiones.

Para realizar esto deberemos de escuchar el evento `submit`, que es que ocurre cuando enviamos el formulario, cuando ocurra utilizaremos dicho evento y lo evitaremos con el metodo `preventDefault`, importante que hagamos la escucha del evento asincrona, ya que utilizaremos llamadas a nuestro servidor y tendremos que esperarlo

```javascript
const unitForm = document.getElementById("unitForm");

unitForm.addEventListener("submit", async (e) => {
	e.preventDefault();
})
```

Una vez hemos hecho esto, mediante las id de los campos del formulario obtendremos los datos y se lo mandaremos a nuestro servidor y una vez obtengamos la respuesta, ocultaremos el formulario y mostraremos la respuesta.

```javascript
unitForm.addEventListener("submit", async (e) => {
    e.preventDefault();
    
    const data = {
        amount: document.getElementById("amount").value,
        unitFrom: document.getElementById("unitFrom").value,
        unitTo: document.getElementById("unitTo").value
    };

    const request = await fetch('/longitud', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(data)
    });

    const result = await request.json();

    if (result) {
        const divForm = document.getElementById("divForm");
        const divResult = document.getElementById("divResult");

        divForm.style.display = "none";

        divResult.innerHTML = result;
        divResult.style.display = "block";
    }
});
```

De esta manera ya recibimos los datos y los modificamos en la pagina web, ahora solo tendremos que manejar los datos correctamente en nuestro servidor de manera que devolvamos los datos correctos.

### Transformación de los datos

Para tratar los datos en nuestro servidor crearemos una ruta `.post()`, que es la que recibira los datos y los tratara correctamente, por lo que primero vamos a hacer una pequeña comprobación de que los recibimos correctamente

```javascript
app.post('/longitud', (req, res) => {
    const { amount, unitFrom, unitTo } = req.body;
    
    return result
});
```

Una vez tenemos la base, solo toca hacer la logica correspondiente para los cambios de unidades, crearemos un objeto el cual tenga todas las medidas a las que se pueden cambiar para hacer esto deberemos de tener una conversión teniendo en cuenta una unidad como predeterminada, en mi caso tomare las conversiones a metro, ya que es más habitual. 

Por lo que convertiré todos los valores a metro y de metro los convertire a la unidad correspondiente.

```javascript
const conversionRates = {
    meter: 1,
    centimeter: 0.01,
    millimeter: 0.001,
    kilometer: 1000,
    inch: 0.0254,
    yards: 0.9144,
    miles: 1609.34,
};

const meters = amount * conversionRates[unitFrom];

const result = meters / conversionRates[unitTo];
```

De esta manera tenemos una forma sencilla de convertir los valores facilmente y de forma precisa, por lo que ya habriamos terminado el funcionamiendo de esta parte. Para el resto del proyecto solamente deberiamos de hacer lo mismo pero con las unidades correspondientes.

Tendremos que hacer un pequeño cambio en el HTML para mostrarlo correctamente, este cambio sera crear otro formulario con una pequeñita función que se un boton de reset.

```html
<div id="divResult" style="display: none;">
    <h1 id="amountText"></h1>
    
    <form id="resetForm">
        <input type="submit" value="Reset">
    </form>
</div>
```

Y esta sera la función correspondiente:

```javascript
unitForm.addEventListener("submit", async (e) => {
    e.preventDefault();

    unitForm.reset();
    
    divForm.style.display = "block";
    divResult.style.display = "none";
})
```

Con esta información solo necesitaremos hacer lo mismo pero con las diferentes unidades de medida que tenemos en la pagina


