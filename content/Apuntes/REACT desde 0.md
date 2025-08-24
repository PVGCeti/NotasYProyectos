---
title: React desde 0
---


<img src="https://desarrolloweb.com/storage/tag_images/actual/n3m2Az0VWHFdb4ng3q9JbDrbCjar8hE7K0SV7M8n.png" alt="React" width="200"/>

-   [[#Fundamentos basicos]]
-   [[#Frameworks]]
-   [[#Proyectos para el aprendizaje]]
-   [[#Bibliografia]]
-   [[#Paginas Utiles]]

## Fundamentos basicos

[CURSO REACT 2024 - Aprende desde cero](https://www.youtube.com/watch?v=7iobxzd_2wY) - 2:24:22

### Que es React?

-   React es una biblioteca de JS para construir interfaces de usuario (UI) (es agnostico al lugar)
-   React es universal, sirve tanto para servidor como para cliente
-   React Native es para desarrollar aplicaciones movil, windows, macs...

Algunas ventajas de React es que al ser su forma de programar declarativa es muy facil de escalar a diferencia de JS vanilla que es imperativo.

React debido a su forma de funcional, necesita un elemento root, ya que podriamos decir que es como un arbol.

### Cosas relevantes a saber:

-   Los nombres de los componentes tienen que ser PascalCase
-   Para añadir una clase en react es con className
-   Se pueden añadir funciones a los props
-   <></> es lo mismo que <React.Fragment></React.Fragment>
-   Cuando pasamos un boolean como prop, al poner el nombre llegara como true. (No hay una forma corta para hacer el false)
-   Si queremos concatenar una string con un valor de una prop en alguna etiqueta, debemos poner {} y dentro lo que queremos
-   Los useStates tambien son conocidos como Hooks
-   Los nombres de los componentes son PascalCase (Es decir, empiezan en masyucula)
-   Un componenete es una factoria de elementos, Un elemento es lo que redenriza React (Ejemplo: < h1 >hola</ h1>)
-   Las props deben ser inmutables, no debemos modificarlas directamente.
-   Es una buena praxis inicializar los useState con el tipo de datos que vayamos a usar

### Renderizado de componentes:

Los componenetes se renderizan siempre que haya un cambio en
su propio componente o si hay un cambio en la parte de arriba,
es decir, cuando algo cambia en la parte superior, todo lo que esta
por debajo cambia. Aunque el componente no cambie, se vuelve a renderizar

> [!CAUTION] Propiedad key
> Cuando renderizamos algo con una lista, debemos añadirle una prop llamada key, la cual tiene que ser unica, si la base de datos tiene un id, es lo mejor que se puede usar

### Funcionamiento del useState:

const [nombre, setNombre] = useState("valor"); </br>
Nombre: guarda el valor (el primer indice del array) </br>
setNombre: Cambia el valor (el segundo indice del array) </br>

Al hacer un cambio en el useState, se volvera a renderizar el componenete y todo lo que sea necesario. </br>

Cuando se usa una prop para inicializar un estado, el nombre deberia ser initialNombreDelEstado

> [!TIP] Consejo de declaración
> Ejemplo: </br>
> initialNombre (prop) </br>
> const [nombre, setNombre] = useState(initialNombre)

> [!CAUTION] Inicialización de props
> Si inicializas un estado con una prop, aunque cambies esa prop, no se va a volver a inicializar, ya que solo se inicializa una vez desde la prop

[Curso de React desde cero: Crea un videojuego y una aplicacion para aprender useState y useEffect](https://www.youtube.com/watch?v=qkzcjwnueLA) - 2:06:15

### Cosas relevantes a saber:

-   En las props, si quieres pasar una funcion, tienes que pasar la funcion sin ejecutarla, es decir, sin ()
-   Siempre que usamos un array que viene de props hay que copiarlo, es decir [... array], poner los 3 puntos
-   La actualizacion de los estados en React es asincrono
-   Los hooks no pueden estar dentro de un if

### cuando se renderiza useEffect()?

useEffect(()=>{

}, /[]/[enabled])

[] -> Solo se ejecuta una vez cuando se monta el componente </br>
[enabled] -> se ejecuta cuando cambia enabled y cuando se monta el componente </br>
undefined -> se ejecuta cada vez que se renderiza el componenete </br>

> [!IMPORTANT] Funcionamiento del useEffect
> El return () =>{} dentro de un useEffect se usa para limpiar las subcripciones, este se ejecuta al desmontar el componente y antes de que carge el efecto

> [!TIP] Consejo para realizar la comprobación
> Para comprobar limpieza de subcripciones puedes usar getEventListener(Object)

> [!WARNING] React.stricmode
> En modo dev, el react.stricmode hace que el useEffect se te active 2 veces, para ayudar a debugear, que aparezca 2 veces la primera vez, no es un errors

[Aprende a pasar una Prueba Tecnica de React. Entiende useMemo, useCallback u useRef](https://www.youtube.com/watch?v=GOEiMwDJ3lc) - 2:05:51

[Tienda y Carrito con React + Estado Global con useCOntext + Manejo de estado con useReducer](https://www.youtube.com/watch?v=B9tDYAZZxcE) - 2:02:39

[APRENDE React Query DESDE CERO: Paginacion, infinite Scroll, DevTools](https://www.youtube.com/watch?v=WKfVjQUa6nE) - 1:31:46

## Frameworks:

[Custom Hooks + testing con playwright: curso de react desde cero Parte 4](https://www.youtube.com/watch?v=x-LcbVw99o8) - 43:45

[No necesitas Redux en React! Aprende a usar Zustand, alternativa sencilla.](https://www.youtube.com/watch?v=p2wF2wRjcN0) - 1:34:31

## Proyectos para el aprendizaje

Estos proyectos estan creados unica y exclusivamente con la intencion de aprender y mejorar mis conocimientos y fundamentos en React. La mayoria de estos proyectos provienen de algun video o guia, en caso de que no se especifique lo contrario.

## Bibliografia

Todos los videos a los que hacen referencia los enlaces son de MiduDev de su canal de youtube [Midulive](https://www.youtube.com/@midulive)

## Paginas Utiles

[https://www.reactjs.wiki](https://www.reactjs.wiki) - Documentación de preguntas típicas </br>
[https://es.react.dev](https://es.react.dev) - Documentación de React </br>
[HeroIcons](https://heroicons.com) - Iconos </br>
[ChakraUI](https://chakra-ui.com) - biblioteca de componentes de React </br>
