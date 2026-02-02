---
title: Expense Tracker
tags:
  - nodejs
  - principiante
---
## Requerimientos
La aplicación debera funcionar desde la lina de comando y tener las siguientes caracteristicas

- El usuario puede añadir un gasto con cantidad y descripcion
- El usuario puede actualizar un gasto
- El usuario puede borrar un gasto
- El usuario puede ver todos los gastos
- El usuario puede ver un resumen de todos los gastos
- El usuario puede ver un resumen de todos los gastos de un mes especifico (del año actual)

Estos son los comandos que se pueden hacer y el resultado que espera:
```
$ expense-tracker add --description "Lunch" --amount 20
# Expense added successfully (ID: 1)

$ expense-tracker add --description "Dinner" --amount 10
# Expense added successfully (ID: 2)

$ expense-tracker list
# ID  Date       Description  Amount
# 1   2024-08-06  Lunch        $20
# 2   2024-08-06  Dinner       $10

$ expense-tracker summary
# Total expenses: $30

$ expense-tracker delete --id 2
# Expense deleted successfully

$ expense-tracker summary
# Total expenses: $20

$ expense-tracker summary --month 8
# Total expenses for August: $20
```

## Preparación del proyecto

A diferencia del [[Task Tracker CLI]], en este caso no lo haremos de manera posicional, sino por el argumento que introduzca el usuario, es decir `--description "Lunch" --amount 20`, pueden ir en cualquier orden:

- `--description "Lunch" --amount 20`
- `--amount 20 --description "Lunch"`

Para hacer esto de manera efectiva y limpia, utilizaremos una dependencia de Node JS llamada `commander`, para instalarlar podeis visitar la pagina oficial y obtener el comando, o usar este, el cual es el actual a día 2/1/2026:

```npm
npm i commander
```

## Utilizando commander

Es importante cuando estamos utilizando **commander**, crear un `Command como programa principal`, el cual tenga los datos basicos de información de nuestra aplicación, aunque trabajeremos principalmente con `command`, es necesario tenerlo para utilizar de manera correcta `commander`.

Para esto simplemente crearemos una instancia nueva de `command()`, que sera el core del programa, donde tendremos que añadir **nombre**, **descripcion** y **version**.

```javascript
import { Command } from "commander";
const program = new Command();

program
    .name('expense-tracker')
    .description('CLI para manejar tus gastos')
    .version('0.1.0');
```

Una vez tenemos esto, crearemos la estructura para el comando de **añadir** un gasto, para hacer esto tendremos que usar `.command()` y añadir los datos pertinentes como:

- Descripción
- Opciones
- Acción

Por lo que siguiendo el ejemplo de uso del principio, obtendriamos algo similar a esto:

```javascript
program.command('add')
    .description('Añade un gasto')
    .option('--description <string>', 'Descripción del gasto')
    .option('--amount <int>', 'Cantidad del gasto')
    .action((options) => {
        console.log(options);
    });
```

No utilizamos `.argument()`, ya que lo estamos haciendo mediante opciones, no de forma posicional y `.argument()` funciona posicionalmente, por lo que tendremos que utilizar solamente `.option()` debido a los requisitos.

Ademas, es importante añadir un `program.parse()` para que funcione de manera correctamente.

Como información adicional, en commander, cuando queremos hacer que nos tengan que pasar un parametro de manera obligatoria al usar una opción deberemos de usar `<>`, mientras que si queremos que sea opcional tendremos que usar `[]`.

Una vez sabemos como funciona y tenemos un `.command()` hecho, haremos los demas que necesitamos pero no le añadiremos la logica, simplemente los dejaremos con un `console.log()` para comprobar que el comando se ejecuta bien.

```javascript
import { Command } from "commander";
const program = new Command();

program
    .name('expense-tracker')
    .description('CLI para manejar tus gastos')
    .version('0.1.0');


program.command('add')
    .description('Añade un gasto')
    .option('--description <string>', 'Descripción del gasto')
    .option('--amount <int>', 'Cantidad del gasto')
    .action((options) => {
        console.log("Añadir");
    });

program.command('list')
    .description('Muestra los gastos')
    .action(() => {
        console.log("Lista");
    });

program.command('summary')
    .description('Muestra un resumen de los gastos')
    .option('--month <int>', 'Mes del año del que quieres mostrar los gastos')
    .action((options) => {
        console.log("Resumen");
    });

program.command('delete')
    .description('Borra un gasto')
    .option('--id <int>', 'ID de la tarea a borrar')
    .action((options) => {
        console.log("Borrar");
    });

program.parse();
```

