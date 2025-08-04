# Agrupamiento Jerárquico  
**Hierarchical clustering**

También conocido como agrupación de clusters basado en conectividad, agrupa los puntos de datos en función de la proximidad y la conectividad de sus características. Este método determina los clusters basándose en qué tan cerca están datos unos de otros a través de todas las dimensiones. La idea es que los datos que estén más próximos unos de otros se encuentren más relacionados que los que se encuentran más alejados, asignándoles así una jerarquía. 

A diferencia de k-means, no se requiere de pre-especificar el número de clusters. En su lugar, el algoritmo crea una red gráfica de los clusters en cada nivel jerárquico. La forma de presentación más común es un dendrograma [1][4] (Ver Figura 2).

**Figura 3. Dendograma de Cluster**

## Métodos 

**Aglomerativo:**  
Se comienza definiendo cada punto de datos como un subgrupo o subcluster. Definimos una métrica para medir la distancia entre todos los pares de subclusters en cada paso y seguimos fusionando los dos subclusters más cercanos en cada paso. Se repite el procedimiento hasta que solo quede un cluster en el sistema. Tiene como desventaja que es más lento que otros métodos de jerarquía.

**División:**  
El algoritmo empleado para realizar este clustering divisivo es k-means. Donde se realiza en cada cluster el procedimiento de k-means, hasta encontrar todas las muestras de datos en el sistema o el número de clusters a obtener. Se debe conocer cuántos clusters se desean obtener a continuación. 

**Figura 4. Formación de cluster de división o aglomerativos. Donde cada punto representa un punto de datos. [2] [5]**

## Tabla 1. Ventajas y desventajas de agrupamiento jerárquico.

| Ventajas | Desventajas |
|---------|-------------|
| Facilita el trabajar con formas más complejas, ya que se posee una forma gráfica y visualmente atractiva para la visualización de los clusters. | Es un método fuertemente influenciado por heurísticas. |
| Es fácil definir el número de clusters, al agrupar los datos en diversos niveles jerárquicos. | El algoritmo matemático es pesado, debido a que se deben calcular las distancias entre los subclusters cada vez. |
| | Entré más aumente el tamaño de nuestro conjunto de datos, el analizar de manera visual se volverá más complejo. |

## Para saber más:

- *Hierarchical clustering: Visualization, feature importance and model selection.* Appl Soft Comput. 2023;141: 110303. doi: [10.1016/j.asoc.2023.110303](https://doi.org/10.1016/j.asoc.2023.110303)

- Pai P. *Hierarchical clustering explained.* In: Towards Data Science [Internet]. 7 May 2021 [cited 4 Jun 2024]. Available: [https://towardsdatascience.com/hierarchical-clustering-explained-e59b13846da8](https://towardsdatascience.com/hierarchical-clustering-explained-e59b13846da8)
