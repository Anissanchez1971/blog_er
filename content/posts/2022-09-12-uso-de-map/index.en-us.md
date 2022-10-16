---
title: Uso de map en Rstudio
author: Andrea Díaz
date: '2022-09-12'
slug: index.en-us
categories: []
tags:
  - uso de map
layout: post
math: no
---


<!-- :::: {style="display: flex;"} -->

<!-- ::: {} -->





## Función maps

Devuelve una lista en columnas, estableciendo como argumentos: 

map(.x, .f, ...)

x = vectores;

f = fórmula o función, si es un vector de caracteres, un vector numérico o una lista, se convierte en una función extractora. 

En nuestro caso fue útil para crear una listas de  variables(diciconario de variables) y una lista de etiquetas(diccionario de etiquetas). Por tanto, permitió extraer información más detallada sobre las variables tomadas en cuenta acerca de los indicadores ODS de Agua, Saneamiento e Higiene. 



Más información sobre las diferentes funciones de map y su uso en R
<https://www.rdocumentation.org/packages/purrr/versions/0.3.5/topics/map>