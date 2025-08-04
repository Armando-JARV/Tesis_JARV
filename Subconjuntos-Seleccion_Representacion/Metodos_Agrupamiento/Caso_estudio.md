
# Introducción

## Fundamento teórico
La clasificación anatómica-terapeútica-química dada por la OMS es un sistema que nos permite clasificar los fármacos en función de su acción anatómica, terapéutica y química. Este sistema nos permite tener una mejor organización, y promueve mejorar la calidad del uso de los medicamentos, facilitando su análisis.

A través de métodos de agrupación de datos, como lo es el clustering, es posible identificar patrones y similitudes en sus propiedades químicas. Esto puede resultar especialmente útil en tareas de análisis de datos de grandes bases de datos farmacéuticas y la identificación de tendencias terapéuticas.

## Objetivo
Aprender a aplicar métodos de agrupamiento (clustering) para visualizar y analizar las relaciones entre diferentes códigos ATC, empleando herramientas computacionales y técnicas de visualización de datos.

---

## Materiales necesarios

1. Python 3.x  
2. Librerías:  
   - rdkit  
   - pandas  
   - sklearn  
   - scipy  
   - seaborn  
   - matplotlib  
   - numpy  
3. Archivo CSV con datos de las moléculas y códigos ATC.  
   *Nota:* El archivo se encuentra para descarga y consulta directa en el código.

---

## Índice

- Instalación de paqueterías  
- Carga de la base de datos  
- Clasificación ATC  
- Separar códigos ATC  
- Clasificar códigos ATC  
- Ordenar por grupo terapéutico  
- Descriptores Químicos  
- Calcular descriptores químicos de biodisponibilidad oral  
- Normalización de datos  
- Métodos de visualización  
- Clustering Jerárquico  
- Mapa de calor con clustering jerárquico  
- Gráficos de radar  
- Formas de agrupación  
- PCA  
- k-means  
- k-medoids  
- DBSCAN  
- HDBSCAN  
- Métricas de evaluación  
- Corrección de clusters  

---

## Metodología

Para el siguiente ejercicio, emplearemos una base de datos descargada de ChEMBL que contiene todos los compuestos asociados con al menos un código ATC, resultando en 3674 moléculas. Las columnas de esta base de datos nos informan la clasificación ATC, el año de primera aprobación, la máxima fase aprobada, el identificador dado por ChEMBL (ID), el tipo de molécula, el nombre preferido y los SMILES canónicos.

Emplearemos Google Colab para ejercicio.

