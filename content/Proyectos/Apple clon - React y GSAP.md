
Enlace a la web: https://apple-clon-iota.vercel.app/ <br/>
Enlace al repositorio: https://github.com/PezEjecutivo/appleClon

---

Creamos un proyecto de react, para hacerlo más ligero lo hacemos mediante [vite](https://vite.dev/):

```bash
npm create vite@latest
```

---
## Instalando GSAP para Animaciones 🎨 

Después de crear el proyecto, el siguiente paso es instalar las **dependencias clave** que usaremos para las animaciones y el manejo de *media queries*: **GSAP** y **react-responsive**. Ejecuta esta línea para instalar todas las dependencias necesarias:

```bash
npm install gsap @gsap/react react-responsive
```

### Preparando el Entorno 
Antes de continuar, es una buena práctica dejar limpio el proyecto. Por ello, te recomendamos: 

1. Eliminar el contenido de las carpetas y archivos que no utilizaremos: 
	* `assets` 
	* `App.jsx`
	* `App.css`
	* `index.css` 

Ahora en el archivo de `App.jsx`haremos un **rafce**, para crear la estructura basica de un componente de React, si tenemos desplegado el proyecto y lo vemos, deberia de aparecer un texto que pone "App" en un fondo blanco

### Registro de Plugins de GSAP 
Vamos a utilizar dos plugins esenciales de GSAP: **ScrollTrigger** y **SplitText**. Para que estos plugins funcionen correctamente, debemos registrarlos. 

Sigue estos dos pasos para la importación y el registro: 
1. **Importa los plugins** que necesitas usando esta línea de código: 
```javascript 
import { ScrollTrigger, SplitText } from 'gsap/all' 
```
 
2. **Registra los plugins** usando la función `gsap.registerPlugin()`: 
```javascript 
gsap.registerPlugin(ScrollTrigger, SplitText)
``` 

> **Consejo Clave:** Para mantener tu código **limpio y organizado**, lo mejor es registrar estos plugins en el archivo **`App.jsx`**. Si no los registras allí, tendrás que importarlos y registrarlos en *cada* componente donde vayas a utilizarlos. ¡Así te ahorras trabajo! 

---

## Instalando Tailwind CSS en Vite ✨

[Tailwind CSS](https://tailwindcss.com/) nos permitirá crear estilos de manera increíblemente rápida. Seguiremos la guía oficial para integrarlo con Vite.

A continuación, los pasos para la instalación:

1.  **Instala las dependencias** de Tailwind CSS desde la terminal:

```bash
npm install tailwindcss @tailwindcss/vite
```

2.  **Configura el plugin** en el archivo `vite.config.js`. Debes hacer dos cosas:
    * Importar el plugin: `import tailwindcss from '@tailwindcss/vite'`
    * Añadir `tailwindcss()` al *array* de `plugins`.

    Tu archivo `vite.config.js` final debe lucir similar a este:

```javascript
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'
import tailwindcss from '@tailwindcss/vite'

// https://vitejs.dev/config/
export default defineConfig({
    plugins: [react(), tailwindcss(),],
})
```

3.  **Importa Tailwind CSS** en tu archivo CSS (por ejemplo, `index.css`) con esta línea:

```css
@import "tailwindcss";
```

4. Para comprobar que tailwindcss esta funcionando, cambiaremos el div que pone App en el archivo `App.jsx` por el siguiente h1, en caso de que veamos los estilos aplicados, sabremos que esta funcionando

```html
<h1 class="text-3xl font-bold underline text-indigo-700"> Hello world! </h1>
```

---

### Componente Navbar

Para crear un componente, deberemos de hacerlo en la carpeta **components** que tenemos que crear dentro de la carpeta **src**, los componentes deben empezar siempre por mayusculas, en nuestro caso sera: Navbar.jsx.

Para hacer el componente más sencillo, crearemos una carpeta llamada **constants** que ira en el directorio raiz, donde pondremos las constantes que vamos a utilizar para nuestros componentes, como en este caso cada enlace de la navbar. Es importante exportar las constantes para poder usarlas en los otros archivos

```javascript
export const navLinks = [
    { label: "Store" },
    { label: "Mac" },
    { label: "iPhone" },
    { label: "Watch" },
    { label: "Vision" },
    { label: "AirPods" },
];
```

En caso de que tuvieramos multiples constantes, podriamos exportarlas todas a la vez al final, en vez de poner export en todas y cada una de ellas. 

Una vez tenemos las constantes, ya podemos usarlas junto a la función .map() para crear listas de maneras dinamicas, esto lo podemos hacer la siguiente manera:

```javascript
<ul>
    {navLinks.map(({ label }) => (
        <li key={label}>
            <a href="#">{label}</a>
        </li>
    ))}
</ul>
```

Es importante tener en cuenta, que cuando generemos algo de manera dinamica en react, deberemos de añadirle una propiedad key, sin esto, no funcionara.

Para utilizar imagenes tendremos que tenerlas en la carpeta **public** en nuestro directorio raiz, una vez esten ahi podremos importarlas desde cualquier pagina o componente para utilizar, es importante que la ruta empezara desde la carpeta que tiene las imagenes, no desde la carpeta plubic.

Una vez tenemos el componente hecho, el cual seria el siguiente:

```javascript
<header>
    <nav>
        <a href="/#">
            <img src="/logo.svg" alt="Apple logo" />
        </a>

        <ul>
            {navLinks.map(({ label }) => (
                <li key={label}>
                    <a href={label}>{label}</a>
                </li>
            ))}
        </ul>
        
        <div className='flex-center gap-3'>
            <button>
                <img src="/search.svg" alt="Search" />
            </button>

            <button>
                <img src="/cart.svg" alt="Cart" />
            </button>
        </div>
    </nav>
</header>
```

Es importante mantener una estructura de pagina correcta, por lo que aunque este sea el componente de la navbar, como es lo unico que estara dentro del header, deberemos ponerlo, ya que sino, nuestra estructura final no tendra header y puede causarnos problemas de SEO, ademas de ser negativo en otros aspectos.

---

## Hero layout

Lo primero que haremos sera creare su archivo correspondiente en la carpeta de componentes y añadirlo al App.jsx, de esta manera todo lo que hagamos podremos verlo facilmente en la pagina principal, empezaremos creando la estructura principal que es la siguiente:

```html
<section id='hero'>
    <div>
        <h1>MacBook Pro</h1>
        <img src="/title.png" alt="MacBook Title" />
    </div>
    
    <video src="/videos/hero.mp4" autoPlay muted playsInline />
</section>
```

Es importante que cuando vayamos a usar un video en React le añadamos un ref, esto es debido a que si usamos un useRef() junto al video, podremos modificarlos de muchas más maneras que si no, para hacer esto es tan sencillo como crear una constante que sera la referencia del video y se la pondremos al video:

```javascript
const videoRef = useRef()
...
<video ref={videoRef} src="/videos/hero.mp4" autoPlay muted playsInline />
```

De esta manera, ahora si utilizamos un useEffect(), podremos cambiarle cosas al video, como la velocidad en la que se reproduce para que la "animación" quede más rapida y visual, ya que si la animación es muy lenta puede parecer que la pagina funciona mal o perjudicar la experiencia del usuario al tener que "esperar" para ver la animación completa:

```javascript
useEffect(() => {
    if (videoRef.current) videoRef.current.playbackRate = 2;
}, []);
```

Una vez tenemos el video a nuestro gusto, podemos terminar la estructura del componente añadiendole un poquitin de html extra, siendo este nuestro resultado final:

```html
<section id='hero'>
    <div>
        <h1>MacBook Pro</h1>
        <img src="/title.png" alt="MacBook Title" />
    </div>

    <video ref={videoRef} src="/videos/hero.mp4" muted playsInline />

    <button>Buy</button>
    <p>From $1599 or %133/mo for 12 months</p>
</section>
```

---

## Componente ProductViewer 

Lo primero que haremos sera creare su archivo correspondiente en la carpeta de componentes y añadirlo al App.jsx, de esta manera todo lo que hagamos podremos verlo facilmente en la pagina principal, empezaremos creando la estructura principal que es la siguiente:

```html
<section id='product-viewer'>
    <h2>Take a closer look.</h2>

    <div className='controls'>
        <p className='info'>MacbookPro 16" in Space Black</p>
        
        <div className='flex-center gap-5 mt-5'>
            <div className='color-control'>
                <div className='bg-neutral-300' />
                <div className='bg-neutral-900' />
            </div>

            <div className='size-control'>
                <div><p>14"</p></div>
                <div><p>16"</p></div>
            </div>
        </div>
    </div>
    
    <p className='text-white text-4xl'>Render Canvas</p>
</section>
```

En este caso, como queremos renderizar un canvas, el cual sera el propio Macbook Pro, deberemos de instalar las cosas necesarias, aunque se pueden hacer de diferentes maneras, en este caso lo haremos con [ZUSTAND](https://zustand-demo.pmnd.rs/), para instalarlo solo tendremos que usar el siguiente comando:

```
npm install zustand clsx
```

Una vez tenemos instalado zustand y clsx, deberemos de crear una carpeta en el directorio src, la cual se llamara store y tendra un archivo llamado index.js, en dicho archivo deberemos de añadir lo siguiente:

```javascript
import { create } from 'zustand';

const useMacbookStore = create((set) => ({
    color: '#2e2c2e',
    setColor: (color) => set({ color }),

    scale: 0.08,
    setScale: (scale) => set({ scale }),

    reset: () => set({ color: '#2e2c2e', scale: 0.08 })
}));

export default useMacbookStore;
```

Esto sirve para darle las propiedades que queremos al MacbookPro, de esta manera, podremos cambiarle los colores y tamaño mediante los set de manera sencilla y rapida. Basicamente lo que estamos haciendo es crear un customHook para el modelo 3D.

Con esto hecho, volveremos al componente de ProductViewer y añadiremos el hook que hemos creado, obteniendo todas las propiedas, color, escala y los setters:

```javascript
const { color, scale, setColor, setScale } = useMacbookStore();
```

Con dicha constante creada, ahora deberemos de modificar los divs, los cuales estamos usando como "botones" para controlar los colores y tamaños y añadirle una funciona onClick, la cual sera el setColor o setScale, de esta manera, al pulsar el div cambiaremos el valor de dicha variable al que queremos, ademas de ello, utilizaremos clsx en los classname, para poder añadirle condiciones ternarias para comprobar si estan "seleccionados" y añadirle la clase de activo, aqui hay un ejemplo simple:

```javascript
<div
    onClick={() => setColor('#adb5bd')}
    className={clsx('bg-neutral-300', color === '#adb5bd' && 'active')}
/>
```

De esta manera, al pulsar el div, cambiaremos el color al correspondiente y el clsx al comprobar los colores, una vez lo hemos cambiado a ese, la condición sera verdadera y añadira la clase de 'active'.

Gracias a esto, podremos cambiar multiples cosas del div principal, para que se hagan de manera dinamica y fluida, por lo que el div principal del componente ahora mismo seria el siguiente:

```javascript
<div className='controls'>
    <p className='info'>MacbookPro {scale} in {color}</p>

    <div className='flex-center gap-5 mt-5'>
        <div className='color-control'>
            <div
                onClick={() => setColor('#adb5bd')}
                className={clsx('bg-neutral-300', color === '#adb5bd' && 'active')}
            />
            <div
                onClick={() => setColor('#2e2c2e')}
                className={clsx('bg-neutral-900', color === '#2e2c2e' && 'active')}
            />
        </div>

        <div className='size-control'>
            <div
                onClick={() => setScale(0.06)}
                className={clsx(scale === 0.06 ? 'bg-white text-black' 'bg-transparent text-white')}
            >
                <p>14"</p>
            </div>
            <div
                onClick={() => setScale(0.08)}
                className={clsx(scale === 0.08 ? 'bg-white text-black' 'bg-transparent text-white')}
            >
                <p>16"</p>
            </div>
        </div> 
    </div>
</div>
```

Para comprobar que todo funciona correctamente, si pulsamos un controlador, ya sea el de los colores o el de los tamaños, veremos cambiar el texto, aunque obviamente ese no sera el texto que vamos a dejar, ya que es poco legible, sobretodo el color, ya que esta en hexadecimal en vez de en texto.

Una vez tenemos esto, vamos a renderizar el modelo 3D, para ello vamos a utilizar la libreria de [three.js](https://threejs.org/), en este caso, al estar utilizando react, ademas de instalar three.js, instalaremos dos librerias adiccionales para ayudarnos a utilizar three.js, para ello tendremos que usar el siguiente comando:

```shell
npm install three @react-three/drei @react-three/fiber
```

Una vez tenemos instalado todas estas librerias, volveremos a nuestro componente, ProductViewer y en la etiqueta p que teniamos, la cambiaremos por un Canvas, el cual tiene que ser importado de @react-three/fiber y ademas, importaremos una caja (Box) de @react-three/drei, para comprobar que funciona, por lo que ahora, nuestro componente deberia tener una caja blanca (en 3d) y los siguientes imports:

```javascript
import { Canvas } from '@react-three/fiber';
import { Box } from '@react-three/drei';

...

<Canvas id='canvas'>
    <Box position={[-1 ,1, 0]}></Box>
</Canvas>
```

Es importante que nuestro Canvas tenga una id, ya que luego le iremos cambiando propiedades a dicho modelo, la propiedad position sirve para mover el modelo, hay que tener en cuenta, que al ser un modelo 3D, tiene 3 ejes, X, Y, Z, los cuales son, horizontal, vertical y profundidad, aunque no hayas tocado modelos 3Ds, puede que estes familiarizado gracias a propiedades de css como z-index.

Para comprobar que nuestros botones funcionan, añadiremos las propiedades correspondientes a la caja y veremos como cambian en función del boton que pulsemos, para cambiar el tamaño usaremos scale y para cambiar el color usaremos material-color y como valor pondremos las variables de nuestro customHook:

```javascript
<Canvas id='canvas'>
    <Box position={[-1, 1, 0]} scale={10 * scale} material-color={color}></Box>
</Canvas>
```

Una vez tenemos hecho que cuando pulsemos los botones cambie de color y tamaño nos encargaremos de hacer la camara, para ello deberemos de añadir la propiedad camera y ajustarla como nos guste, pero por mucho que lo cambiemos, no podremos mover el modelo, para pdoer mover el modelo deberemos de añadir dentro del Canvas la etiqueta "OrbitControls" y como no queremos que puedan hacer zoom le añadiremos la propiedad de "enableZoom={false}".

```javascript
<Canvas id='canvas' camera={{ position: [0, 2, 5], fov: 50, near: 0.1, far: 100 }}>
    <Box position={[0, 0, 0]} scale={10 * scale} material-color={color}></Box>

    <OrbitControls enableZoom={false} />
</Canvas>
```

## Renderizado de modelo

Aunque tengamos el modelo 3D que necesitamos o queremos, no podremos usarlo si no lo convertimos en un componente de React, para ello utilizaremos GLTFJSX, con el siguiente comando, el cual basicamente convierte el modelo en un componente de React utilizando gltfjsx, utilizamos el -T para darle una estructura más sencilla y limpia:

(Es importante que pongas la ruta correctamente, en este caso mi ruta era .\ ya que estoy en windows y en la carpeta donde se encuentra los modelos, la cual tengo dentro de public y models, por lo que si estas en la carpeta del proyecto tendras que usar una ruta similar a ./public/models/macbook-14.glb)

```shell
npx gltfjsx .\macbook-14.glb -T
```

Una vez hemos hecho esto veremos que en la propia carpeta se nos ha creado un archivo, él cual es el modelo 3D pero en formato .jsx, para tener una mejor estructura crearemos una carpeta models dentro de la carpeta components en la cual añadiremos dichos archivos.

En el archivo de modelo .jsx, deberemos de cambiar algunas cosas, como el nombre, ya que no lo llamaremos Model, sino su nombre correspondiente que en este caso es: MacbookModel14, cambiaremos a que el export sea un export default y ademas deberemos de cambiar la ruta, en nuestro caso solo tendremos que añadirle /models/ ya que los modelos estan dentro de la carpeta /public/models/

Para poder añadirle una textura utilizaremos una constante el cual sera un hook, especificamente useTexture(), una vez hecho esto deberemos de añadirle en el mesh node 123, la siguiente etiqueta:

```javascript
<meshBasicMaterial map={texture} />
```

De esta manera cambiaremos la textura, una vez hecho esto, deberemos de hacer lo mismo pero con los demas modelos, ya que no vamos a utilizar solamente 1, es importante que hagamos este proceso en los 3 modelos, macbook14, macbook16 y macbook.

En este caso, con los modelos ya hechos, podremos cambiar nuestra caja, por el modelo, en este  caso el que estara renderizado sera el MacbookModel14, por lo quenuestro canvas deberia verse de la siguiente manera:

```javascript
<Canvas id='canvas' camera={{ position: [0, 2, 5], fov: 50, near: 0.1, far: 100 }}>

    <MacbookModel14 scale={0.06} position={[-1, 0, 0]} />
    <OrbitControls enableZoom={false} />

</Canvas>
```

Es importante recordar importar el modelo, una vez hecho esto, no se vera practicamente nada, solamente la textura de la pantalla que le hemos dado antes, para hacer que sea visible deberemos de añadirle una luz ambiental, ya que el modelo esta completamente a oscuras, para hacer eso deberemos de utilizar otra etiqueta, la cual es ambientLight y añadirle la propiedad de intensity, dandonos el siguiente resultado:

```javascript
<Canvas id='canvas' camera={{ position: [0, 2, 5], fov: 50, near: 0.1, far: 100 }}>

    <ambientLight intesity={1} />
    <MacbookModel14 scale={0.06} position={[0, 0, 0]} />
    <OrbitControls enableZoom={false} />

</Canvas>
```

Es importante no confundir ambientLight con AmbientLight de Three, ya que son cosas diferente y probablemente no funcione, una vez tenemos ese Canvas, si pulsamos los botones no pasara nada, ya que le hemos puestos valores fijos, por lo que no te preocupes si ves que han dejado de funcionar. Nuestro siguiente paso es modificar la luz para que se vea correctamente, lo cual es un poco más dificil de lo que puede parecera primera vista, ya que no solo sirvecon el ambientLight intesity={1}


## Componente StudioLights

Lo primero que haremos sera crear una carpeta llamada three dentro de components, ya que este sera un componente para three.js, ahora crearemos el archivo StudioLights.jsx dentro de dicha carpeta, que sera este sera el componente que utilizaremos en vez del ambientLights.

Es importante saber que cuando hagamos el cambio la pagina se rompera, ya que el modelo 3D no funcionara al tener el componente dentro del canvas sin formar parte de él, para solucionar este problema deberemos de cambiar la etiqueta principal del componente a group, de esta manera funcionara.

```javascript
const StudioLights = () => {
    return (
        <group name='lights'></group>
    );
};
```

Esto lo estamos haciendo para poder crear entorno, el cual lo haremos con Environment el cual importamos de @react-three/drei, le pondremos la propiedad de resolution={256}, él cual es una resolución equilibrado tanto en resolución como en rendimiento.

Dentro del Environment crearemos otro group para poder meter varias cosas, las cuales seran Lightformer, importadas tambien de @react-three/drei, pondremos dos, ya que una vendra de la derecha y otra de la izquierda, para que este completamente iluminado

```javascript
<group>
    <Lightformer
        form="rect"
        intensity={10}
        position={[-10, 5, -5]}
        scale={10}
        rotation-y={Math.PI / 2}
    />

    <Lightformer
        form="rect"
        intensity={10}
        position={[10, 0, 1]}
        scale={10}
        rotation-y={Math.PI / 2}
    />
</group>
```

Una vez tenemos las luces principales, podemos crear spotligths, que son basicamentes, destellos o luces de resalto, las cuales sirven para resaltar ciertas partes, esto lo haremos con la etiqueta spotLight:

```javascript
<spotLight
    position={[-2, 10, 5]}
    angle={0.15}
    decay={0}
    intensity={Math.PI * 0.2}
/>
```

Ahora simplemente añadiremos unos cuantos destellos más para que quede mejor, dando el siguiente componente final:

```javascript
<group name='lights'>

    <Environment resolution={256}>
        <group>
            <Lightformer
                form="rect"
                intensity={10}
                position={[-10, 5, -5]}
                scale={10}
                rotation-y={Math.PI / 2}
            />

            <Lightformer
                form="rect"
                intensity={10}
                position={[10, 0, 1]}
                scale={10}
                rotation-y={Math.PI / 2}
            />
        </group>
    </Environment>

    <spotLight
        position={[-2, 10, 5]}
        angle={0.15}
        decay={0}
        intensity={Math.PI * 0.2}
    />

    <spotLight
        position={[0, -25, 10]}
        angle={0.15}
        decay={0}
        intensity={Math.PI * 0.2}
    />

    <spotLight
        position={[0, 15, 5]}
        angle={0.15}
        decay={0}
        intensity={Math.PI * 1}
    />
</group>
```

---

## Componente ModelSwitcher

Esta vez, en vez de crear el componente en la carpeta de components, deberemos de hacerlo en la carpeta three que hemos creado para el componente anterior, ya que va a ser un componente de three.js, Una vez dentro de esa carpeta, crearemos el componente ModelSwitcher.jsx, el cual nos servira para animar con GSAP los cambios de modelo, ademas añadiremos los PresentationControls, que haran lo que ahora mismo hace OrbitControls, pero de una manera más customizable.

Importaremos el componente ModelSwitcher, ademas de crear la constante de isMobile con el useMediaQuery, esto nos servira para crear una condición ternaria la cual dependiendo de nuestra resolución cambiar el tamaño del modelo, ya que si es mobil lo haremos algo más pequeño para que quepa en pantalla:

```javascript
<ModelSwitcher scale={isMobile ? scale - 0.03 : scale} isMobile={isMobile} />
```

Una vez tenemos este añadimiento, vamos a crear el componente, es importante tener en cuenta, que le estamos pasando propiedades, por lo que a diferencia de los anteriores, deberemos de añadirle dichas propiedades a la hora de crear el componente, ademas, crearemos una referencia para saber cual es el que modelo que vamos a mostrar. Ademas de crear una constante para el tamaño, es importante añadirle un or, ya que en caso de que sea resolución de movil, el tamaño debera ser más pequeño:

```javascript
const SCALE_LARGE_DESKTOP = 0.08;
const SCALE_LARGE_MOBILE = 0.05;

const smallMacbookRef = useRef();
const largeMacbookRef = useRef();

const showLargeMacbook = scale === SCALE_LARGE_DESKTOP || scale === SCALE_LARGE_MOBILE;
```

En vez de tener la etiqueta del componente como section o un div, dejaremos la etiqueta vacia, creando así un reactFragment, el cual contendra el PresntationControls, él cual debemos de importar de @react-thre/drei, dentro de dicha etiqueta tendremos un group, el cual tendra un ref para saber que modelo es y el modelo correspondiente, esto lo duplicaremos, para tener uno por cada modelo, quedando el siguiente resultado:

```javascript
<PresentationControls>
    <group ref={largeMacbookRef}>
        <MacbookModel16 scale={isMobile ? 0.05 : 0.08} />
    </group>
</PresentationControls>

<PresentationControls>
    <group ref={smallMacbookRef}>
        <MacbookModel14 scale={isMobile ? 0.03 : 0.06} />
    </group>
</PresentationControls>
```

Una vez tenemos esto, veremos 3 modelos en la pagina, esto es debido a que aún no habiamos borrado el MacbookModel14 que teniamos, por lo que al borrarlo veremos que solo quedan 2, de los cuales, comentaremos uno, aunque le iremos haciendo los cambios a la vez, esto nos servira para poder ver facilmente lo que estamos haciendo.

Les pasaremos una copia del objeto gracia a los ... y añadiremos la constante controlsConfig, la cual crearemos con la propiedad de `snap: true`, esto sera para cuando movamos el Macbook, se reposicione automaticamente a su posición de origen.

```javascript
<PresentationControls {... controlsConfig}>
    <group ref={largeMacbookRef}>
        <MacbookModel16 scale={isMobile ? 0.05 : 0.08} />
    </group>
</PresentationControls>

{/* <PresentationControls {...controlsConfig}>
    <group ref={smallMacbookRef}>
        <MacbookModel14 scale={isMobile ? 0.03 : 0.06} />
    </group>
</PresentationControls> */}
```

Una vez hemos comprobado que funciona, añadiremos más configuraciones a dicha constante, de esta manera personalizaremos a nuestro gusto los controles de la camara, de la siguiente manera: <br/>

snap: Vuelve a la posición de origen. <br/>
speed: Cambia la velocidad de lo que mueves. <br/>
zoom: Es el zoom de la camara <br/>
polar: [-Math.PI, Math.PI]: Hace que puedas moverlo en el eje Y todo lo que quieras. <br/>
azimuth: [-Infinity, Infinity]: Hace que puedas moverlo en el eje X todo lo que quieras. <br/>
config: { mass: 1, tension: 0, friction: 26 }: Imita las fisicas reales <br/>

De esta manera, nuestro objeto de configuración sera esto:

```javascript
const controlsConfig = {
    snap: true,
    speed: 1,
    zoom: 1,
    polar: [-Math.PI, Math.PI],
    azimuth: [-Infinity, Infinity],
    config: { mass: 1, tension: 0, friction: 26 }
};
```
Ahora vamos a crear algunas constantes que utilizaremos para las animaciones y modelos, como la duración y la distancia a la que vamos a mover los modelos cuando los cambiemos, ya que van a aparecer que se van y vuelven, para esto utilizaremos una función, que en caso de tener un grupo, miraremos cada elemento dentro del grupo y le aplicaremos los estilos y animaciones

```javascript
const ANIMATION_DURATION = 1;
const OFFSET_DISTANCE = 5;

const fadeMeshes = (group, opacity) => {
    if (!group) return;

    group.traverse((child) => {
        if (child.isMesh) {
            child.material.transparent = true;
            gsap.to(child.material, { opacity, duration: ANIMATION_DURATION });
        }
    });
};
```

De esta manera, aplicaremos la animación del gsap.to() a todos los hijos del elemento group, que hara transparante tanto el material como el propio elemento, haciendo ver que "desaparece", ademas de esto, para darle una mejor experiencia al usuario, haremos que se muevan para un lado antes de desaparecer por completo, dando una sensación más natural

```javascript
const moveGroup = (group, x) => {
    if (!group) return;

    gsap.to(group.position, { x, duration: ANIMATION_DURATION });
};
```

Una vez tenemos estas funciones creadas, para poder utilizar de manera correcta deberemos de utilizar el hook de useGSAP(), el cual deberemos hacer que se renderice cada vez que le cambiemos el tamaño al modelo, ya que queremos que la animación aparezca cuando cambiemos el tamaño:

```javascript
useGSAP(() => { }, [scale]);
```

Una vez tenemos el useGSAP() que se actualiza cuando queremos, deberemos de añadirle las funciones que hemos creado anteriormente, para ocultar o mostrar y hacer que se muevan los modelos

```javascript
useGSAP(() => {
    
    if (showLargeMacbook) {
        moveGroup(smallMacbookRef.current, -OFFSET_DISTANCE);
        moveGroup(largeMacbookRef.current, 0);

        fadeMeshes(smallMacbookRef.current, 0);
        fadeMeshes(largeMacbookRef.current, 1);
    } else {
        moveGroup(smallMacbookRef.current, 0);
        moveGroup(largeMacbookRef.current, +OFFSET_DISTANCE);

        fadeMeshes(smallMacbookRef.current, 1);
        fadeMeshes(largeMacbookRef.current, 0);
    }

}, [scale]);
```

Es importante que descomentemos el que habiamos comentado anteriormente, ya que si no, no se mostrara el otro modelo una vez se haga la animación y quedara vacio, despues de haber hecho esto solo nos faltaria hacer que podamos cambiar el color, ya que actualmente no funciona. 

Para hacer que funcione tendremos que ir al modelo y añadirle una constante del color, la cual vendra del useMacbookStore(), una vez tenemos esto, tambien deberemos de obtener la escena del useGLTF, una vez tenemos esas dos cosas adicionales, crearemos un useEffect que se renderice cada vez que actualizamos el color, ya que es lo que nos falta:

```javascript
const { color } = useMacbookStore();
const { nodes, materials, scene } = useGLTF('/models/macbook-14-transformed.glb');

...

useEffect(() => { }, [color]);
```

Con eso ya como base deberemos de modificar el useEffect(), para que pille la escena y a cada elemeto hijo de la escena, en caso de que sea un mesh (un mesh es una parte del modelo 3D) y no sea una de las partes que no debemos cambiar, le cambiaremos el color al correspondiente que estamos sacando del useMacbookStore(). De esta manera, aunque en el ProductViewer no se le esta pasando el color, como lo obtenemos directamente el hook, sigue funcionando los botones:


```javascript
useEffect(() => {

    scene.traverse((child) => {
        if (child.isMesh) {
            if (!noChangeParts.includes(child.name)) {
                child.material.color = new Color(color);
            }
        }
    });

}, [color, scene]);
```

Es importante que hagamos estas modificaciones en los dos modelos, ya que si solo lo hacemos en uno, no funcionara en el otro, ya que no lo tiene implementado, para ello, simplemente deberemos copiar y pegar.

## Componente Showcase

Lo primero que tendremos que hacer es crear un componente, en este caso, si va a ser un componente normal de React, por lo que lo crearemos y lo importaremos justo debajo del componente ProductViewer, la estructura de nuestro nuevo componente sera la siguiente:

```html
<section id='showcase'>
    <div className='media'>
        <video src="/videos/game.mp4"></video>
    </div>
</section>
```

Aunque con esa estructura parece solamente una imagen, es un video, para hacer que se reproduzca de la manera en la que queremos, deberemos de añadir las siguientes etiquetas </br>

loop: Para que se repita indefinidamente </br>
muted: Para que no tenga sonido </br>
autoPlay: Para que se reproduzca automaticamente </br>
playsInline: Para que no tenga reproductor </br>

De esta manera, tendremos la siguiente estructura y el video ahora se reproducira perfectamente:

```html
<section id='showcase'>
    <div className='media'>
        <video src="/videos/game.mp4" loop muted autoPlay playsInline></video>
    </div>
</section>
```

Una vez tenemos el video funcionando, como nuestro objeto es hacer una animación similar a la que hicimos en la otra pagina pero a la inversa, es decir, un enmascaramiento del video con una imagen, para que el video se quede reproduciendo de fondo en la imagen, dandole un toque "impresionante" al componente/sección.

Para ello, en el propio div que contiene el video, añadiremos otro con el className de "mask" y le añadiremos un img, con la imagen que queremos usar para hacer el enmascaramiento, haciendo que el div que contiene el video se vea así.

```html
<div className='media'>
    <video src="/videos/game.mp4" loop muted playsInline></video>
    <div className='mask'>
        <img src="/mask-logo.svg" alt="logo del chip apple" />
    </div>
</div>
```

Aunque actualmente no podremos ver la imagen, a no ser que estemos con una resolución de mobil, añadiremos el siguiente contenido para tener el componente con todo el contenido que queremos, el cual, solo podra verse de momento en modo mobil, una vez comprobemos que esta todo, lo animaremos para resoluciones mayores:

```html
<section id='showcase'>

    <div className='media'>
        <video src="/videos/game.mp4" loop muted autoPlay playsInline></video>

        <div className='mask'>
            <img src="/mask-logo.svg" alt="logo del chip de apple" />
        </div>
    </div>

    <div className='content'>
        <div className='wrapper'>

            <div className='lg:max-w-md'>
                <h2>Rocket Chip</h2>

                <div className='space-y-5 mt-7 pe-10'>
                    <p>Introducing {" "}
                        <span className='text-white'>
                            M4, the next generation of Apple silicon
                        </span>
                        . M4 powers
                    </p>
                    <p>
                        It drives Apple intelligence on iPad Pro, so you can write, create, and accomplish more with ease. All in a design that's unbelievably thin, light, and powerful.
                    </p>
                    <p>
                        A brand-new display engine delivers breathtaking precision, color accuracy, and brightness. And a next-gen GPU with hardware-accelerated ray tracing brings console-level graphics to your fingertips.
                    </p>
                    <p className='text-primary'>Learn more about Apple Intelligence</p>
                </div>

            </div>

            <div className='max-w-3xs spcae-y-14'>

                <div className='space-y-2'>
                    <p>Up to</p>
                    <h3>4x faster</h3>
                    <p>pro rendering perfomance than M2</p>
                </div>

                <div className='space-y-2'>
                    <p>Up to</p>
                    <h3>1.5x faster</h3>
                    <p>CPU perfomance than M2</p>
                </div>
            </div>
        </div>
    </div>
</section>
```

Una vez tenemos la estructura, vamos a empezar a animarlo, para ello, lo primero que vamos a hacer, es mediante el hook, useMediaQuery, crear una variable que guardara el valor para comprobar si la resolución es de una tablet, despues de eso utilizaremos el useGSAP() junto a un condicional usando la variable que hemos creado previamente:

```javascript
const isTablet = useMediaQuery({ query: '(max-width: 1024px)' });

useGSAP(() => {
    if(!isTablet) {

    }
})
```

Es importante tener en cuenta que el if esta negado, ya que esto solo se hara si la resolución es mayor a la de una tablet/movil, de ahi, que antes solamente se viera con una resolución menor, una vez tenemos esto, deberemos de crear una timeline, con un scrollTrigger con las siguientes caracteristicas </br>

trigger: '#showcase' - Para utilizar la section showcase como iniciador/disparador </br>
start: 'top top' - Para que empiece la animación cuando la parte de arriba de la pantalla alcance la parte de arriba del trigger </br>
end: 'bottom top' - Para marcar el final de la animación cuando la parte de abajo de la pantalla alcance la parte de arriba del trigger </br>
scrub: true - Para que la animación se haga a la vez que scrolleamos </br>
pin: true - Para que se quede en mitad de la pantalla a medida que scrolleamos </br>

Dandonos una timeline final de esta manera:

```javascript
const timeline = gsap.timeline({
    scrollTrigger: {
        trigger: '#showcase',
        start: 'top top',
        end: 'bottom top',
        scrub: true,
        pin: true,
    }
});
```

Las animaciones seran muy simples, tales como poner el texto con opacidad 1 y bajar el tamaño de la imagen con scale, de esta manera podremos hacer una animación simple pero bonita:

```javascript
timeline
.to('.mask img', {
    transform: 'scale(1.2)'
})
.to('.content', { opacity: 1, y: 0, ease: 'power1.in' });
```

Una vez hemos hecho esto, puede que no funcione o que solamente funciona cuando cambiamos de tamaño a la ventana, esto se debe a que cuando cambiamos el tamaño de la ventana se aplica automaticamente un ScrollTrigger.refresh(). Y por que necesitamos un ScrollTrigger.refresh()? Esto es debido a que el scrollTrigger se aplica antes de que cargue el contenido de la pagina, es decir, la imagen y el video.

Es por esto, que si cambiamos el tamaño de la ventana, como lo hacemos una vez han cargado tanto el video como la imagen, el refresh() se activara y modificara el pin acorde, para solucionar este problema no basta con un setTimeout(), ya que esto lo que haria sería retrasar la creación del scrollTrigger, por lo que no se aplicaria nunca el pin, ya que el pin es un elemento que se crea en el DOM.

Como lo solucionamos? Para solucionar este problema, deberemos de utilizar los metodos onLoadedData, en caso del video y onLoad, en caso de la imagen y le pasaremos una función por expresión que sera la siguiente:

```javascript
const refreshScroll = () => {
    ScrollTrigger.refresh();
};

...

<video src="/videos/game.mp4" loop muted autoPlay playsInline onLoadedData={refreshScroll} ></video>
<div className='mask'>
    <img src="/mask-logo.svg" alt="logo del chip apple" onLoad={refreshScroll} />
</div>
```

De esta manera, se hara el ScrollTrigger.refresh() de manera automatica, el unico problema que tiene esta solución es que necesitaremos importar de nuevo el ScrollTrigger ya que estamos usando una función de dicho objeto directamente, por lo que crearemos un poco de duplicidad en el codigo, aunque repetir partes de codigo no es lo mejor, al ser algo tan pequeño y una unica vez adiccional, no es tan negativo.

```javascript
import { ScrollTrigger } from 'gsap/ScrollTrigger';
gsap.registerPlugin(ScrollTrigger);
```

Una vez con todo esto quedaria el siguiente componente, funcionando de manera adecuada en todas las resoluciones y momentos

```javascript
import { useGSAP } from '@gsap/react';
import gsap from 'gsap';
import { useMediaQuery } from 'react-responsive';
import { ScrollTrigger } from 'gsap/ScrollTrigger';
gsap.registerPlugin(ScrollTrigger);

const Showcase = () => {
    const isTablet = useMediaQuery({ query: '(max-width: 1024px)' });

    useGSAP(() => {
        if (!isTablet) {
            const timeline = gsap.timeline({
                scrollTrigger: {
                    trigger: '#showcase',
                    start: 'top top',
                    end: 'bottom top',
                    scrub: true,
                    pin: true,
                }
            });

            timeline
                .to('.mask img', {
                    transform: 'scale(1.2)'
                })
                .to('.content', { opacity: 1, y: 0, ease: 'power1.in' });
        }
    }, [isTablet]);

    const refreshScroll = () => {
        ScrollTrigger.refresh();
    };

    return (
        <section id='showcase'>
            <div className='media'>
                <video src="/videos/game.mp4" loop muted autoPlay playsInline onLoadedData={refreshScroll} ></video>
                <div className='mask'>
                    <img src="/mask-logo.svg" alt="logo del chip de apple" onLoad={refreshScroll} />
                </div>
            </div>

            <div className='content'>
                <div className='wrapper'>
                    <div className='lg:max-w-md'>
                        <h2>Rocket Chip</h2>

                        <div className='space-y-5 mt-7 pe-10'>
                            <p>Introducing {" "}
                                <span className='text-white'>
                                    M4, the next generation of Apple silicon
                                </span>
                                . M4 powers
                            </p>
                            <p>
                                It drives Apple intelligence on iPad Pro, so you can write, create, and accomplish more with ease. All in a design that's unbelievably thin, light, and powerful.
                            </p>
                            <p>
                                A brand-new display engine delivers breathtaking precision, color accuracy, and brightness. And a next-gen GPU with hardware-accelerated ray tracing brings console-level graphics to your fingertips.
                            </p>
                            <p className='text-primary'>Learn more about Apple Intelligence</p>
                        </div>
                    </div>

                    <div className='max-w-3xs space-y-14'>
                        <div className='space-y-2'>
                            <p>Up to</p>
                            <h3>4x faster</h3>
                            <p>pro rendering perfomance than M2</p>
                        </div>
                        <div className='space-y-2'>
                            <p>Up to</p>
                            <h3>1.5x faster</h3>
                            <p>CPU perfomance than M2</p>
                        </div>
                    </div>
                </div>
            </div>
        </section>
    );
};

export default Showcase;
```

## Componente Perfomance

Crearemos el componente como un componente normal de React y cambiaremos el div por un section, como acostumbramos a hacer en este proyecto. Es recomendable que tu pagina siga una estructura, de ahi que todos los componentes o la gran mayoria, utilicen section en vez del div que sale al crearlo con rafce.

Como en este componente vamos a utilizar muchas imagenes, para no llenar el HTML y dejarlo limpio para cuando vayamos a ver el codigo, renderizaremos las imagenes de manera dinamica, para ello, mapearemos las imagenes desde nuestro archivo de constantes:

```javascript
<div className='wrapper'>
    {performanceImages.map(( image ) => (
        <img key={image.id} src={image.src} alt={image.id} />
    ))}
</div>
```

Es importante saber que aunque esto funciona, no es lo más optimo, ya que estamos repitiendo multiples veces la palabra "image", añadiendo ruido y duplicidad al codigo, para solucionar esto, en vez de poner image en el map, pondremos corchetes {} y pondremos la propiedad que queremos sacar de dicho objeto, quedando el .map de la siguiente manera:

```javascript
<div className='wrapper'>
    {performanceImages.map(({ id, src }) => (
        <img key={id} src={src} alt={id} />
    ))}
</div>
```

Una vez tenemos hecho el mapeado de imagenes, simplemente añadiremos el contenido de la pagina y empezaremos a crear las animaciones, al igual que hemos hecho anteriormente es importante que funcionen de manera diferente en una resolución más pequeña por lo que volveremos a crear la variable de isMobile con el useMediaQuery.

```javascript
const isMobile = useMediaQuery({ query: '(max-width: 1024px)'});
```

Una vez tenemos esto animaremos el texto primero, ya que es sencillo de animar pero lo haremos de manera diferente a lo habitual, en este caso utilizaremos un fromTo, en vez de solamente un to, aunque tocaremso pocas cosas en el from, ya que solo vamos a moverlo un poco y ponerle 0 de opacidad, para que con el to, lo volvamos a su sitio y la opacidad correspondiente, haciendo que parezca que salen del suelo, le añadiremos un scrollTrigger para que vaya segun scrolleamos y ya estaria la animación del texto, es importante coger tanto el .content como el p. 

```javascript
gsap.fromTo(
    ".content p",
    { opacity: 0, y: 10 },
    {
        opacity: 1,
        y: 0,
        ease: "power1.out",
        scrollTrigger: {
            trigger: ".content p",
            start: "top bottom",
            end: "top center",
            scrub: true,
        },
    }
);
```

Con la animación del texto ya hecha, haremos la animación principal de la sección, que al igual que la de la sección anterior, esta no se aplicara en mobiles, por lo que deberemos de poner un condicional para comprobar si esta en mobil gracias a la variable que hemos creado antes y devolver un return en caso de que sea verdadero

```javascript
if(isMobile) return;
```

Importante hacer esto que vamos a hacer despues de dicho condicional, ya que si no, se ejecutara aunque este en mobil, ahora crearemos una timeline con ciertos valores por defectos para que a la hora de hacer las animaciones mantenga esos valores y con el scrollTrigger indicaremos cuando acaban

```javascript
const tl = gsap.timeline({
    defaults: { duration: 2, ease: "power1.inOut", overwrite: "auto" },
    scrollTrigger: {
        trigger: '#performance',
        start: "top bottom",
        end: "center center",
        scrub: 1,
    },
});
```

Una vez hecho esto, toca crear las animaciones de dicha timeline, lo cual va a ser algo más complejo de lo que hemos hecho con anterioridad, ya que esta vez sacaremos las animaciones de una de nuestra constantes que tenemos, voy a mostrar una de ejemplo para que se pueda entender mejor la animación:

```javascript
{
    id: "p1",
    left: 5,
    bottom: 65,
},
```

Nuestra constante performanceImgPositions, tiene multiples objetos los cuales tienen valores diferentes y segun esos valores nosotros crearemos la animación, pero en este caso en especifico, hay una imagen que no queremos que se anime, que sera la "p5", por lo que haremos un condicional para que ignore dicha imagen en el forEach().

```javascript
performanceImgPositions.forEach((item) => {
    if (item.id === "p5") return; 
})
```

Una vez tenemos el forEach de tal manera que ignora la imagen que queremos, nos tocara hacer un poquito de logica para animar las cosas adecuadamente, tendremos que hacer 2 variables, una para saber que elemento vamos a animar y otra que sera un objeto, para almacenar las animaciones que vamos a hacer, es importante que antes de añadir nada a la variable que es un objeto, comprobemos que no este vacia.

```javascript
const selector = `.${item.id}`;
const vars = {};

if (typeof item.left === "number") vars.left = `${item.left}%`;
if (typeof item.right === "number") vars.right = `${item.right}%`;
if (typeof item.bottom === "number") vars.bottom = `${item.bottom}%`;
if (item.transform) vars.transform = item.transform;
```

De esta manera, ya tendriamos una variable con el nombre del selector y una variable que es un objeto con las animaciones que queremos hacer, por lo que solo nos tocara, usar dichas variables junto a .to() para animarlo:

```javascript
tl.to(selector, vars, 0);
```

De esta manera, si hemos seguido los pasos de manera correctamente, acabaremos con el siguiente useGSAP():

```javascript
useGSAP(() => {

    gsap.fromTo(
        ".content p",
        { opacity: 0, y: 10 },
        {
            opacity: 1,
            y: 0,
            ease: "power1.out",
            scrollTrigger: {
                trigger: ".content p",
                start: "top bottom",
                end: "top center",
                scrub: true,
                invalidateOnRefresh: true,
            },
        }
    );

    if (isMobile) return;
    
    const tl = gsap.timeline({
        defaults: { duration: 2, ease: "power1.inOut", overwrite: "auto" },
        scrollTrigger: {
            trigger: '#performance',
            start: "top bottom",
            end: "center center",
            scrub: 1,
        },
    });

    performanceImgPositions.forEach((item) => {
        if (item.id === "p5") return;

        const selector = `.${item.id}`;
        const vars = {};

        if (typeof item.left === "number") vars.left = `${item.left}%`;
        if (typeof item.right === "number") vars.right = `${item.right}%`;
        if (typeof item.bottom === "number") vars.bottom = `${item.bottom}%`;
        if (item.transform) vars.transform = item.transform;

        tl.to(selector, vars, 0);
    });
},
);
```

## Componente Feature
Este componente seguira siendo un componente de React normal y corriente, aunque vamos a utilizar un modelo 3D otra vez, por lo que le haremos un pequeño cambio al index.js que tenemos en la carpeta store, ya que ahora tenemos otra necesidad más, y esa sera cambiarle la textura, por lo que deberemos de crear lo mismo que hizimos para el color y la escala, pero para la textura:

```javascript
texture: '/videos/feature-1.mp4',
setTexture: (texture) => set({ texture }),

reset: () => set({ color: '#2e2c2e', scale: 0.08, texture: '/videos/feature-1.mp4' })
```

Usando el unico modelo que no llegamos a utilizar, es decir, el del Macbook Pro normal, no el model 14 o 16, deberemos de añadirle el soporte para la textura y el color, ya que queremos modificarselo, al igual que previamente, deberemos de usar useMacbookStore() para obtener tanto el color como la textura:

```javascript
const { color, texture } = useMacbookStore;
```

Ademas de esto, tenemos que añadirle la escana al useGLTF para modificar la pantalla, una vez tenemos ese cambio podemos crear una variable para la pantalla, usando el useVideoTexture de @react-three/drei y como ya teniamos hecho el useEffect para modificar los colores, podemos reutilizarlo

```javascript
const { nodes, materials, scene } = useGLTF('/models/macbook-transformed.glb');

const screen = useVideoTexture(texture);

useEffect(() => {
  scene.traverse((child) => {
    if (child.isMesh) {
      if (!noChangeParts.includes(child.name)) {
        child.material.color = new Color(color);
      }
    }
  });
}, [color, scene]);
```

Al igual que en los modelos anteriores, deberemos de cambiar el mesh con el nodes.Object_123 y en este caso le quitaremos el material y añadiremos el nuestro, aunque en los otros pusimos texture, en este pondremos screen, ya que hemos hecho previamente que screen sea el useVideoTexture(texture):

```javascript
<mesh geometry={nodes.Object_123.geometry} rotation={[Math.PI / 2, 0, 0]}>
  <meshBasicMaterial map={screen} />
</mesh>
```

Una vez tenemos esto, para representar el modelo 3D, al igual que en el otro componente, deberemos de crear un Canvas importado de @react-three/fiber, ademas, aprovecharemos el componente de three que creamos previamente, StudioLigths, de momento no pondremos el modelo del Macbook Pro, porque prepareremos la sección.

```javascript
<Canvas id='f-canvas' camera={{}}>
    <StudioLights />
    <ambientLight intensity={0.5} />
    {/* 3D MODEL */}
</Canvas>
```

Para preparar la sección mapearemos las features de nuestro archivo de constantes, el cual tendra clases que se renderizan dinamicamente, al igual que hicimos previamente, usando clsx

```javascript
<div className='absolute inset-0'>
    {features.map((feature, index) => (
        <div className={clsx('box', `box${index + 1}`, feature.styles)}>{feature.text}</div>
    ))}
</div>
```

Para cargar el modelo 3D en este caso, como tendra varios videos puede llegar a tardar, por lo que crearemos una función por expresión, lá cual muestre un texto de "Loading..." en caso de que no se haya terminado de cargar, aunque no es muy complicado, puede llegar a dificultarse si no sabemos lo que estamos haciendo, por lo que lo primero que tendremos que hacer es crear un group de three, con una referencia de useRef, la cual llamaremos groupRef, y dentro de la etiqueta gruoup crearemos una que es Suspense, la cual tenemos que importar de React. Con esto, deberemos de añadirle el metodo de fallback={}, la cual contendra una etiqueta Html que viene de @react-three/drei, y ahora ya podremos poner dentro nuestro H1 que pone Loading... y solamente tendremos que añadir el modelo, dependiendo el tamaño de si es mobil o no.

```javascript
<Suspense fallback={<Html><h1 className='text-white text-3xl uppercase'>Loading...</h1></Html>}>
    <MacbookModel scale={isMobile ? 0.05 : 0.08} position={[0, -1, 0]} />
</Suspense>
```

Con eso hecho, ya podemos ponerlo en nuestro Canvas y podremos verlo de manera adecuada, ahora carga rapido ya que es solamente un video, en caso de que tuviera que cargar varios podria tardar más pero para ello utilizaremos una carga virtual de los videos.

```javascript
<Canvas id='f-canvas' camera={{}}>
    <StudioLights />
    <ambientLight intensity={0.5} />
    <ModelScroll />
</Canvas>
```

Para crear dicho elemento virtual utilizaremos un useEffect, en el cual obtendremos una de nuestras constantes (featureSequence) y de cada feature crearemos una variable que sera un video que creamos mediante el document.createElement('video') y mediante Object.assign(), le asignamos las propiedades que queremos que tenga el video y con .load() lo cargamos, de esta manera estaremos cargando los videos sin mostrarlos en la pagina, para que cuando queramos mostrarlo ya esten listos

```javascript
useEffect(() => {
    featureSequence.forEach((feature) => {
        const v = document.createElement('video');

        Object.assign(v, {
            src: feature.videoPath,
            muted: true,
            playsInline: true,
            preload: 'auto',
            crossOrigin: 'anonymous'
        });

        v.load();
    });
}, []);
```

Una vez tenemos esto, vamos a crear una animación sencilla, en la cual rotaremos el modelo 3D para darle un efecto "espectacular" mientras scrolleamos y mostramos las caracteristicas de dicho modelo, para ello, como todas las animaciones hechas en el proyecto, lo haremos mediante useGSAP(), para ello crearemos una timeline sencilla con la propiedad de pin activada:

```javascript
const modelTimeline = gsap.timeline({
    scrollTrigger: {
        trigger: '#f-canvas',
        start: 'top top',
        end: 'bottom top',
        scrub: 1,
        pin: true,
    }
});
```

Usando dicha timeline y la referencia que pusimos en el group, es decir, groupRef, crearemos una animación sencilla que simplemente haga girar el modelo, para ello utilizaremos un condicional, para saber si ya esta cargado el grupo a partir de la referencia y animaremos exclusivamente la rotación, por lo que debemos aplicar la animación al groupRef.current.rotation y para que gire completamente utilizaremos Math.PI * 2, ya que PI es medio radio.

```javascript
if (groupRef.current) {
    modelTimeline.to(groupRef.current.rotation, { y: Math.PI * 2, ease: 'power1.inOut' });
}
```

Un dato curioso que se puede apreciar en la animación es que el giro se hace horizontalmente, pero nosotros estamos cambiando el valor de Y, que es el eje vertical, entonces... Porque gira de manera horizontal al cambiar el valor del eje vertical? Esto se debe a que estamos cambiando la propiedad de rotation, no el valor del eje en si, por lo que al cambiar el valor de la rotación girara en horizontal, ya que si cambiamos el valor del eje en sí, ya si que se movera en vertical. Para simplificarlo, simplemente debemos acordanos que en rotación los ejes estan "invertidos", así es más facil de recordar.

Ahora crearemos otra timeline, la cual sera practicamente indentica a la primera, pero sera una variable diferente, ya que esta la utilizaremos para los textos y cambiarle el video del modelo, por lo que tendriamos otra timeline igual a la anterior, excepto porque empezara despues, ya que esta empieza cuando la parte de arriba del trigger, alcance la mitad de la pantalla:

```javascript
const timeline = gsap.timeline({
    scrollTrigger: {
        trigger: '#f-canvas',
        start: 'top center',
        end: 'bottom top',
        scrub: 1,
    }
});
```

Con la timeline ya creada y ajustada, tocara haciar las animaciones, que en este caso, ademas del .to(), tambien utilizaremos .call(), que esto sirve para llamar a una función, la cual sera la función de cambiar la textura (es decir, el video de la pantalla) y despues lo animaremos.

```javascript
timeline
    .call(() => setTexture('/videos/feature-1.mp4'))
    .to('.box1', { opacity: 1, y: 0, delay: 1 });
```

Una vez tenemos esa simple animación, ahora lo que haremos sera repetirla por cada video/caracteristica que tenemos cambiando los valores para que se correspondan y no salga todo el rato la misma, quedando tal que así:"

```javascript
timeline
    .call(() => setTexture('/videos/feature-1.mp4'))
    .to('.box1', { opacity: 1, y: 0, delay: 1 })

    .call(() => setTexture('/videos/feature-2.mp4'))
    .to('.box2', { opacity: 1, y: 0})

    .call(() => setTexture('/videos/feature-3.mp4'))
    .to('.box3', { opacity: 1, y: 0 })

    .call(() => setTexture('/videos/feature-4.mp4'))
    .to('.box4', { opacity: 1, y: 0 })

    .call(() => setTexture('/videos/feature-5.mp4'))
    .to('.box5', { opacity: 1, y: 0, })
```

Una vez tenemos esto, podemos ver como se hace la animación y el video de la pantalla va cambiando según bajas, ademas del texto que aparece, para darle un toque más elegante cambiaremos un poco el texto para que quede mejor. Es importante saber que aunque hemos cargado los videos previamente, es posible que en el cambio de videos el modelo desaparezca por unos instantes y vuelva a aparecer, eso es debido a que se le esta aplicando el cambio de textura por primera vez, una vez se han aplicado, ya no volvera a pasar por mucho que scrolles nuevamente.

```javascript
{features.map((feature, index) => (
    <div key={feature.id} className={clsx('box', `box${index + 1}`, feature.styles)}>
        <img src={feature.icon} alt={feature.highlight} />
        <p>
            <span className='text-white'>{feature.highlight}</span>
            {feature.text}
        </p>
    </div>
))}
```

Con esto, ya tendriamos tanto la animación, como el texto, por lo que el componente ya esta terminado y completado.

## Componente Hightligths
Al igual que en los componentes anteriores que eran componentes de React y no de Three, crearemos su estructura basica y cambiaremos el div que se autogenera por un section con su id correspondiente:

```javascript
import React from 'react';

const Highlights = () => {
    return (
        <section id='highlights'>Highlights</section>
    );
};

export default Highlights;
```

Una vez tenemos esto, crearemos la estructura inicial de esta sección, la cual sera un titulo con un subtitulo y un bento (Los bentos son cajas de comida asiaticas que tienen su espacio ordenado/distribuido de una manera bastante elegante y popular) o masonry (Un estilo muy similar a los bentos que tambien es conocido por "estilo pinterest", ya que fue esta aplicación/web la que lo popularizo), que son estilos de cajas/tarjetas ordenados de manera elegante.

```javascript
<section id='highlights'>
    <h2>There's never been a better time to upgrade</h2>
    <h3>Here's what you get with the new Pro.</h3>

    <div className='masonry'></div>
</section>
```

A medida que vayamos haciendo las cajas veremos que no podemos verlas, esto se debe a que por defecto tienen opacidad de 0, algo que cambiaremos más adelante con animaciones pero de momento, vamos a hacer la estructura y el contenido

```javascript
<div className='left-column'>
    <div>
        <img src="/laptop.png" alt="Laptop" />
        <p>Fly through demanding task up to 9.8x faster.</p>
    </div>
    <div>
        <img src="/sun.png" alt="Sun" />
        <p>A stunning <br />
            Liquid Retina XDR <br />
            display.</p>
    </div>
</div>

<div className='right-column'>
    <div className='apple-gradient'>
        <img src="/ai.png" alt="AI" />
        <p>Built for <br />
            <span>Apple Intelligence.</span>
        </p>
    </div>
    <div>
        <img src="/battery.png" alt="Battery" />
        <p>Up to
            <span className='green-gradiente'>{'  '}14 more hours{'  '}</span>
            battery life.
            <span className='text-dark-100'>{'  '}(Up to 24 hours total.) </span><br /></p>
    </div>
</div>
```

Aunque tenemos la estructura y el contenido ya puesto, si vamos a la pagina seguiremos sin ver nada, ya que todavia no hemos cambiado lo de la opacidad 0 (puedes cambiarla un momento en el index.css para ver si ha quedado bien), pero lo que si podremos ver sera un espacio negro, que es donde esta el contenido.

Ahora crearemos una animación sencilla para hacerlos visibles de 1 en 1, al igual que en las animaciones anteriores, deberemos de hacerla empezar diferente si la resolución es de mobil o no, en este caso simplemente la haremos visible y le añadiremos un stagger, para que se hagan las animaciones de 1 en 1.

```javascript
const isMobile = useMediaQuery({ query: '(max-width: 1024px)' });

useGSAP(() => {
    gsap.to(['.left-column', '.right-column'], {
        scrollTrigger: {
            trigger: '#highlights',
            start: isMobile ? 'bottom bottom' : 'top top'
        },
        y: 0,
        opacity: 1,
        stagger: 0.5,
        duration: 1,
        ease: 'power1.inOut'
    });
}, []);
```

Con esto, ya estarian practicamente todos y cada uno de los componentes terminados y animados, a excepción del footer.

## Componente Footer

El footer, aunque es algo bastante sencillo, tambien debemos de hacerlo como componente, ya que si no, "manchariamos y ensuciariamos" el archivo App.jsx, por lo que al igual que el componente anterior, crearemos un componente especifico para ello, a diferencia de que este en vez de ser un section, sera un footer.

```javascript
import React from 'react'

const Footer = () => {
  return (
    <footer>Footer</footer>
  )
}

export default Footer
```

Una vez tenemos esto, simplemente crearemos una estructura sencilla del footer, que lo unico especial que tendra, sera que los links estaran mapeados por una constante

```javascript
<footer>
    <div className='info'>
        <p>More ways to shop: <span>Find and Apple Store</span> or <span>other reailer</span> near you. Or call 000800 040 1966.</p>
        <img src="/logo.svg" alt="apple logo" />
    </div>

    <hr />

    <div className='links'>
        <p>Copyright 2024 Apple Inc. All rights reserved. <br /> This page is not intended to replace, substitute, or deceive anyone; it is created solely for the purpose learning.</p>
        <ul>
            {footerLinks.map(({ label, link }) => (
                <li key={label}>
                    <a href={link}>{label}</a>
                </li>
            ))}
        </ul>
    </div>
</footer>
```

Con esto, la pagina ya estaria terminada.

## Animación extra

Como sugerencia de un amigo, he hecho un cambio en el componente Features.jsx, el cuál es un cambio de color en el modelo una vez termina su animación de girar, de esta manera da un efecto más "impresionante" y muestra que ambos modelos pueden hacerlo, para hacer esto, simplemente tuve que crear una función extra, la cual dependiendo del color cambia a uno u otro

```javascript
toggleColor: () => set((state) => ({
    color: state.color === '#2e2c2e' ? '#adb5bd' : '#2e2c2e'
})),
```

El state es basicamente el objeto de ZUSTAND completo, por lo que tiene todas las propiedades, includa la propiedad de color guardada ahi, asi que asignamos el color en función al color del state que ya tenemos y mediante la condición ternaria sabemos a cual cambiar



