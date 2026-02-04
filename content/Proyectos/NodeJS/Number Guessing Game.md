---
title: Number Guessing Game
tags:
  - nodejs
  - principiante
  - CLI
---
## Información del proyecto

### Requisitos
Es un juego de CLI, por lo que el usuario necesita utilizar la linea de comando para interactuar con el juego. El juego debe de funcionar de la siguiente manera:

- Cuando el juego empiece, debe de mostrar un mensaje de bienvenida con las reglas del juego
- El juego debe de escoger un numero aleatorio entre el 1 y el 100
- El usuario debera de escoger la dificultad (facil, medio, dificil) cada dificultad determinara el numero de intentos para adivinar el numero
- El usuario debe poder introducir sus respuestas
- Si el usuario responde correctamente, el juego debera de mostrar un mensaje de celebración junto a la cantidad de intentos que necesito
- Si el usuario responde incorrectamente, el juego mostrara un mensaje indicando si es mayor o menor que el numero introducido por el usuario
- El juego acabara cuando el usuario acierte de manera correcta o se quede sin intentos

### Ejemplo de uso

```bash
Welcome to the Number Guessing Game!
I'm thinking of a number between 1 and 100.
You have 5 chances to guess the correct number.

Please select the difficulty level:
1. Easy (10 chances)
2. Medium (5 chances)
3. Hard (3 chances)

Enter your choice: 2

Great! You have selected the Medium difficulty level.
Let's start the game!

Enter your guess: 50
Incorrect! The number is less than 50.

Enter your guess: 25
Incorrect! The number is greater than 25.

Enter your guess: 35
Incorrect! The number is less than 35.

Enter your guess: 30
Congratulations! You guessed the correct number in 4 attempts.
```

## Preparación del proyecto

Para este proyecto vamos a necesitar que el usuario introduzca multiples inputs en una sola ejecución del programa, por lo que estaremos usando `readline`, la cual es una clase nativa de Node JS que nos permite recibir y usar inputs del usuario.

Importaremos dicho `readline` de las funciones nativas de Node JS y ademas, para facilitar la legibilidad del codigo, importaremos `stdin` y `stdout`, como `input` y `output`, ya que puede que haya desarrolladores que hayan trabajado poco a bajo nivel y no lo entiendan correctamente.

```javascript
import readline from 'node:readline/promises';
import { stdin as input, stdout as output } from 'node:process';
```

Con las dependencias nativas necesarias importadas, es momento de ponerlas en uso, utilizano la clase de `readline`, crearemos una interfaz mediante el metodo `createInterface()`, la cual deberemos de pasarle como objeto, el input y el output para que pueda usarlos

```javascript
const rl = readline.createInterface({ input, output });
```

Una vez tenemos todos los preparativos hechos, podemos crear una variable simple y darle el valor del metodo `.question()`, al cual le pasaremos por parametro un string que sera la pregunta, es importante que le añadamos un await, ya que tiene que esperar al input del usuario

```javascript
const answer = await rl.question("Como estas? ");
```

Y de esta manera ya tendriamos guardado el input del usuario para utilizarlo como quisieramos, es necesario que al igual que con cualquier interfaz que creemos, la cerremos al final.

_Codigo de ejemplo de readline_
```javascript
import readline from 'node:readline/promises';
import { stdin as input, stdout as output } from 'node:process';

const rl = readline.createInterface({ input, output });
const answer = await rl.question("Como estas? ");

console.log(answer);

rl.close();
```

## Creación del bucle principal

Es importante que al ser un juego que requiere de multiples inputs del usuario, creemos un bucle principal que tendra toda la funcionalidad del juego, aunque no es donde haremos las funciones, si es donde llamaremos a todas y cada una de las funciones que necesitmos para su uso.

Por ello crearemos una función asincrona, ya que tendremos que esperar el input del usuario y la llamaremos `mainLoop()`.

```javascript
async function mainLoop(){
    
}
```

De momento lo dejaremos vacio, pero es util tener una estructura clara y pensada de lo que vamos a hacer, a medida que implementos funciones las iremos añadiendo a este bucle principal.

### Mensaje de bienvenida

Para no ensuciar la legibilidad del bucle principal, ya que puede llegar a ser complejo de entender al ser un bucle con multiples funciones y parametros introducidos por el usuario, vamos a crear una función aparte para esto, aunque sea solamente un `console.log()` algo bonito.

```javascript
function wellcome() {
    console.log("Bienvenido a Adivina el numero!! \nTienes que adivinar el numero que estoy pensando, entre el 1 y el 100 \nTienes una cantidad limitada de intentos según la dificultad");
}
```

Aunque podriamos hacerlo de esta manera y funcionaria perfectamente, para una mejor legibilidad separare las lineas en `console.log()`, de esta manera ademas de ser más legible, si cambia algo en el futuro, sera más facil de encontrarlo.

```javascript
function wellcome() {
    console.log("Bienvenido a Adivina el numero!!");
    console.log("Tienes que adivinar el numero que estoy pensando, entre el 1 y el 100");
    console.log("Tienes una cantidad limitada de intentos según la dificultad");
}
```

### Selector de dificultad

