---
title: Node JS
---
![[Node-js-Logo.png]]

**Node.js** es un entorno en tiempo de ejecución multiplataforma, de código abierto, para la capa del servidor (pero no limitándose a ello) basado en el lenguaje de programación [JavaScript](https://es.wikipedia.org/wiki/JavaScript "JavaScript"), asíncrono, con [E/S](https://es.wikipedia.org/wiki/I/O "I/O") de datos en una [arquitectura orientada a eventos](https://es.wikipedia.org/wiki/Programaci%C3%B3n_dirigida_por_eventos "Programación dirigida por eventos") y basado en el motor [V8](https://es.wikipedia.org/wiki/V8_\(motor_JavaScript\) "V8 (motor JavaScript)") de Google.

Antes de empezar, me gustaria dar una mención de honor a midudev, ya que mucha información de estos apuntes ha sido gracias a el.

## Como instalar NodeJS

Para instalar **NodeJS** tenemos 2 opciones oficiales, independientemente del **sistema operativo** que estemos usando (**Linux, Windows, MacOS**), las cuales son instalarlo mediante una **terminal/consola** utilizando **comandos** y la otra opción es descargar un **binario/ejecutable** e instalarlo mediante dicho archivo; aunque esta era la manera recomendada antiguamente, actualmente **NodeJS** recomienda la **línea de comandos**.

A la hora de instalar **NodeJS** hay que tener en cuenta la **versión** que queremos, ya que hay 2 versiones importantes (aunque puedes descargar una versión en concreto ya sea porque tienes un **proyecto antiguo** o el motivo que sea). Estas versiones son:

- **Current** -> **Última versión** estable.
- **LTS** -> **Long Term Support**, es la última versión que tiene un **mantenimiento prolongado**.

A la hora de hacer cualquier instalación, **hágalo** desde la **[página oficial de NodeJS](https://nodejs.org/es/download)** para evitar problemas; por ello, aunque se muestre una imagen, no se proporcionarán los **comandos**.

![[NodeJSInstalacion.png]]

## Como usar NodeJS

### Primeros pasos

Para empezar a utilizar NodeJS, crearemos un pequeño archivo que nos servira para probar su funcionalidad y uso. Este archivo lo llamaremos **index.js**, y pondremos un codigo simple, como podria ser un **console.log()**.

```javascript
console.log("Hello World")
```

Para comprobar su funcionalidad tendremos que usar el comando **node** y el nombre del archivo:

```javascript
node .\index.js
```

![[nodejsConsoleLog.png]]

Una vez hecho esto, hemos comprobado que funciona de manera correcta y nos muestra el resultado que esperabamos.

### Como importar archivos

Aunque ya hemos visto como podemos usar NodeJS con un archivo, es importante saber como utilizarlo cuando tenemos multiples archivos, ya que un proyecto real puede hacerse bastante largo y complejo, por lo que es importante saber separar y modular las cosas. Para ello utilizaremos multiples archivos y los importaremos.

Para el ejemplo utilizaremos un archivo adiccional que tiene una función muy simple que te dice que día es:

```javascript
export const day = new Date(Date.now()).toISOString().split("T")[0];
```

Es importante que el archivo este exportando alguna función, que es lo que vamos a utilizar, en caso de que no lo hicieramos función, se ejecutaria directamente y en caso de que no lo exportaramos, no podriamos usarlo.

Con el archivo externo ya creado, con una función exportada, es el momento de utilizar el archivo principal e importar este otro archivo, para ello, utilizaremos la siguiente linea de codigo (teniendo en cuenta que el archivo esta en la misma carpeta y se llama "day.js"):

```javascript
import { day } from "./day.js";
```

Hay que tener en cuenta que NodeJS se ejecuta en **Run time** y no en **build time**, por lo que necesitamos aclarar y especificar las rutas y extensiones de los archivo, cuyo ultimo punto puede ser opcional en el front-end ya que este suele utilizar empaquetadores y pueden tomarse tiempo para averiguar la extensión, cosa que con NodeJS no pasa.

(archivo final)
```javascript
import { day } from "./day.js";

console.log("Hoy es: " + day);
```

![[NodeJSModulo.png]]

>[!warning] Warning al importar modulos
>Es posible que te aparezca un warning a la hora de importar un modulo de esta manera, para saber como solucionarlo vaya al apartado de [[#Warning al importar modulos]]

## Como hacer un script en Nodejs

Lo primero de todo, es que si no sabes lo que es un script, puedes diriguirte al apartado de [[Conocimientos  basicos]] y podras encontrar más información sobre ellos.

Para crear estos scripts sencillos que mostrare acontinuación, estamos utilizando modulos nativos de NodeJS.

>[!note] Modulos Nativos de NodeJS #tip 
>Aunque no es necesario que pongamos **node:** delante de los modulos nativos, es recomendable, ya que si en el futuro un nombre coincide, nos aseguramos que utilice el modulo nativo.

Es importante saber que NodeJS tendra los permisos que su binario tenga, por lo que es muy probable que pueda hacer cosas maliciosas/peligrosas si ejecutas codigo que desconoces, para evitar este tipo de problema, es importante que utilices `--permission` como argumento en tu comando de NodeJS. Esto lo que hara es prohibir todo lo que pueda llegar a ser malicioso aunque este no lo sea y te dara las indicaciones para que vayas dandole los persmisos necesarios, permitiendote escoger exactamente a que archivos y a que permisos.

### Leer ficheros con NodeJS

En nuestro caso, crearemos un script sencillo que interactua con tu sistema operativo, para ello crearemos otro archivo, con un nombre adecuado. 

Para poder lograr que NodeJS interactue con nuestro sistema operativo utilizaremos el sistema de modulos nativos que trae para leer y escribir ficheros en nuestro sistema operativo, utilizando la siguiente linea de comandos:

```javascript
import { readFile } from "node:fs/promises"
```

De esta manera importamos el modulo nativo **"node:fs/promises"**, especificamente la función de leer archivos (readFile).

Una vez lo tenemos importado, solamente deberemos de utilizar la función correctamente, indicando todos sus parametros (archivo y codificación) y usando await, ya que no puede leer el archivo instantaneamente, en caso de que fuese un archivo como **rockyou.txt**, notariamos como tarda en leerlo aunque en nuestro caso parezca instantaneo.

```javascript
import { readFile } from "node:fs/promises";

const content = await readFile("./texto.txt", "utf-8");

console.log(content);
```

>[!warning] Contenido ilegible
>Es posible que te aparezca el buffer en vez del contenido a la hora de utilizar la función, para saber como solucionarlo vaya al apartado  de [[#Contenido ilegible con readFile]]

### Escribir ficheros con NodeJS

Al igual que en el apartado anterior, podemos escribir ficheros si utilizamos la función correcta que trae por defecto el modulo nativo de NodeJS, función que es **writeFile**

```javascript
import { writeFile } from "node:fs/promises"
```

Como la función de readFile, esta tiene dos parametros, el primero sigue siendo el archivo/ruta, pero el segundo, en vez de ser la codificación, es el contenido que queremos añadirle a dicho archivo. 

```javascript
import { writeFile } from "node:fs/promises";

const content = await writeFile("./texto.txt", "Que tal estamos?");

console.log(content);
```


>[!note] Escribir en un archivo que no existe #tip
>Si ponemos que escriba en una ruta/archivo que no existe, NodeJS nos creara dicho archivo, por lo que es importante saber en que ruta/archivo estamos escribiendo para no generar contenido inecesario.

### Crear carpetas con NodeJS

Aunque estas cosas pueden parecer simples o meras curiosidades, es importante saber hacerlas, ya que si las juntas tienen un gran potencial, como por ejemplo, crear un programa que analice facturas (lea los archivos) y cree documentos con información importante (escriba un archivo) y los organice en carpetas para que sea más facil de ordenar y legible para el ser humano.

Por lo que, si quisieramos hacer dicho programa, solo nos faltaria saber como crear carpetas y para ello, al igual que los anteriores utilizaremos  el modulo nativo de NodeJS, pero esta vez importaremos  la función **mkdir**.

```javascript
import { mkdir } from "node:fs/promises";
```

Mediante el uso de esta función podremos crear carpetas, tanto carpetas padres como carpetas hijas o recursivas, (es decir, carpetas dentro de carpetas), para ello solamente deberemos de añadir un segundo parametro que sera la recursividad.

```javascript
import { mkdir } from "node:fs/promises";

const outputDir = "output/archivos/documentos";

mkdir(outputDir, { recursive: true });
```

>[!warning] Directorio no existe
>Es posible que te aparezca un error de que el directorio o fichero no existe, para saber como solucionarlo vaya al apartado [[#Directorio no existe al crear carpetas con mkdir errores]]

#### Leer nombre y extensiones de archivos #tip

Es posible que a la hora de crear carpetas y archivos, querramos asegurarnos de que archivo se ha creado y con que extensión, para comprobar que se ha hecho correcto o simplemente para debuggear, para ello podemos utilizar dos funciones extras que vienen del modulo nativo de NodeJS de Path, **node:path**, estas funciones son **basename** y **extname**.

```javascript
import { basename, extname } from "node:path"
```

Un ejemplo sencillo podria ser el siguiente:

```javascript
import { basename, extname } from "node:path";

const dir = "output/archivos/documentos/hola.txt";

console.log("nombre archivo " + basename(dir));
console.log("Extension archivo " + extname(dir));
```

### Instalar dependencias en NodeJS

Es importante saber que cada dependencia/modulo puede tener una forma u otra, algunas pueden estar soportadas por más metodos y otros tener menos, por lo que es importante ir a la dependencia/modulo y asegurarnos de cual es su manera de instalarla. Por norma general solamente tendremos que usar el siguiente comando en la carpeta raiz del proyecto:

(siendo dependencia el nombre de la dependencia/modulo)
```bash
npm install dependencia
```

Una vez hecho esto, veremos que se habran creado diferentes archivos:

- node_modules (carpeta) -> Guarda la dependencia/modulo y todo su contenido
- package.json -> Añade la dependencia y su versión
- package-lock.json -> Añade información especifica de la dependencia que puede ser util

>[!note] Importar dependencia
>Es importante saber, que al igual que los modulos nativos, deberemos de importar dicha dependencia/modulo con la función que queramos para poder utilizarlo.


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