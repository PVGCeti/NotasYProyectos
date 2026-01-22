---
title: Como desplegar tu API en Vercel
tags:
  - nodejs
  - principiante
---
Es importante tener en cuenta que la API que vamos a desplegar es la API desarrollada en las guias:

- [[Como hacer una API Rest con Node JS]]
- [[Como aplicar MVC a una API Rest]]

Por lo que para esta guia se tiene en cuenta que ya tienes una API creada y no se explicaran detalles sobre como hacerla.

Lo primero que deberemos de hacer es modificar nuestro `app.js` ligeramente, la modificación sera exportar por defecto al final del archivo la constante de `app` y poner que solamente se realice el `app.listen`, en caso de que no este en producción.

_Archivo app.js entero_
```javascript
import express from "express";
import { foodsRouter } from "./routes/foods.js";
import cors from "cors";

import { DEFAULTS } from "./config.js";

const PORT = process.env.PORT ?? DEFAULTS.PORT;
const app = express();

app.use(express.json());
app.use(cors());

app.use("/foods", foodsRouter);

if (process.env.NODE_ENV !== "production") {
    app.listen(PORT, () => {
        console.log(`Servidor desplegado en http://localhost:${PORT}`);
    });
}

export default app;
```

Una vez hemos hecho estos pequeños cambios tocara utilizar vercel (en este caso) para desplegarlo, ya que vercel ofrece una capa gratuita muy buena y es bastante sencillo.

### Vercel CLI

Lo primero que deberemos de hacer es instalar Vercel CLI, para ello iremos a la pagina oficial de vercel y ejecutaremos el comando que pone, por temas de seguridad es mejor utilizar `pnpm` a `npm`, aunque ambos son validos.

Una vez lo tenemos instalado, solamente tendremos que iniciar sesión mediante el comando `vercel login`, el cual nos abrira el navegador para que iniciemos sesion y confirmemos.

Con la sesion ya iniciada y confirmada solamente tendremos que hacer `vercel deploy` y lo tendremos desplegado rapidamente, esto nos mostrara tanto la URL en la que esta alojada como la URL de vercel para que puedas ver los detalles.