---
title: Como crear un bot de discord con Node JS
tags:
  - nodejs
  - principiante
---
Para crear un bot de Discord necesitaremos instalarnos `discord.js`; este es el paquete oficial y está esponsorizado por **Vercel** y **Workers**. Esta es la página oficial actualmente:

```
https://discord.js.org/
```

Por lo que primero iniciaremos nuestro proyecto de Node con:

```bash
npm init -y
```

Y después instalaremos la dependencia necesaria para hacer el bot de Discord.

```bash
npm install discord.js
```

Además de esta dependencia, deberemos instalar `dotenv` para que el bot pueda usar las variables de entorno.

```bash
npm install dotenv
```

## Obtención del **token** para el bot

Para obtener el **token** para nuestro bot, deberemos ir a la **página** de `Discord` específica para los **desarrolladores**, que actualmente es: [https://discord.com/developers/applications](https://discord.com/developers/applications)

Una vez en la **página**, simplemente le daremos al **botón** de crear **nueva aplicación** y le pondremos un nombre; en mi caso será `News Bot`, ya que tengo pensado que sea un bot que me vaya diciendo **noticias** que _obtendrá_ a través de una `API`.

Con el bot ya creado, simplemente deberemos ir al apartado de `Bot` y obtener el **token**; dicho **token** deberemos ponerlo en nuestras **variables de entorno**.

Es importante que no **mostréis** ni deis vuestro **token** a nadie que no sea de confianza, ya que se pueden hacer **múltiples** cosas _maliciosas_ si una persona con malas intenciones tiene tu **token**.

## ### **Punto principal**

Una vez tenemos las **dependencias instaladas**, deberemos instalar `src`, donde crearemos un archivo llamado `bot.js`, que dicho archivo **será** el **archivo principal** de nuestro bot.

Con la **variable de entorno** del `token` que hemos creado en el paso anterior, utilizaremos nuestra dependencia de `dotenv` para **importarlas** con el método `.config()`.

```javascript
require('dotenv').config();
```

Una vez tenemos esto listo, vamos a importar nuestra dependencia principal que es `discord.js`. Lo primero que deberemos de hacer es **loguearlo**, obteniendo el `Client` y crear una **nueva clase** a partir de dicho `Client`; para ello pondremos la siguiente línea de código:

```javascript
import { Client, GatewayIntentBits } from "discord.js";

const client = new Client();
```

Aunque con eso técnicamente ya **estaría**, deberemos enviarle el `GatewayIntentBits` como **parámetro** al inicializar la clase; esto indicará los **permisos** de nuestro bot. Si queremos saber qué poner, podemos verificarlo en la siguiente página: [https://discord-intents-calculator.vercel.app/](https://discord-intents-calculator.vercel.app/)

En mi caso pondré lo siguiente, ya que es lo que necesito para que pueda **escribir mensajes**:

```javascript
const client = new Client({
    intents: [
        GatewayIntentBits.Guilds,
        GatewayIntentBits.GuildMessages,
        GatewayIntentBits.MessageContent
    ]
});
```

Para **loguearlo**, será tan fácil como utilizar el método `.login()` de nuestra clase y pasarle el `token` del bot mediante las **variables de entorno**.

```javascript
client.login(process.env.DISCORDJS_TOKEN);
```

Con esto ya **podríamos** ver que el bot está como **conectado** en el servidor, aunque actualmente no tenemos el bot en el servidor para poder comprobarlo, por lo que ahora _meteremos_ el bot en nuestro **servidor**.

## Añadiendo el bot a nuestro servidor

### ### **Antes de empezar**

Deberemos tener **permisos** para añadir el bot al servidor, ya que si no, no nos saldrán para poder añadirlo y puede que nos dé **error**; por lo que, una vez nos aseguramos de que tenemos al menos un servidor con **permisos** para añadir bots, podemos proceder con este paso.

(_Si no tienes ningún servidor, puedes simplemente crear un servidor de prueba y añadirlo a dicho servidor_).

### ### **Verificando el bot**

Deberemos visitar la misma **página** que visitamos para obtener el **token**; una vez ahí, iremos al apartado de `OAuth2` y copiaremos el `client id`, ya que necesitaremos dicho valor para **verificar** nuestro bot.

Una vez tenemos el `client id`, tendremos que ir a la siguiente **URL** (esta **URL** puede ser obtenida desde la **página oficial**, aunque puede ser algo más compleja, ya que en nuestro caso no lo necesitamos).

**DEBES CAMBIAR** el número que hay en `client_id=` por la **ID** de tu bot.
```URL
https://discord.com/api/oauth2/authorize?client_id=123456789012345678&scope=bot
```

Una vez hecho esto, te **saldrá** para añadirlo a los servidores en los que tengas **permisos**; cuando lo hagas, podrás comprobar el **funcionamiento** de dicho `Bot`, ya que al ejecutar el archivo, el bot aparecerá como **conectado** en vez de **desconectado**.

## ### **Creación de scripts para facilitar el desarrollo**

Para facilitar el **desarrollo** del bot, vamos a crear diferentes `scripts` en el archivo `package.json`; los `scripts` que crearemos **serán** el de `start` (este lo iniciará de manera correcta) y el de `dev`, para poder ver los **cambios al instante** y probar las cosas.

```javascript
"start": "node ./src/bot.js",
"dev": "nodemon ./src/bot.js"
```

>[!note] Nodemon
>Si no tienes `Nodemon`, puedes **instalarlo** fácilmente con el siguiente comando: (_recomiendo buscar el comando por si ha cambiado_)
>
>```
>npm install -g nodemon
>```

De esta manera, en caso de que queramos ejecutar alguno de estos `scripts`, solamente necesitamos usar `npm run start` o `npm run dev`, dependiendo del que queramos hacer; para **probar** y **desarrollarlo** utilizaremos el `npm run dev`.

## ### **Crear un prefijo al que responda el bot**

A la hora de crear bots de `Discord`, es muy típico que estos tengan un **prefijo**, como podrían ser: `/, !, ?, -` para que, cuando lo escriban, respondan en función a ello. Aunque ese no es mi **objetivo final**, voy a explicarlo: para hacer eso deberemos usar el método `.on` y el **evento de creación de mensaje** `messageCreate` y pasarle dicho mensaje.

```javascript
client.on("messageCreate", (msg) => {})
```

Con esto como base para nuestro comando, deberemos revisar que el **autor del mensaje** no sea un `bot` para evitar un **bucle infinito de mensajes**; esto lo haremos obteniendo el valor como si fuera un **objeto** y usando un **condicional**.

```javascript
client.on("messageCreate", (msg) => {
    if (msg.author.bot) return;
});
```

Después de esto, definiremos una variable de tipo `let` (ya que solamente queremos usarla dentro de esta **función**) que tendrá el **prefijo** que vamos a usar; en este ejemplo, usaré el `?` y, para facilitarnos el **manejo de datos**, crearemos otra variable que tenga el **contenido del mensaje**:

```javascript
client.on("messageCreate", (msg) => {
    if (msg.author.bot) return;

    let prefix = "?";
    let message = msg.content;
});
```

Ahora solamente queda comprobar si el **prefijo** está en el mensaje; para comprobarlo solo tendremos que hacer un **condicional** y usar el método `startsWith()` y, si es así, que ejecute el **comando** en cuestión. En mi caso, voy a hacer que **responda** a la persona que ha ejecutado el comando.

Para obtener el **comando** deberemos quitar el prefijo; para ello, utilizaremos el método `slice()` con la **longitud del prefijo** y recuperaremos el _array_ con el comando. Para mi ocasión no habrá un comando, simplemente que no esté **vacío**.

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

En caso de que **quisiéramos** crear **múltiples comandos específicos**, simplemente **deberíamos** de cambiar el último **condicional**.

![[botbuendia.png| 700]]

## Mandar mensajes cada día a la misma hora

### Tareas cron

En mi caso, mi objetivo final es que todos los **días** por la mañana me mande **noticias** sobre informática (1, 2 o 3); de esta manera, puedo mantenerme informado fácilmente aunque vaya a realizar actividades de ocio en `Discord`.

Para hacer esto necesitaremos una **dependencia extra**, que será `node-cron`. Para quien no sepa qué son las **cron/tareas**, son _scripts_/archivos/programas que se ejecutan siempre a la **hora** que se les indique mediante un sistema de **5 números**, siendo:

- **Minuto**
- **Hora**
- **Día del mes**
- **Mes**
- **Día de la semana**

Como quiero que mande un mensaje todos los días a las **9 en punto**, tendré que poner el siguiente "código": `0 9 * * *`, ya que el `*` significa que es en **todos**.

>[!note] Instalar node-cron
>Para instalar `node-cron`, te **sugiero** que busques la **página web oficial** y lo obtengas de ahí; actualmente puedes instalarlo con este comando, pero ten en cuenta que en el futuro puede **cambiar**:
>
>```
>npm install node-cron
>```


### Haciendo que mande un mensaje

Antes de nada, como el bot va a mandar el mensaje de manera **proactiva**, deberemos indicarle en qué **canal** del servidor tiene que hacerlo; para esto, obtendremos el **ID del canal**.

Necesitamos poner Discord en **modo desarrollador**: esto se hace desde los ajustes, en el apartado de **Avanzado**. Una vez activado, simplemente haremos clic derecho en el canal que queramos y seleccionaremos **"Copiar ID de canal"**.

Aunque podríamos poner el ID del canal directamente, prefiero hacerlo mediante una **variable de entorno**, por lo que crearemos una con un nombre similar a `CHANNEL_ID` y es lo que utilizaremos cuando la necesitemos.

Ahora tendremos que hacer, mediante el método `.once()` de `client`, que cuando el cliente esté activo obtenga el canal y mande el mensaje; esto podemos hacerlo de la siguiente manera:

```javascript
client.once("clientReady", () => {

    const channel = client.channels.cache.get(process.env.CHANNEL_ID);
    channel.send("Estoy encendido");

});
```

![[botestoyencendido.png| 700]]

Aunque esto está técnicamente correcto y es funcional, deberemos añadirle un **condicional** antes de mandar el mensaje por si no se ha **recibido correctamente** el canal o este ya **no existe**.

```javascript
client.once("clientReady", () => {

    const channel = client.channels.cache.get(process.env.CHANNEL_ID);

    if (channel){
        channel.send("Estoy encendido");
    }

});
```

Una vez sabemos que esto funciona correctamente, es hora de probar **Cron** y hacer que funcione correctamente. Aunque haré que mande mensajes desde una **API**, actualmente dejaremos ese mensaje estándar e iremos probando.

Para usar **Cron** y que mande los mensajes cuando queremos, deberemos **importar** `cron` para poder usarlo:

```javascript
const cron = require('node-cron');
```

Ahora podemos envolver todo lo que está dentro de nuestro `client.once()` con **cron** y usar el método `schedule` y poner el "código" que hemos mencionado anteriormente (`0 9 * * *`), pero como no es a la hora en la que me encuentro, lo cambiaré por una **hora cercana** para poder probarlo de manera **efectiva**, sin tener que esperar al día siguiente.

```javascript
cron.schedule('27 13 * * *', async () => {
    const channel = client.channels.cache.get(process.env.CHANNEL_ID);
    
    if (channel) {
        await channel.send("Estoy encendido");
    }
});
```

![[Estoyencendido1327bot.png| 700]]

Al comprobar que funciona correctamente, solamente tendremos que cambiar el "código" para que sea a las **9:00** y listo, ya mandaría el mensaje de **manera automática** a la **hora correcta**.

## Usar una API para recibir noticias

Hay múltiples **APIs** de noticias disponibles; en mi caso, voy a probar la de `News API`: [https://newsapi.org/](https://newsapi.org/)

Una vez te has registrado y obtenido tu `API Key` (de esta o cualquier otra **API** de noticias), deberemos añadirlo a las **variables de entorno**, con un nombre similar a `API_KEY` o `NEWS_API_KEY`.

Crearemos un archivo aparte llamado `utils.js`; este tendrá **funciones de utilidad**, aunque en este caso solamente tendrá dos: nuestra **petición a la API** y un **formateador de fechas** para la petición.

### Función para obtener la fecha

Debido a cómo funciona esta **API**, es necesario que le pongamos un **rango de fecha**, ya que te permite buscar hacia atrás, por lo que tenemos que crear una función que te dé la **fecha actual** (menos 2 días, ya que la versión gratis no te permite obtenerlas más recientes) en este formato `año-mes-día`.

Para esto simplemente deberemos de hacer un poco de manejo de `Date()`:

```javascript
function dateformater() {
    const date = new Date();
    date.setDate(date.getDate() - 2);
    const year = date.getFullYear();
    const month = String(date.getMonth() + 1).padStart(2, '0');
    const day = String(date.getDate()).padStart(2, '0');

    const formatedDate = `${year}-${month}-${day}`;
    return formatedDate;
}
```

### Fetch a la API

A diferencia de la otra función, esta, al ser una **petición**, deberemos volverla una **función asíncrona**, ya que nos devolverá una **promesa**, además de que esta recibirá por parámetro el **tema de la noticia**.

```javascript
async function fetchData(theme){}
```

Lo primero que tenemos que hacer es **recibir los datos** que necesitamos, es decir, la **fecha** y montar la **URL** con los datos que tenemos y nuestra `API Key`:

```javascript
async function fetchData(theme) {
    const date = dateformater();
    const url = `https://newsapi.org/v2/top-headlines?category=${theme}&from=${date}&sortBy=popularity&apiKey=${process.env.NEWS_API_KEY}`;
    
}
```

Con la **URL** ya formada correctamente, solamente necesitamos crear el `fetch`; es importante hacerlo con un `try-catch`, ya que no queremos que el bot tenga un **error** y deje de funcionar.

En nuestro caso, en el `catch` no lanzaremos ningún error, ya que no queremos manejarlo, pero en el `try` sí, ya que puede ser que la **solicitud** se haga pero no la devuelva correctamente, por lo que no deberemos de mostrarla.

```javascript
async function fetchData(theme) {
    const date = dateformater();
    const url = `https://newsapi.org/v2/top-headlines?category=${theme}&from=${date}&sortBy=popularity&apiKey=${process.env.NEWS_API_KEY}`;

    try {
        const response = await fetch(url);

        if (!response.ok) {
            throw new Error("No se pudo conseguir el recurso");
        }

        const data = await response.json();
        return data;
    } catch (error) {
        console.error(error);
    }
}
```

Con la función ya creada, solamente tendremos que **exportar** la función mediante:

```javascript
module.exports = { fetchData };
```

### Enviando los datos

En nuestro archivo principal del bot, **importaremos** el archivo de `utils.js` para poder utilizar la **función** que hace `fetch` a la **API**:

```javascript
const { fetchData } = require('./utils.js');
```

Con esto y usando lo que teníamos ya hecho, simplemente deberemos de **obtener los datos** que queremos, que en mi caso es el **título** y la **url**, además de solamente querer el de la **noticia más reciente**:

```javascript
client.once("clientReady", () => {

    cron.schedule('0 9 * * *', async () => {
        const channel = client.channels.cache.get(process.env.CHANNEL_ID);
        const data = await fetchData("technology");

        if (channel) {
            await channel.send("Noticia: " + data.articles[0].title + "\nEnlace: " + data.articles[0].url);
        }
    });

});
```

![[NoticiaBot.png]]

Con esto ya tendríamos un **bot** que manda **noticias** de manera **automática**, todos los días a las **9:00**.

**Repositorio de GitHub:** https://github.com/PezEjecutivo/Discord-Bot

