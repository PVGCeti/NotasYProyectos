---
title: Task Tracker CLI
tags:
  - nodejs
  - proyecto
---
## Información del proyecto

### Requisitos

La aplicación debe funcionar en la linea de comando, permitiendo que el usuario introduzca argumento y guardando las tareas en un archivo JSON. El usuario debe poder:

- Añadir, Actualizar y Borrar tareas
- Marcar una tarea como en progreso o hecha
- Mostrar todas las tareas
- Mostrar todas las tareas hechas
- Mostrar todas las tareas no hechas
- Mostrar todas las tareas en progreso

### Propiedades de las tareas

Cada tarea debe de tener las siguientes propiedades:

- id: Identificador unico
- description: Descripción de la tarea
- status: El estado de la tarea (todo, in-progress, done)
- createdAt: La fecha en la que se creo la tarea
- updateAt: La fecha en la que se hizo la ultima actualización

## Ejemplos de uso

```
# Adding a new task
task-cli add "Buy groceries"
# Output: Task added successfully (ID: 1)

# Updating and deleting tasks
task-cli update 1 "Buy groceries and cook dinner"
task-cli delete 1

# Marking a task as in progress or done
task-cli mark-in-progress 1
task-cli mark-done 1

# Listing all tasks
task-cli list

# Listing tasks by status
task-cli list done
task-cli list todo
task-cli list in-progress
```

## Preparación del proyecto

Lo primero que deberemos hacer es inicializar un proyecto de **Node.js**; para esto, solamente tenemos que ejecutar el comando:

```bash
npm init -y
```

Esto nos creará un `package.json`, al cual deberemos añadirle el **autor** y cambiar el `type` a `module` en vez de `commonJS`. Gracias a esto, podremos utilizar el **sistema de importaciones moderno**.

Con el proyecto ya iniciado, vamos a importar las **dependencias nativas** de **Node.js** que utilizaremos para este proyecto. Al necesitar escribir y leer un archivo de tipo **JSON**, necesitaremos importar `readFile` y `writeFile`:

```javascript
import { writeFile, readFile } from "node:fs/promises";
```

Aunque no es lo primero que vamos a usar, es importante tenerlo desde el principio para mantener una **estructura clara** de las herramientas disponibles y no ir improvisando después. Al tener ya definidos los requisitos, es más fácil mantener un orden, aunque esto no significa que, si en el futuro surge una necesidad, no podamos añadir otra _dependencia_ o similar.

## Obtención de los argumentos

Como ya vimos en [[Cómo hacer un CLI con Node.js]], para obtener los **argumentos** simplemente deberemos acceder a `process.argv` y eliminar los dos primeros elementos. Podremos hacer esto mediante un `.slice(2)` de manera **sencilla y rápida**.

Tomaremos como referencia los _inputs_ que nos proponen:
```bash
task-cli add "Buy groceries"
task-cli update 1 "Buy groceries and cook dinner"
task-cli delete 1
task-cli mark-in-progress 1
task-cli mark-done 1
```

Podemos ver que aquí, que sigue un patron y que solo se puede hacer una acción a la vez, por lo que podemos hacerlo mediante posición, por lo que nos facilitara mucho el desarrollo de dicha CLI

![[taskcli arg clog.png]]

De esta manera, podemos ver facilmente como estamos obteniendo los datos en args, por lo que solamente tendremos que hacer un `switch` con el primer argumento `args[0]` y comprobar que `args[0]` exista.

```javascript
if (args) {
    switch (args[0]) {

    }
}
```

Con esto solamente deberemos de "complementar" los casos del switch y añadir uno por defecto en caso de que no ponga ningún comando correcto, ademas de manera adicional a los requisitos, añadire un `-h`, ya que es algo muy común en los cli.

```javascript
if (args) {
    switch (args[0]) {
        case "add":
            break;
        case "update":
            break;
        case "delete":
            break;
        case "mark-in-progress":
            break;
        case "mark-done":
            break;
        case "list":
            break;
        case "-h":
            break;
        default:
            console.log("No has introducido ningún comando valido");
    }
}
```

De esta manera, como se puede apreciar, aunque se contemplan todos los casos de manera **efectiva**, resulta _inadecuado_ ya que será complicado de leer una vez que tengan contenido. Por ello, podríamos hacerlo de otra manera **más fácil de visualizar**, aunque pueda resultar algo más complejo de entender si no estás acostumbrado a códigos similares.

