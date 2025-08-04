### Basado en rejillado

**Clustering basado en rejillado (Grid-based clustering)**

Este método se centra en dividir el espacio del conjunto de datos en una estructura de rejilla. El agrupamiento se lleva a cabo hacia las celdas de la rejilla en lugar de los puntos de datos. Gracias a esto presenta como principal ventaja ante otros métodos, un mejor desempeño en el tiempo de procesamiento \[1]. \[8]

#### Método

**Método STING**, del inglés “STatistical INformation Grid”, es un algoritmo que divide el espacio en celdas rectangulares y en varios niveles de celda con diferentes niveles de resolución. Es decir, usa una estructura de rejilla multidimensional, cuantificando el espacio en un número finito de celdas.

**Imagen 6.** *Método de agrupación STING. Donde para cada celda, el nivel superior se divide en varias celdas más pequeñas para el siguiente nivel inferior* \[2] \[9].

Las ventajas del clustering basado en rejillado son:

* Al ser un método basado en cuadrícula, es **query-independent**, ya que la información estadística de cada celda representa un resumen de la información de los datos que se encuentran en esta, permitiendo hacer consultas independientes.
* La estructura de cuadrícula facilita el **procesamiento en paralelo** y las **actualizaciones incrementales**.

A pesar de esto, presenta como principal desventaja, que los **límites de cluster son horizontales o verticales**, no puede detectar límites diagonales.

#### Para saber más:

* Improve IF. *STING - Statistical Information Grid in Data Mining*. In: GeeksforGeeks \[Internet]. 2 Apr 2022 \[cited 4 Jun 2024]. Available: [https://www.geeksforgeeks.org/sting-statistical-information-grid-in-data-mining/](https://www.geeksforgeeks.org/sting-statistical-information-grid-in-data-mining/)
* *Grid-Based Clustering - STING, WaveCluster & CLIQUE*. Blogger; 6 Apr 2020 \[cited 4 Jun 2024]. Available: [https://www.datamining365.com/2020/04/grid-based-clustering.html](https://www.datamining365.com/2020/04/grid-based-clustering.html)
