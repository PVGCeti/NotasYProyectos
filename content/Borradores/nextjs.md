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