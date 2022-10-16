---
title: 
author: Alex Bajaña
        Andrea Díaz 
date: "2022-08-30"
slug: index.en-us
categories: []
tags:
  - UPM
layout: post
math: no
---

<style>
body {
text-align: justify}
</style>


<!-- ::: -->

<!-- ::: {} -->

##Generalidades

¿Cómo se aglomeran las personas y cómo podemos realizar un diseño de muestreo con base en esta forma de aglomeración? 
En primer lugar, las personas se aglomeran en hogares, los cuales a su vez se aglomeran en comunidades más grandes: barrios, comunas, segmentos. Estas comunidades forman ciudades, veredas, centro poblados, etc. y la reunión de estas divisiones da como resultado el conjunto completo de unidades de interés en el país.
<!-- ::: -->

<!-- ::: {} -->
Ahora bien, ¿cómo se puede representar la realidad de todo un país.? Pues…si se selecciona de forma probabilística una muestra de sectores y dentro de cada sector se selecciona de forma probabilística una muestra de hogares, entonces de forma indirecta estaremos seleccionando una muestra de hogares que puede representar la realidad de todo un país.

![](/posts/2022-08-30-UPM/index.en-us_files/q_es_upm.png)
<!-- ::: -->

<!-- ::: {} -->
Los países de latinoamérica muchas veces realizan censos cada diez años siguiendo recomendaciones internacionales. Los censos poseen información sobre las viviendas, hogares, los habitantes y se observan otras variables de interés, como la educación, la orientación sexual, la posesión de servicios básicos, que servirán para la creación de políticas públicas y la comparación de las cifras en los siguientes diez años.
<!-- ::: -->

<!-- ::: {} -->
 Los Institutos Nacionales de Estadística (INE) latinoamericanos utilizan las particiones geográficas y cartográficas generadas en el levantamiento del censo con el fin de seleccionar, mediante diseños en varias etapas, muestras de hogares. Comúnmente, estas particiones reciben el nombre de secciones cartográficas y están formadas por un número determinado de hogares contiguos. Estas particiones no son más que las  unidades primarias de muestreo (UPM).


<!-- ::: -->

<!-- ::: {} -->


## En la ENEMDU, ¿Qué rayos es la UPM?

<!-- :::: {style="display: flex;"} -->

<!-- ::: {} -->


El muestreo de la ENEMDU es probabilístico y bi-etápico , donde la Unidad Primaria de Muestreo (UPM) es el conglomerado y la Unidad Secundaria de Muestreo (USM) son las viviendas ocupadas. Se considera a la UPM, la agrupación de viviendas ocupadas que tienen actualmente los conglomerados y van de 30 a 60 viviendas ocupadas, tanto para el área amanzanada como para el área dispersa, próximas entre sí y con límites definidos.

### Estructura del código de identificación del UPM 

El número de identificación de la UPM (conglomerado) se define con cuatro variables y está formado por 12 dígitos de la siguiente manera:

![](/posts/2022-08-30-UPM/index.en-us_files/codigo_UPM.png)


**Fuente:** INEC

<!-- ::: -->

<!-- ::: {} -->

Es normal que tras el levantamiento de un nuevo censo se actualice el marco de muestreo con el que se seleccionarán las viviendas y hogares para todas las encuestas subsiguientes. Por la naturaleza de los censos, los Institutos nacionales de estadísticas deben recorrer la geografía de los países produciendo una nueva cartografía que derivará en la actualización de los marcos de muestreo.
Kish en su libro sobre Diseño estadístico para la investigación de 1995, en el capítulo 5, sobre las Muestras y Censos(pág., 181, ver Kish - Diseño Estadístico para La Investigación | .PDF (scribd.com)), afirma que la selección de UPM con tamaño desigual acarrea algunos problemas técnicos como que el tamaño de muestra final se convierte en una variable aleatoria, que depende de la probabilidad de selección de las UPM más grandes o más pequeñas. Lo anterior aumenta la incertidumbre en el costo final del operativo, pues si en una primera instancia se seleccionan UPM con pocas viviendas, será necesario volver a realizar un proceso adicional de selección de nuevas UPM para cumplir con la cuota de viviendas.
Por eso es recomendable que la  actualización de la cartografía y de los marcos de muestreo se realice mínimo cada diez años.


<!-- ::: -->

<!-- ::: {} -->



Por ende, el diseño vigente, desde 2017, contempla una actualización dentro de la construcción de la Unidad Primaria de Muestreo (UPM). Para años anteriores, se realizaba la selección de sectores censales bajo un criterio operativo para la ejecución y levantamiento de información del Censo de Población y Vivienda. Sin embargo, en función al crecimiento y disminución de la población en ciertas áreas geográficas a través del tiempo, estas UPM’s pasaron a ser heterogéneas en cuanto al número de viviendas ocupadas que tienen dentro de sus límites, generando así, probabilidades de selección inadecuadas en la segunda etapa. Esta heterogeneidad se solventa al reconstruir y equilibrar el tamaño de las UPM con respecto al número de viviendas ocupadas con el objetivo de obtener conglomerados que cumplan con las necesidades del diseño muestral. Este procedimiento contó con el acompañamiento técnico de KOSTAT y CEPAL

### Referencia:

<https://www.ecuadorencifras.gob.ec/diseno-muestral-2/#:~:text=El%20muestreo%20de%20la%20ENEMDU,Primaria%20de%20Muestreo%20(UPM)>


<https://www.ecuadorencifras.gob.ec/documentos/web-inec/EMPLEO/2018/Disenio_Muestral_2018/SIEH%20-MMM.pdf>

<https://www.ecuadorencifras.gob.ec/documentos/web-inec/Multiproposito/201812_Calculo_de_errores_estandar_y_declaracion_de_muestras_complejas_Multi.pdf>

Enlace que te puede interesar:
<https://psirusteam.github.io/HHS-Handbook/>

