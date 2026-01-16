#nodejs #errores 

Este error puede deberse principalmente a que estamos intentado crear multiples carpetas (o tenemos la ruta mal, dentro de una carpeta que no existe o similares) y no tenemos la recursividad activada, para ello simplemente deberemos de añadir un segundo parametro a la función que sera un objeto llamado **recursive**, con el valor de true.

```javascript
mkdir(outputDir, { recursive: true });
```