Lo que haremos será crear un **objeto** que tendrá todos los comandos con sus respectivas funciones, actuando de forma similar a un **array de clave-valor**. Así, accederemos mediante su `clave` y obtendremos el `valor`, que será la **función**:

```javascript
if (args) {

    const commands = {
        add: () => { },
        update: () => { },
        delete: () => { },
        "mark-in-progress": () => { },
        "mark-done": () => { },
        list: () => { },
        "-h": () => { }
    };

    const command = commands[args[0]];
}
```

Una vez con este sistema creado e implementado, toca **ponerlo a prueba**. Por ello, aunque aún no vamos a desarrollar la **lógica**, haremos que cada comando ejecute un `console.log()` para verificarlo.

Mediante este pequeño cambio en el código (añadir el `console.log` y _llamar a la función_), comprobamos que todos se **ejecutan correctamente** cuando lo deseamos:

```javascript
    const commands = {
        add: () => { return console.log("Comando add"); },
        update: () => { return console.log("Comando update"); },
        delete: () => { return console.log("Comando delete"); },
        "mark-in-progress": () => { return console.log("Comando mark-in-progress"); },
        "mark-done": () => { return console.log("Comando mark-done"); },
        list: () => { return console.log("Comando list"); },
        "-h": () => { return console.log("Comando -h"); }
    };

    const command = commands[args[0]];

    command();
```

Con esto hecho es momento de ponerse en marcha con las funcionalidades, ya que la estructura basica la tenemos.

## Creando la funcionalidad/logica

Aunque podríamos hacerlo en el mismo archivo, para una **mayor claridad** voy a separar la estructura básica del **CLI** de las funciones y la **lógica**. Por ello, vamos a crear un archivo llamado `functions.js`, ya que almacenará las diferentes funciones.
### Función de añadir

Para crear esta función vamos a necesitar la función `writeFile`, que es **nativa de Node.js**. Con esto ya podríamos escribir un archivo; el hecho de tener que escribir un _archivo por nota_ nos facilita el manejo, ya que no hay que ir **sobrescribiendo** el mismo **JSON**.

Lo primero que haremos después de importar `writeFile` en este archivo es crear la función que reciba un **parámetro**, que será la `tarea` (tal como nos muestra el _input_ de ejemplo).

```javascript
import { writeFile, readFile } from "node:fs/promises";

function createTask(name) {

}
```

Con esto, deberemos de crear un `mock` del json, el cual vamos a utilizar para luego añadirlo al archivo en cuestión.

```javascript
function createTask(name) {

    const task = [{
        id: 0,
        description: "",
        status: "",
        createdAt: "",
        updatedAt: ""
    }];

}
```

Una vez tenemos esto, solamente deberemos cambiar el contenido al que necesitamos, pero aquí es donde llegamos al primer **punto de inflexión** importante. Al no especificarlo el proyecto, debemos intuir cómo lo requiere el cliente; en este caso, se podría usar algún _método_ para generar **IDs aleatorias y seguras**.

Sin embargo, viendo el ejemplo de uso de `update` y `delete`, parece que utiliza **IDs simples** de números bajos. Para mantener esa simplicidad, crearé un sistema para comprobar el número de archivos que hay en la carpeta `task` y añadir dicho número como **ID**.

Para hacer esto, deberemos importar la función `readdir`, que también es **nativa de Node.js**. Con ella, podremos ver el contenido dentro de una carpeta de manera sencilla y simplemente obtendremos la longitud utilizando `.length`.

```javascript
async function getNewId() {
    const tasks = await readdir("./Tasks");
    return tasks.length;
}
```

Con esta **función de utilidad** podemos continuar creando nuestra función de añadir; ahora podemos obtener el `id` de manera sencilla y correcta. La **descripción** la obtendremos del parámetro de la función, ya que esta la añade el usuario.

El **estado** será por defecto `todo`, ya que el usuario aún no la ha puesto en progreso. El `createdAt` será la **fecha actual**, mientras que el `updatedAt` será un _string_ avisando de que no ha sido modificado aún:

```javascript
function createTask(name) {

    const task = [{
        id: await getNewId(),
        description: name,
        status: "Todo",
        createdAt: new Date(),
        updatedAt: "Esta tarea no ha sido actualizada."
    }];

}
```

