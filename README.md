# Grupo 1 — DBSCAN: Clustering Basado en Densidad
**Inteligencia Artificial I — Actividad 2**

---

## Integrantes

| Nombre | 
|--------|
| Ariza Vargas Sariaht Eyleen Xiomara |
| Carreño Medina Adriana Lucía |
| Linares Viasus Brandon Felipe |

---

## Descripción del proyecto

Implementación completa del algoritmo **DBSCAN** (*Density-Based Spatial Clustering of Applications with Noise*), un método de clustering no supervisado que descubre grupos de forma arbitraria y detecta outliers automáticamente, sin necesidad de especificar el número de clusters de antemano.

El proyecto se divide en dos notebooks:

- **Notebook 1**: implementación manual desde cero usando solo NumPy, sin librerías de ML.
- **Notebook 2**: implementación profesional con `scikit-learn` aplicada a datos reales de clientes de un centro comercial.

---

## Estructura del repositorio

```
Grupo1_Actividad2_DBSCAN/
├── Grupo1_Notebook1_Manual_FINAL.ipynb      # DBSCAN manual (NumPy)
├── Grupo1_Notebook2_Librerias_FINAL.ipynb   # DBSCAN con scikit-learn
├── Mall_Customers.csv                        # Dataset (200 registros)
├── requirements.txt                          # Dependencias Python
├── ESPECIFICACIONES_VISUALIZACION.md         # Rúbrica de visualización del curso
└── README.md
```

---

## Requisitos

- Python 3.10 o superior
- Las dependencias están en `requirements.txt`:

```
numpy==1.24.3
pandas==2.0.3
matplotlib==3.7.2
seaborn==0.12.2
scikit-learn==1.3.0
jupyter==1.0.0
```

---

## Cómo ejecutar

### 1. Crear y activar el entorno virtual

```bash
python -m venv venv
source venv/bin/activate        # macOS / Linux
# venv\Scripts\activate         # Windows
```

### 2. Instalar dependencias

```bash
pip install -r requirements.txt
```

### 3. Ejecutar los notebooks

**Desde VS Code:**
- Abrir el archivo `.ipynb`
- Seleccionar el kernel del entorno virtual (`venv`)
- `Kernel → Restart & Run All Cells`

**Desde terminal:**
```bash
jupyter nbconvert --to notebook --execute --inplace Grupo1_Notebook1_Manual_FINAL.ipynb
jupyter nbconvert --to notebook --execute --inplace Grupo1_Notebook2_Librerias_FINAL.ipynb
```

> `Mall_Customers.csv` debe estar en la misma carpeta que los notebooks.

---

## Dataset

**Mall Customers** — fuente: [Kaggle / vjchoudhary7](https://www.kaggle.com/datasets/vjchoudhary7/customer-segmentation-tutorial-in-python)

| Columna | Descripción |
|---------|-------------|
| `CustomerID` | Identificador único |
| `Genre` | Género del cliente |
| `Age` | Edad (años) |
| `Annual Income (k$)` | Ingreso anual en miles de dólares |
| `Spending Score (1-100)` | Índice de comportamiento de compra |

- **200 registros** · 5 columnas
- El modelo usa únicamente `Annual Income (k$)` y `Spending Score (1-100)` tras estandarización z-score.

---

## Resultados principales

| Notebook | Dataset | Clusters | Outliers | Silhouette Score |
|----------|---------|----------|----------|-----------------|
| Notebook 1 (manual) | Two moons sintético | 2 | 0 | 0.328 |
| Notebook 2 (sklearn) | Mall Customers | 4 | 20 | 0.581 |

- Validación Notebook 1: **100% de coincidencia** de etiquetas con `sklearn.cluster.DBSCAN` usando los mismos parámetros (`eps=0.20`, `min_samples=5`).
- Hiperparámetros Notebook 2 seleccionados mediante gráfico k-distancia: `eps=0.25`, `min_samples=5`.

---

## Algoritmo DBSCAN — resumen

DBSCAN clasifica cada punto en una de tres categorías:

- **Core point**: tiene al menos `min_samples` puntos en su vecindad de radio `eps`.
- **Border point**: está en la vecindad de un core point pero no es core.
- **Noise point**: no es core ni cae en la vecindad de ninguno (outlier).

Los clusters se forman expandiendo los core points mediante búsqueda en anchura (BFS), conectando todos los puntos densamente alcanzables.

---

## Referencias (formato IEEE)

[1] M. Ester, H.-P. Kriegel, J. Sander, and X. Xu, "A density-based algorithm for discovering clusters in large spatial databases with noise," in *Proc. KDD-96*, Portland, OR, USA, 1996, pp. 226-231.

[2] J. Sander, M. Ester, H.-P. Kriegel, and X. Xu, "Density-based clustering in spatial databases: The algorithm GDBSCAN and its applications," *Data Mining and Knowledge Discovery*, vol. 2, no. 2, pp. 169-194, Jun. 1998.

[3] E. Schubert, J. Sander, M. Ester, H.-P. Kriegel, and X. Xu, "DBSCAN revisited, revisited: why and how you should (still) use DBSCAN," *ACM Trans. Database Syst.*, vol. 42, no. 3, pp. 1-21, 2017, doi: 10.1145/3068335.

[4] F. Pedregosa et al., "Scikit-learn: Machine learning in Python," *Journal of Machine Learning Research*, vol. 12, pp. 2825-2830, 2011.

[5] T. Hastie, R. Tibshirani, and J. H. Friedman, *The Elements of Statistical Learning*, 2nd ed. New York, NY, USA: Springer, 2009.

[6] Scikit-learn Developers, "sklearn.cluster.DBSCAN," *scikit-learn 1.3 documentation*, 2024. [Online]. Available: https://scikit-learn.org/stable/modules/generated/sklearn.cluster.DBSCAN.html.

[7] N. Rahmah and I. S. Sitanggang, "Determination of optimal epsilon (eps) value on DBSCAN algorithm to clustering data on peatland hotspots in Sumatra," *IOP Conf. Series: Earth and Environmental Science*, vol. 31, no. 1, p. 012012, 2016.

[8] T. Ali, S. Asghar, and N. A. Sajid, "Critical analysis of DBSCAN variations," in *Proc. 2010 Int. Conf. Information and Emerging Technologies (ICIET)*, Karachi, Pakistan, 2010, pp. 1-6.

[9] Mall Customers Dataset, *Kaggle*, 2018. [Online]. Available: https://www.kaggle.com/datasets/vjchoudhary7/customer-segmentation-tutorial-in-python.
