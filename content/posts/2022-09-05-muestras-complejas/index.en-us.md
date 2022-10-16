---
title: Muestras complejas
author: ''
date: '2022-09-05'
slug: index.en-us
categories: []
tags:
  - muestras complejas
layout: post
math: no
---

## Generalidades

Una población es un conjunto de elementos similares que son de interés de análisis. Una muestra es un subconjunto o parte del universo o población. y el muestreo es el procedimiento para obtener esa muestra. Como muchas veces se torna imposible analizar la población, la muestra se convierte en  un pilar fundamental para el análisis de la población. Es más, si esta muestra es aleatoria nos proporciona resultados más precisos y confiables sobre la población de interés.

<!-- ::: -->

<!-- ::: {} -->


Ahora bien, una muestra aleatoria simple muestra que los elementos se seleccionan aleatoriamente con la misma probabilidad y sin reposición (SR) directamente a partir de la totalidad de la población, por el contrario, una muestra compleja  tiene, además, ciertas características adicionales.

<!-- ::: -->

<!-- ::: {} -->


## ¿Qué características tiene una muestra compleja?

Las muestras complejas son las incorporan distintos elementos de los diseños muestrales básicos tales como :

Estratificación. El muestreo estratificado implica seleccionar muestras independientemente dentro de los subgrupos de la población que no se solapan, creando estratos. Los estratos pueden ser grupos socioeconómicos, categorías laborales, grupos de edad o grupos étnicos. 
Agrupación en clúster. El muestreo por clusters implica la selección de grupos de unidades muestrales o clústeres. Los clústeres pueden ser escuelas, hospitales o zonas geográficas y las unidades muestrales pueden ser alumnos, pacientes o ciudadanos.
Múltiples etapas. En el muestreo polietápico, se selecciona una muestra de primera etapa basada en clústeres. A continuación, se crea una muestra de segunda etapa extrayendo submuestras a partir de los clústeres seleccionados. Si la muestra de segunda etapa está basada en subclústeres, entonces, puede añadir una tercera etapa a la muestra. 
Muestreo no aleatorio. La selección de la muestra es según el juicio del equipo investigador puede ser por conveniencia, por cuestiones económicas, etc.
Probabilidades de selección desiguales. Cuando se muestrean clústeres que contienen números de unidades desiguales, puede utilizar el muestreo probabilístico proporcional al tamaño (PPS) para que la probabilidad de selección del clúster sea igual a la proporción de unidades que contiene. El muestreo PPS también puede utilizar esquemas de ponderación más generales para seleccionar unidades.
Muestreo no restringido. El muestreo no restringido selecciona las unidades con reposición (CR). Por lo tanto, se puede seleccionar más de una vez una unidad individual para la muestra.
Todo lo anterior implica que  la estimación de las varianzas de los estimadores  es bastante compleja. Por tanto, se trata de simplificar los estimadores usando ponderadores o “factores de expansión” de las unidades muestrales. Las ponderaciones de muestreo, de forma ideal se corresponden con la "frecuencia" que cada unidad muestral representa en la población objetivo.  Los procedimientos de análisis de muestras complejas requieren las ponderaciones de muestreo para poder analizar correctamente una muestra compleja. 

<!-- :::: -->

<!-- ::: {} -->


Se entiende entonces que, los diseños muestrales que incorporan combinaciones de estrategias alternativas de muestreo, como la estratificación, la selección en etapas, la formación de conglomerados o el empleo de probabilidades de selección desiguales se denominan complejos. El análisis de los datos obtenidos mediante diseños muestrales complejos puede resultar más complicado por la posible existencia de una correlación entre las observaciones de un mismo conglomerado.

En síntesis, el diseño de una encuesta es complejo si tiene: varias etapas de selección, probabilidad de selección desigual y estratificación
 
<!-- :::: -->

<!-- ::: {} -->

## Estimación de muestras complejas en R, usando la Encuesta Multipropósito 2019 sobre salud

El tamaño de muestra de la Encuesta Multipropósito 2020 fue calculado considerando los siguientes parámetros: margen de error relativo del 12%, nivel de confianza del 95%, tasa de no respuesta del 20%; y menciona los algoritmos que se utilizaron para el cálculo de los tamaños de muestra tanto de personas, como de viviendas y UPM.
Consultas en:
<https://www.ecuadorencifras.gob.ec/documentos/web-inec/Multiproposito/2020/202012_Metodologia%20Multiproposito.pdf>



