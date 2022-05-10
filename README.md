# Encomiendas Ya!

![Envias Ya! cover](assets/logistica.jpg)

## Antes de empezar: algunos consejos

El enunciado tiene **mucha** información, van a necesitar leerlo varias veces. La sugerencia es que lo lean entero una vez (para tener una idea general) y luego vuelvan a consultarlo las veces que hagan falta.

En otras palabras: trabajen completando cada sección antes de pasar a la siguiente, con los tests que aseguran que funciona incluidos. Si al avanzar en las secciones les parece necesario refactorizar, adelante, van a tener los tests que garantizan que no rompieron nada. :smirk:

## El negocio

EncomiendasYa es una empresa líder en envíos a través de todo el país. Posee
sucursales en todo el territorio, incluyendo la Antártida e Islas del Atlántico Sur. Estos envíos se realizan mediante distintos medios de transporte, tanto aéreos como terrestres (cada uno con sus particularidades). EncomiendasYa se acerco a la facultad porque necesita herramientas fiables para el cálculo del costo de los envíos, historial de clientes y rastreo de envíos.

## 1. Las Encomiendas

El objetivo es desarrollar un modelo que permita registrar los envíos que maneja
EncomiendasYa y el movimiento de sus transportes, pudiendo obtener información relevante
para manejar el día a día de la organización.

Cada envío se genera cuando un cliente se presenta en una sucursal con los artículos que
desea enviar, indicando la sucursal de destino.

Cuando el envío llega a la sucursal destino, alguien designado por el cliente que hizo el
envío lo pasa a retirar.

La ubicación de cada sucursal está representada por un lugar, del que se conocen las
coordenadas en la forma de una posición (X,Y), y en qué tipo de region del pais está, continental o insular. Por ejemplo, Córdoba, Jujuy son de tipo "continental", Malvinas, Ushuaia y la Base Marambio están una región "insular".

La distancia entre 2 lugares se mide en kilómetros.

Para calcular la distancia entre 2 lugares se pueden usar las siguientes ecuaciones:
puntoA = (X1, Y1) , puntoB = (X2, Y2)

- distanciaTierra(puntoA,puntoB) = | X1 – X2 | + | Y1 – Y2 |
- distanciaDirecta(puntoA,puntoB) = √((X1-X2)2 + (Y1-Y2)2)

Hay artículos peligrosos y artículos no peligrosos. De todos se conoce su peso, largo,
ancho, alto y nombre.

A su vez, las sucursales tienen un conjunto de vehículos disponibles para realizar los transportes.

## 2. El viaje

Con los datos del cliente, los artículos a ser enviados, la sucursal de destino y la sucursal
donde se presenta el cliente (sucursal origen), se crea lo que en EncomiendasYa llaman
envío. Previamente se chequea si puede ser realizado.

Para que un envío se pueda realizar se debe cumplir que haya en la “sucursal origen” al
menos un transporte capaz de llevar el envío hasta la ubicación de la “sucursal destino”.

* Cada envío se asigna a algún transporte que está en la sucursal origen, que pueda
enviarlo.
* Para que un transporte pueda llevar un envío debe poder soportar el peso del
mismo (o sea, que su peso máximo sea mayor que el peso del envío + el peso de
los envíos que ya tiene asignados).
* También debe tener un volumen suficiente para el envío (análogo al peso)
* Todos los destinos de sus envíos deben tener la misma sucursal destino (es decir,
un transporte va a una única sucursal destino).
* La sucursal de destino debe poseer espacio físico para poder recibir el envío. Cada
sucursal posee un deposito (alto, ancho y profundidad) el cual podrá tener envíos
en espera para ser retirados. El espacio disponible será el volumen total menos la
cantidad de envíos que estén en la sucursal y menos los envíos que están viajando
hacia la sucursal.

Además, hay limitaciones específicas para algunos transportes, según lo que se indica más
abajo:

El peso de un envío es la suma del peso de los artículos que lo componen.

El volumen de un envío es la suma de los volúmenes de sus artículos más un porcentaje
de corrección de volumen del 5%.

## 3. Transportes

En algún momento, el transporte parte hacia la sucursal destino de los envíos que tiene
asignados. Nos interesa saber el kilometraje de todos los transportes, que aumenta según
los kilómetros recorridos llevando envíos.

EncomiendasYa usa transportes de estos tipos:

* Camión
  - El volumen se indica en forma particular para cada camión.
  - Velocidad promedio: 90km/h
  - Su peso máximo está determinado por el kilometraje.
  - desgastePorKilometraje = kilometraje / 100
  - Su peso máximo es 3000 Kg. – desgastePorKilometraje pero nunca es menor a 500 kg
  - Sólo se desplaza en la region continental.
  - Puede llevar artículos peligrosos.
  - Precio: $300/km

* Moto
  - Volumen fijo 1 m3
  - Su peso máximo está especificado para cada moto
  - No pueden hacer viajes de más de 400 km.
  - No puede salir de la continental ni llevar envíos peligrosos.
  - Velocidad promedio: 110km/h
  - Precio: El valor por kilómetro depende del peso según la cuenta kilos*80 $/km, pero se tomará como mínimo un valor de 500 $/km.
  - Esto teniendo en cuenta los kilos de cada envío en particular, no importa qué otros envíos pueda tener cargada la moto.

* Avión
  - Pueden viajar entre regiones.
  - Puede llevar artículos peligrosos, tienen un peso máximo de 10000 kg pero por ley no pueden llevar más de 10 artículos peligrosos juntos.
  - Su volumen máximo es 5000 m3
  - Velocidad promedio: 700km/h
  - Precio: $2000 por km para envíos de hasta 1500 kg, $2500 por km para envíos de más de 1500 kg.
  - Si bien técnicamente el avión no llega a la “sucursal destino” el costo del posterior traslado a la sucursal se considera despreciable.

