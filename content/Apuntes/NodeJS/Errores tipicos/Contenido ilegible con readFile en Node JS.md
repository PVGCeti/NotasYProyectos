#nodejs #errores 

Es posible que hayas utilizado la función de **readFile** del modulo nativo de **"node:fs/promises"** y tu resultado haya sido algo extraño como "<Buffer 48 6f 6c ...", esto es debido a que no le hemos dado ningún segundo parametro a la función y esta espera que le pasemos el tipo de codificación, ya que al haber diferentes lenguajes puede que algunos no esten disponibles en ciertos metodos y utiliza por defecto el buffer para evitar estos problemas, pero como no es legible para nosotros, deberemos de darle un sistema de codificación adecuado, el cual para el español es **utf-8**

```javascript
// Esto daria el "error" que hemos mencionado
const content = await readFile("./texto.txt");\

// Esto devuelve el texto en castellano/español
const content = await readFile("./texto.txt", "utf-8");
```
