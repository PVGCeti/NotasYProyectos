---
title: Como hacer un CLI con Node JS
tags:
  - nodejs
---
Lo primero de todo, es que si no sabes lo que es un CLI, puedes diriguirte al apartado de [[Conocimientos basicos]] y podras encontrar más información sobre ellos.

En este caso, haremos uno muy simple que es una copia de **ls**, es decir, crearemos un cli que liste los archivos de un directorio.

En NodeJS, para poder obtener los parametros de nuestro comando es tan sencillo como utilizar el objeto process y accerder al valor de argv, de la siguiente manera podriamos comprobarlo facilmente:

```javascript
console.log(process.argv)
```

Es importante tener en cuenta, que los dos primeros valores que estan en dicha variable, son el binario de node y el archivo del CLI, por lo que deberiamos de hacer empezar a obtener los valores desde el segundo, para poder obtener los valores/parametros que escribe el usuario.

```javascript
const args = process.argv.slice(2)
console.log(args)
```

Para crear este CLI utilizaremos varias funciones de modulos nativos de NodeJS, como lo son: **readdir** (para leer carpetas) y **stat** (nos da la información de un archivo). Ademas de esto, para utilizar correctamente las rutas, usaremos **join**.

Lo primero que debemos de hacer es recuperar la carpeta que vamos a listar

```javascript
import { readdir, stat } from "node:fs/promises"
import { join } from "node:path"

// Recuperamos la carpeta
const dir = process.argv[2] ?? "."
```

De esta manera, en caso de que el usuario no escriba ningún parametro adiccional, listaremos desde el directorio actual.

Despues leeremos los nombres de los archivos de la carpeta

```javascript
const files = await readdir(dir)
console.log(files)
```

De esta manera, ya tendriamos una versión "primitiva" de nuestro ls, ya que funciona correctamente pero es muy poco estetico y no muestro casi nada de información, para remediar el problema de la información utilizaremos stat.

```javascript
const entries = await Promise.all(
    files.map(async (name) => {
        const fullPath = join(dir, name);
        const info = await stat(fullPath);

        return {
            name,
            isDir: info.isDirectory(),
            size: info.size()
        };
    })
);
```

Con la información de cada archivo/directorio obtenida, nuestro siguiente paso sera renderizarlo de la manera que a nosotros nos parezca correcta:

```javascript
for (const entry of entries) {
    const icon = entry.isDir ? '📂' : '🗃️';
    const size = entry.isDir ? '-' : `${entry.size}`;
    console.log(`${icon}    ${entry.name} ${size}`);
}
```

Dando un archivo final:

```javascript
import { readdir, stat } from "node:fs/promises";
import { join } from "node:path";

const dir = process.argv[2] ?? ".";

const files = await readdir(dir);

const entries = await Promise.all(
    files.map(async (name) => {
        const fullPath = join(dir, name);
        const info = await stat(fullPath);

        return {
            name,
            isDir: info.isDirectory(),
            size: info.size
        };
    })
);

for (const entry of entries) {
    const icon = entry.isDir ? '📂' : '🗃️';
    const size = entry.isDir ? '-' : `${entry.size}`;
    console.log(`${icon}    ${entry.name} ${size}`);
}
```

Con el siguiente resultado:

![[NodeJSCLIResult.png]]

