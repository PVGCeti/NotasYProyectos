---
title: GitHub User Activity
tags:
  - nodejs
  - principiante
  - CLI
  - API
---
## Información del proyecto

### Requisitos
La aplicación debe funcionar desde la linea de comando, aceptar un usuario de GitHub como argumento, hacer una petición de la actividad reciente del usuario utilizando la API de Github, y mostrar la información en la terminal. El usuario debe poder hacer:

- Mandar el nombre de usuario de GitHub como argumento

```bash
github-activity <username>
```

- Obtener la información reciente del usuario de GitHub usando la API de GitHub, usando el siguiente endpoint:

```bash
https://api.github.com/users/<username>/events
```

- Mostrar la actividad reciente en la terminal

### Ejemplos de uso

```bash
github-activity <username>

# Output:
- Pushed 3 commits to kamranahmedse/developer-roadmap
- Opened a new issue in kamranahmedse/developer-roadmap
- Starred kamranahmedse/developer-roadmap
- ...
```

No especifica cuantos resultados debe de mostrar, pero al mostrar solamente 3 en el ejemplo de uso y ser para obtener la "actividad reciente", mostrare unicamente las 3 actividades más reciente del usuario.

## Logica de Fetch a la API

Este proyecto es bastante no es tan simple como parece al principio, ya que la mayor dificultad de dicho proyecto es obtener los datos y tratarlos correctamente y añadir multiples tratamiento de errores para casos genericos y de filo.

### Obtención de los datos

### Obteniendo los datos generales

Crearemos una función asincrona que haga la petición a la API, para mantener un buen desarrollo y trato de errores basico, lo haremos dentro de un `try-catch` y comprobaremos que nos devuelve el estado correcto

```javascript
async function fetchData(username) {

    try {
        const response = await fetch(`https://api.github.com/users/${username}/events`);
        if (!response.ok) {
            throw new Error("No  se pudo conseguir el recurso");
        }
        const data = await response.json();
        console.log(data);
    }

    catch (error) {
        console.error(error);
    }
}
```

De esta manera obtenemos todos los datos de la API, como solo necesitamos los 3 ultimos, voy a separarlos y devolver solamente esos, de esta manera tenemos una mayor facilidad a la hora de tratar los datos.

```javascript
async function fetchData(username) {

    try {
        const response = await fetch(`https://api.github.com/users/${username}/events`);
        if (!response.ok) {
            throw new Error("No  se pudo conseguir el recurso");
        }

        const data = await response.json();
        return data.slice(0, 3);
    }

    catch (error) {
        console.error(error);
    }
}
```

Con esto, crearemos otra función para tratar los datos correctamente, esta función sera la función principal, por lo que a medida que creemos las funciones necesarias, las iremos añadiendo y se ira haciendo más robusta.

```javascript
async function getActivity(username) {
    const result = await fetchData(username);

    result.forEach(event => {
        console.log(`Usuario: ${event.actor.login} \nEvento: ${event.type} \nRepositorio: ${event.repo.name} \n\n`);
    });
}
```

Ya que han cambiado la API de GitHub desde que se creo los requisitos y requirimientos del proyecto y ahora es necesario un Token API para obtener información como la cantidad de commits y similar, por lo que añadiremos algo más de manejo de errores

```javascript
if (!response.ok) {
    if (response.status == 404) {
        throw new Error("El usuario que has introducido no existe");
    }
    
    throw new Error("No se pudo conseguir el recurso");
}
```

```javascript
async function getActivity(username) {
    if (!username) {
        return console.error("No has introducido ningún usuario");
    }

    const result = await fetchData(username);

    if (result) {
        result.forEach(event => {
            console.log(`Usuario: ${event.actor.login} \nEvento: ${event.type} \nRepositorio: ${event.repo.name} \n\n`);
        });
    }

    return console.error("Ha sucedido un error al obtener los datos");
}
```

## Repositorio del proyecto

Repositorio del proyecto: https://github.com/PezEjecutivo/NodeJS/tree/main/GitHub%20User%20Activity

