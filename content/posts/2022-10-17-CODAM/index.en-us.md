---
title: Media muestral y poblacional
author: Alex Bajaña
        Andrea Díaz 
date: "2022-10-12"
slug: index.en-us
categories: []
tags:
  - media muestral
  - media poblacional
layout: post
math: no
---

<style>
body {
text-align: justify}
</style>


<!-- ::: -->

<!-- ::: {} -->

<!-- ::: -->

<!-- ::: {} -->


## MEDIA MUESTRAL Y MEDIA POBLACIONAL

<!-- :::: {style="display: flex;"} -->

<!-- ::: {} -->

La media muestral es aquella media que se calcula sobre los valores de una muestra estadística, es decir, se calcula sobre una parte del conjunto de valores de una variable.

La media poblacional es aquella media que se calcula sobre una población estadística, es decir, sobre todos los valores de una variable. Por lo tanto, la media poblacional coincide con la esperanza matemática de la variable.

La media muestral se puede considerar prácticamente igual a la media poblacional si se conoce un número de datos suficientemente grande. Pero el valor de la media poblacional es muy difícil de obtener, ya que en la realidad raramente se conocen todos los valores de una distribución.


Factor de expansión (se utiliza en las encuestas por muestreo). De acuerdo a la teoría de muestreo el factor de expansión es la capacidad que tiene cadaindividuo seleccionado en una muestra probabilística para representar el universo en el cual estacontenido. Es decir, es la magnitud de representación que cada selección posee para describir una parte del universo de estudio.
También se entiende que, se interpreta como la cantidad de personas en la población, que representa una persona en la muestra.La estimación de un total dado para una variable se obtiene, primero, ponderando el valor de la variable en cada persona por su factor de expansión y luego, sumando todas las personas de la muestra.Factores de Expansión: para llevar la información de la muestra a niveles poblacionales.


## FUNCIONES A USAR

Para la Media muestral se usa las funciones group by y summarise.
Para la Media poblacional se usa la libreria srvyr, con muestras complejas.

## INTERPRETACIÓN

Para el análisis se usa la libreria survey en muestras complejas considerando la unidad primaria de análisis, los estrasos, los pesos y el factor de correción de las variables





## Referencias:

<https://anda.inec.gob.ec/anda/index.php/catalog/837/sampling#:~:text=Factores%20de%20expansi%C3%B3n%20El%20procedimiento%20de%20ponderaci%C3%B3n%20general,ajuste%20por%20no%20respuesta%20a%20nivel%20de%20UPM>
 