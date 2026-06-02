# Differential Expression Pipeline

Un pipeline completo para el análisis de expresión diferencial de RNA-seq, incluyendo análisis de vías metabólicas y aplicación de técnicas de Machine Learning.

**Autores:** Sebastián Toro & Gemini  
**Fecha:** 2026-05-08

---

## 📋 Descripción General

Este repositorio contiene un flujo de trabajo completo para:

1. **Análisis de Expresión Diferencial**: Utilizando PyDESeq2 para identificar genes diferencialmente expresados
2. **Análisis Funcional**: Con GseaPy para interpretación biológica de los genes significativos
3. **Machine Learning**: Clasificación y predicción usando features de expresión génica
4. **Análisis de Vías Metabólicas**: Visualización e interpretación de pathways biológicos alterados

---

## 🗂️ Estructura del Proyecto

### Notebooks

#### **0. PyDESeq2.ipynb** 
**Análisis de Expresión Diferencial y Preparación de Features para ML**

Este notebook es el punto de partida del pipeline. Guía a través de:

- **Carga y Preprocesamiento**: 
  - Importación de datos de conteos (FeatureCounts)
  - Preparación de metadatos y variables de interés
  
- **Análisis DESeq2**:
  - Normalización de conteos
  - Estimación de dispersión
  - Cálculo de estadísticos de expresión diferencial
  - Identificación de genes significativos

- **Transformación VST** (Variance Stabilizing Transformation)
  - Normalización para análisis exploratorio
  
- **Visualizaciones Iniciales**:
  - PCA (Principal Component Analysis) para separación de muestras
  - Volcano Plot para identificación gráfica de genes diferencialmente expresados
  
- **Exportación de Master Table**:
  - Matriz de features normalizadas para Machine Learning

**Requisitos**: Archivo de conteos de FeatureCounts + metadatos con variables clínicas

---

#### **1. PyDESeq2_GseaPy_Analysis.ipynb**
**Análisis de Enriquecimiento Funcional con Gene Set Enrichment Analysis (GSEA)**

Realiza interpretación biológica de los genes diferencialmente expresados:

- **Gene Set Enrichment Analysis (GSEA)**:
  - Enriquecimiento de términos GO (Gene Ontology)
  - Análisis de vías KEGG
  - Identificación de procesos biológicos alterados
  
- **Visualizaciones de Resultados**:
  - Gráficos de barras con top pathways
  - Network plots para visualizar relaciones entre términos
  - Bubble plots para múltiples categorías
  
- **Interpretación Biológica**:
  - Identificación de funciones celulares alteradas
  - Caracterización de mecanismos de enfermedad

**Entrada**: Resultados del Notebook 0 (genes diferencialmente expresados)

---

#### **2. Genes ML.ipynb**
**Machine Learning para Clasificación Basada en Expresión Génica**

Aplica técnicas de ML para clasificación predictiva:

- **Preparación de Datos**:
  - División train/test
  - Normalización y escalado
  - Selección de features relevantes
  
- **Modelos de Clasificación**:
  - Regresión Logística
  - Random Forest
  - Support Vector Machines (SVM)
  - Gradient Boosting
  
- **Evaluación del Desempeño**:
  - Métricas: Accuracy, Precision, Recall, F1-Score
  - Curvas ROC-AUC
  - Matrices de confusión
  - Validación cruzada
  
- **Interpretabilidad**:
  - Feature importance
  - SHAP values para explicabilidad
  - Identificación de genes discriminatorios

**Entrada**: Master Table del Notebook 0

---

#### **3. Pathway_Analysis.ipynb**
**Análisis Avanzado de Vías Metabólicas y Procesos Biológicos**

Análisis profundo de vías biológicas alteradas:

- **Análisis de Vías Múltiples**:
  - KEGG Pathways
  - Reactome
  - BioPlanet
  - WikiPathways
  
