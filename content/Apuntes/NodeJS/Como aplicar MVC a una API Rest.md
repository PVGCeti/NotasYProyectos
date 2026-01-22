---
draft: "true"
---
#nodejs #intermedio

En caso de no sepas que es MVC, puedes consultarlo en [[Conocimientos basicos]].

>[!note] IMPORTANTE
>Es importante saber, que en esta guia se sobre entiende que ya saber hacer una API Rest, por lo que solo explicaremos el como aplicar el MVC a dicha API, en nuestro caso utilizaremos la API de [[Como hacer una API Rest con Node JS]] como base

### Por que es importante utilizar MVC?

Para tener un proyecto legible y escalable es importante tener capas de aislamiento y seguridad, ya que esto hara que nuestro proyecto sea mejor y más facil de entender y programar, ayudandonos a entenderlo mejor y facilitar el desarrollo futuro y mantenimiento.

### Antes de empezar con el MVC

Debido a que actualmente lo tenemos todo en un mismo archivo (index.js), separaremos la estructura principal y el punto de entrada a un archivo, comunmente llamado `app.js`.

```javascript
import express from "express";
import cors from "cors";
import { DEFAULTS } from "./config.js";

const PORT = process.env.PORT ?? DEFAULTS.PORT;
const app = express();

app.use(express.json());
app.use(cors());

app.listen(PORT, () => {
    console.log(`Servidor desplegado en http://localhost:${PORT}`);
});
```

Como podemos ver en el codigo, unica y exclusivamente tenemos lo necesario indispensable para desplegar el servidor de manera correcta, en caso de que quisieramos personalizar o crear un middleware, para mantener la claridad del codigo, deberemos de hacerlo en otro archivo dentro de una carpeta llamada `middlewares` y simplemente importarlo y utilizarlo, pero no deberemos de tener dicha logica en este archivo.

Con esto el servidor se desplegaria, pero no haria nada, esto es debido a que todavia no hemos terminado de adaptarlo, para que pueda funcionar (cuando terminemos de implementar el MVC), deberemos de añadir una linea de codigo más a dicho archivo, para poder acceder a las rutas

```javascript
app.use("/foods", foodsRouter)
```

Ahora deberemos de crear dicho `foodsRouter` para que funcione correctamente.

### Creando el router

El principalme motivo de implementar un patron de diseño MVC, es mantener un aislamiento y claridad del codigo, por lo que aunque el router no es parte del MVC es bastante util. El router lo crearemos en un archivo aparte en una carpeta aparte: `Routes/foods.js`, la carpeta se llamara Routes, ya que contiene las rutas y el archivo foods.js ya que contendra las rutas de foods, en caso de que tuvieramos otras rutas de un recurso diferente, deberiamos de crearle dicho archivo diferente pero en la misma carpeta.

![[APIMVCRoutesFolder.png| 700]]

Lo primero que deberemos de hacer es importar el Router que trae Express por defecto, de esta manera no necesitaremos instalar dependencias adicionales y tendremos un Router para trabajar sobre el facilmente.

```javascript
import { Router } from "express";
```

Una vez hemos importado dicho Router, deberemos de crearlo, para ello simplemente crearemos una variable, en este caso `foodsRouter` y lo guardaremos ahi, una vez tenemos dicho Router creado, copiaremos todas las rutas de nuestro recurso que teniamos hecha en `index.js` pero deberemos de cambiar el `app.` por nuestro router, es decir por `foodsRouter.`.

Ademas de esto, deberemos de cambiar la ruta, ya que en nuestras funciones teniamos de ruta `/foods`, pero al cargarlo ahora directamente en `/foods`, es incesario, por lo que solamente tendremso `/` o `/:id`, de esta manera lo hacemos independiente de la url y funcionaria siempre y cuando lo carguemos en una ruta, abstrayendolo del codigo y haciendolo reutilizable para otros proyectos incluso.

_Ejemplo de una ruta con id y de una ruta sin id_
```javascript
foodsRouter.get("/:id", (req, res) => {
    const { id } = req.params;

    const food = foods.find(food => food.id === id);

    if (!food) {
        return res.status(404).json({ error: "Food not found" });
    }

    return res.json(food);
});

