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

##### Ejemplo:

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

#### gsap.from()

El metodo **gsap.from()** es usado para animar un elemento desde un estado nuevo a su estado actual.

Como hemos mencionado previamente, es similar al metodo **gsap.to()**, pero tienen sus diferencias, en resumen, hacen lo opuesto el uno al otro.

Para saber más del metodo gsap.from() puedes visitar la [pagina oficial](https://gsap.com/docs/v3/GSAP/gsap.from()).

##### Ejemplo:

Debido a que from() es practicamente el mismo metodo, pero la animación empieza desde el final y va al principio, para evitar tener el mismo texto 2 veces, pueden mirar la teoria, ya que es la misma, desde el apartado de [gsap.to()](#gsap.to).

```
useGSAP(() =>{
  gsap.from('.caja', {
    scale: 1.1,
    duration: 1,
    repeat: -1,
    yoyo: true,
  })
}, [])
```


#### gsap.fromTo()

El metodo **gsap.fromTo()** es utilizado para animar un elemento desde un estado nuevo a un estado nuevo

El metodo **gsap.fromTo()** es similar a los metodos antes mencionados, **gsap.to()** y **gsap.from()**, pero la diferencia reside en que este metodo anima de un estado nuevo a otro estado que tambien es nuevo, mientras los otros animaban un estado y mantenian otro identico, de esta manera ningun estado sera igual que antes de ser animado.


Para saber más del metodo gsap.fromTo() puedes visitar la [pagina oficial](https://gsap.com/docs/v3/GSAP/gsap.fromTo()).

##### Ejemplo:

En este caso, el codigo sera algo distinto, ya que en vez de tener un solo objeto, tendremos dos, el primero para el from y el segundo para el to, de esta manera podremos darle las propiedades que queramos a los dos apartados, en esta caso vamos a convertir un cuadrado, en un circulo:

```
  useGSAP(() =>{
    gsap.fromTo('.caja',{
      x:0,
      rotation: 0,
      borderRadius: '0%'
    },
    {
      x:250,
      repeat: -1,
      yoyo: true,
      borderRadius: '100%',
      rotation: 360,
      duration: 2,
      ease: "bounce.out"
    })
  }, [])
```

