---
title: Error al recibir los datos
tags:
  - nodejs
  - errores
---
Es posible que si tenias una API Rest normal funcionara y al implementar el MVC dejase de recibir los datos correctamente, puede haber dos casos tipicos:

-  Recibes datos como undefined
- Recibes datos como {}

Si recibes los datos como undefined, probablemente se deba que a la hora de recibir los parametros los estas estructurando como objetos, es decir:

```javascript
//Codigo incorrecto
getId({id})

//Codigo correcto
getId(id)
```

Si recibes los datos como {}, es debido a que estas utilizando el modelo para pasar los datos pero no tienes puesto un await, por lo que intenta obtener los datos antes de que se hayan cargado y devuelve un {}

```javascript
//Codigo incorrecto
const newFood = FoodModel.create(title, type);

//Codigo correcto
const newFood = await FoodModel.create(title, type);
```