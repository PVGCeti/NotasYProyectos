---
draft: "true"
---
Antes de empezar, vamos a añadir el turbopack file system catching que nos permite compilar las cosas mucho más rapido, para activarlo solamente tendremos que ir al archivo: `next.config.ts`
y añadir el siguiente codigo:

```typescript
experimental: {  
    turbopackFileSystemCacheForDev: true,  
}
```

(Puede que vuestro editor de texto no reconozca turbopack como una palabra, por lo que para evitar ese warning, debereis de añadirlo al diccionario).

El archivo de configuración debera de quedar tal que asi:

```typescript
import type { NextConfig } from "next";  
  
const nextConfig: NextConfig = {  
  experimental: {  
      turbopackFileSystemCacheForDev: true,  
  }  
};  
  
export default nextConfig;
```

---

Estructura de Next.js

Postcss.config.mjs: Es una herramienta para procesar el CSS con los plugins, 
Package-lock.json: Guarda las dependencias y versiones
Package.json: Todas las dependencias y scripts
next.env.d.ts: No hay que tocarlos
next.config.ts: Te permite configurar next.js
eslint.config.mjs: Te permite configurar eslint
gitignore: ignora lo que no quieres subir a tu repositorio

Una vez tenemos esto, simplemente iremos a App > page.tsx y lo cambiaremos por un ejemplo de prueba para ver si todo funciona correctamente con un componente de React:

```typescript
import React from 'react';

const Home = () => {
    return (
        <div className='text-5xl underline'>Welcome to Next.js!</div>
    );
};

export default Home;
```

Para entender más como funciona next.js, si hacemos un console.log() en esa pagina, veremos, que pone `server`, indicandonos que el componente se ha pre-renderizado en el servidor antes de mandarlo al cliente, haciendo que cargue más rapido.

Para convertirlo en un componente de cliente, deberemos de poner en la primera linea del componente `'use cliente'`.

Al igual que en los proyectos anteriores, deberemos de crear una carpeta llamada `components` y crear los componentes ahi dentro.


Instalaremos este plugin para que optimice el renderizado

```shell
npm install babel-plugin-react-compiler@latest
```

---

Como funciona el router de next.js??

Cuando creamos una carpeta en el directorio App, automaticamente esta se crea como una ruta de la pagina, por lo que si añadimos un archivo llamado `page.tsx` (Importante que este en minusculas, ya que si lo pones en mayusculas no lo reconocera), cuando vayamos a `/about`, tendremos el contenido de dicha pagina.


## Como hacer una Api con Next.js

Cualquier archivo que pongamos dentro de la carpeta api, se tratara como un endpoint de API, por lo que solamente sera un componente de servidor y no afectara de ninguna manera al cliente.

### Ejemplo en mi proyecto DevEvents

Lo primero que deberemos de hacer es, en la carpeta `app`, crear una subcarpeta la cual se llamara `api` y dentro de esa, crearemos una subcarpeta por cada modelo que necesitemos que tenga la API, en este caso vamos a crear una carpeta llamada `events` y dentro de ella, el archivo `route.ts` (.ts ya que estamos usando typescript, si estuvieramos usando javascript, seria .js)

#### Como hacer un POST
Lo primero que deberemos de hacer es crear la "cabecera" de la función, una función que tenemos que exportar y hacer asincrona, de la siguiente manera:

```typescript
export async function POST(req: NextRequest) { }
```

Es importante tener en cuenta, que el `NextRequest`, es importado de `"next/server"` y que es obligatorio tenerlo, ya que esto es la manera en la que Next.js recibe las peticiones.

Con la cabecera ya creada, empezaremos con la gestión de errores, para ello deberemos de usar un trycatch, en el cual vamos a añadir primero la gestión de errores, ya que esto va a ser probablemente comun en el 90% de las peticiones que vayamos a hacer, así que es importante tenerlo bien y saber como funciona.

