---
title: Error al usar la ruta getAll
tags:
  - nodejs
  - errores
---
Es importante saber que este error puede deberse a que esta ruta esta utilizando parametros para los filtros y algunos de ellos pueden venir vacio, por lo que necesitamos asegurarnos que tenemos un objeto vacio por defecto en caso de que no recibamos nada.

```javascript
//Codigo incorrecto
static async getAll({ type, limit = DEFAULTS.LIMIT_PAGINATION, offset = DEFAULTS.LIMIT_OFFSET })

//Codigo correcto
static async getAll({ type, limit = DEFAULTS.LIMIT_PAGINATION, offset = DEFAULTS.LIMIT_OFFSET } = {})
```

