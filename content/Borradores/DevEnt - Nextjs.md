---
draft: "true"
---
 Enlace a la web:
 Enlace al repositorio:

---

Creamos un proyecto de Next.js 16, [pagina oficial](https://nextjs.org/)  (En caso de que Next.js ya no sea la ultima versión, solo tendreis que cambiar el @latest por la versión que quieras)

```bash
npx create-next-app@latest
```

Una vez usemos ese comando, nos preguntara que tipo de configuración queremos, en nuestro caso utilizaremos la configuración recomendada, la cual esta configurada con: `TypeScript, ESLint, Tailwind CSS, App Router, Turbopack`.

Para comprobar que hemos creado el proyecto sin problemas, utilizaremos el comando `npm run dev`, si vamos al puerto 3000 de nuestro local host podremos ver que esta la pagina por defecto de Nextjs:

![[NextjsDefault.png]]

Aunque TailwindCSS viene instalado (debido a la configuración que hemos escogido) tambien necesitaremos instalar `tw-animate-css`, para ello, tendremos que usar el siguiente comando:

```shell
npm install --save-dev tw-animate-css
```

Una vez tenemos todas las dependencias que necesitamos instaladas y cambiado el favicon de la carpeta `app`, necesitaremos cambiar el titulo de la pagina, para cambiarlo tendremos que ir a `app > layout.tsx` y modificar la metadata:

```typescript
export const metadata: Metadata = {
  title: "DevEvent",
  description: "The Hub for Every Dev Event You Mustn't Miss",
};
```

Además de esto, como vamos a utilizar diferentes tipografias, tendremos que cambiarlas/importarlas en el mismo archivo, es decir `app > layout.tsx`, las tipografias que vamos a usar seran: **Schibsted_Grotesk** y **Martian_Mono**, una vez hemos cambiado las tipografias y los metadatos, nuestos layout.tsx deberia verse tal que así:

```typescript
import type { Metadata } from "next";
import { Schibsted_Grotesk, Martian_Mono } from "next/font/google";
import "./globals.css";

const schibstedGrotesk = Schibsted_Grotesk({
  variable: "--font-schibsted-grotesk",
  subsets: ["latin"],
});

const martianMono = Martian_Mono({
  variable: "--font-martian-mono",
  subsets: ["latin"],
});

export const metadata: Metadata = {
  title: "DevEvent",
  description: "The Hub for Every Dev Event You Mustn't Miss",
};

export default function RootLayout({
  children,
}: Readonly<{
  children: React.ReactNode;
}>) {
  return (
    <html lang="en">
      <body
        className={`${schibstedGrotesk.variable} ${martianMono.variable} min-h-screen antialiased`}
      >
        {children}
      </body>
    </html>
  );
}
```

---

## React bits - Light Rays
Este es un componente que vamos a utilizar para crear una ambientación y mejorar la experiencia del usuario en la pagina, este es el enlace a la [web oficial](https://www.reactbits.dev/backgrounds/light-rays). Una vez en la pagina, cambiaremos de preview a code y pondremos CLI en vez de manual, una vez hecho eso, solo tenemos que seguir los pasos que nos indica para instalarlo:

```shell
npx shadcn@latest add https://reactbits.dev/r/LightRays-JS-CSS
```

Cuando pongamos ese comando, nos preguntara varias cosas a las que tenemos que decirle que si (y), las cuales son sin queremos instalar `shadcn` y si queremos crear `components.json`. Una respondido afirmativamente a esas preguntas, nos preguntara por el color, el cual escogeremos el neutro (neutral).

Una vez hemos hecho eso, crearemos la mitica carpeta de `components` y añadiremos `LightRays.tsx`, y pondremos el codigo que nos ofrece el componente, para nuestras especificaciones (TypeScript y TailwindCSS).

A la hora de utilizarlo, no lo vamos a utilizar en un archivo `page.tsx`, ya que lo usaremos en el layout global, para que este esté efecto en todas y cada una de las paginas, haciendo que sea más consistente y optimizado, ya que solamente lo usaremos una vez, y no una vez por cada pagina.

Una vez estamos en el `app > layout.tsx`, justo al principio del body, utilizaremos el componente, para usarlo, simplemente tendremos que pegar el apartado de `Usage` de la pagina y asegurarnos de que hemos importado bien el componente, en nuestro caso, como no necesitamos el div, podemos borrarlo y le cambiaremos el color a `#5dfeca`.

Nuestro root-layout deberia de verse de esta manera:

```typescript
import type { Metadata } from "next";
import { Schibsted_Grotesk, Martian_Mono } from "next/font/google";
import "./globals.css";
import LightRays from "@/components/LightRays";

const schibstedGrotesk = Schibsted_Grotesk({
  variable: "--font-schibsted-grotesk",
  subsets: ["latin"],
});

const martianMono = Martian_Mono({
  variable: "--font-martian-mono",
  subsets: ["latin"],
});

export const metadata: Metadata = {
  title: "DevEvent",
  description: "The Hub for Every Dev Event You Mustn't Miss",
};

export default function RootLayout({
  children,
}: Readonly<{
  children: React.ReactNode;
}>) {
  return (
    <html lang="en">
      <body
        className={`${schibstedGrotesk.variable} ${martianMono.variable} min-h-screen antialiased`}
      >
        <LightRays
          raysOrigin="top-center-offset"
          raysColor="#5dfeca"
          raysSpeed={1.5}
          lightSpread={0.8}
          rayLength={1.2}
          followMouse={true}
          mouseInfluence={0.1}
          noiseAmount={0.1}
          distortion={0.05}
          className="custom-rays"
        />
        {children}
      </body>
    </html>
  );
}
```

Una vez tenemos el componente funcionando, deberemos de cambiarle algunas propiedades para hacer la animación más simple y elegante, ya que al ser una animación que sigue a tu ratón, es importante que se note pero no demasiado, ya que queremos que la gente vea la pagina, no que se ponga a jugar con la animación.

Los cambios son los siguientes:

```typescript
        <LightRays
          raysOrigin="top-center-offset"
          raysColor="#5dfeca"
          raysSpeed={0.5}
          lightSpread={0.9}
          rayLength={1.4}
          followMouse={true}
          mouseInfluence={0.02}
          noiseAmount={0.0}
          distortion={0.01}
        />
```

Ahora, ademas de esto, lo vamos a meter en un div (tal y como estaba al principio) para darle nuestros estilos propios, tales como:

- absolute: para que no interactue con ningun otro elemento de la pagina
- inset-0: para que ocupe todo del contenedor
- top-0: para que empiece arriba del todo
- z-\[-1\]: para que este atras del todo
- min-h-screen: para que pille el tamaño de toda la pagina

Además de esto, pondremos al objeto en un main, en vez de dejarlo sin nada como estaba antes:

```typescript
        <div className="absolute inset-0 top-0 z-[-1] min-h-screen">
          <LightRays
            raysOrigin="top-center-offset"
            raysColor="#5dfeca"
            raysSpeed={0.5}
            lightSpread={0.9}
            rayLength={1.4}
            followMouse={true}
            mouseInfluence={0.02}
            noiseAmount={0.0}
            distortion={0.01}
          />
        </div>
        <main>
          {children}
        </main>
      </body>
```

---

## Home page

### Componente ExploreBtn 

Aunque es un proyecto pequeño y en el que solamente estoy trabajando yo, para mantener las mejores practicas posibles, trabajare mediante el uso de ramas, aunque no utilizare la metodologia de gitflow, ya que como he comentado, debido a las caracteristicas del proyecto y los participantes, seria como matar una mosca a cañonazos.

Para trabajar por ramas, una vez estamos en la rama main, simplemente haremos el siguiente comando:

```shell
git checkout -b homepage
```

De esta manera, habremos creado una rama llamada homepage y nos habremos cambiado a ella para trabajar directamente sobre ella.

Lo primero que haremos sera crear un componente, él cuál sea un boton, esto es debido a next.js y todo su server side rendering, por lo que necesitaremos un componente que sea un boton para interactuar de mejor manera con el cliente, una vez hemos creado el componente (en este caso lo he llamado `ExploreBtn`), lo añadiremos a la pagina principal `( app > page.tsx )`.

Como podeis apreciar, si ponemos un `onClick` al boton, nos dara un error, diciendo que necesitamos que sea un componente de cliente para poder usar esas funcionalidades, es por eso, que hemos creado el boton como componente aparte, ahora solamente tendremos que usar el `'use client'`, en el componente del boton y tendremos todos los beneficios. Es decir, la pagina cargara mucho más rapido al ser un componente de servidor, pero podremos seguir interactuando con ella, ya que tenemos un componente de cliente.

```typescript
'use client';

const ExploreBtn = () => {
    return (
        <button onClick={() => console.log('CLICK')}>ExploreBtn</button>
    );
};

export default ExploreBtn;
```

Una vez tenemos un boton funcional, sin que nos de ningun error Next.js, podemos empezar a estilarlo y darle uso, terminando con un boton, tal que así:

```typescript
const ExploreBtn = () => {
    return (
        <button type="button" id="explore-btn" className="mt-7 mx-auto" onClick={() => console.log('CLICK')}>
        
            <a href="#events">Explore Events
                <Image src="/icons/arrow-down.svg" alt="arrow-down" width={24} height={24} />
            </a>
            
        </button>
    );
};
```

Es importante, tener en cuenta que al estar en  Next.js, deberemos de usar `Image from "next/image"`, para las imagenes, no podemos usar el tipico `<img />`. 

### Componente Navbar

Lo primero que haremos sera crear un componente llamado `Navbar.tsx` y a diferencia de los demas, al ser la navbar, este no estara en vuelto en etiquetas `<section>`, sino en la etiqueta `<header>`, ya que necesitamos mantener una estructura para que nuestra pagina sea más accesible y tenga mejor `SEO`, al igual que antes, en vez de tener una etiqueta `<div>`, tendra una etiqueta `<nav>`, con esto en mente, montamos el componente:

```typescript
import Image from "next/image";
import Link from "next/link";

const Navbar = () => {
    return (
        <header>
            <nav>
                <Link href='/' className="logo">
                    <Image src="/icons/logo.png" alt="logo" width={24} height={24} />

                    <p>DevEvent</p>
                </Link>

                <ul>
                    <Link href='/'>Home</Link>
                    <Link href='/'>Events</Link>
                    <Link href='/'>Create Event</Link>
                </ul>
            </nav>
        </header>
    );
};

export default Navbar;
```

Por norma general, en vez de hacer una `ul` para los links, haria una constante que lo guardara todo y simplemente lo mapearia, pero como en este caso, son solamente 3, no creo que merezca la pena, aunque si en un futuro deseara expandirlo, si que lo convertiria en una constante la cual mapeara.

A diferencia de los proyectos de Vite + React, que he hecho anteriormente, la navbar, no va en la pagina principal, sino que va en el layout, ya que queremos que esta navbar este en todas (o en la gran mayoria) de nuestras paginas.


## Componente EventCard

Lo primero que haremos, sera en nuestro pagina principal `app > page.tsx`, crear un pequeño div, debajo del boton, donde mapearemos los eventos, de momento haremos lo dejeremos practicamente vacio, como un placeholder, para cambiarlo más adelante por los datos correctos:

```typescript
<div className="mt-20 space-y-7">
    <h3>Featured Events</h3>

    <ul className="events">
        {[1, 2, 3, 4, 5].map((event) => (
        <li key={event}>Event {event}</li>
        ))}
    </ul>
</div>
```

Crearemos un componente llamado `EventCard`, el cual, recibira los eventos mediante parametros, es decir:

```typescript
const EventCard = ({ title, image }: Props) => {
    return (
        <div>EventCard</div>
    );
};

export default EventCard;
```

Es importante, tener en cuenta que estamos usando typescript, por lo que debemos definir que tipo es cada propiedad que le pasamos al componente, para esto, lo haremos en `interface Props{}`, de la siguiente manera:

```typescript
interface Props {
    title: string;
    image: string;
}
```

Si juntamos esto, tendremos el esqueleto de nuestro componente:

```typescript
interface Props {
    title: string;
    image: string;
}

const EventCard = ({ title, image }: Props) => {
    return (
        <div>EventCard</div>
    );
};

export default EventCard;
```

El componente final, simplemente utilizara las propiedades, para crear una tarjeta, usando tanto el titulo, como la imagen, es importante recordar, que al estar en Next.js, tendremos que usar el `Link` y el `Image` de Next.js:

```typescript
import Image from "next/image";
import Link from "next/link";

interface Props {
    title: string;
    image: string;
}

const EventCard = ({ title, image }: Props) => {
    return (
        <Link href={'/events'} id="event-card">
            <Image src={image} alt={title} width={410} height={310} className="poster" />

            <p className="title">{title}</p>

        </Link>
    );
};

export default EventCard;
```

Una vez tenemos este componente, volveremos a la pagina principal `app > page.tsx`, en la cual habiamos dejado un placeholder para modificarlo ahora con nuestro componente, lo primero que haremos, antes de tocar dicho placeholder, sera crear una constante de eventos arriba del todo, la cual tendra los eventos que vamos a mapear:

```typescript
const events = [
  { image: '/images/event1.png', title: 'Event 1' },
  { image: '/images/event2.png', title: 'Event 2' },
  { image: '/images/event3.png', title: 'Event 3' },
];
```

Una vez con esto, en vez de un array del 1 al 5 que teniamos puesto, podremos poner nuestra `const events` y en vez de renderizar una lista, renderizaremos el componente, al cual le vamos a pasar las propiedades, las cuales son los datos de la constante que hemos creado anteriormente (Importante no olvidar pasarle una key):

```typescript
<ul className="events">
  {events.map((event) => (
    <EventCard key={event.title} title={event.title} image={event.image} />
  ))}
</ul>
```

Otra forma de hacerlo, podria ser pasandole una copia del objeto completo directamente:

```typescript
        <ul className="events">
          {events.map((event) => (
            <li key={event.title}>
              <EventCard {...event} />
            </li>
          ))}
        </ul>
```

De ambas manera funcionara, pero en la segunda, al pasarle una copia del objeto, en caso de que el componente modificase algo de la constante, solamente lo haria para el, ya que no tiene la constante en si, sino una copia de ella.

En este caso, aunque no vaya a modificar los datos dentro del componente, prefiere la segunda opción, ya que es una mejor practica que la anterior, aunque ambas funcionan correctamente. (En caso de que te moleste el puntito de la li, puedes usar `list-none`, para eliminarlo, o usar directamente un `ul` en vez de `li`).

Una vez hemos comprobado que funciona nuestro componente y que podemos pasarle las propiedades sin ningun problema, vamos a añadir todas las propiedades que de verdad necesitamos:

```typescript
export const events = [
  {
    slug: "react-conf-2024",
    image: "/images/event1.png",
    title: "React Conf 2024",
    location: "San Francisco, CA",
    date: "March 15, 2024",
    time: "9:00 AM - 6:00 PM",
  },
  ...
];
```

Como se puede apreciar, esta constante ya es bastante grande, por lo que deberemos de crear un archivo para las constantes, ya que si no, "ensuciara" mucho el codigo y sera dificil de leer más adelante, para ello crearemos una carpeta llamada `lib` y dentro de ella un archivo llamado `constants.ts`, donde guardaremos las constantes. Una vez hayamos hecho el cambio, asegurate de importar la constante de los eventos.

Ademas de añadirlo a esa constante, deberemos de modificar el `interface Props{}`, para que tenga todos los tipos que queremos:

```typescript
interface Props {
    title: string;
    image: string;
    slug: string;
    location: string;
    date: string;
    time: string;
}
```

Una vez tenemos todas las propiedades con sus tipos y valores, deberemos de modificar el componente de EventCard, para que pueda obtenerlas y mostrarla, lo primero que debemos de hacer, es añadir dichas propiedades a los parametros y luego estructurarlo como queramos:

```typescript
import Image from "next/image";
import Link from "next/link";

interface Props {
    title: string;
    image: string;
    slug: string;
    location: string;
    date: string;
    time: string;
}

const EventCard = ({ title, image, slug, location, date, time }: Props) => {
    return (
        <Link href={`/events/${slug}`} id="event-card">
            <Image src={image} alt={title} width={410} height={310} className="poster" />

            <div className="flex flex-row gap-2">
                <Image src="/icons/pin.svg" alt="location" width={14} height={14} />
                <p>{location}</p>
            </div>

            <p className="title">{title}</p>

            <div className="datetime">
                <div>
                    <Image src="/icons/calendar.svg" alt="date" width={14} height={14} />
                    <p>{date}</p>
                </div>
                <div>
                    <Image src="/icons/clock.svg" alt="time" width={14} height={14} />
                    <p>{time}</p>
                </div>
            </div>

        </Link>
    );
};

export default EventCard;
```


