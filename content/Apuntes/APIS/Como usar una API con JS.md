---
title: Como usar una API con JS
---
En este ejemplo utilizaremos una API muy simple y conocida, como es: <a href="https://pokeapi.co/">PokeAPI</a>

Podemos hacerlo utilizado .then o usando await, en caso de que queramos hacerlo usando await tendremos que usar una función si o si.

```js
fetch("https://pokeapi.co/api/v2/pokemon/pikachu")
    .then(response => {
       if(!response.ok){
           throw new Error("No se pudo conseguir el recurso")
       }

       return response.json()
   })
   .then(data => console.log(data))
   .catch(error => console.error(error))
```

```js
fetchData()

async function fetchData(){

    try{
        const response = await fetch("https://pokeapi.co/api/v2/pokemon/ditto")
        if(!response.ok){
            throw new Error("No  se pudo conseguir el recurso")
        }
        const data = await response.json()
        console.log(data)
    }

    catch(error){
        console.error(error)
    }
}
```