---
[Código Google Colab](https://colab.research.google.com/drive/1ek91sXWxh6N2ah6Cv_JuW8XPym5Yfv3-?usp=sharing)
---

## Instalación de librerías

Para poder realizar la aplicación de métodos de agrupación y visualización de datos, debemos comenzar primero instalando las siguientes paqueterías:

```python
# rdkit
!pip install rdkit

# Importar funciones
import pandas as pd
from rdkit import Chem
from rdkit.Chem import Descriptors
from rdkit.Chem.Crippen import MolLogP
from rdkit.Chem.rdMolDescriptors import CalcTPSA
from sklearn.preprocessing import LabelEncoder
from scipy.cluster.hierarchy import dendrogram, linkage
import matplotlib.pyplot as plt
import seaborn as sns
import plotly.figure_factory as ff
from scipy.spatial.distance import pdist, squareform
import numpy as np
from sklearn.preprocessing import MinMaxScaler
from scipy.cluster.hierarchy import linkage, to_tree
````

---

## Carga de base de datos

Con las paqueterías necesarias instaladas en el notebook, haremos la carga de nuestra base de datos, este es un archivo `.csv`, que contiene los datos previamente descargada de ChEMBL.

```python
# Cargar la base de datos desde un archivo CSV compartido en Google
df = pd.read_csv('https://docs.google.com/spreadsheets/d/1OZolUkY3fxXZ64SjF1HSiyS7gjfRAp93wJtAIhrMpYo/export?format=csv')
df
```

Al ejecutar el código, obtendremos una tabla que nos mostrará el ID de cada molécula, su SMILES y su clasificación ATC.

*imagen*

---

## Clasificación ATC

Con la base de datos ya cargada en nuestro notebook, debemos limpiar y estandarizarla para poder trabajar con ella. Primero eliminaremos las moléculas sin clasificación ATC y/o que carezcan de SMILES canónicos.

```python
# Eliminar filas con lista vacía de código ATC
df = df[df['atc_classifications'] != '[]']
# Remover SMILES vacíos o nulos
df = df[df['canonical_smiles'].notna() & (df['canonical_smiles'] != '')]
```

Posteriormente, algunas moléculas pueden contener más de un código ATC, por lo que separamos las moléculas con múltiples códigos ATC en filas independientes.

```python
# Definir la función para expandir filas con múltiples códigos ATC en múltiples filas individuales
def expand_atc(df):
    new_rows = []
    # Iterar a través de cada fila del DataFrame
    for _, row in df.iterrows():  
        # Convertir cadena en lista de códigos ATC
        atc_list = eval(row['atc_classifications']) 
        for atc in atc_list:  # Crear una fila por cada código ATC
            new_row = row.copy()
            new_row['atc_classifications'] = atc
            new_rows.append(new_row)
    return pd.DataFrame(new_rows)  # Crear un nuevo DataFrame con las filas expandidas

# Aplicar la función
expanded_df = expand_atc(df)
# Reemplazar el DataFrame original con el expandido
df = expanded_df 
```

Ahora que tenemos nuestras moléculas con su único o múltiples códigos ATC separados, las clasificamos para obtener el tipo y el grupo terapéutico de cada código ATC en nuevas columnas.

```python
# Definir la función para obtener el tipo ATC (primer carácter)
def get_atp_type(atc):
    return atc[0] if pd.notna(atc) and len(atc) > 0 else None

# Definir la función para obtener el grupo terapéutico (primeros tres caracteres)
def get_g_therapeutic(atc):
    return atc[:3] if pd.notna(atc) and len(atc) >= 3 else None

# Aplicar las funciones para crear las nuevas columnas
df['ATC Type'] = df['atc_classifications'].apply(get_atp_type)  # Columna con tipo ATC
df['G Terapeutic'] = df['atc_classifications'].apply(get_g_therapeutic)  # Columna con grupo terapéutico
```

Reordenaremos la base de datos por grupo terapéutico para facilitar su análisis.

```python
# Ordenar el DataFrame por el grupo terapéutico (columna G Terapeutic)
df_sorted = df.sort_values(by='G Terapeutic')
```

Al finalizar con la limpieza y estandarización de la base de datos, esta debería lucir de esta forma:

*imagen*

---

## Descriptores químicos

Para evaluar nuestros datos y ver similitudes o patrones entre ellos, necesitamos información de estos. Por esto calcularemos descriptores químicos de relevancia farmacéutica:

* Peso molecular (MW)
* Coeficiente de partición octano/agua (LogP)
* Área superficial polar (TPSA)
* Número de enlaces rotables (NRB)
* Número de aceptores de hidrógeno (HBA)
* Número de donadores de hidrógeno (HBD)

---

## Cálculo de descriptores

```python
# Definir la función para calcular propiedades químicas básicas a partir de SMILES
def calculate_properties(smiles):
    mol = Chem.MolFromSmiles(smiles)
    if mol:
        # Calcular propiedades químicas
        mol_weight = Descriptors.MolWt(mol)  # Peso molecular
        logp = MolLogP(mol)  # Coeficiente de partición LogP
        tpsa = CalcTPSA(mol)  # Área superficial polar total (TPSA)
        nrb = Descriptors.NumRotatableBonds(mol)  # Número de enlaces rotables
        hba = Descriptors.NumHAcceptors(mol)  # Número de aceptores de hidrógeno
        hbd = Descriptors.NumHDonors(mol)  # Número de donadores de hidrógeno

        # Devolver propiedades como una serie
        return pd.Series([mol_weight, logp, tpsa, nrb, hba, hbd])
    else:
        # Si el SMILES no es válido, devolver valores nulos
        return pd.Series([None, None, None, None, None, None])

# Aplicar la función a cada fila del DataFrame para calcular propiedades
df_sorted[['MolWt', 'LogP', 'TPSA', 'nRB', 'HBA', 'HBD']] = df_sorted['canonical_smiles'].apply(calculate_properties)
```

Con esto ya poseemos los seis descriptores calculados para cada una de nuestras moléculas.

Ahora realizamos el promedio de cada descriptor para todas las moléculas clasificadas ante un mismo código ATC.

```python
# Calcular el promedio de cada propiedad química para cada tipo de "ATC Type"
averages = df_sorted.groupby('ATC Type')[['MolWt', 'LogP', 'TPSA', 'nRB', 'HBA', 'HBD']].mean().reset_index()
```

Para garantizar la compatibilidad entre los diferentes descriptores que poseen escalas distintas, normalizamos los datos empleando el método de Z-score:

---

## Normalización de los datos

```python
# Seleccionar las columnas de datos a normalizar
data = averages[['MolWt', 'LogP', 'TPSA', 'nRB', 'HBA', 'HBD']].values

# Normalización Z-score
from sklearn.preprocessing import StandardScaler
scaler = StandardScaler()
scaled_data = scaler.fit_transform(data)  # Normalizar los datos usando Z-score
averages.set_index('ATC Type', inplace=True)
scaled_df = pd.DataFrame(scaled_data, index=averages.index, columns=averages.columns)
```

---

## Métodos de visualización

Existen diversas formas de visualización en la agrupación de datos, estos nos permiten observar de una manera gráfica la distribución de los datos y sus tendencias. Las formas que emplearemos son:

* Dendograma
* Mapa de calor
* Árbol filogenético
* Gráfico de radar
* Reducción de componentes principales (PCA)

---

## Dendograma

Graficamos un dendograma de nuestra agrupación de datos. Con esto podemos observar qué similitud existe entre diferentes códigos ATC de una manera jerárquica.

Emplearemos la paquetería de Scipy, haciendo uso del método de Ward para el agrupamiento.

```python
# Realizar clustering jerárquico empleando el método Ward
from scipy.cluster.hierarchy import linkage, dendrogram

# Calcular la matriz de distancias y crear clusters
Z = linkage(scaled_df, method='ward')  

# Visualizar dendrograma simple
plt.figure(figsize=(15, 10))
dendrogram(Z, labels=averages.index, leaf_rotation=90., leaf_font_size=10)
plt.title('Hierarchical Clustering Dendrogram')
plt.xlabel('ATC Types')
plt.ylabel('Distance')
plt.show()
```

Es posible añadirle un mapa de calor para ver la similitud entre propiedades entre los distintos códigos ATC. Al tener en conjunto estos dos gráficos, podemos no solo observar la similitud de manera jerárquica entre códigos ATC, sino también de manera visual ver qué descriptor(es) son los más parecidos en valores.

---

## Mapa de calor y dendograma

```python
# Crear un heatmap con dendrograma
row_clusters = linkage(scaled_df, method='ward')  # Usar método Ward para agrupar filas
sns.clustermap(scaled_df, row_cluster=True, col_cluster=False, row_linkage=row_clusters, cmap="viridis", figsize=(10, 10))
plt.title("Heatmap con Dendrograma")
plt.show()

````


## Gráfico de radar

Otra forma de ver la distribución de los datos es a través de un gráfico de radar.

Podemos ir modificando el tipo de ATC.

```python
# Función para graficar un radar chart para un ATP Type específico
def plot_radar(scaled_df, row, title):
    categories = list(scaled_df.columns)  # Nombres de las propiedades químicas
    values = scaled_df.loc[row].values.flatten().tolist()  # Valores para el ATP Type seleccionado

    N = len(categories)
    values += values[:1]  # Repetir el primer valor para cerrar el círculo
    angles = [n / float(N) * 2 * np.pi for n in range(N)]
    angles += angles[:1]

    ax = plt.subplot(111, polar=True)
    plt.xticks(angles[:-1], categories)  # Añadir las etiquetas de categorías
    ax.plot(angles, values, linewidth=1, linestyle='solid')  # Dibujar los datos
    ax.fill(angles, values, 'b', alpha=0.1)  # Rellenar el área
    plt.title(title, size=20, color='blue', y=1.1)
    plt.show()

# Graficar radar chart para un ATP Type específico
plot_radar(scaled_df, 'V', 'Perfil de ATP Type V')
````

También es posible observar todos nuestros códigos ATC en un mismo gráfico.

```python
# Definir la función para graficar todos los ATP Types en un radar chart
def plot_radar_all(scaled_df, title):
    categories = list(scaled_df.columns)
    N = len(categories)
    angles = [n / float(N) * 2 * np.pi for n in range(N)]
    angles += angles[:1]

    fig, ax = plt.subplots(figsize=(10, 10), subplot_kw=dict(polar=True))
    plt.xticks(angles[:-1], categories)

    for index, row in scaled_df.iterrows():
        values = row.values.flatten().tolist()
        values += values[:1]
        ax.plot(angles, values, linewidth=1, linestyle='solid', label=index)
        ax.fill(angles, values, alpha=0.1)

    plt.title(title, size=20, color='blue', y=1.1)
    plt.legend(loc='upper right', bbox_to_anchor=(1.1, 1.1))
    plt.show()

# Graficar radar chart para todos los ATP Types
plot_radar_all(scaled_df, 'Perfil de Todos los ATP Types')
```

---

## Árbol filogenético

*Código pendiente de implementación*

Otra forma es mediante un gráfico bidimensional. Al emplear PCA, técnica de reducción de dimensiones, se obtiene una visualización de los datos en un espacio químico bidimensional.

---

## PCA

```python
from sklearn.decomposition import PCA
from sklearn.cluster import KMeans

# Reducir a dos componentes principales usando PCA
pca = PCA(n_components=2)
pca_results = pca.fit_transform(scaled_df)
pca_df = pd.DataFrame(pca_results, index=averages.index, columns=['PC1', 'PC2'])

# Visualizar PCA
plt.figure(figsize=(10, 10))
plt.scatter(pca_df['PC1'], pca_df['PC2'])
for label, x, y in zip(pca_df.index, pca_df['PC1'], pca_df['PC2']):
    plt.annotate(label, xy=(x, y), xytext=(5, 5), textcoords='offset points')
plt.title("PCA de ATP Types")
plt.xlabel("PC1")
plt.ylabel("PC2")
plt.grid(True)
plt.show()
```

---

## Formas de agrupación

Ahora que conocemos sobre las formas de visualización de nuestros agrupamientos, podemos cambiar las formas de agrupación aplicada.

Recordando que en la agrupación, puede ser jerárquica, por densidad o partitioning.

---

## K-means

El método k-means hace una agrupación por centroides.

Se usará rdkit, eso nos permite modificar el número de clusters y random state:

* n\_clusters:
* random\_state:

```python
# Aplicar K-means clustering
kmeans = KMeans(n_clusters=3, random_state=0).fit(scaled_df)
pca_df['Cluster'] = kmeans.labels_

# Visualizar K-means en componentes principales
sns.scatterplot(x=pca_df['PC1'], y=pca_df['PC2'], hue=pca_df['Cluster'], palette='viridis')
for label, x, y in zip(pca_df.index, pca_df['PC1'], pca_df['PC2']):
    plt.annotate(label, xy=(x, y), xytext=(5, 5), textcoords='offset points')
plt.title("Clusters generados por K-means")
plt.show()
```

---

## K-medoids

Instalación:

```python
!pip install scikit-learn-extra
from sklearn_extra.cluster import KMedoids
```

```python
# Configuración del modelo K-Medoids
kmedoids = KMedoids(n_clusters=3, random_state=0)  # Definir el número de clusters
clusters_kmedoids = kmedoids.fit_predict(scaled_df)  # Aplicar K-Medoids a los datos

# Añadir los clusters generados al DataFrame
scaled_df['KMedoids Cluster'] = clusters_kmedoids

# Visualizar los clusters generados con PCA
sns.scatterplot(x=pca_df['PC1'], y=pca_df['PC2'], hue=clusters_kmedoids, palette="viridis")
for label, x, y in zip(pca_df.index, pca_df['PC1'], pca_df['PC2']):
    plt.annotate(label, xy=(x, y), xytext=(5, 5), textcoords='offset points')
plt.title("Clusters generados por K-Medoids")
plt.show()
```

---

## DBSCAN

```python
from sklearn.cluster import DBSCAN
```

```python
# Configuración del modelo DBSCAN
dbscan = DBSCAN(eps=3.0, min_samples=5)  # eps define el radio, min_samples define el número mínimo de puntos en un cluster
clusters_dbscan = dbscan.fit_predict(scaled_df)  # Aplicar DBSCAN a los datos normalizados

# Añadir los clusters generados al DataFrame para análisis
scaled_df['DBSCAN Cluster'] = clusters_dbscan

# Visualizar los clusters generados con PCA
sns.scatterplot(x=pca_df['PC1'], y=pca_df['PC2'], hue=clusters_dbscan, palette="viridis")
for label, x, y in zip(pca_df.index, pca_df['PC1'], pca_df['PC2']):
    plt.annotate(label, xy=(x, y), xytext=(5, 5), textcoords='offset points')
plt.title("Clusters generados por DBSCAN")
plt.show()
```

---

## HDBSCAN

Instalación:

```python
!pip install hdbscan
import hdbscan
```

```python
# Configuración del modelo HDBSCAN
hdbscan_model = hdbscan.HDBSCAN(min_cluster_size=2, min_samples=3)  # Tamaño mínimo de cluster y muestras mínimas
clusters_hdbscan = hdbscan_model.fit_predict(scaled_df)  # Aplicar HDBSCAN a los datos

# Añadir los clusters generados al DataFrame
scaled_df['HDBSCAN Cluster'] = clusters_hdbscan

# Visualizar los clusters generados con PCA
sns.scatterplot(x=pca_df['PC1'], y=pca_df['PC2'], hue=clusters_hdbscan, palette="viridis")
for label, x, y in zip(pca_df.index, pca_df['PC1'], pca_df['PC2']):
    plt.annotate(label, xy=(x, y), xytext=(5, 5), textcoords='offset points')
plt.title("Clusters generados por HDBSCAN")
plt.show()
```

---

## Métricas de evaluación

Existen diversas métricas de evaluación de nuestros clusters...

Vamos a ocupar...

### Silhouette Score

```python
from sklearn.metrics import silhouette_score

# Silhouette Score para K-means
silhouette_kmeans = silhouette_score(scaled_df, kmeans.labels_)
print(f"Silhouette Score para K-means: {silhouette_kmeans}")

# Silhouette Score para DBSCAN
silhouette_dbscan = silhouette_score(scaled_df, clusters_dbscan) if len(set(clusters_dbscan)) > 1 else "No aplica"
print(f"Silhouette Score para DBSCAN: {silhouette_dbscan}")

# Silhouette Score para K-Medoids
silhouette_kmedoids = silhouette_score(scaled_df, clusters_kmedoids)
print(f"Silhouette Score para K-Medoids: {silhouette_kmedoids}")

# Silhouette Score para HDBSCAN
silhouette_hdbscan = silhouette_score(scaled_df, clusters_hdbscan) if len(set(clusters_hdbscan)) > 1 else "No aplica"
print(f"Silhouette Score para HDBSCAN: {silhouette_hdbscan}")
```

---

### Davies-Bouldin Score

```python
from sklearn.metrics import davies_bouldin_score

# Davies-Bouldin Index para K-means
db_index_kmeans = davies_bouldin_score(scaled_df, kmeans.labels_)
print(f"Davies-Bouldin Index para K-means: {db_index_kmeans}")

# Davies-Bouldin Index para K-Medoids
db_index_kmedoids = davies_bouldin_score(scaled_df, clusters_kmedoids)
print(f"Davies-Bouldin Index para K-Medoids: {db_index_kmedoids}")
```

---

## Análisis de resultados preliminares


---

## Corrección de clusters


---
