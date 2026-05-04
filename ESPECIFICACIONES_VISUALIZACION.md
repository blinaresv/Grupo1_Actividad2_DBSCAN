# Especificaciones de Visualización — Rubrica del Curso
## Unidades 1, 2 y 3 — Requisitos obligatorios para todas las gráficas

Este documento contiene las reglas exactas que deben cumplir TODAS las gráficas
de los notebooks. Úsalo como checklist antes de cualquier cambio visual.

---

## UNIDAD 1 — Tipo de gráfico correcto

### Regla fundamental
El tipo de gráfico debe alinearse con la naturaleza del dato y el mensaje a comunicar.
**Nunca elegir un gráfico por estética.**

### Tabla de tipos permitidos

| Gráfico | Usar cuando | Tipo de dato | Usado en este proyecto |
|---------|------------|--------------|------------------------|
| **Scatter** | Relación entre dos variables cuantitativas | Cuantitativo | Clusters DBSCAN ✓ |
| **Histograma** | Distribución de una variable continua | Cuantitativo | EDA por variable ✓ |
| **Barra** | Comparación entre categorías nominales | Nominal/Ordinal | Distribución por género ✓ |
| **Línea** | Tendencia en datos ordenados | Series ordenadas | Gráfico k-distancia ✓ |
| Pastel/Donut | Composición proporcional (<6 partes) | Proporción | NO usar aquí |
| Mapa de calor | Intensidad en matriz 2D | Matricial | NO usar aquí |

### Reglas éticas de visualización (U1)
- **NO** truncar el eje Y para exagerar diferencias.
- **NO** usar gráficos 3D que distorsionen proporciones.
- **SÍ** incluir el cero en el eje Y de histogramas y barras cuando corresponda.
- **SÍ** mencionar la fuente de los datos cuando aplique.

---

## UNIDAD 2 — Teoría del color

### Regla fundamental
**Elegir la paleta según la naturaleza del dato, no por preferencia estética.**
El color no es decoración — es un canal de información.

### Tipos de paleta y cuándo usarlos

| Tipo de paleta | Cuándo usar | Paletas aprobadas | Ejemplo en este proyecto |
|---------------|-------------|-------------------|--------------------------|
| **Secuencial** | Datos continuos u ordenados (magnitud, densidad, intensidad). Un solo tono varía en luminosidad. Claro = bajo, Oscuro = alto. | `Blues`, `Greens`, `Oranges`, `Viridis`, `Plasma`, `Cividis` | Histogramas de EDA |
| **Divergente** | Datos con punto neutro central y dos extremos opuestos (ganancia/pérdida, acuerdo/desacuerdo) | `RdBu`, `PuOr`, `coolwarm` | No aplica en este proyecto |
| **Cualitativa** | Categorías sin orden ni jerarquía (clusters, regiones, marcas) | `Set2` de ColorBrewer (máx. 8 colores) | Clusters DBSCAN, género |

### Paleta obligatoria para clusters (cualitativa)

Usar **siempre** los hex exactos de ColorBrewer Set2 para garantizar reproducibilidad:

```python
# ColorBrewer Set2 — paleta cualitativa, 8 colores máximo
# Fuente: colorbrewer2.org — accesible para daltonismo rojo-verde
SET2 = [
    "#66C2A5",  # teal
    "#FC8D62",  # naranja
    "#8DA0CB",  # azul-violeta
    "#E78AC3",  # rosa
    "#A6D854",  # verde-lima
    "#FFD92F",  # amarillo
    "#E5C494",  # beige
    "#B3B3B3",  # gris
]
```

**NO** usar `sns.color_palette("Set2")` directamente — en seaborn 0.12.2 puede renderizar
colores incorrectos. Usar siempre los hex de arriba.

### Paletas para histogramas (secuenciales — un color por panel)

```python
col_age    = sns.color_palette("Blues", 8)[4]    # azul medio
col_income = sns.color_palette("Greens", 8)[4]   # verde medio
col_score  = sns.color_palette("Oranges", 8)[4]  # naranja medio
gender_pal = ["#66C2A5", "#FC8D62"]              # Set2 para 2 categorías
```

### Paletas para gráficos de línea (secuenciales)

```python
line_color = sns.color_palette("Blues_d", 3)[1]   # curva principal
codo_color = sns.color_palette("Oranges_d", 3)[1] # referencia/umbral (NO usar rojo)
```

### Reglas de color obligatorias

1. **Máximo 7–8 colores** en cualquier paleta cualitativa. El ojo humano no distingue más.
2. **Nunca usar rojo para datos neutros o de referencia.** El rojo evoca peligro/error.
   Usar naranja (`Oranges_d`) para líneas de umbral o referencias.
