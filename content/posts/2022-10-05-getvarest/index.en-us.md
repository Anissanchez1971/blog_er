---
title: Uso de get_var_est
author: Alex Bajaña
        Andrea Díaz 
date: "2022-10-05"
slug: index.en-us
categories: []
tags:
  - getvarest
layout: post
math: no
---

<style>
body {
text-align: justify}
</style>


<!-- ::: -->

<!-- ::: {} -->


## get_var_est: 

Permite obtener las estimaciones de la varianza en la variables de una encuesta. Esta función ayuda a convertir el resultado de una función de encuesta, en un dataframe con una estimación y medidas de varianza con parámetros predefinidos. La función se encuentra en el paquete srvyr. Primero se debe instalar y cargar el paquete srvyr antes de usarla.

Función en R:
```
get_var_est(
  stat,
  vartype,
  grps = "",
  level = 0.95,
  df = Inf,
  pre_calc_ci = FALSE,
  deff = FALSE
)
```

### Argumentos de la función

stat -> Objeto estadístico de la encuesta, donde se encuentran las variables que deseamos analizar.

vartype -> Un vector que indica qué estimaciones de varianza calcular (las opciones son: se para el error estándar, ci para el intervalo de confianza, var para la varianza o cv para el coeficiente de variación). Se permiten múltiples opciones.

grps -> Vector que indica los nombres de las variables de agrupación para encuestas agrupadas ("" indica que no hay grupos).

level -> Uno o más niveles para calcular un intervalo de confianza.

df -> grados de libertad, muchas funciones de encuesta tienen por defecto Inf, pero las funciones srvyr generalmente tienen por defecto el resultado de llamar a degf en el objeto de encuesta.

pre_calc_ci -> si el intervalo de confianza está precalculado (como en svyciprop)

deff -> Si se debe devolver el efecto de diseño (calculado usando la funci+on survey:: deff)

<!-- ::: -->

<!-- ::: {} -->


### Referencias:
<https://www.rdocumentation.org/packages/srvyr/versions/1.1.1/topics/get_var_est>


<!-- ::: -->

<!-- ::: {} -->



## prueba svychisq
Usado en tablas de contingencia (representa datos categóricos en términos de conteos de frecuencia) para datos de encuestas. La función calcula una tabulación cruzada ponderada. Esto es especialmente útil para producir gráficos. Es fácil de usar aunque también producen errores estándar, efectos de diseño, etc


Ejemplo:

```
data(api)
  xtabs(~sch.wide+stype, data=apipop)

  dclus1<-svydesign(id=~dnum, weights=~pw, data=apiclus1, fpc=~fpc)
  summary(dclus1)

  (tbl <- svytable(~sch.wide+stype, dclus1))
  plot(tbl)
  fourfoldplot(svytable(~sch.wide+comp.imp+stype,design=dclus1,round=TRUE), conf.level=0)

  svychisq(~sch.wide+stype, dclus1)
  summary(tbl, statistic="Chisq")
  svychisq(~sch.wide+stype, dclus1, statistic="adjWald")

  rclus1 <- as.svrepdesign(dclus1)
  summary(svytable(~sch.wide+stype, rclus1))
  svychisq(~sch.wide+stype, rclus1, statistic="adjWald")
  
```
  
  

<https://rdrr.io/rforge/survey/man/svychisq.html>


<!-- ::: -->

<!-- ::: {} -->

## surveytable
Nos muestra tablas de contingencia y pruebas chisquared de asociación para datos de encuestas.

Ejemplo de las diferentes usos de surveytable:


```
# S3 method for survey.design
svytable(formula, design, Ntotal = NULL, round = FALSE,...)
# S3 method for svyrep.design
svytable(formula, design, Ntotal = sum(weights(design, "sampling")), round = FALSE,...)
# S3 method for survey.design
svychisq(formula, design, 
   statistic = c("F",  "Chisq","Wald","adjWald","lincom","saddlepoint"),na.rm=TRUE,...)
# S3 method for svyrep.design
svychisq(formula, design, 
   statistic = c("F",  "Chisq","Wald","adjWald","lincom","saddlepoint"),na.rm=TRUE,...)
# S3 method for svytable
summary(object, 
   statistic = c("F","Chisq","Wald","adjWald","lincom","saddlepoint"),...)
degf(design, ...)
# S3 method for survey.design2
degf(design, ...)
# S3 method for svyrep.design
degf(design, tol=1e-5,...)

```

Argumentos de las diferentes funciones usadas:
fórmula: se escribe la fórmula del modelo que especifica los márgenes de la tabla

design es el diseño del objeto de la encuesta

statistic es la estadística de las pruebas tales como chi cuadrado, el test de wald, la prueba F, etc.

na.rm implica quitar valores faltantes, por lo general siempre es verdadero

Más información:

<https://www.rdocumentation.org/packages/survey/versions/4.1-1/topics/svytable>
<https://rpubs.com/corey_sparks/53683>




