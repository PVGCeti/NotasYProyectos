---
title: Como crear un bot de discord con Node JS
tags:
  - nodejs
  - principiante
---
Para crear un bot de discord necesitaremos instalarnos `discord.js`, este el paquete oficial y esta sponsorizado por **Vercel** y **Workers**, esta es la pagina oficial actualmente:

```
https://discord.js.org/
```

Por lo que primeros inciaremos nuestro proyecto de node con:

```bash
npm init -y
```

y despues instalaremos la dependencia necesaria para hacer el bot de discord.

```bash
npm install discord.js
```

Ademas de esta dependencia deberemos de instalar `dotenv`, para que el bot pueda usar las variables de entorno.

```bash
npm install dotenv
```

## Obtención del token para el bot

Para obtener el token para nuestro bot, deberemos ir a la pagina de discord especifica para los desarrolladores, que actualmente es: https://discord.com/developers/applications

Una vez en la pagina, simplemente le daremos al boton de crear nueva aplicación y le pondremos un nombre, en mi caso sera `News Bot`, ya que tengo pensado que sea un bot que me vaya diciendo noticias que obtendra a traves de una API.

Con el bot ya creado, simplemente deberemos de ir al apartado de Bot y obtener el Token, dicho Token deberemos de ponerlo en nuestras variables de entorno.

Es importante que no mostreis ni deis vuestro token a nadie que no sea de confianza, ya que se pueden hacer multiples cosas maliciosas, si una persona con malas intenciones tiene tu token.

## Punto principal

Una vez tenemos las dependencias instalada, deberemos de crear una carpeta llamara `src`, donde crearemos un archivo llamado `bot.js`, que dicho archivo sera el archivo principal de nuestro bot.

Con la variable de entorno del token que hemos creado en el paso anterior, utilizaremos nuestra dependencia de `dotenv` para importarlas con el metodo `.config()`.

```javascript
require('dotenv').config();
```

Una vez tenemos esto listo, vamos a importar nuestra dependencia principal que es `discord.js`, lo primero que deberemos de hacer es logearlo, obteniendo el `Client` y crear una nueva clase a partir de dicho `Client`, para ello pondremos la siguiente linea de codigo:

```javascript
import { Client, GatewayIntentBits } from "discord.js";

const client = new Client();
```

