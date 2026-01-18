---
tit: Como hacer un servidor con Node JS
---
#nodejs

## Primeros pasos

Lo primero que debemos de saber a la hora de crear un servidor con NodeJS, es que existe un modulo nativo para hacer esto, el cual es **node:http**, con la función **createServer**, es importante que lo primero que hagamos a la hora de crear nuestro servidor sea asignarle un puerto.

```node
import { createServer } from "node:http"

const port = 3000

// Si quieres hacerlo de una manera más correcta deberias de pasarle una variable de entorno
```

>[!note]  Como usar un puerto desocupado #tip
>Puede que intentes desplegar un servidor en un puerto en especifico pero esté ya este ocupado, para evitar dicho problema, si le das el valor de 0, lo que hara es desplegarlo en el primer puerto libre que encuentre. Para saber que puerto es deberas mirar el server.address()

Es muy importante saber, que cuando creamos un servidor, este necesita tener 2 parametros, los cuales son **req, res**, que significan:

- req = Request (Petición)
- res = Resolve (Respuesta)

```node
const server = createServer((req, res) => { })
```

Una vez hemos creado el servidor, como mencione anteriormente es de extremada importancia asignarle el puerto, por lo que para ello utilizaremos la función **listen**, es importante tener en cuenta, que la función que pongamos dentro, se ejecutara siempre que iniciemos el servidor.

```node
server.listen(port, () => { })
```

Con este poco contenido ya tendriamos un servidor "funcional", si utilizamos el fichero con node y vamos a nuestro localhost al puerto 3000, podremos ver que no ocurre nada, pero se queda cargando, esto es debido a que no hemos creado ninguna respuesta, pero si le añadimos una respuesta al servidor, podremos verla desde nuestro navegador.

```node
import { createServer } from "node:http";

const port = 3000;

const server = createServer((req, res) => {
    res.end("Servidor funcionando");
});

server.listen(port, () => {
    console.log("Servidor desplegado en el puerto " + port);
});
```

>[!note] Actualizar servidor automaticamente #tip 
>En caso de que hagamos algún cambio al servidor, deberemos de apagarlo y encenderlo de nuevo para poder ver dichos cambios, en caso de que quisieramos ahorranos dicho paso, podemos encender el servidor con el parametro/flag **--watch**, haciendo que se actualice automaticamente.

## Añadir cabeceras

Es importante que hay información adicional que debera de saber nuestro servidor para poder transmitirsela al navegador, para ello utilizara las cabeceras, en caso de que queramos añadir una deberemos de utilizar el metodo **setHeader()**.

```node
res.setHeader()
```

En caso de que quisieramos utilizar una ñ o algun emoji, podremos ver como el servidor nos pone letras raras o numeros, esto es debido a que no sabe en que lenguaje/idioma codificar dicho valor ya que no es "universal", por lo que deberemos de añadirle una cabecera especificandole como codificar dicho contenido, para ello antes de enviar la respuesta, añadiremos:

```node
res.setHeader('Content-Type', 'text/plain; charset=utf-8');
```

## Discriminación de rutas

A la hora de crear nuestro servidor, probablemente querremos que cada ruta tenga algo diferente o incluso no querremos tener multiples rutas, para ello deberemos de discriminar las rutas y lo haremos con **if**, para poner las rutas que queramos:

```node
const server = createServer((req, res) => {
    res.setHeader('Content-Type', 'text/plain; charset=utf-8');

    if (req.url === "/") {
        return res.end("Este es el inicio jefe");
    }

    if (req.url === "/about") {
        return res.end("Información sobre nosotros");
    }

    res.statusCode = 404;
    return res.end("No existe la pagina");
});
```

Es importante que al final del todo, pongamos la pagina por defecto que sea algo similar a "no encontrada" y darle el codigo de estado 404, ya que es el codigo comunmente utilizado cuando no se ha podido encontrar algo.

>[!warning] Error al cargar las rutas despues de la primera
>Es posible que tengas un error y que la primera ruta funcione correctamente, pero cuando vayas a otra ocurra un error. Para saber como solucionarlo vaya al apartado de [[Error al cargar rutas despues de la primera]] 


### Ruta de HealthCheck

Una ruta de **HealthCheck**, es una ruta creada con un proposito simple de mostrar si el servidor/API esta funcionando de manera correcta y si quieres, de manera adiccional, el tiempo que lleva desplegado, de esta manera puedes detectar caidas inesperadas.

```node
if (req.url === '/health') {
    return res.end("ok " + process.uptime());
}
```

Es importante tener en cuenta que el uptime te lo devuelve en segundos.

## Gestión de metodos

Es importante que gestionemos los metodos que nos llegan de manera correcto, para ello deberemos de utilizar **method**, que lo obtenemos del parametro **req**.

De momento, en este ejemplo sencillo solamente permitiremos el metodo GET, aunque más adelante crearemos una API completa.

```node
    const { method, url } = req;

    if (method !== "GET") {
        res.statusCode = 405;
        return res.end("Metodo no permitido");
    }
```

