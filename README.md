# Prueba Técnica Data Analyst

En el presente reporte se muestra el analisis realizado a los datos entregados por Andesiete. Si bien acostumbro a trabajar utilizando Python, Pandas y Matplotlib, no necesariamente en Jupyter Notebooks, un Dashboard de Power BI facilita el la exploación de datos debido a las visualizaciones predefinidas.

## Supuestos

* Se nos pide elaborar un ranking de equipos en base en su eficiencia en el transporte de tonelaje. Para esto es necesaria una definición de eficiencia y hacer una comparación coherente entre datos o grupos de datos. Para esto consideraremos la eficiencia de pares caex-pala mediante la formula

		Eficiencia = Toneladas / (Tiempo)
  
	Entendiendo Tiempo como **truck_total_cycle**, asumiendo que este tiene sumado el tiempo de la pala (loader_total_cycle)

* Además, considerando que las condiciones en la mina pueden cambiar en el mediano plazo, se da énfasis a los datos mas recientes (**Mayo 2024**) por sobre la totalidad de datos históricos, puesto que algunas condiciones de ambiente muy antiguas, como Enero 2023 podrían haber desaparecido o cambiado. Sin embargo tenemos en cuenta que la información histórica proporciona un contexto general de los datos

## Ranking eficiencia

Haciendo uso del dashboard de Power BI para el análisis preliminar de la palas notamos que:

#### Palas
* Al analizar todos los datos disponibles, se observa una proporción inversa entre cantidad de cargas y tiempo de ciclo, lo que sugiere que mientras más tarda una pala en un día, menos cargas hará
*  En el periodo de tiempo seleccionado, la pala que más toneladas cargó fue PH48, además de tener un alto número de cargas, lo que sugiere un mayor tiempo de operación. Además tiene un menor tiempo de carga promedio y un menor promedio de paladas por carga, lo cual indica una mayor eficiencia.
* En el periodo seleccionado, la pala con mayor número de cargas fue PH58, sin embargo no es la que más toneladas cargó, lo que sugiere que esta pala podría estar operando de forma ineficiente.
* La pala con una mayor carga promedio es PH06, pero tambien notamos, no es la más eficiente en términos de número de paladas promedio.

#### CAEX

* En la ventana de tiempo global, se observa que la tendencia es un aumento lineal en el tiempo de camión según la distancia, lo cual sugiere que la velocidad de los CAEX es similar. Por otro lado se observa que, a mayor distancia total, mayor número de cargas, y por ende mayor cantidad de toneladas. 
* Adicionalmente notamos que PH55 es la pala en promedio más cercana, mientras que PH58 es la pala más lejana. Tomando como indicador la suma de la distancia llena y vacía.
* Finalmente notamos que se repiten algunos camiones con mayor cantidad de toneladas para distintas palas. Historicamente, el CAEX 66 es el más eficiente según esta métrica, por lo que podríamos suponer que es, de alguna manera, más eficiente que los otros CAEX.


#### Pares Pala Caex

Por último, consideramos la métrica de eficiencia definida anteriormente para los pares pala-caex. Tenemos que para la ventana seleccionada el ranking es:

1. PH06-CAEX 13 con 0.23 [ton/s] y 3.16 paladas en promedio por carga
2. PH06-CAEX 39 con 0.22 [ton/s] y 3.22 paladas en promedio por carga
3. PH06-CAEX 62 con 0.22 [ton/s] y 3.25 paladas en promedio por carga


## Factores Críticos

Del análisis realizado anteriormente podemos identificar los siguientes factores críticos:

* Número de palas: Se observa que los días con mayor total de carga son aquellos con 4 palas funcionando. Esto tiene sentido, pues si falta una pala y el número de camiones se mantiene constante, se podrían generar colas en las palas restantes y/o los CAEX tendrían que recorrer una mayor distancia promedio por carga.
* Paladas vs carga promedio: Si bien una pala que carga los camiones con un menor número de paladas será a priori más eficiente, si las cargas no son próximas a la "capacidad máxima" del CAEX, la eficiencia se ve acotada, pudiendo darse el escenario en donde es preferible una pala que tarde más en cargar los camiones, pero los cargue "más llenos", como ocurre en la ventana seleccionada con las palas PH48 y PH06. 
* Eficiencia palas: Se cumple que los pares pala-caex más eficientes están conformados en su mayoría por la pala más eficiente, lo cual indica que la eficiencia de la pala es determinante. En el caso de PH06, no solamente carga más en promedio, sino que también está más cerca.
* Por otro lado, los CAEX con menores tiempos promedio por carga no configuran necesariamente los pares pala-caex más eficientes. No obstante, es importante tomar en cuenta que existen camiones con mayor tiempo de operación y todos tienen una distribución distinta de palas en donde cargan.
* Notamos una tendencia entre una menor distancia total y una mayor eficiencia. Esto tiene sentido, pues manteniendo la velocidad constante, una menor distancia implica menor tiempo de viaje.

## Recomendaciones/Trabajo futuro

En base a los resultados obtenidos anteriormente se recomienda:
* Reducir al mínimo posible el tiempo en el cual una pala deja de funcionar 
* Optimizar la distribución de CAEX y palas, esto con el fin de evitar que se formen filas de CAEX en una misma pala o enviar un CAEX a una pala lejana si hay una más cerca disponible. Un approach interesante para esto podría ser la predicción de tiempos de carga de las palas utilizando modelos de Machine Learning.
* Mejorar la operación de palas y CAEX, para cumplir con prácticas eficientes, como que que las palas esperan el aculatamiento de los CAEX con el balde ya cargado.
* Relación entre número de cargas y eficiencia: Observamos que, de aumentar la eficiencia mediante la reducción de tiempos de carga y/o viaje, esto permitiría aumentar el número de cargas, sin embargo, es posible que estas nuevas cargas sean ineficientes, lo cual a su vez reduciría la eficiencia, de modo que, dependiendo de los KPI definidos, aumentar la eficiencia podría no ser tan simple como aumentar tiempos de operación. Un ejercicio interesante sería realizar un análisis de sensibilidad entre número de cargas y eficiencia.
