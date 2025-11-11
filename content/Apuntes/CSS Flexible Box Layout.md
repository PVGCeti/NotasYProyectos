---
title: CSS Flexible Box Layout
---
Flexbox, también conocido como Diseño de Caja Flexible, es un modelo de diseño CSS que permite organizar elementos en una dimensión, ya sea en filas o columnas, de forma flexible y dinámica. Ya que para organizar elementos en dos dimesiones existe Grid, el cual se aborda en otra pagina de apuntes!

## display: flex

Lo primero que debemos de saber es que para utilizar flexbox, deberemos de aplicar la propiedad de estilo: `display: flex;` al contenedor padre del cual queremos ordenar, es decir, si tenemos `un div` que contiene `tres div` y queremos ordenar esos 3 divs, lo que derebemos de hacer es aplicar las clases de flexbox al div principal, no a los 3 subs divs.

![[display flex.png]]

## flex-direction
Una de las propiedades de CSS cuando usamos **Flexbox** es `flex-direction`, esta propiedad lo que hace es **Definir el eje principal de los elementos**. Esto significa que dictaminara si las cosas ven en el eje X (horizontalmente) o en el eje Y (verticalmente). flex-direction puede tener 4 valores: `row, row-reverse, column y column-reverse`, de los cuales, `row` es el por defecto.

- row: Define el eje principal en el eje X (horizontalmente)
- row-reverse: Define el eje principal en el eje X inverso (horizontalmente)
- column: Define el eje principal en el eje Y (verticalmente)
- column-reverse: Define el eje principal en el eje Y inverso (verticalmente)

En esta imagen puedes ver como definiria tu contenido cada una de estas propiedades:
![[flex-direction.png]]

## justify-content

Una de las propiedades de CSS cuando usamos **Flexbox** es `justify-content`, esta propiedad lo que hace es **alinear el contenido en su eje principal.** justify-content puede tener 5 valores: `flex-start, flex-end, center space-around y space-between`, de los cuales, `flex-start` es el por defecto.

- flex-start: Alinea el contenido desde el principio del eje
- flex-end: Alinea el contenido desde el final del eje
- center: Alinea el contenido en el centro del eje
- space-around: Alinea el contenido a lo largo del espacio, dejando una separación igual entre cada elemento
- space-between: Alinea el contenido dejando el mayor espacio posible entre cada elemento 

En esta imagen puedes ver como alinearia tu contenido cada una de estas propiedades:
![[justify-content.png]]

## align-items, align-content y align-self
Una de las propiedades de CSS cuando usamos **Flexbox** es `align-items`, esta propiedad lo que hace es **alinear el contenido en su eje cruzado**, esto quiere decir que si el eje principal es X, alinea en el eje Y o  si el eje principal es Y, alinea en el eje X. Al igual que justify-content, align-items puede tener 5 valores, aunque no son todos los mismos: `flex-start, flex-end, center, baseline stretch`, de los cuales, `flex-start` es el por defecto.

- flex-start: Alinea el contenido desde el principio del eje 
- flex-end: Alinea el contenido desde el final del eje
- center: Alinea el contenido en el centro del eje
- baseline: Alinea en función de la baseline del contenido
- stretch: Alarga el contenido para que ocupe todo el espacio disponible en el eje

>[!note] Importante
>Recuerda que align-items hace referencia al eje cruzado, no al principal

En esta imagen puedes ver como alinearia tu contenido cada una de estas propiedades:
![[align-items.png]]

>[!danger] align-content y align-self
>Estas propiedades funcionan exactamente igual que align-items, la diferencia entre ellas esta en función a los elementos que afectan, ya que:
>- align-items: Afecta a todos los elementos de la misma linea
>- align-content: Afecta a todos los elementos de todas las lineas
>- align-self: Solo afecta a un elemento en especifico

## order
Una de las propiedades de CSS cuando usamos **Flexbox** es `order`, esta propiedad lo que hace es **cambiar el orden del elemento en especifico**, esto lo que haria, es si tenemos 3 elementos y queremos mover el primer elemento al tercero, solamente tendriamos que poner: `order: 2`.

![[order.png]]

## flex-wrap
Una de las propiedades de CSS cuando usamos **Flexbox** es `flex-wrap`, esta propiedad lo que hace es **"envolver" el contenido**, es decir, si todo el contenido ocupa más de una linea, achicara el contenido para que quepa en esa sola linea, en caso de que este en su valor por defecto, los valores que puede tener son 3: `no-wrap, wrap y wrap-reverse`, siendo `no-wrap` el por defecto.

- no-wrap: Si el contenido ocupa más de una linea, lo estrecha para que ocupe solamente esa linea
- wrap: Si el contenido ocupa más de una linea, lo dividira en diferentes lineas sin tener que "achicar" el contenido
- wrap-reverse: Si el contenido ocupa más de una linea, lo dividira en diferentes lineas sin tener que "achicar" el contenido de manera inversa

En esta imagen puedes ver como "achicaria" tu contenido cada una de estas propiedades:
![[flex-wrap.png]]

>[!note] Importante
>Ten en cuenta que el contenido ocupa más de una linea, en la primera imagen, si hubiera contenido verias como no estaria con su aspect ratio correcto.




