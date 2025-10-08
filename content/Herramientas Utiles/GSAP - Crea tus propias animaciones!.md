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


#### gsap.timeline()

El metodo **gsap.timeline()** es utilizado para crear multiples animaciones

Aunque es similar a los metodos anteriores, su objetivo es completamente diferente, ya que este tiene el objetivo de tener un elemento con multiples animaciones diferentes.

Para saber más del metodo gsap.timeline() puedes visitar la [pagina oficial](https://gsap.com/docs/v3/GSAP/gsap.timeline()).

##### Ejemplo:

En este caso, para poder utilizar este metodo, deberemos de crear una linea de tiempo, que basicamente es un objeto el cual mantendra las animaciones, esto lo haremos mediante una constante, la cual podremos añadirle las animaciones que queramos:

```
const timeline = gsap.timeline({ repeat:1, repeatDelay:1, yoyo:true })
```

Una vez tenemos la linea de tiempo creada, ahora podemos crear la función de useGSAP como hemos estado usando anteriormente para animar la timeline con los metodos antes mencionados:

```
  useGSAP(()=>{
    timeline.to('#yellow-box', {
      x:250,
      rotation: 360,
      borderRadius: '100%',
      duration: 2,
      ease: 'back.inOut'
    })

    timeline.to('#yellow-box', {
      y: 250,
      scale: 2,
      rotation: 360,
      borderRadius: '100%',
      duration: 2,
      ease: 'back.inOut'
    })

    timeline.to('#yellow-box', {
      x: 500,
      scale: 1,
      rotation: 360,
      borderRadius: '8px',
      duration: 2,
      ease: 'back.inOut'
    })
  },[])
```

De esta manera, tendremos un elemento con 3 animaciones, (aunque se le pueden añadir aún más animaciones), en caso de que solo quisieras usar dos animaciones, podrias usar el metodo gsap.fromTo().

Algo positivo de utilizar una timeline, es que podemos usar un boton para parar o poner en marcha la timeline cuando queramos con el siguiente codigo:

```
<button onClick={() => {
  if(timeline.paused()) {
    timeline.play()
  } else{
    timeline.pause()
  }
}}>Play/Pause</button>
```


#### gsap stagger

GSAP stagger es una propiedad que te permite aplicar animaciones con un retraso escalonado a un grupo de elementos.

Al utilizar la propiedad stagger en GSAP, puedes especificar la cantidad de tiempo para escalonar las animaciones entre cada elemento, así como personalizar la suavidad y la duración de cada animación individual. Esto te permite crear efectos dinámicos y visualmente atractivos, como desvanecimientos escalonados, rotaciones, movimientos y más.

Para saber más de la propiedad stagger puedes visitar la [pagina oficial](https://gsap.com/resources/getting-started/Staggers/).

##### Ejemplo:

Para utilizar stagger, necesitaremos estar aplicando gsap a más de un elemento a la vez, en caso de querer aplicar propiedades adicionales, declareremos stagger, como una propiedad objeto

-- Completar --

#### GsapScrollTrigger

El Gsap Scroll Trigger es un complemento que te permite crear animaciones que se activan por la posición de desplazamiento de la página.

Con ScrollTrigger, puedes definir diversas acciones que se activarán en puntos de desplazamiento específicos, como iniciar o finalizar una animación, avanzar a través de animaciones mientras el usuario se desplaza, fijar elementos en la pantalla y más.

##### Ejemplo:

Para utilizar el ScrollTrigger, deberemos de importarlo, ya que esto es un plugin, por lo que tendremos que añadir la siguiente linea de codigo a nuestro proyecto:

```
import { ScrollTrigger } from "gsap/all";
```

Despues de haberlo importado, hay que registrarlo mediante el uso de gsap.registerPlugin()

```
gsap.registerPlugin(ScrollTrigger)
```

Con el plugin ya importado y registrado, deberemos de crear una referencia, esto lo haremos mediante la propia función de React, useRef(), (Es importante recordar, que deberemos de importar el useRef() de React para poder usarlo). Para esto, ademas de crear la constante, deberemos añadirle dicha propiedad al div que queramos:

```
const scrollRef = useRef()

<div className="mt-20 w-full h-screen" ref={scrollRef}>
```

scrub hace que vaya despacito

```
  useGSAP(() =>{
    const boxes = gsap.utils.toArray(scrollRef.current.children);

    boxes.forEach((box) => {
      gsap.to(box, {
        x: 150,
        rotation: 360,
        borderRadius: '100%',
        scale: 1.5,
        scrollTrigger: {
          trigger: box,
          start: 'bottom bottom',
          end: 'top 20%',
          scrub: true,
        },
        ease: 'power1.inOut'
      })
    })
  }, { scope: scrollRef })
```


####  Gsap text
Podemos usar el mismo método que gsap.to(), gsap.from(), gsap.fromTo() y gsap.timeline() para animar texto.

Usando estos métodos, podemos lograr varias animaciones y efectos de texto, como desvanecerse, aparecer, deslizarse hacia adentro, deslizarse hacia afuera, y muchos más.

Para animaciones y efectos de texto más avanzados, puedes explorar el GSAP TextPlugin u otras bibliotecas de terceros que se especializan en animaciones de texto.




