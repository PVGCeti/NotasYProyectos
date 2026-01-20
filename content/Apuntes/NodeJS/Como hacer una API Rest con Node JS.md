---
title: Como hacer una API Rest con Node JS
---
#nodejs 

Lo primero de todo, es que si no sabes lo que es una API Rest o Express, puedes diriguirte al apartado de [[Conocimientos basicos]] y podras encontrar más información sobre ellos.

## Antes de empezar

Para utilizar express primero deberemos de crear un proyecto de node con lo minimo necesario, esto lo haremos utilizando el comando:

```bash
npm init -y
```

Esto lo que hara sera crear un archivo llamado `package.json`, donde tendra información general de nuestro proyecto, tendremos que modificar dos cosas de dicho archivo

- Autor -> Por nuestro nombre/empresa
- Type -> module, para poder importar las cosas de la manera actual

## Como crear una API Rest con Express

### Como instalar Express

Con el proyecto de Node JS ya creado, instalaremos express para utilizarlo, esto es tan facil de hacer como escribir el siguiente comando:

```bash
npm install express
```

Esto nos creara una carpeta llamado `node_modules`, donde estaran todas y cada una de las dependencias necesaria para utilizar express y el propio express, ademas creara un archivo llamado `package-lock.json` que tendra información especifica sobre las dependencias y el proyecto.

### Como crear un servidor con Express

Como el servidor va a ser la raiz del proyecto, lo **tenemos** que crear en el `index.js`, aunque tendra muchas similitudes a [[Como hacer un servidor con NodeJS]] tambien tendra diferencias, la principal sera que deberemos de importar express con la siguiente linea de codigo:

```javascript
import express from "express"
```

Al ser un servidor deberemos de asignarle un puerto, aunque podriamos crear la variable con el valor directamente como en la guia de [[Como hacer un servidor con NodeJS]], esta vez lo haremos de una forma más correcta, que es utilizando las variables de entorno, para ello deberemos de crear un archivo llamado `.env`, donde guardaremos las variables y las obtendremos mediante  `process.env.VARIABLE`, que en este caso sera el puerto. Como puede haber errores y que no encuentre el archivo (no es lo común pero es una buena practica por si hay un error) deberemos de poner `??` (nullish coalescing operator) y 3000, ya que es puerto por defecto de Express.

```javascript
const PORT = process.env.PORT ?? 3000
```

Una vez tenemos estos preparativos, crearemos nuestro servidor, para ello utilizaremos la función de **express**,  `express()`, ya que esto sera el core del servidor, lo guardaremos en una constante llamade `app`.

```javascript
const app = express()
```

Con esto ya tenemos nuestro servidor creado, pero ahora mismo esta vacio, por lo que no podemos comprobarlo, para ello crearemos una ruta `get` que nos devuelva un simple `Hello World`.

```javascript
app.get("/", (request, response) => {
    return response.send("hello World!");
});
```

Aunque tenemos el servidor creado y una ruta para comprobar su funcionamiento, aún no esta desplegado, asi que para ellos haremos que escuche en el puerto que hemos asignado anteriormente y ejecutaremos el archivo `index.js` con node.

