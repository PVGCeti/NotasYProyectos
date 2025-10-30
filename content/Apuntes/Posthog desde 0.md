---
title: Posthog desde 0
---
## Posthog

Simplemente deberemos de crear una cuenta, seleccionar todo y luego clickar en Next.js, despues de eso, utilizaremos el comando de IA automatico:

```
npx -y @posthog/wizard@latest
```

Una vez lo hayamos instalado todo correctamente, para verificar la instalación, deberemos de hacer que mande una señal, para eso es tan facil como darle a cualquier evento, ya que estos tienen enlaces a paginas que aun no existen y nos llevara a un 404, lo cual mandara una alerta automaticamente.

Si te esta dando algun tipo de error a la hora de hacer esto, probablmente se deba a que estas usando VPN o algun tipo de Addblock, si lo desactivas, ya deberia de enviarse la petición correctamente.

Importante seleccionar el plan gratuito, ya que simplemente lo vamos a usar para aprender y ver como funciona.

![[PostHog.png]]

Lo primero que haremos, sera skipear el apartado de `Get Started`  e ir directamente al apartado de `Product Analytics` y clickar en el primer apartado, `Create Inisght`. Modificaremos lo que sale por defecto, deberemos de cambiar el nombre a `Home Page Views`, en el apartado del medio seleccionaremos `page view` y `Total count` y en el de abajo pondremos `Line Chart (cumulative)`

![[PostHog cumulative.png]]

Si vamos a Home, podremos ver un dashboard, como se puede apreciar en la imagen, al principio lo tendremos vacio, ya que la aplicación ni si quiera esta desplegada y nos muestra las tipicas alertas por defactos.

![[Posthog default dashboard.png]]

En nuestro caso, como estamos aprendiendo, vamos a crear un Dashboard propio en el que mostremos la alerta que hemos creado anteriormente, para ello, tendremos que ir Dashboard en la parte de la izquierda y una vez en ese apartado, clickar en `new dashboard`. 

![[Posthog new dashboard.png]]

Una vez hemos creado un dashboard vacio, nos saldra un popup en mitad de la pantalla para añadir las alertas que queramos, lo que deberemos de hacer es añadir la alerta que hemos creado nosotros antes, esto lo haremos clickando en el +, ya que si clickamos en la alerta en si, nos llevara a ella para poder verla mejor.

![[Posthog add insight.png]]

En caso de que queramos cambiarle el nombre, simplemente tendremos que borrar lo de `New Dashboard`, por el nombre que quieras, en mi caso `General App Usage`. Ahora, cuando vayamos al menu de Dashboard en la izquierda, nos aparecen ambos Dashboard, tanto el general, como el que acabamos de crear:

![[Posthog dashboard menu.png]]

Asi que  si ahora entramos a nuestro Dashboard, podremos ver exactamente lo que hemos puesto/queremos:

![[Posthog general app usage dashboard.png]]

Una vez tenemos esto, lo cual es una pequeña muestra de como crear alertas y un dashboard para visualizarlas facilmente, lo que haremos sera que capture los errores de la pagina, para ello, en el menu lateral izquierda, clickaremos en `Error tracking` y activaremos `Enable exception autocapture`.

![[Posthog error tracking.png]]

Una vez hecho eso, en la misma pestaña, podremos ir viendo los errores a medida que sucedan:

![[Posthog error page.png]]

