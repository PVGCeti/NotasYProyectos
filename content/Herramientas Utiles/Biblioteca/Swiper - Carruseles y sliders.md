Swiper es una biblioteca de JavaScript que permite crear carruseles y sliders de contenido de manera fácil y eficiente. Es especialmente popular en el desarrollo web para mostrar imágenes, testimonios, productos y otros tipos de contenido en un formato deslizante.

Para poder utilizar swiper, en mi caso lo hare linkeando swiper mediante un enlace de cdn para no tener que usar un proyecto de node. Por lo que nuestro archivo html debera tener la siguiente linea en la cabeza (head):

```
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/swiper/swiper-bundle.min.css" />
```

Ademas de esta linea en la cabeza, deberemos de tener un script en el body, el cual es la siguiente linea, ademas de eso, añadiremos un archivo script propio nuestro:

```
    <script src="https://cdn.jsdelivr.net/npm/swiper/swiper-bundle.min.js"></script>
    <script src="script.js"></script>
```

Es importante añadir nuestro archivo propio porque si no, no funcionara de la manera en la que queremos, este archivo, tendra ademas del JS que nosotros queramos añadir a la pagina web, la siguiente funcion constructora, aunque puede parecer una función anonima, no es el caso, ya que lo que hacemos es crear una nueva instancia de un objeto y no iniciar una función:

```
const swiper = new Swiper('.mySwiper-1', {
    loop: true,
    navigation: {
        nextEl: '.swiper-button-next',
        prevEl: '.swiper-button-prev',
    },

    pagination: {
        el: '.swiper-pagination',
        clickable: true,
    },

});
```

Deberemos de crear el objeto con el nombre de la clase que vamos a utilizar, en este caso mySwiper-1

Como se puede ver en el objeto, tenemos un valor llamado **loop**, que solo puede tener true o false, en caso de estar en true, el carrusel o slider, puede ir del primero al ultimo y del ultimo al primero, en caso de estar en false, tendrias que ir de 1 en 1, no podrias pasar del primero al ultimo o del ultimo al primero. Si tuvieramos un carrusel de 3 imagenes, en true podriamos pasar de la imagen 1 a la 3 directamente y viceversa, pero si esta en false, tendriamos que pasar de la 1 a la 2 y luego a la 3 y viceversa.

navigation es simplemente para añadir los botones de adelante y atras, esto esta marcado por nextEL y prevEL, el contenido que le añadamos a estos es la clase que tendremos que usar para que funcione.

pagination pondra iconos en la parte inferior del slider para mostrar la cantidad que hay, si ponemos clickable en true, podremos saltar al que queramos clickando, en caso de estar en false, tendremos que usar los botones obligatoriamente.

Este seria un codigo de ejemplo del html de dicho swiper:

```
<div class="swiper mySwiper-1">
                <div class="swiper-wrapper">
                
                    <div class="swiper-slide">
                        <div class="slaider">
                            <div class="slider-text">
                                <h1>Hamburguesa</h1>
                                <p>Lorem ipsum dolor sit amet consectetur adipisicing elit. Ducimus illo dolorum magnam quam reprehenderit error doloribus</p>
                                <div class="botones">
                                    <a class="btn-1" href="#">Comprar</a>
                                    <a class="btn-1" href="#">Menu</a>
                                </div>
                            </div>
                            <div class="slider-img">
                                <img src="../images/slider1.png" alt="">
                            </div>
                        </div>
                    </div>

                    <div class="swiper-slide">
                        <div class="slaider">
                            <div class="slider-text">
                                <h1>Burrito</h1>
                                <p>Lorem ipsum dolor sit amet consectetur adipisicing elit. Ducimus illo dolorum magnam quam reprehenderit error doloribus</p>
                                <div class="botones">
                                    <a class="btn-1" href="#">Comprar</a>
                                    <a class="btn-1" href="#">Menu</a>
                                </div>
                            </div>
                            <div class="slider-img">
                                <img src="../images/slider2.png" alt="">
                            </div>
                        </div>
                    </div>

                    <div class="swiper-slide">
                        <div class="slaider">
                            <div class="slider-text">
                                <h1>Hamburguesa</h1>
                                <p>Lorem ipsum dolor sit amet consectetur adipisicing elit. Ducimus illo dolorum magnam quam reprehenderit error doloribus</p>
                                <div class="botones">
                                    <a class="btn-1" href="#">Comprar</a>
                                    <a class="btn-1" href="#">Menu</a>
                                </div>
                            </div>
                            <div class="slider-img">
                                <img src="../images/slider3.png" alt="">
                            </div>
                        </div>
                    </div>

                </div>

                <div class="swiper-button-next"></div>
                <div class="swiper-button-prev"></div>
                <div class="swiper-pagination"></div>

</div>
```

De esta manera tendriamos un carrusel utilizando swiper (sin estilar) de manera sencilla.
