---
title: MongoDB Atlas
---
Lo primero que debemos de hacer, es escoger el plan gratuito, ya que lo estamos haciendo para aprender o para un proyecto pequeño el cual, no nos va a dar beneficios y otra cosa importante es selecionar la region más cercana que tengamos, en mi caso, como vivo en españa, la más cercana actualmente es Paris. Hecho esto, le daremos a `Create Deployment`.

![[Deploy atlas payment.png]]

Una vez lo hemos desplegado, nos saldra esta pantalla (Los datos privados han sido censurado en blanco, por si notas algo extraño), la cual nos dice que solamente los usuarios e ips que debemos accesos podran conectarse a la base de datos, es importante que no pierdas el acceso, ya que si no, no tendras forma de recuperarle, pero tambien es importante que nadie más que tu o las personas de confianza/encargadas lo tenga, ya que podria venir algun gracioso y borrarte la base de datos entera o algo peor.

![[Connect to cluster0 1.png]]

En esta pantalla, lo unico que debereis de hacer es copiar las creedenciales, con el boton de `Copy` y crear el usuario con el boton de `Create Database User` y clickaremos en el boton de `Choose a connection method` para continuar la configuración.

En mi caso, como voy a conectarlo directamente a la aplicación, utilizare el metodo de `Drivers`, tal y como aparece el primero en la imagen, es importante que escogas el metodo que se te adapte a ti, ya que cada proyecto/persona tiene sus propias necesidades.

![[Connect to cluster0 2.png]]

Una vez hecho esto, si habeis escogido la misma pestaña que yo, os saldra la siguiente información (al igual que en la primera imagen, las cosas estan censurados en blanco), simplemente deberemos de copiar el codigo del apartado 3 y pegarlo como una variable de entorno en nuestro proyecto.

![[Connect to cluster0 3.png]]

Una vez has hecho esto, ya tienes la base de datos lista para funcionar. En caso de que quieras saber algunos comandos basicos de MongoDB, puedes verlo en el apartado de [[Comandos basicos]].

Es importante tener en cuenta, que aunque lo tengas descargado, si estas usando una VPN, lo más probable es que te de un error de conexión a la base de datos, por lo que si vas a utilizar una API en localhost, deberias desactivar tu VPN para evitar este tipo de errores.

Por si quieres sabera que se debe este error en concreto, es que alguna VPN tienen bloqueados los puertos a los que te conectas a bases de datos y similares, esto es debido a que los nodos de las VPNs, por norma general, no soy de un unico accesorio, es decir, la IP que te proporciona la VPN, probablemente haya sido usada antes o se le dara a alguien despues, por lo que si permiten el acceso a este tipo de cosas, una persona random que se conectara a ese mismo nodo, tendria acceso a tu base de datos, en la cual puedes tener información confidencial tuya o de tus clientes, en resumen, por seguridad las VPNs bloquean el acceso a esos puertos, simplemente desactivala y todo deberia funcionar.