## Creando su funcionalidad

Con el programa principal creado deberemos de crear su funcionalidad y sistema para hacerlo, ya que necesitaremos guardar la información de los gastos y poder actualizar, ademas deberemos de crear un sistema de ID sencillo para que funcione de manera comoda para el usuario.

### Sistema de ID

Como realizamos en el proyecto de [[Task Tracker CLI]], crearemos un archivo aparte, que sera un `.txt` y simplemente tendra el ID actual, esto nos permite que no se repitan los IDs en caso de borrar gastos y crearlos de nuevo, tener un seguimiento de cuantos se han realizado y ser un numero sencillo, ya que es poco probable que una persona llegue a más de 6 cifras de ID.

Aunque es poco probable, no significa que sea imposible, por lo que en caso de que llegasemos a ese punto y fuese molesto el tener que escribir dicho ID largo, se podria reiniciar facilmente cambiando el contenido de dicho archivo, al ser un `.txt` simple es muy sencillo el realizarlo

_El contenido del archivo es 0 actualmente_
![[idtrackertxt.png| 700]]

Una vez tenemos el archivo deberemos de usar `writeFile` y `readFile` para leer el archivo e incrementarlo cada vez que llamemos a la función, esta función es muy simple pero util.

```javascript
async function getNewId() {
    const id = await readFile("./idtracker.txt", "utf-8");
    const newId = Number(id) + 1;
    await writeFile(`./idtracker.txt`, newId.toString());

    return newId;
}
```

### Crear un gasto

Para crear un gasto deberemos de hacer una función simple que cree un objeto con los datos de nuestros gastos `id, fecha, descripción, cantidad`, teniendo en cuenta que obtenemos el ID de la función que hemos creado previamente, las descripción y la cantidad como parametros de la función y la fecha la obtendremos desde `new Date()`.

```javascript
async function createExpense(desc, amount) {
    const id = await getNewId();

    const expense = {
        id: id,
        date: new Date(),
        description: desc,
        amount: amount
    };
}
```

Al hacer esto, obtendriamos un objeto correcto, pero la fecha no la tenemos de manera correcta, ya que nos esta dando más información de la que deberia, para hacer que simplemente nos pase el dia, mes y año, haremos un pequeñito cambio utilizando `.toLocaleString()` y `.split(",")` y obtendremos la fecha como queremos

_Output de date: '2/1/2026'_
```javascript
async function createExpense(desc, amount) {
    const id = await getNewId();

    const expense = {
        id: id,
        date: new Date().toLocaleString().split(",")[0],
        description: desc,
        amount: amount
    };
}
```

En este momento hay dos posibilidades que debemos de contemplar:

- No hay ningún dato previo y por ende, no esta el archivo que guarda los datos creados
- Ya hay datos y debemos añadir el nuevo dato sin sobreescribirlo

Como el orden natural seria que no exista dicho archivo, contemplaremos esa posibilidad primero y crearemos el codigo para poder crear el archivo de manera correcta y sin problemas, pero teniendo en cuenta que debemos de hacer la segunda posibilidad tambien.

Al ser nuestro primer dato, no hay una array previa a la que podamos hacerle `push()` de dicho objeto, por lo que deberemos de crear un array vacio y añadirlo a dicho array, ya que si en el futuro queremos tener más datos, es necesario usar un array.

```javascript
async function createExpense(desc, amount) {
    const id = await getNewId();
    let file = [];

    const expense = {
        id: id,
        date: new Date().toLocaleString().split(",")[0],
        description: desc,
        amount: amount
    };

    file.push(expense);
    await writeFile("./expenses.json", JSON.stringify(file));

    console.log(`Expense added successfully (ID: ${id})`);
}
```

