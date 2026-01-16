---
draft: "true"
---

## Solución de problemas #errores

### Warning al importar modulos #errores

Es posible que a la hora de realizar una importación en tu proyecto te aparezca un warning, esto es debido a que para las importaciones NodeJS utiliza/utilizaba CommonJS por defecto, este es el sistema de importación antiguo de NodeJS, a dia de hoy existe uno mucho mejor creado por ECMAScript y es el que se suele utilizar. Para utilizar este simplemente deberemos de añadir al archivo **package.json** la linea de `"type": "module"`. Si no tienes el archivo, crealo y añade el siguiente contenido de la siguiente manera:

```
{
	"type": "module",
}
```

### Contenido ilegible con readFile #errores

Es posible que hayas utilizado la función de **readFile** del modulo nativo de **"node:fs/promises"** y tu resultado haya sido algo extraño como "<Buffer 48 6f 6c ...", esto es debido a que no le hemos dado ningún segundo parametro a la función y esta espera que le pasemos el tipo de codificación, ya que al haber diferentes lenguajes puede que algunos no esten disponibles en ciertos metodos y utiliza por defecto el buffer para evitar estos problemas, pero como no es legible para nosotros, deberemos de darle un sistema de codificación adecuado, el cual para el español es **utf-8**

```javascript
// Esto daria el "error" que hemos mencionado
const content = await readFile("./texto.txt");\

// Esto devuelve el texto en castellano/español
const content = await readFile("./texto.txt", "utf-8");
```

### Directorio no existe al crear carpetas con mkdir #errores 

Este error puede deberse principalmente a que estamos intentado crear multiples carpetas (o tenemos la ruta mal, dentro de una carpeta que no existe o similares) y no tenemos la recursividad activada, para ello simplemente deberemos de añadir un segundo parametro a la función que sera un objeto llamado **recursive**, con el valor de true.

```javascript
mkdir(outputDir, { recursive: true });
```

### Problemas con las rutas en Windows #errores 

Es posible que experimentes problemas al utilizar los codigos proporcionados o similares, ya que el sistema de archivos (path) de Windows, utiliza la barra invertida para las rutas y no la barra normal, una forma de solucionar este problema es utilizar WSL (Windows Subsystem for Linux), pero esto es algo que deberia de tener el usuario/cliente y en caso de querer que otra persona lo use en su dispositivo personal, es posible que no lo tenga o no lo quiera descargar, por lo que para ello hay otra solución.

Esta solución es utilizar la función **join** del modulo nativo de NodeJS **node:path**, por lo que cuando fueramos a crear una ruta, deberias de hacerlo utilizando join de la siguiente manera

```javascript
// Importamos el modulo
import { join } from "node:path"

// Esta es la ruta que estamos creando con join: "output/archivos/documentos"
const outputDir = join("output", "archivos", "documentos")
```