foodsRouter.post("/", (req, res) => {
    const { title, type } = req.body;

    const newFood = {
        id: crypto.randomUUID(),
        title,
        type
    };

    foods.push(newFood);

    return res.status(201).json(newFood);
});
```

Actualmente este codigo no funcionaria, ya que es solamente el router, necesitaremos de crear el MVC correctamente, aunque este no es el codigo final del router, lo importaremos en `app.js` y empezaremos a crear el MVC.

```javascript
import foodsRouter from "./routes/foods.js";
```

### Creando el Controlador

Al igual que con el Router, crearemos una carpeta para los controladores donde tendremos un archivo para `foods`, de esta manera aislamos los recursos, evitamos la sobrecarga en un archivo y lo hacemos más sostenible y escalable a futuro.

![[APIMVCControllersFoods.png| 700]]

Lo primero que deberemos de hacer en dicho archivo, es crear una clase que sera la del controllador, imprescindible exportar dicha clase para poder utilizar en otros archivos. Una vez tenemos la clase creada, simplemente deberemos de crear los metodos para los recursos, utilizando el codigo que ya teniamos en las rutas.

Es importante que al crear los metodos, los creemos como `static async` por los siguientes motivos:

- static -> Al crearlo static podemos utilizar la clase directamente sin crear una instancia de esta, es decir, no tenemos la necesidad de hacer `new FoodController()`.
- async -> Aunque no necesitamos actualmente que sea asincrono, en caso de que necesitasemos llamar algo asincrono en el futuro es util mantenerlo para no evitar cambiar codigo en el futuro.


_Ejemplo del get_
```javascript
    static async getAll(req, res) {
        const { type, limit = DEFAULTS.LIMIT_PAGINATION, offset = DEFAULTS.LIMIT_OFFSET } = req.query;
        let filteredFoods = foods;

        if (type) {
            const searchTerm = type.toLowerCase();
            filteredFoods = filteredFoods.filter(food => food.type.toLowerCase().includes(searchTerm));
        }

        const limitNumber = Number(limit);
        const offsetNumber = Number(offset);

        const paginatedFoods = filteredFoods.slice(offsetNumber, offsetNumber + limitNumber);

        return res.json(paginatedFoods);
    }
```

Una vez tenemos esto, en el router, en vez de tener el codigo, lo cambiaremos por la clase y el metodo correspondiente:

```javascript
foodsRouter.get("/", FoodController.getAll);
```

Aunque ya tenemos la clase y el metodo creado, el codigo aún no es el correcto, ya que este esta utilizando datos que no deberian de cargarse en el, por lo que deberemos de cambiarlo, aunque ahora mismo crearemos todas y cada uno de los metodos que vamos a necesitar.

A la hora de crear los nombres para los metodos, no es necesario que se llamen como el verbo HTTP, es mejor crearlos con un nombre descriptivo de la acción que hacen, por ejemplo:

- post -> create
- put -> update
- patch -> partialUpdate

Una vez tenemos todos los metodos creados, podemos ver que nuestro router queda mucho más limpio y sencillo de entender para cuando sea necesario en el futuro.

![[APIMVCRouterGucci.png]]

Para terminar el codigo correcto del controlador necesitaremos primero el modelo, por lo que es el momento de crearlo.

### Creando el Modelo

Como hemos hecho anteriormente, crearemos una carpeta llamada `models`, la cual tendra un archivo que sera en nuestro caso `food.js` (en singular), es decir, el nombre del recurso del que tiene los datos correspondiente.

![[APIMVCModelsFolder.png| 700]]

El modelo sera el apartado que tendra los datos, por lo que necesitaremos importar nuestra base de datos aqui (Recordamos que la base de datos es un archivo .json, ya que no es el punto principal de la guia).

```javascript
import foods from "./datos.json" with { type: "json"};
```

Al igual que en el controller, crearemos una clase, que esta tendra los metodos para realizar los filtros y devolver los datos correspondiente, al igual que en controllers, haremos los metodos static async:

_Ejemplo_
```javascript
export class FoodModel {
    static async getAll({ type, limit = DEFAULTS.LIMIT_PAGINATION, offset = DEFAULTS.LIMIT_OFFSET } = {}) {
        let filteredFoods = foods;

        if (type) {
            const searchTerm = type.toLowerCase();
            filteredFoods = filteredFoods.filter(food => food.type.toLowerCase().includes(searchTerm));
        }

        const limitNumber = Number(limit);
        const offsetNumber = Number(offset);

        const paginatedFoods = filteredFoods.slice(offsetNumber, offsetNumber + limitNumber);

        return paginatedFoods;
    }
}
```

Una vez hacemos esto mismo pero con todos los metodos, al igual que hicimos en el Router, tendriamos el siguiente archivo en `models/food.js`

_Los metodos estan colapsado, tienen codigo dentro, no es solo una linea como en el router_
![[APIMVCModelsCompleto.png]]

Con la clase creada y los metodos creados, solamente tendriamos que ir al controller y eliminar toda la logica para cambiarla por dichos metodos, de la siguiente manera:

_Ejemplos de como seria el codigo_
```javascript
static async getAll(req, res) {
    const { type, limit = DEFAULTS.LIMIT_PAGINATION, offset = DEFAULTS.LIMIT_OFFSET } = req.query;

    const paginatedFoods =  await FoodModel.getAll(type, limit, offset);

    return res.json(paginatedFoods);
}

static async getId(req, res) {
    const { id } = req.params;

    const food = await FoodModel.getId(id);
    
    return res.json(food);
}
```

Una vez hemos hecho todos estos pasos, nos quedaria una estructura de carpetas más limpia y facil de entender, ademas de los archivos mejor separados, más seguros y mucho más facil de escalar y manejar a futuro.


>[!warning] Error al usar la ruta getAll
>Puede que experimentes que el servidor colapsa cuando realizar una petición de getAll sin pasar ningún filtro, para saber como solucionarlo vaya al apartado [[Error al usar la ruta getAll]]

>[!warning] Error al recibir los datos 
>Puede que experimentes que el servidor devuelve los datos como undefined o como `{}`, para saber como solucionarlo vaya al apartado [[Error al recibir los datos]]