Una vez tenemos hecho para que cree y guarde los datos en caso de que no haya ninguno, es importante hacer que cuando vaya a crear más datos, no sobreescriba los ya existeten, por lo que nuestra función tendra dos ramas principales, es decir, necesitaremos utilizar un condicional.

Para facilitarnos la comprobación de la existencia del archivo, utilizaremos una función nativa de Node JS, que es `existsSync`

```javascript
if (existsSync("./expenses.json")) {
    
} else {
    file.push(expense);
    await writeFile("./expenses.json", JSON.stringify(file));
}
```
 
Ahora solamente deberemos de leer el archivo, añadir nuestro objeto y escribir el archivo

```javascript
if (existsSync("./expenses.json")) {
    file = await readFile("./expenses.json", "utf-8");

    const jsonFile = JSON.parse(file);
    jsonFile.push(expense);
    
    await writeFile("./expenses.json", JSON.stringify(jsonFile));
}
```

De esta manera nuestra función estaria creada y funcionando en ambos casos, sin causar errores ni borrando datos que no deba.

_Función completa_
```javascript
async function createExpense(desc, amount) {
    const id = await getNewId();
    let file = [];

    const expense = {
        id: id,
        date: new Date().toLocaleString().split(",")[0],
        description: desc,
        amount: amount
    };

    if (existsSync("./expenses.json")) {
        file = await readFile("./expenses.json", "utf-8");

        const jsonFile = JSON.parse(file);
        jsonFile.push(expense);

        await writeFile("./expenses.json", JSON.stringify(jsonFile));
    } else {
        file.push(expense);
        await writeFile("./expenses.json", JSON.stringify(file));
    }

    console.log(`Expense added successfully (ID: ${id})`);
}
```

### Actualizar un gasto

En este caso, al no haber ejemplos de uso, deberemos de pensarlos nosotros mismo, en mi caso, creo que seria conveniente el poder actualizar tanto la cantidad como la descripción, por si te has equivocado en la descripción o puesto una coma cuando no debes.

Al hacer esto nuestra función deberia de recibir 3 parametros:

- Id del gasto
- descripción del gasto
- cantidad del gasto

Ademas de esto, deberemos de buscar en nuestra base de datos (archivo JSON), la tarea en cuestión, para hacer esto, lo podremos hacer mediante la función de `findIndex`:

```javascript
async function updateExpense(id, desc, amount) {
    const file = await readFile("./expenses.json", "utf-8");
    const jsonFile = JSON.parse(file);
    const index = jsonFile.findIndex(expense => expense.id == id);
}
```

A diferencia del `Task Tracker CLI`, no necesitamos añadir cuando se actualizo dicho gasto, por lo que solamente tendremos que cambiar la descripción y la cantidad, con el `id` obtenido, solamente deberemos de cambiarlos y sobreescribir el archivo.

```javascript
async function updateExpense(id, desc, amount) {
    const file = await readFile("./expenses.json", "utf-8");
    const jsonFile = JSON.parse(file);
    const index = jsonFile.findIndex(expense => expense.id == id);

    jsonFile[index].description = desc;
    jsonFile[index].amount = amount;

    await writeFile("./expenses.json", JSON.stringify(jsonFile));

    console.log(`Expense updated successfully (ID: ${id})`);
}
```

Como no estaba en el ejemplo de uso, no lo habiamos añadido como un comando del commander, por lo que deberemos de añadirlo, en este caso, al no mostrar como debemos hacerlo, utilizaremos el metodo `argument`, para mostrar su uso y funcionalidad.

```javascript
program.command('update')
    .argument('<int>', 'ID del gasto')
    .option('--description <string>', 'Descripción del gasto')
    .option('--amount <int>', 'Cantidad del gasto')
    .action((options) => {
        console.log("Actualizar");
    });
```

Al estar usando argument, en caso de que no lo introduzcamos nos mostrara un error por predeterminado, lo cual no es lo que queremos, ya que si hay un error, queremos que se muestre de manera más humana, por lo que en el action, ademas de tener `options`, deberemos de tener el id y comprobarlo ahi, ya que haremos que sea opcional.