Las muestras complejas en el software R se analizan con el paquete survey, donde, básicamente, en vez de tener que especificar en cada estimación las variables de peso o, en su caso, las correspondientes a cada uno de los conglomerados  o UPM´s, la librería survey, ligado a un objeto de datos, declara inicialmente las características técnicas de la encuesta. Entonces, se debe especificar las variables de estratos, conglomerados y pesos, para que, una vez el paquete reconozca a la encuesta, se pueden utilizar cualquiera de las técnicas de análisis y modelización que nos proporciona el paquete sin necesidad de re-especificar la parte técnica.

<!-- :::: -->

<!-- ::: {} -->

Esta herramienta posibilita la inclusión del tipo de ponderación adecuado (sampling weights, precision weights or frequency weights), unidades primarias de muestreo, estratos, efectos de diseño y demás características intrínsecas a varios tipos de muestreo. 

A continuación observamos el código utilizado en R para el análisis de la calidad de la  atención a pacientes en un centro de  salud pública.


### Código utilizado

```{r}

# Librerías ---------------------------------------------------------------

library(haven)
library(magrittr)
library(tidyverse)
library(srvyr)


# Lectura de la base de datos ---------------------------------------------

multibdd_salud <- read_sav("201912_multibdd_salud.sav")


# Diseño muestral ---------------------------------------------------------
#Se observa que utilizamos ´para el análisis de esta muestra compleja , los upm, estratos y pesos. 

svy <-  multibdd_salud %>%  
  as_survey_design(ids = upm, 
                   strata = estrato5,
                   weights = fexp5)


# Descripción de las nuevas variables a crear--------------------------------------

#caso 1 –> Hogares donde alguno de sus miembros tuvo alguna enfermedad o necesidad de atención de su salud y recibió atención.
#caso 2 -> Hogares donde alguno de sus miembros recibió tuvo una consulta de control
#proc_instal -> Trato en el establecimiento de salud (1: si, 2: no)
#servicio -> Calificación del servicio de salud
#espera -> tiempo de espera en horas y minutos
#x2 -> numerador del ratio
#y2 -> denominador del ratio




# Creación de nuevas variables --------------------------------------------

base1 <- svy %>% 
  mutate(caso1 = if_else(s102p1 == 1 & s102p2 <= 2, true = 1, false = 0 ),
         caso2 = if_else((s102p1 == 1 & s102p2 >= 3 & s102p4 == 1) | (s102p1 == 2 & s102p4 == 1), true = 1, false = 0),
         proc_instal = if_else((s102p91 == 1 & s102p92 == 1 & s102p93 == 1 & s102p94 == 1 & s102p95 == 1), true = 1, false = 0),
         servicio = if_else(s102p10 <= 2, true = 1, false = 0),
         espera = if_else(s102p6a == 0 & s102p6b <= 30, true = 1, false = 0),
         x2 = if_else((caso1 == 1 | caso2 == 1) & s102p5 <= 5 & proc_instal == 1  & servicio == 1 & espera == 1, true = 1, false = 0),
         y2 = if_else((caso1 == 1 | caso2 == 1) & s102p5 <= 5, true = 1, false = 0),
         ratio = x2/y2
  )


# Construcción del indicador atención en el centro de salud---------------------

base1 %>% 
  group_by(area) %>% 
  summarise(ratio = survey_mean(ratio))

survey::svyratio(~x2, ~y2, base1)                                          

```

<!-- ::: -->

<!-- ::: {} -->


El ratio calculado comprende: 

X2:ser atendido por enfermedad, accidente, dolor, etc.  y de ser curado en un centro de salud público o con medicina ancestral, además de recibir una atención respetuosa, a tiempo, con una explicación clara del diagnostico con buenas instalaciones, que brinde consejos para el cuidado de la salud y un tiempo de espera menor a 30 minutos lo que implicaría un buen servicio de salud; 

Y2:ser atendido en un centro de salud publico por accidente, dolor, etc.

Entonces, la probabilidad de que suceda el X2 frente a Y2 es de 38. 83%

CONCLUSIÓN: Los resultados nos muestran que existe una deficiencia en la calidad de atención en un centro de salud pública.



### Referencias:
<https://www.ibm.com/docs/es/spss-statistics/saas?topic=testing-complex-samples>
<https://rpubs.com/jcms2665/CS>
<https://scielosp.org/pdf/rpsp/2004.v15n3/176-184/es>
<https://rpubs.com/jaortega/EncuestaR2>
