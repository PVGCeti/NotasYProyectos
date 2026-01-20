---
title: No funciona el filtro por id
---
#errores #nodejs

Es posible que si estas creando una el CRUD de una API y realizas una busqueda por la id obtenida del pathname, no obtengas el resultado que esperabas. Esto puede deberse a que tu id sea un numero y al obtenerla del pathname siempre vas a obtenerlo como string, aunque este sea un numero, por lo que deberas de transformar la id obtenida del pathname en un numero para poder compararlos de manera correcta.

_Codigo incorrecto_
```javascript
app.get("/foods/:id", (req, res) => {
    const { id } = req.params;
    
    const food = foods.find(food => food.id === id);

    if (!food) {
        return res.status(404).json({ error: "Food not found" });
    }

    return res.send(food);
});
```

---

_Codigo Correcto_
```javascript
app.get("/foods/:id", (req, res) => {
    const { id } = req.params;
    const idNumber = Number(id);

    const food = foods.find(food => food.id === idNumber);

    if (!food) {
        return res.status(404).json({ error: "Food not found" });
    }

    return res.send(food);
});
```