Lo primero que haremos sera hacer un console.error() del error (importante saber, que al ser algo que sucede en el servidor, esto no lo podra ver un usuario normal, por lo que no debes preocuparte, pero tampoco puedes tener console.error/log por todos lados del servidor), el cual llamaremos e, ya que si lo llamamos error directamente puede causar confusiones o problemas de nomenclaturas ya que hay cosas llamadas error por defecto:

```typescript
try {
} catch (e) {
    console.error(e)
}
```

Una vez tenemos esa parte, hay que cerrar la petición, es decir, hay que mandar una respuesta, en este caso, diciendo que ha dado error, para ello, tendremos que utilizar `NextResponse`, que es la manera en la que Nextjs manda las respuestas de las API.

Es importante que el mensaje sea lo más claro posible, ya que si nos ocurre, nos gustaria saber todo lo posible para poder solucionarlo, por lo que mandaremos un mensaje del problema que ha habido, en este caso que no se pudo crear el evento (porque era un petición POST) y mandaremos el error, en caso de que no sea del tipo error, lo mandaremos como desconocido.

```typescript
try {

} catch (e) {

    console.error(e)
    return NextResponse.json({message: 'Event Creation Failed', error: e instanceof Error ? e.message: 'Unknown'}, {status: 500})

}
```

Es importante añadir al final el `, {status: 500}`, ya que necesitamos ponerle un codigo, para saber facilmente que es lo que ha sucedido.

Una vez con la gestión de errores, ya podemos empezar a crear el codigo para la request, lo primero que deberemos de hacer es conectarnos a la base de datos, en nuestro caso MongoDB, es importante tener en cuenta, que necesitamos si o si estar conectado, por lo que deberemos de poner un await:

```typescript
try {
    await connectDB()
}
```

Una vez tenemos la conexión, deberemos de recuperar los datos que queremos introducir en la base de datos, en otros casos usarias algo similar a `req.body`, pero en nextjs utilizaremos `req.formData()`, es importante esperar a estos datos, ya que sin ellos crearemos un elemento vacio y despues de eso, crearemos una variable donde guardaremos los datos y un trycatch:

```typescript
try {
    await connectDB()

    const formData = await req.formData();
    let event;

    try{

    }catch(e){

    }

}
```

Con esta estructura, podremos utilizar el trycatch para convertir los datos en un objeto y en caso de que de un error, mandar una respuesta adecuada, en este caso, con el codigo de status 400:

```typescript
try{
    event = Object.fromEntries(formData.entries())
}catch(e){
    return NextResponse.json({message: 'Invalidad JSON data format'}, {status: 400})

}
```

Despues de este trycatch, en caso de que no haya dado error, crearemos el elemento en la base de datos y mandaremos una respuesta adecuada:

```typescript
const createdEvent = await Event.create(event);

return NextResponse.json({ message: 'Event create successfully', event: createdEvent }, { status: 201 });
```

Es importante tener en cuenta, que el Event.create() viene de nuestro modelo de la base de datos, el cual deberemos de importar. Una vez tenemos todo esto, nuestra función de POST deberia quedar tal que así:

```typescript
export async function POST(req: NextRequest) {
    try {
        await connectDB();
        
        const formData = await req.formData();
        let event;
        
        try {
            event = Object.fromEntries(formData.entries());
        } catch (e) {
            return NextResponse.json({ message: 'Invalidad JSON data format' }, { status: 400 });
        }
        
        const createdEvent = await Event.create(event);
        
        return NextResponse.json({ message: 'Event create successfully', event: createdEvent }, { status: 201 });
        
    } catch (e) {
        console.error(e);
        return NextResponse.json({ message: 'Event Creation Failed', error: e instanceof Error ? e.message : 'Unknown' }, { status: 500 });
        
    }
}
```


Para más información, puedes visitar la [documentación oficial](https://nextjs.org/docs/pages/building-your-application/routing/api-routes).