```javascript
program.command('update')
    .argument('[id]', 'ID del gasto')
    .option('--description <string>', 'Descripción del gasto')
    .option('--amount <int>', 'Cantidad del gasto')
    .action((id, options) => {
        if (!id) {
            console.error("Debes de proporcionar el ID");
            process.exit(1);
        }
        console.log("Actualizar");
    });
```

### Borrar un gasto

Esta función es muy similar a la de actualizar, pero en vez de usar `findIndex`, utilizaremos `find` para filtrar los objetos que no tengan el id que queremos, de esta manera, solamente tendremos que guardarlo y dar dicha variabla para que se sobreescriba

```javascript
async function deleteExpense(id) {
    const file = await readFile("./expenses.json", "utf-8");
    const jsonFile = JSON.parse(file);
    const expense = jsonFile.filter(expense => expense.id != id);

    await writeFile("./expenses.json", JSON.stringify(expense));

    console.log(`Expense deleted successfully (ID: ${id})`);
}
```

Es importante saber, que de esta manera funciona correctamente, pero no estamos haciendo ningún tipo de validación o manejo de errores, ya que primero tendremos las funcionalidades del programa y despues haremos test y la gestión de errores que sea pertinente.

### Ver todos los gastos

Esta función es bastante simple de hacer, ya que solamente deberemos de obtener los datos y mostrarlos de manera correcta, aunque podriamos usar dependencias externas para mostrarlos de mejor manera, prefiero mantenerlo sin más dependencias externas excepto `Commander`.

Por lo que para hacer esta función solamente necesitamos leer el archivo JSON y mostrar los datos formateados de la manera correcta:

```javascript
async function listExpense() {
    const file = await readFile("./expenses.json", "utf-8");
    const jsonFile = JSON.parse(file);

    console.log("ID  Date       Description  Amount");
    jsonFile.forEach(expense => {
        console.log(`${expense.id}  ${expense.date}   ${expense.description}        ${expense.amount}`);
    });
}
```

Aunque de esta manera nos mostraria los datos como en el ejemplo de uso, es importante saber que las columnas se van a desalinear, ya que los espacios estan puesto de manera "hard codeada", pero sin dependencias externas es muy complejo el mantener separado de forma correcta los datos.

![[listexpense.png]]

### Ver resumen de los gastos

Esta función sigue el mismo principio que la anterior, solo que necesitaremos sumar los gastos y mostrarlos al final, esto es tan sencillo como hacer un `forEach` que sume a una variable, lo unico a tener en cuenta es que al leer los datos, los obtendremos como string, por lo que deberemos de convertirlos en un numero (int) para poder sumarlos correctamente

```javascript
async function summaryExpense() {
    const file = await readFile("./expenses.json", "utf-8");
    const jsonFile = JSON.parse(file);
    let total = 0;

    jsonFile.forEach(expense => {
        total += Number(expense.amount);
    });

    console.log(`Total expenses: ${total}$`);
}
```

### Ver resumen de los gastos según el mes

Esta función es la misma que la anterior pero con un filtro, por lo que solamente tendremos que modificar un poco la anterior, dandole un parametro y añadiendo un filtro

```javascript
async function summaryExpense(month) {
    const file = await readFile("./expenses.json", "utf-8");
    const jsonFile = JSON.parse(file);
    let total = 0;

    if (month) {
        const filteredExpenses = jsonFile.filter(expense => expense.date.slice("/")[0] == 2);
        filteredExpenses.forEach(expense => {
            total += Number(expense.amount);
        });

        console.log(`Total expenses: ${total}$ in `);

    } else {
        jsonFile.forEach(expense => {
            total += Number(expense.amount);
        });
        console.log(`Total expenses: ${total}$`);
    }

}
```

De esta manera lo tendriamos perfecto, el unico problema se que no tenemos el nombre del mes para mostrarlo, para hacer eso deberemos de crear una mini función de utilidad para no molestar ensuciar el codigo y funcionalidad de esta función.

Para dicha función simplemente crearemos una fecha con el mes y obtendremos el nombre mediante el uso de `month: long`