_Codigo completo hasta el momento_
```javascript
import express from "express";

const PORT = process.env.PORT ?? 3000;
const app = express();

app.get("/", (req, res) => {
    return res.send("hello World!");
});

app.listen(PORT, () => {
    console.log(`Servidor desplegado en http://localhost:${PORT}`)
})
```

![[ExpressPrueba.png]]

>[!note] Respuesta de la API
>Es importante saber que aunque hemos puesto "hello World!", podriamos ponerle codigo HTML y este funcionaria, es decir si pones un **h1**, este se mostraria como un **h1**  normal y corriente.
>
>Gracias al metodo `send`, el content type se ajusta automaticamente, haciendo que si mandamos un json o texto plano, se mande de la manera correcta.


### Como crear el health check

Lo primero de todo, es que si no sabes lo que es un healt check, puedes diriguirte al apartado de [[Conocimientos basicos]] y podras encontrar más información sobre ellos.

Lo más importante que debemos saber, es que es una ruta de `get`, ya que solamente queremos obtener información, ni modificarla, ni borrarla, ni añadirla, por lo que utilizaremos el metodo get y le pondremos como ruta `/health` y haremos que las respuesta sea un **json**, con el estado de ok y el tiempo que lleva el servidor desplegado (esto es util para comprobar si ha tenido caidas o si se ha reiniciado).

```javascript
app.get("/health", (req, res) => {
    return res.json({
        status: "ok",
        uptime:  process.uptime()
    })
})
```

![[expressHealth.png]]

### Creación de middlewares

Lo primero de todo, es que si no sabes lo que es un middleware, puedes diriguirte al apartado de [[Conocimientos basicos]] y podras encontrar más información sobre ellos.

Es importante saber que para crear un middleware en **express**, tenemos que utilizar el metodo **use**, esto hara que antes de acceder a una ruta **SIEMPRE** se ejecute dicho codigo. Para utilizar el metodo use, deberemos de pasarle tres parametros:

- Request -> La petición al servidor
- Response -> La respuesta del servidor
- next -> Para pasar a la ruta 

```javascript
app.use((req, res, next) => {
    next()
})
```

>[!note] Importancia del next
>Es importante que al final de nuestro middleware siempre pongamos el next(), ya que si no esta dicha función, el servidor nunca respondera de manera correcta al intentar acceder a una ruta, esta se quedara colgada sin saber que hacer.

Una utilidad muy simple que podemos hacer para observar el funcionamiento correcto y el flujo de nuestro servidor, es añadirle un poco de codigo a dicho **middleware** para que nos muestre la hora a la que se intento acceder a una ruta (mostrando tambien a que ruta estaban intentado acceder).

```javascript
app.use((req, res, next) => {
    const timeString = new Date().toLocaleTimeString();
    console.log(`[${timeString}] ${req.method} ${req.url}`);
    next();
});
```

![[expressMiddlewarePrueba.png]]

#### Middleware especifico

En caso de que tuvieramos una ruta especifica que quisieras que tenga un middleware adicional para mayor seguridad u otros motivos, simplemente deberemos de crearlo aparte como una función flecha normal y corriente.

```javascript
const middlewareEspecifico = (req, res, next) => {
    console.log("Middleware especifico de /health")
    next()
}
```

y deberemos de añadirselo a la ruta en especifico que queremos que lo tenga entre el nombre de la ruta y la función flecha

```javascript
app.get("/health", middlewareEspecifico, (req, res) => {
    return res.json({
        status: "ok",
        uptime: process.uptime()
    });
});
```

De esta manera, este middleware solo se ejecutara cuando accedamos a la ruta `/health`, mientras que el otro middleware se ejecutara siempre, aunque vayamos a `/health` o `/` o cualquier ruta que tenga nuestro servidor.

![[middlewareHealth.png]]

>[!note] Funcionamiento de los middlewares
>Es importante saber que si has creado un middleware con el metodo `use` y tienes un  middleware especifico para una ruta, cuando accedas a esa ruta se van a ejecutar los dos middlerware, no solamente el de la ruta especifica.

### Creación de las rutas principales

Por normal general una API Rest, tendra diferentes rutas principales, las cuales son:

- Get -> Obtener los resultados
- Post -> Subir resultado
- Put -> Actualizar resultado completamente
- Patch -> Actualizar resultado parcialmente
- Delete -> Borrar resultado

>[!warning] Problema de CORS
>Es posible que a la hora de utilizar tu API desde otra pagina/proyecto, obtengas un error de cors. Para saber como solucionarlo vaya al apartado de [[Error de CORS al utilizar mi API en Express]]

#### Ruta Get

Por lo que primero vamos a crear la ruta `GET`, para comprobar que nuestra API muestra los datos, ya que si no sera bastante complicado comprobar el funcionamiento de los otros metodos. Para hacer esto solamente deberemos de hacer lo mismo que con `/` o `/health` pero añadiendo el contenido que queremos mostras, en mi caso, hare una API Rest de comida como ejemplo.

```javascript
app.get("/get-foods", (req, res) => {
    return res.json({
        comps: [
            { id: 1, title: 'Pizza' },
            { id: 2, title: 'Burger' },
            { id: 3, title: 'Noodles' },
        ]
    });
});
```

Aunque esto funciona, en caso de que quisieramos tener un unico resultado, ya sea porque hay muchos datos y se nos hace dificil encontrar el que queremos cuando mostramos todos o por algún motivo en especifico, simplemente deberemos de poner en la ruta `/:id` y obtenerlo desde  el parametro `req` con `.params`.

```javascript
app.get("/get-food/:id", (req, res) => {
    const { id } = req.params;
    
    return res.json({
        food: { id, title: `Food with id ${id}` }
    });
});
```

Es importante saber que cuando recuperas el parametro, lo estas recuperando como una cadena de texto, por lo que si es un numero (como en este caso), deberas de convertilo a numero con la función `Number()` o lo que corresponda según tu caso. Esto es necesario ya que si no, no podremos aplicar las validaciones de manera correcta.

```javascript
app.get("/get-food/:id", (req, res) => {
    const { id } = req.params;
    const idNumber = Number(id);

    return res.json({
        food: { id: idNumber, title: `Food with id ${id}` }
    });
});
```

>[!note] Rutas especiales
>Es util saber que hay formas de crear rutas especiales, una que contiene una parte opcional, otra que contiene contenido extra e incluso con expresiones regulares, estas rutas se hacen de la siguiente manera:
>
>**Ruta con parte opcional:** `/obli{gato}rio` esta ruta funcionaria aunque las letras entre {} no estuvieran.
>
>**Ruta con parte extra:** `/file/*filename` esta ruta funcionaria escribas lo que escribas despues de /file/

![[expresGetFood.png]]

#### Mejorando nuestra gestión de datos

Este metodo funciona correctamente pero actualmente los datos de nuestra API Rest estan escritos en el propio codigo, por lo que vamos a cambiarlo para hacerlo mejor, aunque podriamos usar una base de datos realista de MySQL o MongoDB esto haria más complicado nuestro punto principal, por lo que obtendremos los datos de un archivo externo, el cual llamaremos en este ejemplo `datos.json` (ya que es un archivo .json).

Es importante saber que en este contexto tenemos dos metodos correctos para hacerlo, el primero seria utilizando el `readFile` de los modulos nativos de Node JS:

```javascript
const foods = JSON.parse(readFileSync("./datos.json", "utf-8"))
```

Aunque esto es correcto y es un metodo que podrias utilizar, necesitas importar el `readFileSync`, que no es negativo pero puedes hacerlo sin necesidad de importar ninguna dependencia y es importando el json directamente, para ello simplemente deberemos de hacer el import

```javascript
import foods from "./datos.json" with { type: "json"}
```

>[!warning] Error al importar el JSON en Express
>Es posible que te suceda un error de ATTRIBUTE_MISSING, Para saber como solucionarlo vaya al apartado de [[Error al importar el JSON en Express]]

En este caso podriamos importar el archivo al inicio y no habria ningún tipo de problema ya que el archivo tiene poco contenido y no supone una carga para el servidor, pero esto es un problema ya que estariamos importando el archivo cada vez que alguien hace una petición sea donde sea, por lo que deberiamos de cambiarlo a que haga la petición unica y exclusivamente cuando sea necesario.

Si quisieramos realizar esto, solamente deberiamos de convertir la ruta que requiere del contenido y volverla `async`, ya que tendra que esperar a que cargue el contenido y simplemente importalo dentro de dicha función.

```javascript
app.get("/get-foods", async (req, res) => {
    const { default: foods } = await import("./datos.json", { with: { type: "json" } });
    return res.json(foods);
});
```

>[!note] Obtener correctamente los datos
>Si obtenemos los datos de esta manera, antes de nuestros datos habra una etiqueta llamada `default`, es importante tener esto en cuenta ya que puede alterarnos nuestro json, por lo que en vez de obtener los datos directamente, los obtendremos desde default.

#### Implementando filtros a nuestra API

Para crear filtros en nuestra API es importante saber diferenciar los `query` de los `params`, la diferencia es que los params, son los datos que hemos puesto en las rutas despues de los dos puntos (:), como hicimos en el caso del id, mientras que los query es todo aquel contenido que esta en el pathname despues de ?, es decir: `/health?hola=true`, esto seria un query, especificamente con la clave hola y el valor true.

Una vez sabemos que son los `query` y los `params`, para crear los filtros utilizaremos los `query`, en nuestro caso para no complicarnos con logicas complejas de filtros, vamos a crear un filtro sencillo para saber si es comida rapida o no y para paginarlos en caso de que hubieran muchos resultados.

Lo primero que deberemos de hacer es un condicional para comprobar si dicha `query` existe, una vez tenemos los datos que necesitamos, utilizando la función de filter de JS, haremos el filtro y devolveremos lo que nos de como resultado, es importante hacer el filtro incase sensitive para mejorarle la experiencia al usuario, por lo que pasaremos todos los datos a **lowerCase**.

```javascript
app.get("/get-foods", (req, res) => {
    const { type, limit, offset } = req.query;
    let filteredFoods = foods;

    if (type) {
        const searchTerm = type.toLowerCase();
        filteredFoods = filteredFoods.filter(food => food.type.toLowerCase().includes(searchTerm));
    }

    return res.json(filteredFoods);
});
```

![[expresFilter.png]]

>[!note] Filtrar elementos #tip 
>A la hora de filtrar los elementos, es importante que la variable inicial sean todos los elementos, por si la persona que esta realizando la busquedad no pone ningun filtro, por lo que nunca añadas los elementos a lo que vas a mostrar, sino muestra todo y elimina los que no quieres que vean, de esta manera te aseguras que tu pagina muestre contenido.

#### Implementando paginación a nuestra API

Siempre que vayamos a mostrar contenido es importante hacerlo con una paginación, de esta manera evitamos saturar al usuario y genera una sensación de orden que beneficia a la experiencia del usuario, debido a esto es importante tener valores por defecto para la paginación, podriamos ponerlo directamente en nuestra función de `get` de la siguiente manera:

```javascript
const { type, limit = 10, offset = 0 } = req.query;
```

Pero esto no es lo más optimo, ya que puede ser que utilicemos estos datos en otra función y puede ser que no nos acordemos o sean datos que ha puesto otro desarrollador, por lo que para evitar el tener que ir buscando de un lado a otro en el codigo, deberemos de crear un archivo, tipicamente con el nombre de `config.js` o `const.js`, donde crearemos una constante que tenga los valores por defecto:

```javascript
export const DEFAULTS = {
    LIMIT_PAGINATION: 10,
    LIMIT_OFFSET: 0
}
```

Por lo que a la hora de poner nuestros valores por defecto, se hara de la siguiente manera (importante tener en cuenta que debemos de importar el archivo):

```javascript
const { type, limit = DEFAULTS.LIMIT_PAGINATION, offset = DEFAULTS.LIMIT_OFFSET } = req.query;
```

Con los valores por defectos establecidos, solo nos faltaria crear el pagina, el paginado se puede hacer de una manera sencilla utilizando la función `slice()`, por lo que simplemente haciendo uso de esta función en el resultado de los filtros junto a los numeros de la paginación obtendremos los datos paginado de manera sencilla. (Importante tener en cuenta que debemos de convertirlos en numeros ya que el `query` viene en string por defecto)

```javascript
    const limitNumber = Number(limit)
    const offsetNumber = Number(offset)

    const paginatedFoods = filteredFoods.slice(offsetNumber, offsetNumber + limitNumber)

    return res.json(paginatedFoods);
```

### Convirtiendo nuestra API en una API Rest

Es importante saber que no todas las API son API Rest, ya que API Rest es un tipo de arquitectura a la hora de hacer APIs, es importante saber que la principal caracteristica es que las API Rest son predecibles y representativas, ademas de que su función dependa del metodo HTTP y no de la ruta.

Es decir, en vez de llamar a las rutas `get-foods`, deberiamos de llamarla simplemente `foods` y que el resultado que nos de el servidor dependa de el metodo que usemos a dicha ruta, de esta manera simplificamos mucho las rutas. Por lo que si quisieramos eliminar, actualizar, añadir o ver `foods`, lo haremos desde `/foods` (en caso de que necesite la id del elemento usara `/foods/:id`)

_Codigo completo hasta el momento_
```javascript
import express from "express";
import foods from "./datos.json" with { type: "json"};
import { DEFAULTS } from "./config.js";

const PORT = process.env.PORT ?? 3000;
const app = express();

app.use((req, res, next) => {
    const timeString = new Date().toLocaleTimeString();
    console.log(`[${timeString}] ${req.method} ${req.url}`);
    next();
});

app.get("/", (req, res) => {
    return res.send("hello World!");
});

app.get("/health", (req, res) => {
    return res.json({
        status: "ok",
        uptime: process.uptime()
    });
});

app.get("/foods", (req, res) => {
    const { type, limit = DEFAULTS.LIMIT_PAGINATION, offset = DEFAULTS.LIMIT_OFFSET } = req.query;
    let filteredFoods = foods;

    if (type) {
        const searchTerm = type.toLowerCase();
        filteredFoods = filteredFoods.filter(food => food.type.toLowerCase().includes(searchTerm));
    }

    const limitNumber = Number(limit)
    const offsetNumber = Number(offset)

    const paginatedFoods = filteredFoods.slice(offsetNumber, offsetNumber + limitNumber)

    return res.json(paginatedFoods);
});

app.get("/foods/:id", (req, res) => {
    const { id } = req.params;
    const idNumber = Number(id);

    return res.json({
        food: { id: idNumber, title: `Food with id ${id}` }
    });
});

app.listen(PORT, () => {
    console.log(`Servidor desplegado en http://localhost:${PORT}`);
});
```

### Creación del CRUD

Lo primero de todo, es que si no sabes lo que es un CRUD, puedes diriguirte al apartado de [[Conocimientos basicos]] y podras encontrar más información sobre ellos.

#### Terminando el apartado de Read

Empezaremos terminando las rutas get de la API, es decir la R del CRUD, aunque ya estan creadas, una de ellas no funciona de la manera correcta, ya que solamente nos muestra la id que hemos puesto, pero no el valor real, ya que lo hicimos para mostrar como podriamos obtener parametros para utilizarlos.

Lo unico que deberemos de hacer es obtener nuestros datos, que en mi caso la variable que los tiene se llama **foods**,  y filtrar para obtener la id que esta en la url

```javascript
const food = foods.find(food => food.id === id);
```

Una vez tenemos esto, solamente deberemos de devolver el valor correspondiente y asegurarnos de devolver un mensaje de error en caso de que no se haya encontrado dicha comida y el codigo de estado 404, para ello simplemente utilizaremos un condicional y returns.

```javascript
    if (!food) {
        return res.status(404).json({ error: "Food not found" });
    }

    return res.send(food);
```

Dandonos el siguiente codigo para la ruta de ver un elemento especifico en base a su id

```javascript
app.get("/foods/:id", (req, res) => {
    const { id } = req.params;

    const food = foods.find(food => food.id === id);

    if (!food) {
        return res.status(404).json({ error: "Food not found" });
    }

    return res.send(food);
});
```

>[!warning] No funciona el filtro por id
>Es posible que puedas tener un error y al realizar la busqueda por id te aparezca el mensaje de error. Para saber como solucionarlo vaya al apartado de [[No funciona el filtro por id]]

![[expressGetId.png]]

#### Creando el Create

Para crear un CRUD es importante saber los verbos HTTP, en caso de que no lo sepas puedes diriguirte al apartado de [[Conocimientos basicos]] y podras encontrar más información sobre ellos.

Para el `Create` utilizaremos el metodo `.post()`, como vamos a crear una entrada/elemento en nuestra API deberemos de obtener la información que necesitamos, en nuestro caso es solamente el nombre de la comida y el tipo. En este caso no obtendremos los valores a traves del `query`  o el `params`, lo obtendremos mediante el `body/cuerpo` de la petición.

Para obtener los datos del cuerpo de la petición, solamente tendremos que usar nuestro parametro `req`, ya que este tiene todo los datos de la petición.

```javascript
const { title, type } = req.body;
```

Es importante saber que para obtener la petición de manera correcta y que funcione como queramos, necesitamos que la petición venga con el `content/type` de json, por lo que para asegurarnos de que utilizar los datos de manera correcta como tipo `JSON`, utilizaremos un middleware que trae express, para ello simplemente utilizaremos el `app.use()` y el middleware que queremos, en este caso `express.json()`.

```javascript
app.use(express.json());
```

Una vez hemos obtenido los datos del `body/cuerpo` de la petición, junto a esos datos crearemos el nuevo `elemento`, por norma general lo hariamos con una base de datos, pero en este caso como no queremos complicarlo, lo haremos desde en memoria, para ello, simplemente le hacemos una estructura json, ya que nuestros datos estan en un archivo `.json`.

La id de nuestro elemento debe de ser unica, podriamos utilizar metodos para crear ids simples de manera sencilla, pero es mejor utilizar la función `crypto.randomUUID()` que trae el propio express, ya que esta pensada para eso y crea IDs complejos y seguros.

```javascript
const newFood = {
    id: crypto.randomUUID(),
    title,
    type
};
```

Una vez tenemos el cuerpo del nuevo elemento que vamos ha añadir, solamente deberemos de hacer un `.push()` del elemento (en nuestro caso) y devolver que hemos creado con existo la entrada/elemento, con el codigo de estado 201.

```javascript
foods.push(newFood); // Esto seria el codigo de nuestra base de datos

return res.status(201).json(newFood);
```

De esta manera, si queremos comprobar que se crean los elementos, podriamos hacerlo mediante una app para manejos de API, como podria ser [[HTTPIE]] o hacerlo manualmente desde la terminal con un `curl`.

![[expressPostFood.png]]

Como podemos ver, nos devuelve el elemento/entrada que hemos creado, lo cual es necesario por convención y ademas nos ayuda a verificar que datos tiene, para asegurarnos de que lo hemos creado, simplemente deberemos hacerla una petición a la API que nos muestre todos los `elementos/entradas` o este en especifico.

![[expressGetBurrito.png]]

>[!note] Validación de datos
>Es importante saber que deberiamos de verificar/sanitizar los datos para que sean consistente, ya que en este ejemplo, podemos ver que el ultimo que hemos añadido no empieza por mayusculas, esto no deberia succeder en un proyecto real.