3. **Nunca usar la combinación rojo-verde** para codificar oposición.
   El 8% de los hombres tiene daltonismo rojo-verde (deuteranopía/protanopía).
4. **Siempre combinar color + forma** para no depender solo del color (accesibilidad U2).
   - Ruido/outliers: `marker="x"` + color gris `#B0BEC5`
   - Clusters: marcador círculo (por defecto) + color de SET2
5. **No usar hex arbitrarios.** Solo paletas nombradas de seaborn o los hex de ColorBrewer.
6. **Viridis y Plasma** son preferidas para datos continuos con gradiente (heatmaps, coropléticos)
   porque son perceptualmente uniformes y accesibles para daltonismo.
7. El color con mayor saturación/brillo atrae la mirada primero. Reservarlo para el dato principal.

### Código de ruido/outliers (siempre igual)

```python
# Outliers — color gris neutro + marcador X (color + forma para accesibilidad)
ax.scatter(X[mask, 0], X[mask, 1],
           color="#B0BEC5", marker="x", s=60, linewidths=1.3,
           label=f"Ruido ({mask.sum()})", zorder=2)
```

---

## UNIDAD 3 — Diseño gráfico y data-ink ratio

### Regla fundamental (Tufte)
**Cada píxel debe tener propósito.** Eliminar todo elemento que no comunique información.
`data-ink ratio = tinta de datos / tinta total` → maximizar este ratio.

### Tipografía obligatoria — tamaños mínimos

```python
ax.set_title("...", fontsize=12, fontweight="bold")  # título — negrita
ax.set_xlabel("...", fontsize=11)                    # eje X — con unidades
ax.set_ylabel("...", fontsize=11)                    # eje Y — con unidades
ax.tick_params(labelsize=10)                         # números en ejes — mínimo 10pt
ax.legend(fontsize=9)                                # leyenda — mínimo 9pt
```

**Nunca omitir `fontsize` y `tick_params`.** Si no se especifica explícitamente,
el tamaño depende del backend y puede quedar ilegible.

### Eliminación de chartjunk (obligatorio en todas las figuras)

```python
ax.spines[["top", "right"]].set_visible(False)  # eliminar bordes decorativos
```

- **NO** usar sombras 3D.
- **NO** usar bordes decorativos extra.
- **NO** usar grids excesivos — el `style="whitegrid"` de seaborn (líneas grises suaves) es aceptable.
- **NO** usar más de 2–3 familias tipográficas.

### Leyenda fuera del área de datos

Usar siempre cuando hay puntos cerca del borde de la gráfica:

```python
ax.legend(
    bbox_to_anchor=(1.01, 1),
    loc="upper left",
    borderaxespad=0,
    fontsize=9,
    framealpha=0.9
)
plt.tight_layout(rect=[0, 0, 0.82, 1])  # reservar espacio para la leyenda exterior
```

### Títulos descriptivos (obligatorio)

El título debe explicar **qué muestra** la gráfica y **qué decisión informa**, no solo nombrarla.

| ❌ Genérico | ✓ Descriptivo |
|---|---|
| `"Grafico k-distancia"` | `"Grafico k-distancia (k=5) — seleccion del hiperparametro eps"` |
| `"Clusters"` | `"Segmentacion de clientes con DBSCAN \| eps=0.25, min_samples=5"` |
| `"Histograma"` | `"Distribucion de Edad"` |
| `"Scatter"` | `"Relacion Ingreso vs Spending Score (antes del clustering)"` |

### Etiquetas de ejes — incluir unidades siempre que existan

| ❌ Sin unidad | ✓ Con unidad |
|---|---|
| `"Spending Score"` | `"Spending Score (1-100)"` |
| `"Annual Income"` | `"Ingreso Anual (k$)"` |
| `"Age"` | `"Edad (anos)"` |
| `"Puntos"` | `"Puntos ordenados por distancia"` |

### Jerarquía visual — orden de atención

1. **Título** — fontsize=12, fontweight="bold" → lo primero que ve el lector
2. **Datos principales** (clusters) — colores saturados de SET2
3. **Datos secundarios / ruido** — gris neutro `#B0BEC5`, marcador `x`
4. **Ejes, ticks y leyenda** — fontsize 9–11pt

### Plantilla completa para scatter de clusters

