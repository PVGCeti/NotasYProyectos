httpie es una herramienta para hacer solicitudes HTTP de manera sencilla e intuitiva, de codigo abierto, que nos sirve para comprobar API's, hay multiples herramientas de este estilo, alguna de las más conocidas y que he usado anteriormente satisfactoriamente estaria Postman, pero creo que es util saber utilizar diferentes herramientas aunque tengan un proposito similar. 

El utilizar diferentes herramientas ayuda a ver puntos fuertes y puntos flojos de una herramienta de manera objetiva, ya que si sientes que falta algo o es más facil de hacer, te ayudara la proxima vez que tengas que hacer esa tarea a escoger la herramienta adecuada.

![[httpie portada.png]]

Algo positivo que tiene, es que puedes usarla desde el navegador web, pero esto no te servira para aplicaciones/proyectos que aun no esten desplegados, ya que no tiene acceso a tu localhost, como es obvio, para ello tendras que descargarla.

En mi caso, para tener una experiencia completa y poder explicar y utilizarlo mejor, me lo he descargado para Windows desde su [pagina oficial](https://httpie.io/).

![[httpie interfaz.png]]

Es importante tener en cuenta, que aunque lo tengas descargado, si estas usando una VPN, lo más probable es que te de un error de conexión a la base de datos, por lo que si vas a utilizar una API en localhost, deberias desactivar tu VPN para evitar este tipo de errores.

Por si quieres sabera que se debe este error en concreto, es que alguna VPN tienen bloqueados los puertos a los que te conectas a bases de datos y similares, esto es debido a que los nodos de las VPNs, por norma general, no soy de un unico accesorio, es decir, la IP que te proporciona la VPN, probablemente haya sido usada antes o se le dara a alguien despues, por lo que si permiten el acceso a este tipo de cosas, una persona random que se conectara a ese mismo nodo, tendria acceso a tu base de datos, en la cual puedes tener información confidencial tuya o de tus clientes, en resumen, por seguridad las VPNs bloquean el acceso a esos puertos, simplemente desactivala y todo deberia funcionar.