- **Visualizaciones Interactivas**:
  - Gráficos de dominancia de pathways
  - Heatmaps de expresión en vías
  - Network analysis de genes interconectados
  
- **Correlación con Features Clínicos**:
  - Asociación de pathways con fenotipos
  - Análisis de robustez
  
- **Exportación de Resultados**:
  - Tablas resumidas
  - Figuras de alta calidad para publicaciones
  - Anotaciones funcionales detalladas

**Entrada**: Resultados DESeq2 + anotaciones biológicas

---

## 🔄 Flujo de Trabajo

```
1. PyDESeq2.ipynb
   ↓ (genes significativos + master table)
   │
   ├─→ 1. GseaPy_Analysis.ipynb (análisis funcional)
   │
   ├─→ 2. Genes_ML.ipynb (predicción)
   │
   └─→ 3. Pathway_Analysis.ipynb (análisis de vías)
```

---

## 📦 Requisitos

### Ambiente Conda

Se asume un entorno Conda con las siguientes librerías:

**Data Analysis & Visualization:**
- `numpy`
- `pandas`
- `matplotlib`
- `seaborn`
- `scipy`

**Bioinformatics:**
- `pydeseq2` - DESeq2 en Python
- `gseapy` - Gene Set Enrichment Analysis
- `anndata` - Formato de datos para biología
- `scanpy` (opcional) - Single-cell analysis

**Machine Learning:**
- `scikit-learn` - Algoritmos ML clásicos
- `xgboost` - Gradient boosting
- `shap` - Explicabilidad de modelos
- `optuna` (opcional) - Optimización de hiperparámetros

### Instalación

```bash
# Crear ambiente
conda create -n de_pipeline python=3.10

# Activar
conda activate de_pipeline

# Instalar dependencias
conda install numpy pandas matplotlib seaborn scipy scikit-learn xgboost
pip install pydeseq2 gseapy shap
```

---

## 📊 Entrada de Datos

### Archivo de Conteos (FeatureCounts)
- Formato: TSV/CSV
- Columnas: Geneid + información genómica (Chr, Start, End, Strand, Length) + conteos de muestras
- Filas: Genes

### Metadatos
- Formato: CSV
- Columnas: sample_id, variable_de_interes (ej: tratamiento, enfermedad, etc.)
- Una fila por muestra

---

## 📈 Salidas Esperadas

**Del Pipeline:**
1. **Tablas de resultados**: Genes significativos, estadísticas, p-valores
2. **Visualizaciones**: Volcano plots, PCA, heatmaps, network plots
3. **Master Table**: Matriz de features para ML
4. **Modelos entrenados**: Clasificadores con métricas de desempeño
5. **Reportes funcionales**: Términos GO y pathways enriquecidos

---

## 🎯 Casos de Uso

- **Investigación Biomédica**: Identificación de biomarcadores en enfermedades
- **Screening Funcional**: Análisis de resultados experimentales RNA-seq
- **Descubrimiento de Fármacos**: Identificación de targets terapéuticos
- **Medicina Personalizada**: Predicción de respuesta a tratamientos

---

## 📝 Notas Importantes

- Los notebooks están numerados (0, 1, 2, 3) para mantener el orden de ejecución
- Cada notebook es independiente pero usa salidas de notebooks anteriores
- Se recomienda ejecutar en orden: 0 → {1, 2, 3}
- Los parámetros de significancia (p-value, log2FC) deben ajustarse según criterios del experimento

---

## 📚 Referencias

- **PyDESeq2**: https://github.com/owkin/pydeseq2
- **GseaPy**: https://gseapy.readthedocs.io/
- **DESeq2 Paper**: Love et al. (2014) Genome Biology
- **GSEA Method**: Subramanian et al. (2005) PNAS

---

## 📧 Contacto

Para preguntas o sugerencias sobre este pipeline, contactar a los autores.

---

*Last updated: 2026-06-02*