Aunque podriamos modificar el `new Date()`, para que se viera de mejor manera, al no ser un requerimiento principal, centrare mi atención en hacer las cosas de manera funcional y cuando esten todas las funcionalidades, empezare a cuidar detalles extras a lo que el cliente ha pedido.

Una vez tenemos el JSON que queremos escribir, simplemente utilizando la función `writeFile`, podremos crear el archivo en la carpeta de `tasks`.

```javascript
async function createTask(name) {

    const id = await getNewId();

    const task = [{
        id: id,
        description: name,
        status: "Todo",
        createdAt: new Date(),
        updatedAt: "Esta tarea no ha sido actualizada."
    }];

    await writeFile(`./Tasks/${id}`, task);

}
```

Para asegurarnos que funciona de manera correcta, he convertido la función en una función asincrona y he puesto un await a la hora de escribir el archivo, para asegurarnos de cuando avisemos al usuario, la tarea este creada correctamente, ademas de crear una variable para la id, ya que la vamos a utilizar en multiples sitios.

```javascript
async function createTask(name) {
    const id = await getNewId();

    const task = [{
        id: id,
        description: name,
        status: "Todo",
        createdAt: new Date(),
        updatedAt: "Esta tarea no ha sido actualizada."
    }];

    await writeFile(`./Tasks/${id}`, task);

    console.log(`Task added successfully (ID: ${id})`);
}
```

Ahora probaremos la funcionalidad de dicha función importandola y añadiendola a nuestro objeto/array de clave-valor, por lo que simplemente crearemos un condicional, pero en este caso, para mantener que este en una sola linea y facil de ver, lo haremos con una operación ternaria en vez de un if.

```javascript
add: () => { args[1] ? createTask(args[1]) : console.log("No has proporcionado descripción de la tarea"); },
```

Al probarlo, podremos ver que no se pueden pasar objetos JSON directamente a la función de `writeFile`, por lo que deberemos de convertirlo a string, para ello solamente tendremos que usar `JSON.stringify()`

```javascript
await writeFile(`./Tasks/${id}`, JSON.stringify(task));
```

De esta manera, si lo ejecutamos, podremos ver que funciona de manera correcta y se crea dicha tarea con el id correcto, si creamos multiples la id se incrementara y no se sobrepondran, por lo que esto esta hecho de manera correcta, ademas, gracias al operador ternario si solo ponen el comando `add` pero no añaden nada más, saldra un mensaje en vez de ejecutarse la función

![[tracker add.png]]

Como podemos apreciar, de esta manera funciona correctamente; pero si implementásemos la función de **borrar** y eliminásemos un archivo que no fuese el **último ID**, no podríamos crear ningún archivo nuevo ya que el nombre _estaría duplicado_. Por tanto, debemos buscar una manera de crear un `ID` **persistente** que no vaya en función de la cantidad de archivos actuales.

Podríamos crear una variable que guarde el valor en **memoria**, pero esto solo funcionaría durante la ejecución actual. Al no ser un servidor que esté **24/7**, los datos en memoria se resetearían en cada uso. Para solucionar esto y mantener el `id` simple, crearé un archivo que solamente contenga el **número del ID** y lo iremos actualizando progresivamente.

Con el archivo creado, deberemos modificar la función que hemos hecho previamente para obtener el `id`. Esta vez, deberá obtener el **contenido del archivo**, guardarlo para entregárselo a la función principal y **modificarlo** por uno mayor.

```javascript
async function getNewId() {
    const id = await readFile("./idtracker.txt", "utf-8");
    const newId = Number(id) + 1;
    await writeFile(`./idtracker.txt`, newId.toString());

    return newId;
}
```

De esta manera, aunque tengamos los archivos con id `1, 2 y 3`, si elimanmos el id 2, se creara un archivo con id `4`, en vez de `3`, por lo que si que se creara de verdad. 

Si utilizaremos una función para crear IDs verdaderamente robustos y seguros, no haria falta esta función, pero como queremos que sean ids simples para que el usuario pueda manejarlos sin problemas, tenemos esta solución practica y sencilla, que no entorpece mucho la vista al leer codigo.

### Función de actualizar

Como se puede ver en el ejemplo especifico de actualizar:

```bash
task-cli update 1 "Buy groceries and cook dinner"
```

Este recibira un `id` y la `descripción` nueva, por lo que esta vez nuestra función debe recibir dos parametros, ya que necesitamos ambos para poder actualizarlo, debido a como son los inputs de ejemplo, tomaremos que tienen que ser obligatoriamente en orden, en caso de que no quisieramos que fuera en orden podriamos hacer algo como `-id=1`, `-desc="comprar chuches"`, pero no es lo que nos ha pedido el cliente.

