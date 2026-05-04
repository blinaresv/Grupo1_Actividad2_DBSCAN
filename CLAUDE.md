# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Proyecto

**Inteligencia Artificial I — Actividad 2 | Grupo 1**
Implementación del algoritmo DBSCAN (Clustering basado en densidad, No Supervisado).
Integrantes: Ariza Vargas Sariaht, Carreño Medina Adriana, Linares Viasus Brandon Felipe.

## Comandos principales

```bash
# Crear entorno virtual e instalar dependencias
python -m venv venv
source venv/bin/activate          # macOS/Linux
# venv\Scripts\activate           # Windows

pip install -r requirements.txt

# Ejecutar los notebooks en VS Code
# Abrir el .ipynb → seleccionar el kernel del venv → Kernel → Restart & Run All Cells

# Ejecutar desde terminal
jupyter nbconvert --to notebook --execute --inplace Grupo1_Notebook1_Manual_FINAL.ipynb
jupyter nbconvert --to notebook --execute --inplace Grupo1_Notebook2_Librerias_FINAL.ipynb
```

> `Mall_Customers.csv` debe estar en la misma carpeta que los notebooks.

---

## Arquitectura del proyecto

### Notebook 1 — Implementación Manual (`Grupo1_Notebook1_Manual_FINAL.ipynb`)

DBSCAN construido **solo con NumPy** (sin scikit-learn para la lógica de ML).

| Celda | Contenido |
|-------|-----------|
| 3 | Imports + `%matplotlib inline` + seed |
| 4 | `euclidean_distance(p, q)` y `region_query(X, idx, eps)` |
| 5 | `expand_cluster(...)` con BFS + `dbscan_manual(X, eps, min_samples)` |
| 7 | `make_moons_np(n_samples, noise, seed)` — dataset sintético |
| 8 | Llamada principal: `dbscan_manual(X, eps=0.20, min_samples=5)` |
| 10 | Visualización (dos paneles: datos crudos vs. clusters) |
| 13 | Validación contra `sklearn.cluster.DBSCAN` |

**No modificar** la lógica de `euclidean_distance`, `region_query`, `expand_cluster` ni `dbscan_manual`.

### Notebook 2 — Implementación con Librería (`Grupo1_Notebook2_Librerias_FINAL.ipynb`)

Usa `sklearn.cluster.DBSCAN` sobre `Mall_Customers.csv` (variables: ingreso anual y spending score).

| Celda | Contenido |
|-------|-----------|
| 2 | Imports + carga del CSV |
| 5 | EDA: histogramas por variable |
| 6 | Scatter ingreso vs. spending sin etiquetas |
| 8 | `StandardScaler` — estandarización z-score |
| 10 | Gráfico k-distancia → `eps=0.25` |
| 12 | `DBSCAN(eps=0.25, min_samples=5).fit_predict(X_std)` |
| 14 | Silhouette Score + Davies-Bouldin Index |
| 15 | Scatter con clusters coloreados |
| 17 | Grid search `eps ∈ [0.15…0.35]` × `min_samples ∈ [3,5,7]` |
| 19 | Perfilamiento comercial por cluster |

**No cambiar** `eps=0.25`, `min_samples=5`, ni `features = ["Annual Income (k$)", "Spending Score (1-100)"]`.

---

## Rubrica de visualización — checklist obligatorio

Todas las gráficas deben cumplir los siguientes criterios del curso. Verificar antes de hacer cualquier cambio visual.

### Unidad 1 — Tipo de gráfico correcto

- **Scatter**: relación entre dos variables cuantitativas. Correcto para DBSCAN. ✓
- **Histograma**: distribución de una variable continua. ✓
- **Barra**: comparación entre categorías nominales (ej. género). ✓
- **Línea**: tendencia en datos ordenados (ej. gráfico k-distancia). ✓
- No usar gráficos de pastel con más de 6 categorías.
- No usar gráficos 3D que distorsionen proporciones.
- Incluir el cero en el eje Y de histogramas y barras cuando corresponda.

### Unidad 2 — Teoría del color

**Regla principal: elegir la paleta según la naturaleza del dato.**

| Tipo de dato | Paleta | Ejemplos en este proyecto |
|---|---|---|
| Continuo / ordenado | **Secuencial** (un matiz, varía luminosidad) | Histogramas: `Blues`, `Greens`, `Oranges` |
| Bipolar con punto neutro | **Divergente** | No aplica aquí |
| Categorías sin orden | **Cualitativa** | Clusters: `Set2`; género: `Set2` |

- Máximo **7–8 colores** en paletas cualitativas.
- **Evitar rojo-verde** juntos (8 % de hombres tienen daltonismo rojo-verde).
- El rojo evoca peligro/error: no usarlo para referencias neutras (ej. línea de eps → usar naranja `Oranges_d`).
- **Combinar color + forma** para no depender solo del color: ruido usa `marker="x"` + gris `#B0BEC5`.
- No usar hex arbitrarios. Solo paletas nombradas de seaborn o ColorBrewer.
- Viridis y Plasma son preferidas para datos continuos cuando aplica (perceptualmente uniformes, accesibles para daltonismo).
- ColorBrewer (`colorbrewer2.org`) es la referencia académica recomendada.

### Unidad 3 — Diseño gráfico y data-ink ratio

**Data-ink ratio (Tufte): cada píxel debe tener propósito.**

- Eliminar spines superior y derecho en todas las figuras:
  ```python
  ax.spines[["top", "right"]].set_visible(False)
  ```
- Sin sombras 3D, sin bordes decorativos, sin grid excesivo (whitegrid de seaborn es aceptable).

**Tipografía obligatoria:**

```python
ax.set_title("...", fontsize=12, fontweight="bold")   # título
ax.set_xlabel("...", fontsize=11)                     # eje X — con unidades si aplica
ax.set_ylabel("...", fontsize=11)                     # eje Y
ax.tick_params(labelsize=10)                          # marcas del eje — mínimo 10pt
ax.legend(fontsize=9)                                 # leyenda — mínimo 9pt (U3: 9-11pt)
```

**Leyenda fuera del área de datos** (cuando hay puntos cerca del borde):
```python
ax.legend(bbox_to_anchor=(1.01, 1), loc="upper left", borderaxespad=0)
plt.tight_layout(rect=[0, 0, 0.82, 1])
```

**Jerarquía visual (orden de atención):**
1. Título — negrita 12pt
2. Datos principales (clusters) — colores saturados
3. Datos secundarios / ruido — gris neutro `#B0BEC5`
4. Ejes y leyenda — 9–11pt

**Títulos descriptivos:** deben explicar qué muestra la gráfica, no solo nombrarla.
- ✗ `"Grafico k-distancia"` — genérico
- ✓ `"Grafico k-distancia (k=5) — seleccion del hiperparametro eps"` — informa la decisión

**Etiquetas de ejes:** incluir siempre la unidad cuando existe.
- ✗ `"Spending Score"` — incompleto
- ✓ `"Spending Score (1-100)"` — con rango

---

## Dataset

`Mall_Customers.csv` — 200 registros: `CustomerID`, `Genre`, `Age`, `Annual Income (k$)`, `Spending Score (1-100)`.
El modelo usa solo las dos últimas columnas tras estandarización z-score (`StandardScaler`).

## Dependencias exactas

Ver `requirements.txt`: numpy 1.24.3, pandas 2.0.3, matplotlib 3.7.2, seaborn 0.12.2, scikit-learn 1.3.0, jupyter 1.0.0.