```javascript
function getMonthName(number) {
    const date = new Date(`${number}/1/2000`);
    return date.toLocaleString("en-EN", { month: "long" });
}
```

Con esta función hecha, simplemente deberemos de ponerla en el console.log

```javascript
async function summaryExpense(month) {
    const file = await readFile("./expenses.json", "utf-8");
    const jsonFile = JSON.parse(file);
    let total = 0;

    if (month) {
        const filteredExpenses = jsonFile.filter(expense => expense.date.slice("/")[0] == month);

        filteredExpenses.forEach(expense => {
            total += Number(expense.amount);
        });

        console.log(`Total expenses: ${total}$ in ${getMonthName(month)}`);

    } else {
        jsonFile.forEach(expense => {
            total += Number(expense.amount);
        });

        console.log(`Total expenses: ${total}$`);
    }
}
```

Con esto ya tendriamos todas las funciones necesarias creadas, solo toca hacerlas funcionar junto al Commander, lo cual, teniendo en cuenta que esta medio preparado no deberia ser muy complicado, solo tendremos que tener en cuenta que tengamos los datos que necesitamos y listo.

## Añadiendo las funciones al Commander

A la hora de añadir las funciones, en caso de que necesitemos que las opciones esten si o si, simplemente deberemos de hacer un condicional y comprobar que esten.

```javascript
program.command('add')
    .description('Añade un gasto')
    .option('--description <string>', 'Descripción del gasto')
    .option('--amount <int>', 'Cantidad del gasto')
    .action((options) => {
        if (options.description && options.amount) {
            createExpense(options.description, options.amount);
        } else {
            console.log("Debes introcudir descripción y cantidad a la hora de crear un gasto");
        }
    });
```

En el caso de update es lo mismo pero con el simple cambio de que aqui tenemos un argument, por lo que a la hora de usar el `action`, deberemos de añadirle el argument como parametro

```javascript
program.command('update')
    .argument('[id]', 'ID del gasto')
    .option('--description <string>', 'Descripción del gasto')
    .option('--amount <int>', 'Cantidad del gasto')
    .action((id, options) => {
        if (!id) {
            console.error("Debes de proporcionar el ID");
            process.exit(1);
        }

        updateExpense(id, options.description, options.amount);
    });
```

Es importante realizar un pequeño cambio a la función de updateExpense, ya que tal y como estaba, necesitabas poner ambas opciones para cambiarlo, pero es muy probable que solamente quieras cambiar una cosa en vez de las dos a la vez.

```javascript
export async function updateExpense(id, desc, amount) {
    const file = await readFile("./expenses.json", "utf-8");
    const jsonFile = JSON.parse(file);
    const index = jsonFile.findIndex(expense => expense.id == id);

    desc ? jsonFile[index].description = desc : "";
    amount ? jsonFile[index].amount = amount : "";

    await writeFile("./expenses.json", JSON.stringify(jsonFile));

    console.log(`Expense updated successfully (ID: ${id})`);
}
```

En cuanto a la lista, al no requerir de ningún parametro, solamente tendremos que poner la función y ya

```javascript
program.command('list')
    .description('Muestra los gastos')
    .action(() => {
        listExpense();
    });
```

En el caso de summary es basicamente lo mismo pero añadirle un parametro, al no ser obligatorio ni siqueira es necesario que pongamos un condicional

```javascript
program.command('summary')
    .description('Muestra un resumen de los gastos')
    .option('--month <int>', 'Mes del año del que quieres mostrar los gastos')
    .action((options) => {
        summaryExpense(options.month);
    });
```

Con el delete, simplemente añadiremos un condicional y avisaremos en caso de que no introduzcan el id

```javascript
program.command('delete')
    .description('Borra un gasto')
    .option('--id <int>', 'ID de la tarea a borrar')
    .action((options) => {
        if (options.id) {
            deleteExpense(options.id);
        } else {
            console.log("Debes introducir un ID");
        }
    });
```

Con esto el proyecto ya estaria funcionando, con un manejo de errores correcto y sencillo, con una estructura clara y un codigo limpio.

## Repositorio del proyecto

repositorio: https://github.com/PezEjecutivo/NodeJS/tree/main/Expense%20Tracker





