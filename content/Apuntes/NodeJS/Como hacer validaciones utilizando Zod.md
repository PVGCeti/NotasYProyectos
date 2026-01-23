---
title: Como hacer validaciones utilizando Zod
tags:
  - nodejs
  - principiante
---
>[!warning] Base de la guia
>Es importante saber que utilizaremos una API Rest con el patron de diseño MVC, para saber como hacer esto puedes visitar [[Como aplicar MVC a una API Rest]].

### Por que usar Zod?

Puede ser que pienses que podriamos hacer las validaciones directamente con TypeScript en vez de tener que usar una biblioteca externa que pueda dar problemas de compatibilidad o que cambie de sintaxis en un futuro, pero el principal motivo por lo que TypeScript no se puede usar para este tipo de cosas es que TypeScript se ejecuta en **Build Time**, y al nuestra API funcionar en **Run Time**, sus validaciones no funcionarian.

Ademas zod es una biblioteca bastante ligera y actualmente bastante utilizada y conocida, con un gran uso a día de hoy, ademas de ser bastante simple y sencilla de usar.

### Como instalar zod?

Para instalar Zod, utilizaremos la linea de comando al igual que una biblioteca normal y corriente, no voy a proporcionar el comando por motivos de seguridad, pero si la pagina web actual: https://zod.dev/, es importante que te asegures que dicha pagina sigue siendo la pagina oficial.

### Creación del esquema

Es importante saber, que para validar los datos que recibimos, primero deberemos de tener un esquema de dichos datos, de esta manera simplemente deberemos de aplicarlo a los datos que recibimos y si concuerdan nos dara true.

Para esto crearemos una carpeta que se llamara `schema` y esta contrendra un archivo llamado `foods.js`, que sera el esquema de nuestro recurso `food`.

![[schemasFolder.png| 700]]

Una vez tenemos la estructura creada, solamente deberemos de importar todas las funciones de zod, en mi caso las importare como z, y crear un z.object que contendra la estructura json de nuestro recurso

```javascript
import * as z from "zod";

const foodSchema = z.object({
    title: z.string().min(3).max(100),
    type: z.enum(["Fast", "Healthy"])
});
```

Ademas de estos metodos que estoy usando, hay multiples metodos que pueden servirte para tus propias validaciones y en caso de que quisieras mostrar errores de validación especificos de cada caso, podriamos hacerlo de una manera bastante sencilla; Simplemente pondremos el mensaje de error que queremos.

```javascript
const foodSchema = z.object({
    title: z.string({ error: "El titulo es requerido" })
            .min(3, "Debe contener al menos 3 caracteres")
            .max(100, "No puede contener más de 100 caracteres"),
    type: z.enum(["Fast", "Healthy"], "Solo puede ser Fast o Healthy")
});
```

Una vez tenemos nuestro esquema creado, con las validaciones que queremos hacer, deberemos de crear una función que exportaremos que reciba un input (el cual sera los datos) y que los compruebe mediante un `.safeParse()`, ya que en caso de fallar simplemente daria un Warning en vez de un error y destrozar el funcionamiento de la API como haria `.parse()`.

```javascript
export function validateFood(input) {
    return foodSchema.safeParse(input);
};
```

>[!note] Usar parse() en vez de safeParse()
>Podriamos utilizar parse(), pero esto complicaria el desarrollo ya que deberiamos de utilizar un try-catch para tratarlo de manera correcta.

Ademas de tener esa función, la cual seria bastante util para el Post y el Put, es importante tener otra función que lo valide parcialmente ya que tambien tenemos el Patch, en este caso podemos hacerlo simplemente utilizando `.partial()` ademas de `.safeParse()`, pero esta bien saber que en algunos casos más complejos o que quieras una mayor seguridad, es común crear nuevos esquemas pero solamente con los datos a validar parcialmente.

```javascript
export function validatePartialFood(input) {
    return foodSchema.partial().safeParse(input);
}
```

### Implementación de la validación

Una vez tenemos el esquema creado y sus funciones para validarlos, nos toca poner en uso dichas funciones, para ello deberemos de crear una carpeta llamada `middlewares`, ya que haremos la validación mediante middlewares y añadiremos un archivo llamado `validate.js`.

Puede que ya tengas la carpeta creada para el middleware de CORS u otros, en caso de que quieras hacer el middleware de CORS puedes ver como hacerlo en [[Error de CORS al utilizar mi API en Express]].

![[middlewaresFolder.png| 700]]

Una vez tenemos el archivo de validate.js, solamente importaremos las funciones que hemos creado previamente y crearemos otras dos funciones addicionales, que seran las que usen los middlewares, en verdad son la misma función, pero una usa `validateFood()` y otra `validatePartialFood()`.

_Archivo completo_
```javascript
import { validateFood, validatePartialFood } from "../schemas/foods.js";

export function validateCreate(req, res, next) {
    const result = validateFood(req.body);

    if (result.success) {
        req.body = result.data;
        return next();
    }

    return res.status(400).json({ error: "Invalid request" });
}

export function validateUpdate(req, res, next) {
    const result = validatePartialFood(req.body);

    if (result.success) {
        req.body = result.data;
        return next();
    }

    return res.status(400).json({ error: "Invalid request" });
}
```

Con el middleware creado solo falta aplicarlo a las rutas que lo necesitamos, que son:

- Post
- Put
- Patch

Ya que las rutas de Get o Delete no necesitan validar ningún tipo de dato, aunque si quieres tener un control mayor y seguridad absoluta, podrias crear validaciones para comprobar que el id que se pasa por el pathname cumpla tus requisitos.

_Archivo de rutas completo_
```javascript
import { Router } from "express";
import { FoodController } from "../controllers/foods.js";
import { validateCreate, validateUpdate } from "../middlewares/validate.js";


export const foodsRouter = Router();

foodsRouter.get("/", FoodController.getAll);
foodsRouter.get("/:id", FoodController.getId);
foodsRouter.post("/", validateCreate, FoodController.create);
foodsRouter.put("/:id", validateCreate, FoodController.update);
foodsRouter.patch("/:id", validateUpdate, FoodController.partialUpdate);
foodsRouter.delete("/:id", FoodController.delete);
```

>[!note] validateCreate en put
>Aunque put es un metodo de actualización y no de creación, al hacer la distinción entre put y patch, put siempre debe de tener todos los datos para hacer un reemplazo completo, a diferencia de patch que es el que usariamos si solo queremos cambiar una cosa.

Con esto ya estaria funcionando correctamente, para comprobarlo solamente deberemos de hacer una petición con alguno de los metodos Post, Put o Patch y ver si se cumplen las validaciones correctamente.




