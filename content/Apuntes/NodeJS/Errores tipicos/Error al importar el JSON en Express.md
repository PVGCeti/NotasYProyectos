---
title: Error al importar el JSON en Express
---
Es posible que si estas intentando importar un archivo **.json**, a tu proyecto de Node JS/Express directamente te de un error de **ATTRIBUTE_MISSING**, esto es debido a que por seguridad no puedes importar un archivo directamente, ya que no tiene forma de comprobar si es un archivo malicioso o no, por lo que por seguridad no permiten importar archivos si no indicas el tipo de dicho archivo.

_Codigo incorrecto_
```javascript
import foods from "./datos.json"
```

_Codigo correcto_
```javascript
import foods from "./datos.json" with { type: "json" };
```