* Barco
        Pueden viajar entre regiones.
        Puede llevar artículos peligrosos, tienen un peso máximo de 100000 kg pero por
        ley no pueden llevar más de 100 artículos peligrosos juntos.
        Su volumen máximo es 50000 m3
        Velocidad promedio: 60km/h
        Precio: $200 por km para envíos de hasta 1500 kg, $250 por km para envíos de
        más de 1500 kg.
        Si bien técnicamente tampoco llega a la “sucursal destino” el costo del posterior traslado a la sucursal también se considera despreciable.

Las velocidades no pueden cambiar, son fijas. 

## Tipos de encomienda

Algunas encomienda son más urgentes que otras por lo que EncomiendasYa ofrece 3 alternativas

- **Encomienda estándar**. Llega cuando llega. 
- **Encomienda prioritaria**. El cliente puede definir el tiempo máximo que puede tardar. Se le recarga un 50% del costo del envío.

## Requerimientos

1. Saber, para un transporte, cuál sería el costo de llevar un envío desde la sucursal de
origen hasta la sucursal de destino del mismo, y cuántas horas tardaría en llevarlo.  
2. Saber si un transporte puede llevar o no un envío, dados los requisitos de peso,
volumen, manejo de artículos peligrosos, capacidad de llegar del origen al destino (por
distancia y por la eventual necesidad de cruzar continentes), coherencia en los envíos
que se cargan al mismo transporte (deben dirigirse todos a la misma sucursal), y las
condiciones particulares de los envíos no standard (luxe, relajados e intimistas).
Misma aclaración que para el punto 1.  
3. Saber, para una sucursal, cuáles de los transportes que están en esa sucursal pueden
llevar un determinado envío.  
4. Registrar el ingreso de un nuevo envío en la sucursal origen, indicando: cliente,
artículos a incluir, sucursal destino, y persona que retirará el envío en la sucursal
destino.  
En el momento de registrarse, el envío se asigna al transporte que, dentro de los
vehículos que están en la sucursal origen y pueden llevar el envío, tenga el menor
costo para el envío.  

## Casos de prueba

Los siguientes obligatorios casos de prueba sirven para empezar a guiar los tests del
sistema. Bajo ningún concepto testean todas las posibilidades ni todos los casos de uso.

Se deben agregar entonces los casos de prueba que falten para poder testear los casos de
uso faltantes y aquellos que sean particulares del diseño.
Configuraciones generales para todos los tests:

Se poseen 3 sucursales, una en William Morris, una en Rosario y otra en Base Marambio (en
Antártida, insular).

- En la sucursal de William Morris exísten los siguientes vehículos: 1 Camión y 1 Avión
- En la sucursal de Rosario exísten los siguientes vehículos: 2 Camión, 1 Barco y 1 Moto
- En la sucursal de Base Marambio, Antártida exísten los siguientes vehículos: 2 Aviones, 1 Barco y 1 Camión

Todos los transportes están sin envíos asignados.

### Caso 1
    **Condiciones previas**: En la sucursal Morris esta cargado un envío en el avión para ir
    a Marambio y un envío en el Camión para ir a Rosario.
    **Caso de prueba**: Se piden los transportes disponibles en la sucursal de Morris para
    un envío nuevo que tenga como destino a Rosario.
    **Resultados**: Se debe obtener sólo al Camión como transporte disponible


### Caso 2

    **Condiciones previas**: Se crea un envío con 12 artículos peligrosos.
    **Caso de prueba**: Se piden los transportes disponibles para dicho envío en la sucursal de Morris.
    **Resultados**: Se debe obtener sólo al Camión como transporte disponible

### Caso 3

    **Condiciones previas**: Se carga a un avión de la sucursal Marambio un envío con un artículo de 1500 m³ y otro de 1700 m³ y destino Rosario.
    **Caso de prueba**: Se crea un envío con un artículo de 2000 m³ y se piden los transportes disponibles en la sucursal de Marambio.
    **Resultados**: Se debería obtener solamente al segundo avión o al barco como disponible.

## Licencia

Esta obra fue "recortada" por [Miguel Carboni](https://github.com/miguelius) y publicada bajo una [Licencia Creative Commons Atribución-CompartirIgual 4.0 Internacional][cc-by-sa].

[![CC BY-SA 4.0][cc-by-sa-image]][cc-by-sa]

[cc-by-sa]: https://creativecommons.org/licenses/by-sa/4.0/deed.es
[cc-by-sa-image]: https://licensebuttons.net/l/by-sa/4.0/88x31.png

### Créditos

:memo: [Enunciado original - PlanetExpress](http://tadp.wdfiles.com/local--files/tp-200802/Enunciado%20Entrega%201) creado por el equipo de TADP UTN del año 2008 compuesto por: Sergio Magnacco, Deby Fortini, Nico Passerini, Fer Dodino, Guille Polito, [Leonardo Gassman](https://github.com/lgassman) y Alfredo Sanzo y compartido generosamente por los términos de la licencia [![CC BY-SA 3.0][cc-by-sa-image]][cc-by-sa].

:camera_flash: Imagen de portada por Foto de [Marcin Jozwiak](https://unsplash.com/es/@marcinjozwiak) en [Unsplash](https://unsplash.com/es/fotos/oh0DITWoHi4#:~:text=Marcin%20Jozwiak%20en-,Unsplash,-Want%20to%20launch).