Esto es tan simple que podriamos mantenerlo en el propio bucle principal, ya que solamente necesitamos recibir un numero del 1 al 3 y cambiarlo por su respectivo numero que indicara la cantidad de intentos que tiene el usuario.

Este es el primer momento en el que deberemos obtener un input del usuario, por lo que deberemos de crear los preparativos para poder recibirlos y tratarlos correctamente, como en el apartado de [[#Preparación del proyecto]], crearemos esa estructura

```javascript
import readline from 'node:readline/promises';
import { stdin as input, stdout as output } from 'node:process';

const rl = readline.createInterface({ input, output });

// Final del archivo
rl.close();
```

De esta manera podremos usar el `rl.question()` y obtener el input del usuario facilmente, por lo que solamente deberemos de mostrarle la información y obtener el input

```javascript
async function difficultySelector() {
    console.log("Selecciona la dificultad:");
    console.log("1. Facil (10 intentos) \n2. Media (5 intentos) \n3. Dificil (3 intentos)\n");

    const answer = await rl.question("Que dificultad elijes: ");

    return answer;
}
```

Aunque de esta manera funciona correctamente, es mejor que hagamos la conversión directamente en esta función, por lo que deberia devolvernos la cantidad de intentos para hacerlo sencillo y visible, simplemente haremos un array con los intentos en esas posiciones y restaremos uno al introducido por el usuario, en este caso es importante tener un manejo de errores correcto para no mostrar outputs como `undefined`

```javascript
async function difficultySelector() {
    console.log("Selecciona la dificultad:");
    console.log("1. Facil (10 intentos) \n2. Media (5 intentos) \n3. Dificil (3 intentos)\n");

    const answer = await rl.question("Que dificultad elijes: ");

    if (answer > 3 || answer < 1) {
        console.log("No has seleccionado una dificultad valida\n");
        process.exit(1);
    }

    const difficulty = [10, 5, 3];
    const trys = difficulty[answer - 1];

    return trys;

}
```

Con la función creada, podemos añadirla al bucle principal, por lo que despues de añadir el `wellcome()`, el numero aleatorio entre el 1 y el 100 `Math.floor((Math.random() * 100) + 1)` y esta función, tendriamos un bucle principal bastante limpio y con todo lo necesario para hacer el juego.

```javascript
async function mainLoop() {
    wellcome();
    const guessNumber = Math.floor((Math.random() * 100) + 1);
    const trys = difficultySelector();

}
```

### Creación del juego

Para hacer esto, tendremos que hacer un bucle for, utilizando como indice la cantidad de intentos que tenemos, deberemos de restar 1 a dicha variable para terminar el bucle cuando alcance 0, o si el jugador lo acierta hacer un break.

Aunque se podria hacer la función de comprobación en una función aparte, creo que es lo suficientemente sencilla como para que no haga falta aislarla del resto del codigo.

```javascript
if (answer == guessNumber) {
    console.log(`Has acertado el numero!! Felicidades!! Has tardado ${(trys - index) + 1} intentos!!`);
    return;
}

if (answer > guessNumber) {
    console.log("El numero que has introducido es mayor.\n");
} else {
    console.log("El numero que has introducido es menor.\n");
}
```

Por lo que una vez tenemos el metodo para comprobar el numero, la cantidad de intentos que tenemos y sabemos una forma de preguntar por el input del usuario, solamente toca hacer un bucle con lo que queremos, de forma al juntarlo con el resto de la función el juego funcione correctamente:

```javascript
for (let index = trys; index > 0; index--) {
    const answer = await rl.question("Que numero crees que es: ");

    if (answer == guessNumber) {
        console.log(`Has acertado el numero!! Felicidades!! Has tardado ${(trys - index) + 1} intentos!!`);
        return;
    }
    
    if (answer > guessNumber) {
        console.log("El numero que has introducido es mayor.\n");
    } else {
        console.log("El numero que has introducido es menor.\n");
    }
}
```

Esta seria la función completa:
```javascript
async function mainLoop() {
    wellcome();
    console.log("");
    const guessNumber = Math.floor((Math.random() * 100) + 1);
    console.log("");
    let trys = await difficultySelector();

    for (let index = trys; index > 0; index--) {
        const answer = await rl.question("Que numero crees que es: ");

        if (answer == guessNumber) {
            console.log(`Has acertado el numero!! Felicidades!! Has tardado ${(trys - index) + 1} intentos!!`);
            return;
        }

        if (answer > guessNumber) {
            console.log("El numero que has introducido es mayor.\n");
        } else {
            console.log("El numero que has introducido es menor.\n");
        }

    }

    console.log("Has perdido...\nVuelve a intentarlo cuando tengas ganas");

}
```

>[!note] Aclaración
>Quiero aclarar que el main loop, tecnicamente seria solo el bucle, por lo que podria ser una función que reciba la cantidad de intentos y ya, pero para no encapsular demasiado ni tener una función que sea solamente llamar otras funciones, he decidido hacerlo así

De esta manera ya tendriamos el juego con todos los requisitos hecho.

## Repositorio del proyecto

Repositorio: https://github.com/PezEjecutivo/NodeJS/tree/main/Number%20Guessing%20Game