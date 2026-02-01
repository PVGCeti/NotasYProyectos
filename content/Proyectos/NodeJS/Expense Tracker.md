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

# WIP





