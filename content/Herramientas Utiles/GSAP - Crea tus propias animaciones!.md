GSAP es una libreria que puedes utilizar en cualquier framework, ya sea React, Webflow, Wordpress o cualquier otro framework de JS.

En mi caso voy a enseñar a utilizar con React, para ello deberemos de usar los siguientes comandos para instalarlo:

```
npm install gsap
npm install @gsap/react
```

Una vez instalado en nuestro proyecto de React, tendremos que importarlo para poder usarlo, esto lo haremos con las siguientes lineas de codigo:

```
import { gsap } from "gsap";  
import { useGSAP } from "@gsap/react";
```

Por norma general solo haria falta importar gsap, pero como vamos a utilizar en React, es necesario que tambien importemos el useGSAP, ya que esto es un hook de React creado para que sea más comodo la utilización de dicha libreria.

### Animaciones basicas

Vamos a enseñar a utilizar funciones simples de GSAP, con las cuales, una vez aprendas podras realizar verdaderas genialidades de la animación web!

#### gsap.to()

El metodo **gsap.to()** es usado para animar un elemento de su estado actual a uno nuevo.

El metodo **gsap.to()** es similar al metodo **gsap.from()**, pero la diferencia esta en que el metodo **gsap.to()** anima el elemento desde su estado actual a uno nuevo, mientras que el metodo **gsap.from()** anima el elemento de un estado nuevo a su estado actual.

Para saber más del metodo gsap.to() puedes visitar la [pagina oficial](https://gsap.com/docs/v3/GSAP/gsap.to()).

Ejemplo:

En este caso tendremos el siguiente div:

```
<div className="w-20 h-20 bg-blue-500 rounded-lg caja" />
```

para poder animarlo utilizando gsap, necesitaremos importar useGSAP y gsap, como se ha mencionado previamente en la guia, una vez hecho eso, tendremos que usar el siguiente codigo:

```
useGSAP(() => {
	gsap.to('.caja', {
	})
}, [])
```

Este codigo, nos permite, mediante una función flecha, utilizar gsap.to() en nuestro div, de ahi el '.caja', ya que estamos declarando que lo haga en el elemento que tenga la clasa "caja". Con el elemento ya selecionado, simplemente tendremos que animarlo a nuestro gusto, alguna de las cosas que podemos hacer son las siguientes:

x -> Desplazamiento en el eje horizontal
y -> Desplazamiento en el eje vertical
scale -> Escala el elemento
rotation -> Gira el elemento
opacity -> Cambia la opacidad del elemento
yoyo -> Una vez termina la animación, la hace inversamente
repeat -> Indica las veces a repetir (si pones -1 se repite infinitamente)

Aunque estas no son todas, son algunas bastante utiles para empezar a animar y hacer animaciones sencillas, como por ejemplo la siguiente animación:

```
useGSAP(() =>{
  gsap.to('.caja', {
    scale: 1.1,
    duration: 1,
    repeat: -1,
    yoyo: true,
  })
}, [])
```

La cual podria ser una animación sencilla para un call to action en un boton.