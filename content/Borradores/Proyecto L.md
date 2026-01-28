---
draft: "true"
---
## Idea principal

Una aplicación a la que poder poner subir información de tus partidas para trackear tu progreso, un apartado de notas que puedes conectar a las partidas para acordarte mejor.

Esto te ayuda a trackear las partidas (manualmente) y poder mostrar en que fallas (si lo anotas), ya que el poner las cosas tu manualmente hace que tengas una mejor retención de la información que si simplemente lo lees, ademas tendras una forma clara y estructurada de ver y crear el contenido de la pagina.

## Componentes necesarios

1. API Rest en node
2. Front con React y TailwindCSS
3. Sistema de despliegue en local

## Desarrollo de ideas

### API Rest (Node JS)

#### Recursos
La API tendra dos recursos principales, los cuales seran:

- Partidas
- Notas

##### Partidas
El recurso de Partidas tendra un CRUD con las siguientes propiedades:

- Resultado: Victoria o Derrota (Enum)
- Score: x/x/x (String formado por numeros y /)
- Campeon: Smolder/Vayne/Twitch/Ekko/Evelynn/Extra (Enum)
- Nota: (Campo opcional, Si alguien lo rellena a la hora de crear la partida, cuando se crea la partida estara vacio, pero se actualizara automaticamente cuando el recurso de notas se cree y tendra la ID de dicha nota)
  
##### Notas
El recurso de Notas tendra un CRUD con las siguientes propiedades:

- Fallo: (boolean)
- Campeon: Smolder/Vayne/Twitch/Ekko/Evelynn/Extra (Enum)
- Contenido: Contenido de la nota/explicación (String)

Ademas del CRUD tendra un metodo Patch para quitar el fallo en caso de que el jugador lo haya correguido.
### Base de datos de la API
Para facilitar el desarrollo de la API, al ser un proyecto para uso personal, se utilizara un archivo JSON, en el que se escribiran y leeran los datos para mantener persistencia en vez de guardarlos en memoria

### Extra
Deberemos de crear un middleware para el CORS ya que esta API se utilizara en una pagina alojada en un puerto externo.

La API tendra validación de datos para evitar contenido sin mayusculas o contenido diferente al valido, aunque tambien deberan de haber validaciones en el FRONT.

### FRONT React y TailwindCSS
La pagina tendra una homepage que sera el dashboard donde se mostraran tus resultados generales con cada campeon, si clicas en el te llevara a algún fallo especifico de dicho campeón.

![[Imagen de muestra.png]]

Similar a esta pagina, cada tarjetita tendra la foto de su campeon correspondiente, excepto el extra, el cual tendra una imagen del ping de desaparecido.

Los botones seran de añadir partida y crear notas

La barra de abajo el winrate general y el elo.

El header sera diferente, ya que este tendra El icono y el nombre de la pagina a la izquierda pero a la derecha tendra los apartados de "Inicio, Campeon, Notas"

Aquí tienes el archivo en formato **Markdown (`.md`)** listo para copiar, pegar y guardar. He organizado todo para que sea fácil de consultar por un desarrollador o diseñador.

#### Claro

| Elemento                 | Hexadecimal | Descripción                                     |
| :----------------------- | :---------- | :---------------------------------------------- |
| **Fondo (Body)**         | `#F8F9FA`   | Blanco Petricita. Limpio y sólido.              |
| **Texto Primario**       | `#1A222E`   | Azul Marino Profundo. Alta legibilidad.         |
| **Color de Marca**       | `#004A99`   | Azul Demaciano. Identidad institucional.        |
| **Call to Action (CTA)** | `#CDBE91`   | Oro Noble. Botones de acción principal.         |
| **Acento / Resaltado**   | `#A0AAB5`   | Gris Acero. Para iconos y detalles secundarios. |
| **Bordes / Divisores**   | `#E2E8F0`   | Gris suave para mantener la estructura.         |
#### Oscuro

| Elemento | Hexadecimal | Descripción |
| :--- | :--- | :--- |
| **Fondo (Body)** | `#0B121A` | Azul Abisal. Profundidad y elegancia. |
| **Superficie (Cards)** | `#16212E` | Azul Carbón. Para elevar elementos del fondo. |
| **Texto Primario** | `#E3E8EF` | Blanco Hielo. Suave para la vista. |
| **Call to Action (CTA)** | `#D4AF37` | Oro Metálico. Alto contraste para destacar. |
| **Acento / Links** | `#4A90E2` | Azul Brillante. Para enlaces e interacción. |
| **Bordes / Líneas** | `#2C3E50` | Acero Forjado. Estructura discreta. |

### Despliegue
Se hara mediante un simple CLI que desplegara la API y el Front, listos para poder utilizarlos.


## Datos para el json
### Partidas
```
[
{
id: 1
result: Win
score: 17/5/9 - 330
champ: Smolder
note: 1
},
]
```

### Notas
```
[
{
misstake: Si
champ: Smolder
content: No se como continuar la build despues de 4 item, no tengo claro cuando ir smolder tanque, standar o critico, poco farmeo en early, poco map awarness en general y overextend inecesario resultando en dar un bounty.
}
]
```
