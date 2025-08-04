# Basado en centroides  
**Clustering basado en centroides (Centroid-based)**

Este método se centra en la partición o división del conjunto de datos en grupos de datos similares, conocidos como clusters. Se basa en la distancia que tienen los datos respecto al centro del grupo. La distancia a cada centro (cluster's centroid), se refiere a la media o mediana de todos los puntos del grupo, según sean los datos [1][3]. Ver figura.

**Imagen 1. Agrupamiento basado en centroide (Centroid-based clustering)**

Este tipo de algoritmo es ideal para conjuntos de datos que sean fácilmente separables, con clusters bien definidos. Es apropiado cuando los clusters son conocidos o pueden ser rápidamente estimados. Sin embargo, no es la mejor opción cuando hay solapamiento de los clusters o estos no poseen formas uniformes. 

## Métodos  
**Tipos de algoritmos basado en centroides**

**K-Means:**  
Es el más empleado para este método, ya que minimiza la suma de las distancias entre los datos y sus correspondientes centroides. Trabaja de manera adecuada con clusters previamente definidos, de tamaño relativamente similar, y que no posean outliers. Es especialmente sensible a los outliers, debido a que estos provocan un sobreajuste sobre los centroides al tratar de considerarlos.

**K-Medoids:**  
Variación de K-means, que utiliza medoides. Los medoides son datos representativos del conjunto de datos, donde en lugar de poseer un centroide de manera arbitraria como el centro del cluster, el algoritmo crea clusters empleando datos individuales como el centro del cluster (medoide). Debido al uso de medoides en lugar de centroides, es menos sensible a outliers.

**Fuzzy c-Means Clustering:**  
Variación de K-means que permite a los datos pertenecer a más de un cluster, con distintos grados de pertenencia. 

**Expectation Maximization (EM):**  
Modelo que emplea modelos estadísticos para definir las relaciones entre los datos y los clusters.

## Aplicaciones

Las aplicaciones para este tipo de algoritmo van desde la segmentación de imágenes, empleando el algoritmo K-means. Estas imágenes se pueden dividir en múltiples segmentos basándose en color, texto o alguna otra característica, hasta la detección de anomalías al identificar datos diferentes por medio de K-medoids.

## Para saber más:

- Verma A. *Centroid-based clustering: A powerful machine learning technique for partitioning datasets.* In: DEV Community [Internet]. 7 Feb 2023 [cited 4 Jun 2024]. Available: [https://dev.to/anurag629/centroid-based-clustering-a-powerful-machine-learning-technique-for-partitioning-datasets-41im](https://dev.to/anurag629/centroid-based-clustering-a-powerful-machine-learning-technique-for-partitioning-datasets-41im)
