#nodejs #errores 


Es posible que experimentes problemas al utilizar los codigos proporcionados o similares, ya que el sistema de archivos (path) de Windows, utiliza la barra invertida para las rutas y no la barra normal, una forma de solucionar este problema es utilizar WSL (Windows Subsystem for Linux), pero esto es algo que deberia de tener el usuario/cliente y en caso de querer que otra persona lo use en su dispositivo personal, es posible que no lo tenga o no lo quiera descargar, por lo que para ello hay otra solución.

Esta solución es utilizar la función **join** del modulo nativo de NodeJS **node:path**, por lo que cuando fueramos a crear una ruta, deberias de hacerlo utilizando join de la siguiente manera

```javascript
// Importamos el modulo
import { join } from "node:path"

// Esta es la ruta que estamos creando con join: "output/archivos/documentos"
const outputDir = join("output", "archivos", "documentos")
```