Aunque con eso tecnicamente ya estaria, deberemos de enviarle el `GatewayIntentBits` como parametro al inicializar la clase, esto indicara los permisos de nuestro bot, si queremos saber que poner, podemos verificarlo en la siguiente pagina: [https://discord-intents-calculator.vercel.app/](https://discordjs.guide/legacy/popular-topics/intents)

En mi caso pondre lo siguiente, ya que es lo que necesito para que pueda escribir mensajes

```javascript
const client = new Client({
    intents: [
        GatewayIntentBits.Guilds,
        GatewayIntentBits.GuildMessages,
        GatewayIntentBits.MessageContent
    ]
});
```

Para loggearlo, sera tan facil como utilizar el metodo `.login()`, de nuestra clase y pasarle el token del bot mediante las variables de entorno.

```javascript
client.login(process.env.DISCORDJS_TOKEN);
```

Con esto ya podriamos ver que el bot esta como conectado en el servidor, aunque actualmente no tenemos el bot en el servidor para poder comprobarlo, por lo que ahora meteremos el bot en nuestro servidor.

## Añadiendo el bot a nuestro servidor

### Antes de empezar

Deberemos de tener permisos para añadir el bot al servidor, ya que si no, no nos saldran para poder añadirlo y puede que nos de error, por lo que una vez nos aseguramos que tenemos al menos un servidor con permisos para añadir bots, podemos proceder con este paso.

(Si no tienes ningún servidor, puedes simplemente crear un servidor de prueba y añadirlo a dicho servidor).

### Verificando el bot

Deberemos de visitar la misma pagina que visitamos para obtener el token, una vez ahi, iremos al apartado de `OAuth2` y copiaremos el `client id`, ya que necesitaremos dicho valor para verificar nuestro bot.

Una vez tenemos el `client id`, tendremos que ir a la siguiente URL (esta URL puede ser obtenida desde la pagina oficial aunque puede ser algo más completa, ya que en nuestro caso no lo necesitamos ). 

**DEBES DE CAMBIAR** el numero que hay en `client_id=`, por la id de tu bot.
```URL
https://discord.com/api/oauth2/authorize?client_id=123456789012345678&scope=bot
```

Una vez hecho esto, te saldra para añadirlo a los servidores que tengas permisos, cuando lo hagas podras comprobar el funcionamiento de dicho Bot, ya que al ejecutar el archivo, el bot aparecera como conectado en vez de desconectado.

## Creación de scripts para facilitar el desarrollo

Para facilitar el desarrollo del bot vamos a crear diferentes scripts en el archivo `package.json`, los scripts que crearemos sera el de `start`, este lo iniciara de manera correcta y el de `dev`, para poder ver los cambios de al instante y probar las cosas.

```javascript
"start": "node ./src/bot.js",
"dev": "nodemon ./src/bot.js"
```

>[!note] Nodemon
>Si no tienes Nodemon, puedes instarlarlo facilmente con el siguiente comando: (Recomiendo buscar el comando por si ha cambiado)
>
>```
>npm install -g nodemon
>```

De esta manera, en caso de que queramos ejecutar alguno de estos scripts, solamente necesitamos usar `npm run start` o `npm run dev`, dependiendo del que queramos hacer, para probar y desarrollarlo utilizaremos el `npm run dev`.

## Crear un prefijo al que responda el bot

A la hora de crear bots de discord es muy tipico que estos tengan un prefijo, como podrian ser: `/, !, ?, -` para que cuando lo escriban, respondan en función a ello, aunque ese no es mi objetivo final, voy a explicarlo, para hacer eso deberemos de usar el metodo `.on` y el evento de creación de mensaje `messageCreate` y pasarle dicho mensaje.

```javascript
client.on("messageCreate", (msg) => {})
```

Con esto como base para nuestro comando, deberemos revisar que el autor del mensaje no sea un bot, para evitar un bucle infinito de mensajes, esto lo haremos obteniendo el valor como si fuera un objeto y usando un condicional.

```javascript
client.on("messageCreate", (msg) => {
    if (msg.author.bot) return;
});
```

Despues de esto definiremos una variable de tipo `let`, ya que solamente queremos usarla dentro de esta función, que tendra el prefijo que vamos a usar, en este ejemplo usare el `?` y para facilitarnos el manejo de datos, crearemos otra variable que tenga el contenido del mensaje:

```javascript
client.on("messageCreate", (msg) => {
    if (msg.author.bot) return;

    let prefix = "?";
    let message = msg.content;
});
```

Ahora solamente queda comprobar si el prefijo esta en el mensaje, para comprobarlo solo tendremos que hacer un condicional y usar el metodo `startsWith()` y si es así que ejecute el comando en cuestión, en mi caso voy a hacer que responda a la persona que ha ejecutado el comando.

Para obtener el comando deberemos de quitar el prefijo, para ello, utilizaremos el metodo `slice()`, con la longitud del prefijo y recuperaremos el array con el comando. para mi ocasión no habra un comando, simplemente que no este vacio.

```javascript
client.on("messageCreate", (msg) => {
    if (msg.author.bot) return;

    let prefix = "?";
    let message = msg.content;

    if (message.startsWith(prefix)) {
        const command = message.slice(prefix.length).split(" ")[0];

        if (command != "") {
            msg.reply(msg.author.displayName + ", Espero que tengas un buen dia!");
        }
    }
});
```

En caso de que quisieramos crear multiples comandos especifico, simplemente deberiamos de cambiar el ultimo condicional.

![[botbuendia.png| 700]]
