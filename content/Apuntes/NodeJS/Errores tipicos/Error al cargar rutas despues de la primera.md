---
title: Error al cargar rutas despues de la primera
---
Es posible que al crear tu servidor y accedas a una ruta funcione de manera correcta, pero si intentas accedar a otra ruta de manera ocurrira un error, esto es debido a que en el codigo estas enviando la respuesta del servidor sin utilizar un return.

En este caso tenemos 3 rutas, `/`, `/about` y `404`, como se puede apreciar en el codigo,  antes del `res.end`, hay un return, en caso de que no estuviera funcionaria la primera ruta, pero si intentase acceder a otra no funcionaria.

```javascript
const server = createServer((req, res) => {
    res.setHeader('Content-Type', 'text/plain; charset=utf-8');

    if (req.url === "/") {
        return res.end("Este es el inicio jefe");
    }

    if (req.url === "/about") {
        return res.end("Información sobre nosotros");
    }

    res.statusCode = 404;
    return res.end("No existe la pagina");
});
```