```javascript
export async function updateTask(id, desc){
    
}
```

Lo primero que debemos de hacer para el update sera utilizar el `readFile` y obtener la información del archivo en cuestión, al tener el archivo el mismo nombre que el ID, es facil de encontrar y no supone un problema

```javascript
export async function updateTask(id, desc) {
    const file = await readFile(`./Tasks/${id}.json`, "utf-8");
}
```

Una vez tenemos hemos guardado el resultado de la función en una variable, es importante convertirla a JSON para poder manejar la información de manera correcta y facil, para esto solamente tendremos que usar `JSON.parse()`.

```javascript
export async function updateTask(id, desc) {
    const file = await readFile(`./Tasks/${id}.json`, "utf-8");

    const jsonFile = JSON.parse(file);
    console.log(jsonFile);
}
```

![[updateTaskJson.png]]

Como podemos comprobar en la captura, lo recibimos de manera correcta y sin errores, por lo que ahora solamente deberemos de modificar los campos `description` y el `updateAt`, ya que esta función no modifica el estado.

Al estar parseado como `JSON`, es muy facíl acceder a los datos y modificarlo con lo correspondiente

```javascript
export async function updateTask(id, desc) {
    const file = await readFile(`./Tasks/${id}.json`, "utf-8");

    const jsonFile = JSON.parse(file);

    jsonFile[0].description = desc;
    jsonFile[0].updatedAt = new Date();

    console.log(jsonFile);
}
```

![[updateTaskFileJson.png]]

Ahora solamente deberemos de sobreescribir el archivo con los datos actualizado y funcionaria correctamente, despues de un breve cambio en la función principal

```javascript
export async function updateTask(id, desc) {
    const file = await readFile(`./Tasks/${id}.json`, "utf-8");
    const jsonFile = JSON.parse(file);

    jsonFile[0].description = desc;
    jsonFile[0].updatedAt = new Date();

    await writeFile(`./Tasks/${id}.json`, JSON.stringify(jsonFile));

    console.log(`Task updated successfully (ID: ${id})`);
}
```

Al igual que hicimos para la función de añadir, tendremos que crear una operación ternaria que muestre información en caso de que el usuario introduzca algo incorrecto:

```javascript
update: () => { args[1] && args[2] ? updateTask(args[1], args[2]) : console.log("Debes proporcionar el ID de la tarea y la tarea, en ese orden"); },
```

Una vez tenemos todo esto, podemos comprobar su funcionalidad y ver que funciona de manera correcta en todos los casos que se han probado.


### Haciendo un unico archivo

Después de **deliberar** y ver que a la larga, aunque se borrasen las tareas, podrían crearse múltiples archivos y resultar poco limpio o estructurado, he decidido que tendremos un **único archivo JSON**. En vez de crear un archivo `.json` por cada tarea, utilizaremos uno solo; debido a esto, tendremos que hacer algunas modificaciones a las funciones ya creadas.

La función del `id` puede mantenerse, ya que como hemos visto es **bastante útil** y, si usásemos `.length`, podríamos tener problemas de _IDs repetidos_, cuando estos deben ser **únicos**.

La función de **crear** es la primera que deberemos modificar, ya que actualmente está creando un archivo entero en vez de añadirlo a uno ya existente (o crearlo en caso de que no exista). Por lo tanto, deberemos comprobar si el archivo existe; para ello, utilizaremos una **función nativa** de **Node.js**:

```javascript
import { existsSync } from "node:fs";
```

Con la función nativa de Node JS importada, crearemos una variable que sera un array vacio y haremos un condicional, en caso de que el archivo ya exista, lo leeremos y guardaremos nuestro json en dicha variable y añadiremos la tarea y en caso de que no exista, simplemente añadiremos nuestra tarea al array vacio que hemos creado anteriormente

```javascript
export async function createTask(desc) {
    const id = await getNewId();
    let file = [];

    const task = {
        id: id,
        description: desc,
        status: "Todo",
        createdAt: new Date(),
        updatedAt: "Esta tarea no ha sido actualizada."
    };

    if (existsSync("./Tasks.json")) {
        file = await readFile("./Tasks.json", "utf-8");

        const jsonFile = JSON.parse(file);
        jsonFile.push(task);

        await writeFile(`./Tasks.json`, JSON.stringify(jsonFile));

    } else {
        file.push(task);
        await writeFile(`./Tasks.json`, JSON.stringify(file));
    }

    console.log(`Task added successfully (ID: ${id})`);
}
```

