# Agrupamiento (Clustering)

El agrupamiento, también conocido como *clustering*, es una técnica de **aprendizaje no supervisado** que busca agrupar datos en grupos o *clusters* con características similares[^1].

---

![Figura 1. Representación de cómo el clustering puede ser empleado para identificar patrones en un conjunto de datos](Tesis_JARV/Subconjuntos-Seleccion_Representacion/Metodos_Agrupamiento/Figuras/Picture1.png)

Esta técnica, a través de algoritmos que calculan la similitud entre datos, los agrupa en función de su proximidad. Dependiendo del tipo de datos que se posean y del objetivo del análisis, pueden emplearse una gran variedad de algoritmos de agrupamiento. Algunos de los más conocidos son: **k-means**, **DBSCAN** y **agrupamiento jerárquico** (*hierarchical clustering*).

Agrupar los datos en *clusters* nos ayuda a entender relaciones no evidentes entre ellos, al identificar características o rasgos distintivos que comparten una o varias categorías. 

Además, el agrupamiento permite:

- Detectar anomalías observando datos con baja asociación a algún cluster (*outliers*).
- Reducir la complejidad de grandes conjuntos de datos al disminuir el número de variables[^2].

Las aplicaciones del agrupamiento en el análisis de datos son diversas: puede utilizarse en campos como el **marketing empresarial**, **biología** (clasificación de genes), **medicina**, entre otros.

---

## Algoritmos de Agrupamiento

![Figura 2. Algoritmos de agrupamiento](ruta/a/imagen2.png)

Así como existen diferentes algoritmos de agrupamiento, también hay diversas formas de definir qué es un *cluster*. Los modelos funcionan de manera distinta según:

- El tamaño del conjunto de datos,
- La rigidez en las categorías,
- El número de clusters requeridos.

Es posible que un algoritmo funcione bien con un tipo de datos y de forma deficiente con otro[^2].

A continuación, se enlistan las principales técnicas de agrupamiento:

- **Agrupamiento basado en centroides** (*Centroid-based clustering*)
- **Agrupamiento jerárquico** (*Hierarchical clustering*)
- **Agrupamiento basado en distribución** (*Distribution-based clustering*)
- **Agrupamiento basado en densidad** (*Density-based clustering*)
- **Agrupamiento basado en rejillado** (*Grid-based clustering*)

---

## Referencias
[^1]: MisApuntes P. Qué es el clustering y cómo funciona: guía completa. In: MisApuntes [Internet]. 9 Oct 2023 [cited 4 Jun 2024]. Available: https://misapuntesdedatascience.es/que-se-entiende-por-clustering/
[^2]: What is clustering? IBM. 21 Feb 2024 [cited 4 Jun 2024]. Available: https://www.ibm.com/topics/clustering

