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

```node
import express from "express"
```

Al ser un servidor deberemos de asignarle un puerto, aunque podriamos crear la variable con el valor directamente como en la guia de [[Como hacer un servidor con NodeJS]], esta vez lo haremos de una forma más correcta, que es utilizando las variables de entorno, para ello deberemos de crear un archivo llamado `.env`, donde guardaremos las variables y las obtendremos mediante  `process.env.VARIABLE`, que en este caso sera el puerto. Como puede haber errores y que no encuentre el archivo (no es lo común pero es una buena practica por si hay un error) deberemos de poner `??` (nullish coalescing operator) y 3000, ya que es puerto por defecto de Express.

```node
const PORT = process.env.PORT ?? 3000
```

Una vez tenemos estos preparativos, crearemos nuestro servidor, para ello utilizaremos la función de **express**,  `express()`, ya que esto sera el core del servidor, lo guardaremos en una constante llamade `app`.

```node
const app = express()
```

Con esto ya tenemos nuestro servidor creado, pero ahora mismo esta vacio, por lo que no podemos comprobarlo, para ello crearemos una ruta `get` que nos devuelva un simple `Hello World`.

```node
app.get("/", (request, response) => {
    response.send("hello World!");
});
```

Aunque tenemos el servidor creado y una ruta para comprobar su funcionamiento, aún no esta desplegado, asi que para ellos haremos que escuche en el puerto que hemos asignado anteriormente y ejecutaremos el archivo `index.js` con node.

_Codigo completo hasta el momento_
```node
import express from "express";

const PORT = process.env.PORT ?? 3000;
const app = express();

app.get("/", (req, res) => {
    res.send("hello World!");
});

app.listen(PORT, () => {
    console.log(`Servidor desplegado en http://localhost:${PORT}`)
})
```

