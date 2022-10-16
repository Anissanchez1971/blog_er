---
title: Pleno empleo por género
author: Alex Bajaña
        Andrea Díaz 
date: "2022-09-19"
slug: index.en-us
categories: []
tags:
  - indicador de agua
  - código R
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


En el año 2010 la Asamblea General de las Naciones Unidas reconoció el derecho al agua y al saneamiento como un elemento esencial en el derecho a la salud. El derecho al agua implica, básicamente, que el abastecimiento de agua debe ser suficiente, debe ser saludable y estar libre de contaminación, productos químicos o sustancias peligrosas para la vida humana y  debe ser fácilmente accesible debido a que es importante para la salud pública, tanto si se utiliza para beber, para uso doméstico, para producir alimentos o para fines recreativos ( Más información <https://www.who.int/es/news-room/fact-sheets/detail/drinking-water>)

En este caso el Ecuador y tratando de cumplir las metas 6.1 y 6.2 planteadas en el Objetivo 6 de los ODS, cuenta con una encuesta de Agua, Saneamiento e Higiene (ASH), que muestra los indicadores ODS de Agua, Saneamiento e Higiene los cuales constituyen una herramienta primordial en la planificación, diseño, seguimiento y evaluación de la política púbica.  .(<https://www.ecuadorencifras.gob.ec/indicadores-ods-agua-saneamiento-e-higiene/>)(<https://www.investig-arte.com/infografias/indicadores-ods-de-agua-saneamiento-e-higiene-en-ecuador-2019/>)
 

Las cifras sobre el porcentaje de la población con agua segura en la fuente a nivel nacional muestran que el 67,8% de la población utiliza como suministro para beber una fuente tipo A (tubería, pozo o manantial protegido o agua embotellada), en la vivienda, de manera suficiente y libre de contaminación fecal.

![](/posts/2022-09-19-indicador-encuesta-agua/index.en-us_files/ind_agua_segura_nacional.png)

Principales resultados sobre los indicadores de agua

<https://www.ecuadorencifras.gob.ec/documentos/web-inec/EMPLEO/2019/Indicadores%20ODS%20Agua%2C%20Saneamiento%20e%20Higiene-2019/3.%20Principales%20resultados%20indicadores%20ASH%202019.pdf>


A continuación veremos que se pueden replicar estos resultados en R.Como ejemplo se replicará el porcentaje de hogares que utiliza suministros seguros de agua para beber, adicionalmente, se calcula el indicador de agua cuando el jefe de hogar es hombre o mujer:

```{r}
       
# Instalación de librerías ------------------------------------------------
       
library(haven)
library(tidyverse)



# Lectura de la base de datos ---------------------------------------------

tabla_agua <- read_dta("publicacion_1/agua_ash2019.dta")


# Visualización de los datos ----------------------------------------------

glimpse(tabla_agua)


# Verificar el tipo de datos ----------------------------------------------

class(tabla_agua)



# Mostrar los atributos de las variables ----------------------------------

diccionario <- map(.x = tabla_agua,
                   .f = attributes)


# Información sobre diccionario -------------------------------------------

str(diccionario)



# Etiquetas de las variables ----------------------------------------------

diccionario_variables <- map(.x = diccionario,
                            .f = function(x){
                              x [["label"]]
                            }) %>% 
  unlist %>% 
  enframe




# Categorías de cada variable ---------------------------------------------

diccionario_categorias <- map(.x = diccionario,
                              .f = function(x){
                                x [["labels"]]
                              }) %>% 
  map(enframe) %>% 
  imap(.x = ., .f = function(tabla, nombre){
    tabla %>% 
      mutate(variable = nombre)
  }) 


# Escribir archivos rds con un comentario úti -----------------------------

write_rds(x = diccionario_variables, file = "diccionario_variables_agua.rds" )
write_rds(x = diccionario_categorias, file = "diccionario_categorias_agua.rds")





# Construcción de indicadores de agua, saneamiento e higiene (ASH) --------


# Indicador 6.1.1 ---------------------------------------------------------
# Porcentaje de hogares que utiliza suministros seguros de agua para beber

# Componentes del indicador: 1) tipo de suministro
#                            2) calidad del agua
#                            3) cercanía del suministro
#                            4) suficiencia de agua para beber

##############################################################################
#                                FUENTE                                      #
##############################################################################


indicador_6_1 <- 
  tabla_agua %>% 
  # select( vi16, vi17, vi26, 
  #         ra01e_f, ra03_f, ra04_f, ra022_f,
  #         ra023_f, vi17a, vi17b, vi17c) %>% 
  mutate(
    # Tipo de suministro
    tuberia = if_else(condition = vi16 <= 3,
                      true = 1, 
                      false = 2),
    agua_cp1 = case_when(vi17 %in% c(1:3,7,9)   ~ 1,   #tipo A
                         vi17 %in% c(4,8,10,12) ~ 2,   #tipo B
                         vi17 %in% c(11,13)     ~ 3),  #tipo C
    agua_cp1 = case_when(
      vi17 %in% c(5,6) & tuberia == 1 ~ 1,
      vi17 %in% c(5,6) & tuberia == 2 ~ 2,
      TRUE ~ agua_cp1
      ),
    
    
# 2.- Calidad del agua -libre de contaminación fecal (bacteria E. coli)
    #    ra01e_f : Horas de incubación entre 24 y 48 horas
    #    ra022_f: La muestra de agua tiene coloración amarilla?
    #    ra023_f: La muestra se hizo fluorescente al exponerse a la luz UV?
    #    ra03_f: Hubo una pérdida de más del 10 % (10ml) del contenido de la 
    #            muestra de agua?
    #    ra04_f: Estuvo la muestra expuesta a temperaturas menores a 30 grados o 
    #            mayores a 40 grados por más de 4 horas? 
    
# Se identifican pruebas válidas fuente
    
validtest_f = if_else(vi26 == 1 & !is.na(ra01e_f), 0, NA_real_),
validtest_f = case_when((between(ra01e_f, left = 24, right = 48) & ra03_f == 2 & ra04_f == 2) ~ 1,
                        vi26 == 2 ~ 2,
                        TRUE ~ validtest_f),
    
# Presencia de coliformes fuente

coli_f = if_else( validtest_f == 1, 0, NA_real_),
coli_f = case_when(
  validtest_f == 1 & ra022_f == 1 ~ 1,
  validtest_f == 1 & ra022_f == 2 ~ 2,
  validtest_f == 1 ~ 0),
    
# Ausencia de E. coli fuente
    
agua_cp2_f = if_else(validtest_f == 1, 0, NA_real_),
agua_cp2_f = case_when(
  validtest_f == 1 & (ra023_f == 2 | ra022_f == 2) ~ 1,
  validtest_f == 1 & ra023_f == 1  ~ 2,
  TRUE ~ agua_cp2_f
  ),
    

#3.- Cercanía del suministro

agua_cp3 = case_when(vi17a %in% c(1, 2) ~ 1,
                     vi17 %in% c(5, 6) ~ 1,
# Supuesto: el agua embotellada está cerca para el consumo
                     vi17a == 3 & vi17b <= 30 ~ 2,
                     vi17a == 3 & vi17b > 30 ~ 3,
# Se  encuentra fuera de la vivienda, edifico/lote y no sabe a cuanto se encuentra 
#(en minutos) la fuente 
                     vi17b == 999 & (vi17a != 1 | vi17a != 2) ~ NA_real_,
                     TRUE ~ 0),

#4.- Suficiencia de agua para beber
    
agua_cp4 = case_when(vi17c == 1 ~ 1,
                     vi17c > 1 ~ 2,
                     vi17c == 3 ~ NA_real_,
                     TRUE ~ 0)
)


# Corrección indicador de agua: -------------------------------------------


indicador_6_1 <- indicador_6_1 %>%
  mutate(
   i_agua_c = case_when(
      # manejo seguro
      agua_cp1 == 1 & agua_cp2_f == 1 & agua_cp3 == 1 & agua_cp4 == 1 ~ 1,
      # básico 1
      agua_cp1 == 1 & agua_cp2_f == 1 & agua_cp3 == 1 & agua_cp4 == 2 ~ 2,
      agua_cp1 == 1 & agua_cp2_f == 1 & agua_cp3 == 2 ~ 2,
      #básico 2
      agua_cp1 == 1 & agua_cp2_f == 2 & agua_cp3 == 1 ~ 3,
      agua_cp1 == 1 & agua_cp2_f == 2 & agua_cp3 == 2 ~ 3,
      # limitado
      agua_cp1 == 1 & agua_cp3 == 3 ~ 4,
      # no mejorado
      agua_cp1 == 2 ~ 5,
      # superficial
      agua_cp1 == 3 ~ 6),
   
   )



# Comparación del indicador generado por INEC y el replicado por el equipo 
#ERGOSTATS:
       
indicador_agua <- indicador_6_1 %>% 
  group_by(i_agua,i_agua_c) %>% 
  summarise(n = sum(fexpper))

indicador_agua %>%
  ungroup() %>% 
  mutate(total1 = sum(n),
         prop1 = (n/total1) * 100)




 
       ####################################
       # Revisar las condiciones 1, 2 y 3 #
       ####################################
       

       
# Calculo de la proporción de hogares por sexo del jefe de hogar -------------------
       
       
       tabla_agua %>% 
         filter(p04 == 1) %>% 
         
         # Transofrmación variable por variable, con una misma función:
         
         # mutate(p02 = haven::as_factor(p02),
         #        i_agua = haven::as_factor(i_agua)) %>% 
         # 
         # Transofrmación variable por variable, con una misma función:
         
         mutate(across(c(p02,i_agua),haven::as_factor)) %>% 
         
         group_by(p02,i_agua) %>%
         summarise(total_jefes = sum(fexpper)) %>% 
         pivot_wider(names_from = p02,
                     values_from = total_jefes) %>%  
         mutate(
           across(c(hombre,mujer),~replace_na(.x,0)),
           total = hombre + mujer,
           # Transformar a una sintaxis con across:
           prop_mujeres = (mujer/total)*100,
           prop_hombres = (hombre/total)*100
         )
       
       

#indicador_agua <- indicador_6_1 %>% 
 # group_by(i_agua,i_agua_c) %>% 
  #summarise(n = sum(fexpper))

indicador_agua %>%
  ungroup() %>% 
  mutate(total = sum(n),
         prop = n/total)

```


