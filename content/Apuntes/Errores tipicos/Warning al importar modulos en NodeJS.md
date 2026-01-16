#nodejs #errores 

Es posible que a la hora de realizar una importación en tu proyecto te aparezca un warning, esto es debido a que para las importaciones NodeJS utiliza/utilizaba CommonJS por defecto, este es el sistema de importación antiguo de NodeJS, a dia de hoy existe uno mucho mejor creado por ECMAScript y es el que se suele utilizar. Para utilizar este simplemente deberemos de añadir al archivo **package.json** la linea de `"type": "module"`. Si no tienes el archivo, crealo y añade el siguiente contenido de la siguiente manera:

```node
{
	"type": "module",
}
```