De esta manera, funcionara aunque no haya tareas y tambien funcionara si las hay, por lo que solamente nos faltaria la función de actualizar para terminar de complementar los cambios que hemos realizado.

En esta función solamente deberemos de encontrar en index de la tarea que queremos y modificar los datos como hacemos de normal, para encontrar el index, al ser un array, podemos hacerlo facilmente con `findIndex`

```javascript
export async function updateTask(id, desc) {
    const file = await readFile(`./Tasks.json`, "utf-8");
    const jsonFile = JSON.parse(file);
    const index = jsonFile.findIndex(task => task.id == id);

    jsonFile[index].description = desc;
    jsonFile[index].updatedAt = new Date();

    await writeFile(`./Tasks.json`, JSON.stringify(jsonFile));

    console.log(`Task updated successfully (ID: ${id})`);
}
```

Con esto ya esta tanto la función de actualizar como de añadir modificada para que funcione con un unico archivo, en vez de crear un archivo por cada tarea.

### Función de eliminar

Esta función es bastante simple y similar de hacer a la de actualizar, esta solo tendra un unico parametro que sera el id, buscaremos el id mediante un `find`, en vez de un `findIndex`, ya que queremos obtener el objeto completo y no solo su posición y mediante `slice()` lo eliminaremos

```javascript
export async function deleteTask(id) {
    const file = await readFile(`./Tasks.json`, "utf-8");
    const jsonFile = JSON.parse(file);
    const task = jsonFile.find(task => task.id == id);

    const updatedJsonFile = jsonFile.slice(task, 1);

    await writeFile("./Tasks.json", JSON.stringify(updatedJsonFile));

    console.log(`Task deleted successfully (ID: ${id})`);
}
```

De esta manera funcionaria correctamente y en caso de que no hubiera una tarea con dicho ID no daria error, pero mandaria el mensaje de que se ha borrado correctamente, para evitar eso haremos un condicional para comprobar que no se hayan encontrado la tarea y si es es el caso, mandaremos un `console.log()` con la información correspondiente

```javascript
export async function deleteTask(id) {
    const file = await readFile(`./Tasks.json`, "utf-8");
    const jsonFile = JSON.parse(file);
    const task = jsonFile.find(task => task.id == id);

    if (!task) {
        return console.log(`This task doesnt exist (ID: ${id})`);
    }

    const updatedJsonFile = jsonFile.slice(task, 1);

    await writeFile("./Tasks.json", JSON.stringify(updatedJsonFile));

    console.log(`Task deleted successfully (ID: ${id})`);
}
```

Ahora solamente tendriamos que añadirlo a la función principal con una operación ternaria para comprobar que estamos pasando el id y estaria listo.

```javascript
delete: () => { args[1] ? deleteTask(args[1]) : console.log("No has proporcionado id") },
```

Despues de añadirlo y hacer diversas pruebas, podemos ver que el usuario puede introducir caracteres en vez de numero y el programa responde igual, diciendo que no ha encontrado la tarea, por lo que he creado un pequeño condicional que comprueba que lo que hemos introducido es un numero y si no es asi, nos muestre un mensaje

```javascript
if (!Number(id)) {
	return console.log("La ID debe ser un numero");
}
```

Esta solución parece correcta, de hecho sera correcta pero solo en el caso de que lo utilices en el ultimo elemento del JSON, para hacer que funcione correctamente, deberemos de utilizar `filter` y obtener solo los que tengan el id diferente:

```javascript
export async function deleteTask(id) {
    const file = await readFile(`./Tasks.json`, "utf-8");
    const jsonFile = JSON.parse(file);
    const tasks = jsonFile.filter(task => task.id != id);

    if (!Number(id)) {
        return console.log("La ID debe ser un numero");
    }

    await writeFile("./Tasks.json", JSON.stringify(tasks));

    console.log(`Task deleted successfully (ID: ${id})`);
}
```

### Marcar tareas como en progreso o hechas

Una vez tenemos las tres funciones principales: `añadir, actualizar y borrar`, es bastante sencilla hacer las demas, ya que estas siguen bastante la estructura de algunas de ella, en este caso la de actualizar.

