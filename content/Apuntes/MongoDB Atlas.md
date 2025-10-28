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

Una vez has hecho esto, ya tienes la base de datos lista para funcionar.