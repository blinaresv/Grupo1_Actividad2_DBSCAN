=========================================================
GRUPO 1 - DBSCAN - INTELIGENCIA ARTIFICIAL I - ACTIVIDAD 2
=========================================================

INTEGRANTES:
  - ARIZA VARGAS SARIAHT EYLEEN XIOMARA
  - CARRENO MEDINA ADRIANA LUCIA
  - LINARES VIASUS BRANDON FELIPE

ALGORITMO ASIGNADO: DBSCAN (Clustering basado en densidad)

ARCHIVOS INCLUIDOS:
  - Grupo1_Notebook1_Manual_FINAL.ipynb     (implementacion manual con NumPy/Pandas)
  - Grupo1_Notebook2_Librerias_FINAL.ipynb  (implementacion con scikit-learn)
  - Mall_Customers.csv                      (dataset, 300 registros)
  - requirements.txt                        (dependencias Python)
  - Grupo1_Resumen.pdf                      (documento de resumen PDF)
  - Grupo1_Presentacion_FINAL.pptx          (presentacion editable PowerPoint, 12 slides)
  - Grupo1_Presentacion_FINAL.pdf           (presentacion exportada a PDF)

COMO EJECUTAR:
  1. Crear entorno: python -m venv venv && source venv/bin/activate
  2. Instalar dependencias: pip install -r requirements.txt
  3. Abrir notebooks: jupyter notebook
  4. Ejecutar Notebook 1 y luego Notebook 2 (asegurarse de que Mall_Customers.csv
     este en la misma carpeta).

DATASET:
  Mall Customers - estilo Kaggle/UCI
  https://www.kaggle.com/datasets/vjchoudhary7/customer-segmentation-tutorial-in-python
  300 registros, 5 columnas. Cumple el requisito >= 100 para no supervisado.

RESULTADOS PRINCIPALES:
  - Notebook 1 (lunas):     2 clusters, 0 ruido, silhouette=0.328
  - Notebook 2 (Mall):      4 clusters, 20 outliers, silhouette=0.581
  - Validacion: 100% coincidencia de etiquetas con sklearn.cluster.DBSCAN

ENTREGAS:
  - Grupo1_Actividad2_DOCUMENTACION.zip  -> Entrega 2 (notebooks, dataset, requirements, resumen)
  - Grupo1_Presentacion_FINAL.pdf        -> Entrega 3 (presentacion en PDF)

REFERENCIAS PRINCIPALES (formato IEEE en notebooks/PDF/PPT):
  [1] Ester, Kriegel, Sander, Xu - KDD-96 (paper original DBSCAN)
  [3] Schubert et al. - "DBSCAN revisited, revisited" - ACM TODS 2017
  [4] Pedregosa et al. - "Scikit-learn" - JMLR 2011
  [7] Rahmah & Sitanggang - eleccion de eps - IOP 2016
