---
title: Error de CORS al utilizar mi API en Express
---
#errores #nodejs 

Es importante saber que si estamos creando una API en Express y queremos utilizarla desde otra pagina web, tenemos que tener en cuenta el CORS, puede que no tengas este problema, ya que solamente obtendras un problema de CORS si estas usando un Protocolo, Dominio o Puerto diferente al de tu API. Por lo que si tu API esta en `http://localhost:3000/api` y tu pagina esta en `http://localhost:3000/web`, si pides recursos a tu API no tendras ningún problema.

Pero en caso de que tu pagina web se aloje con un protocolo distinto, un dominio o puerto diferente si que lo obtendras, para solucionarlo en express de manera sencilla, tenemos un middleware del paquete `cors`, para instalarlo solamente deberemos de poner el comando e importarlo en nuestra API, ya que este problema es algo que deberemos de solucionar en la API, no en la web.

```bash
npm install cors
```

Para importarlo sera tan facil como hacer:

```javascript
import cors from "cors"
```

Al ser un middleware, tendremos que utiliar el metodo `.use()`  de nuestra `app`, y poner la función `cors()`, una vez hecho esto ya funcionaria, pero tendria permiso cualquier pagina externa, que en caso de no ser una API publica puede ser problematico por diversos temas, para añadir una lista de `hosts` validos, simplemente crearemos un array y dentro de la función `cors`, junto a un callback, filtraremos para dar los resultados correctos a los permitidos y un error a los que no.

```javascript
const ACCEPTED_ORIGINS = [
	"host que quieres 1",
	"host que quieres 2",
	"etc...",
]

app.use(cors({
	origin: (origin, callback) => {
		if (ACCEPTED_ORIGINS.includes(origin)) {
			return callback(null, true)
		}
		
		return callback(new Error("Origen no permitido"))
	}
}))
```

>[!note] Uso de los callbacks
>Aunque este uso de callbacks ya no es el más moderno, es un uso tipico que podras encontrar en multiples proyectos, donde el primer valor se reserva para el error, por lo que si no hay ningún error lo dejas en `null` y el segundo valor lo devuelves como `true`, en caso de que haya un error, solamente mandariamos el primer valor como un `new Error`.

En caso de que quisieramos añadirle una mayor validación podriamos comprobar la cookie o diversas cosas según los requisitos de tu API/proyecto, por lo que este metodo puede ser util para tu objetivo pero alomejor no es lo suficientemente completo.