Tendremos que encontrar el `index` que queremos actualizar, y cambiar en este caso el `status`, en vez de la `description` y en este caso aunque no estemos actualizando la tarea en si, como le estamos cambiando el `status`, creo que tendria sentido cambiar tambien el `updatedAt`

```javascript
export async function markInProgress(id) {
    const file = await readFile(`./Tasks.json`, "utf-8");
    const jsonFile = JSON.parse(file);
    const index = jsonFile.findIndex(task => task.id == id);

    jsonFile[index].status = "In progress";
    jsonFile[index].updatedAt = new Date();

    await writeFile(`./Tasks.json`, JSON.stringify(jsonFile));

    console.log(`Task updated successfully (ID: ${id})`);
}
```

Con la función creada, la añadimos a la función principal con su operación ternaria correspondiente:

```javascript
"mark-in-progress": () => { args[1] ? markInProgress(args[1]) : console.log("Debes proporcionar el ID de la tarea"); },
```

Aunque esto es correcto, dentro de lo que cabe, como tenemos que hacer otra función para marcarlas como hechas, simplemente añadiremos un segundo parametro que sera el estado, y pasaremos dicho parametro en la función principal, haciendo que funcione para ambos casos

```javascript
export async function markProgress(id, status) {
    const file = await readFile(`./Tasks.json`, "utf-8");
    const jsonFile = JSON.parse(file);
    const index = jsonFile.findIndex(task => task.id == id);

    jsonFile[index].status = status;
    jsonFile[index].updatedAt = new Date();

    await writeFile(`./Tasks.json`, JSON.stringify(jsonFile));

    console.log(`Task updated successfully (ID: ${id})`);
}
```

```javascript
"mark-in-progress": () => { args[1] ? markProgress(args[1], "In progress") : console.log("Debes proporcionar el ID de la tarea"); },
"mark-done": () => { args[1] ? markProgress(args[1], "Done") : console.log("Debes proporcionar el ID de la tarea"); },
```

Ahora podremos marcar las tareas como en progreso o hechas de manera facil y con una sola función, sin tener que tener una para cada uno, es cierto que podriamos hacer que la función de actualizar hiciera esto, pero habria que hacer ciertas comprobación que harian que fuese complicado de entender, por lo que seria contraproducente.

### Mostrar las listas

Aunque tecnicamente podria hacer dos funciones, una para mostrar todas las listas y otra para mostrar las listas filtradas según su estado, lo hare todo en la misma y en caso de que no reciba ningún parametro, muestre todas las listas.

En vez de mostrar las tareas completas, mostraremos solamente la ID de la tarea, la descripción y el estado, esto lo podemos hacer facilmente con un `forEach()`

```javascript
export async function listTasks(status) {
    const file = await readFile(`./Tasks.json`, "utf-8");
    const jsonFile = JSON.parse(file);

    jsonFile.forEach(task => {
        console.log(`ID: ${task.id} - Task: ${task.description} - Status: ${task.status}`);
    });
}
```

De esta manera ya tendriamos una manera de listarlos todo, ahora tenemos que hacer la logica para si el usuario le pasa un parametro, filtre por dicho parametro.

Esto es tan sencillo como hacer un `filter` simple utilizando `includes` antes del `forEach` y hacerlo solamente cuando recibimos un valor por el parametro:

```javascript
export async function listTasks(status) {
    const file = await readFile(`./Tasks.json`, "utf-8");
    const jsonFile = JSON.parse(file);

    if (status) {

        jsonFile.filter(task => task.status.toLowerCase().includes(status.toLowerCase())).forEach(task => {
            console.log(`ID: ${task.id} - Task: ${task.description} - Status: ${task.status}`);
        });;

    } else {
        jsonFile.forEach(task => {
            console.log(`ID: ${task.id} - Task: ${task.description} - Status: ${task.status}`);
        });
    }
}
```

De esta manera, funcionaria aunque no le pongamos ningún parametro y en caso de ponerlo por el parametro, filtraria para dichos casos. 

Con toda la logica de las listas hechas, solo nos quedaria ponerlo en el comando principal y comprobar las cosas.

```javascript
list: () => { args[1] ? listTasks(args[1]) : listTasks(); },
```


## Repositorio del proyecto

Repositorio del proyecto: https://github.com/PezEjecutivo/NodeJS/tree/main/Task%20Tracker%20CLI