```python
SET2 = ["#66C2A5", "#FC8D62", "#8DA0CB", "#E78AC3",
        "#A6D854", "#FFD92F", "#E5C494", "#B3B3B3"]

fig, ax = plt.subplots(figsize=(8, 5.2))

for c in sorted(set(labels)):
    mask = labels == c
    if c == -1:
        ax.scatter(X[mask, 0], X[mask, 1],
                   color="#B0BEC5", marker="x", s=60, linewidths=1.3,
                   label=f"Ruido ({mask.sum()})", zorder=2)
    else:
        ax.scatter(X[mask, 0], X[mask, 1],
                   color=SET2[c % len(SET2)], s=48,
                   edgecolor="white", linewidth=0.6,
                   label=f"Cluster {c} ({mask.sum()})", zorder=3)

ax.set_title("TITULO DESCRIPTIVO", fontsize=12, fontweight="bold")
ax.set_xlabel("VARIABLE X (unidad)", fontsize=11)
ax.set_ylabel("VARIABLE Y (unidad)", fontsize=11)
ax.tick_params(labelsize=10)
ax.legend(loc="upper left", fontsize=9, framealpha=0.9,
          bbox_to_anchor=(1.01, 1), borderaxespad=0)
ax.spines[["top", "right"]].set_visible(False)
plt.tight_layout(rect=[0, 0, 0.82, 1])
plt.show()
```

### Plantilla completa para histogramas EDA

```python
fig, axes = plt.subplots(2, 2, figsize=(11, 7))

axes[0, 0].hist(df["COL"], bins=20, color=COLOR_SECUENCIAL, edgecolor="white")
axes[0, 0].set_title("Distribucion de VARIABLE", fontsize=11, fontweight="bold")
axes[0, 0].set_xlabel("VARIABLE (unidad)", fontsize=11)
axes[0, 0].set_ylabel("Frecuencia", fontsize=11)
axes[0, 0].tick_params(labelsize=10)
# ... repetir para cada panel

for ax in axes.flat:
    ax.spines[["top", "right"]].set_visible(False)

plt.tight_layout()
plt.show()
```

### Plantilla completa para gráfico de línea (k-distancia)

```python
line_color = sns.color_palette("Blues_d", 3)[1]
codo_color = sns.color_palette("Oranges_d", 3)[1]  # naranja, NO rojo

fig, ax = plt.subplots(figsize=(8, 4.5))
ax.plot(np.arange(len(kdist)), kdist, color=line_color, lw=2)
ax.axhline(EPS_VALUE, color=codo_color, ls="--", lw=1.5,
           label=f"eps seleccionado = {EPS_VALUE} (punto de codo)")
ax.set_title("Grafico k-distancia (k=K) — seleccion del hiperparametro eps",
             fontsize=12, fontweight="bold")
ax.set_xlabel("Puntos ordenados por distancia", fontsize=11)
ax.set_ylabel("Distancia al Kto vecino mas cercano", fontsize=11)
ax.tick_params(labelsize=10)
ax.legend(fontsize=10)
ax.spines[["top", "right"]].set_visible(False)
plt.tight_layout()
plt.show()
```

---

## Checklist de verificación — antes de entregar

Marcar cada ítem antes de presentar una gráfica:

### Unidad 1
- [ ] El tipo de gráfico es correcto para el tipo de dato (scatter/histograma/barra/línea)
- [ ] No se usa gráfico 3D
- [ ] El eje Y incluye el cero cuando es un histograma o barra
- [ ] La gráfica responde a una pregunta concreta de los datos

### Unidad 2
- [ ] La paleta es cualitativa para clusters (SET2 hex exactos)
- [ ] La paleta es secuencial para datos continuos (Blues/Greens/Oranges/Viridis)
- [ ] No hay más de 7–8 colores en la misma gráfica
- [ ] No se usa la combinación rojo-verde
- [ ] El rojo NO aparece en datos neutros o de referencia
- [ ] El ruido/outliers usa color + forma (marker="x" + gris `#B0BEC5`)
- [ ] No se usan hex arbitrarios — solo paletas de ColorBrewer o seaborn

### Unidad 3
- [ ] `ax.spines[["top", "right"]].set_visible(False)` en todos los ejes
- [ ] Título con `fontsize=12, fontweight="bold"` y redacción descriptiva
- [ ] `set_xlabel` y `set_ylabel` con `fontsize=11` e incluyen unidades
- [ ] `ax.tick_params(labelsize=10)` en todos los ejes
- [ ] `ax.legend(fontsize=9)` — mínimo 9pt
- [ ] La leyenda está fuera del área de datos (`bbox_to_anchor=(1.01, 1)`) cuando tapa puntos
- [ ] `plt.tight_layout()` o `plt.tight_layout(rect=[0, 0, 0.82, 1])` al final
- [ ] Sin sombras 3D, sin bordes decorativos, sin grid excesivo

---

## Configuración inicial obligatoria (primera celda de código)

```python
%matplotlib inline
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

sns.set_theme(style="whitegrid", context="notebook")
```

---

*Referencia académica: colorbrewer2.org (Cynthia Brewer, Penn State)*
*Tufte, E.R. (2001). The Visual Display of Quantitative Information. Graphics Press.*
