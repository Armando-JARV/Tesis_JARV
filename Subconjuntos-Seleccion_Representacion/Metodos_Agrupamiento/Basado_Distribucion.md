# Basado en distribución  
**Clustering basado en distribución**

Llamados también agrupamiento probabilístico, agrupa los datos basándose en su distribución probabilística. Este algoritmo supone que todos los datos son parte de un cluster, basándose en la probabilidad de que pertenezcan a un cluster dado. Funciona de manera que, del punto central, al aumentar la distancia del centro la probabilidad de que pertenezca al cluster disminuye. Los clusters se definen como grupos de datos que son más afines a tener la misma distribución antes que otra. El cálculo de ajuste de distribución puede ser costoso computacionalmente al trabajar con grandes conjuntos de datos.

## Métodos

**Método Gaussian Mixture Model (GMM):**  
Supone que los datos de un conjunto de datos son generados de una mezcla de diversas distribuciones gaussianas. Cada una de estas distribuciones gaussianas representa un cluster diferente. Es fundamental la elección del número de distribuciones para obtener resultados óptimos.


**Imagen 1. Clustering de cuatro entidades mínimas por cluster.  Donde el núcleo tiene cuatro vecinos dentro de la distancia de búsqueda incluyéndose a sí mismo. El punto de borde solo posee tres entidades dentro de la distancia establecida, pero al ser vecino del núcleo, se incluye dentro del clúster. El punto de ruido no posee cuatro entidades de búsqueda y no es vecino del núcleo, por lo que no se incluye dentro de un cluster.**

## Para saber más:

- Cota S. *Fundamental tasks of AI — part 3 — clustering (2 of 2).* In: Medium [Internet]. 7 Dec 2023 [cited 4 Jun 2024]. Available: [https://medium.com/@sasirekharameshkumar/deep-learning-basics-part-9-using-ai-for-clustering-2-of-2-d687b952a265](https://medium.com/@sasirekharameshkumar/deep-learning-basics-part-9-using-ai-for-clustering-2-of-2-d687b952a265)
