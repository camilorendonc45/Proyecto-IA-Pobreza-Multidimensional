# Predicción de Transiciones de Privación Básica en Hogares Colombianos
### Un enfoque longitudinal con la ELCA (2010–2016)

**Autores:** Talia Linares · Camilo Rendón · Nicolás Cubillos · Alejandro Velandia  
**Curso:** Inteligencia Artificial con Aplicaciones en Economía I  
**Universidad Externado de Colombia · 2026**

---

## 🌍 Descripción y Relevancia Económica

Más de seis millones de colombianos presentan privaciones simultáneas en educación, salud y vivienda. Este proyecto construye un **Índice de Privaciones Básicas del Hogar (IPBH)** con datos de la Encuesta Longitudinal Colombiana (ELCA, 2010–2016) y modela las **transiciones entre cuatro estados de privación** (estable, vulnerable, movilidad ascendente y crónico) para 16.957 hogares.

Se incorporan 14 variables nuevas sobre choques climáticos, resiliencia financiera, subsidios del Estado y activos productivos. Se comparan cuatro modelos mediante **GridSearchCV con validación cruzada estratificada (k=5)**: Regresión Logística, Random Forest, Gradient Boosting y **XGBoost**, usando AUC-ROC como métrica principal. El **XGBoost** obtiene el mejor desempeño **(AUC-ROC CV: 0.9223, Test: 0.9245)**. El hallazgo principal es que la **Riqueza (PCA)** es el predictor dominante en los 3 modelos de árbol (59.7% en RF, 84.8% en GB, 50.8% en XGB), confirmando que la acumulación patrimonial es el principal mecanismo de protección frente a la privación.

---

## 📂 Estructura del Repositorio

```
├── data/
│   ├── base_final_modelo.csv        # Base original ELCA (ver enlace Drive abajo)
│   └── base_con_variables.csv       # Base con IPBH + variables nuevas (generada por notebook 02)
├── notebooks/
│   ├── 01_limpieza_COLAB.ipynb      # Carga, limpieza y unión de módulos ELCA → base_final_modelo.csv
│   ├── 02_construccion_ipbh.ipynb   # IPBH corregido + 14 variables nuevas → base_con_variables.csv
│   ├── 03_eda.ipynb                 # Análisis exploratorio (8 gráficas)
│   └── 04_modelo.ipynb              # GridSearchCV, 4 modelos, AUC-ROC, curva ROC, importancia, betas
├── outputs/
│   ├── eda_transiciones.png
│   ├── eda_ipbh.png
│   ├── eda_activos_riqueza.png
│   ├── eda_estrato.png
│   ├── eda_choques.png
│   ├── eda_resiliencia_subsidios.png
│   ├── eda_brecha_rural.png
│   ├── eda_correlaciones.png
│   ├── comparacion_modelos_cv.png
│   ├── matriz_confusion_mejor_modelo.png
│   ├── curva_roc.png
│   ├── importancia_variables.png
│   └── betas_regresion_logistica.png
├── docs/
│   ├── documento_analisis_final.pdf   # Documento de análisis (2.200–2.500 palabras)
│   └── presentacion_final.pptx        # Presentación de clase
├── README.md
└── requirements.txt
```

---

## ⚙️ Instrucciones de Reproducibilidad

### Opción A — Google Colab (recomendada)

1. Descarga los notebooks de la carpeta `notebooks/`
2. Abre [colab.research.google.com](https://colab.research.google.com)
3. Sube cada notebook: **Archivo → Subir notebook**
4. Corre en este orden exacto:

```
1. 01_limpieza_COLAB.ipynb
   → Conecta con Google Drive donde están los archivos originales de la ELCA
   → Limpia, une y construye la variable dependiente
   → Genera y guarda: data/base_final_modelo.csv

2. 02_construccion_ipbh_COLAB.ipynb
   → Sube base_final_modelo.csv cuando lo pida (o usa la misma sesión)
   → Corrige el índice (IPBH, denominador=3) y construye 14 variables nuevas
   → Genera y descarga: base_con_variables.csv

3. 03_eda_COLAB.ipynb
   → Sube base_con_variables.csv cuando lo pida
   → Genera y descarga 8 gráficas del EDA

4. 04_modelo_COLAB.ipynb
   → Sube base_con_variables.csv cuando lo pida
   → GridSearchCV con cv=5 para 4 modelos
   → Genera y descarga 5 gráficas de resultados
```

> ℹ️ Si corres los notebooks en la misma sesión de Colab sin cerrar, los archivos ya están disponibles y no necesitas subirlos de nuevo.

> ℹ️ El notebook 01 requiere acceso a Google Drive con los archivos originales de la ELCA organizados como: `ELCA_proyecto/2010/UHogar10.csv`, `ELCA_proyecto/2013/UHogar13.csv`, etc.

### Opción B — Entorno local

```bash
# 1. Clonar el repositorio
git clone https://github.com/[usuario]/proyecto-elca.git
cd proyecto-elca

# 2. Crear entorno virtual
python -m venv env
source env/bin/activate        # Mac/Linux
env\Scripts\activate           # Windows

# 3. Instalar dependencias
pip install -r requirements.txt
pip install jupyter ipykernel

# 4. Abrir Jupyter
jupyter notebook
```

Correr los notebooks en el mismo orden que en Colab.

---

## 📊 Datos y Fuentes

| Atributo | Detalle |
|---|---|
| **Fuente** | Encuesta Longitudinal Colombiana (ELCA) — Universidad de los Andes, CEDE |
| **Rondas** | 2010, 2013 y 2016 |
| **Módulos** | UHogar, RHogar, UPersonas, RPersonas, UGastos, RGastos, UChoques, RChoques |
| **Unidad de observación** | Hogar (pares de períodos) |
| **Observaciones** | 16.957 pares hogar-período |
| **Variables originales** | 178 (tras limpieza y consolidación) |
| **Variables en el modelo** | 26 (12 base + 14 nuevas) |
| **Datos** | [Enlace a Google Drive](https://drive.google.com) ← reemplazar con enlace real |
| **Fuente oficial ELCA** | https://datoscede.uniandes.edu.co |

---

## 🏆 Resultados Clave

### Comparación de modelos (AUC-ROC)

| Modelo | AUC-ROC (CV k=5) | AUC-ROC (Test) | Mejores hiperparámetros |
|---|---|---|---|
| Regresión Logística | 0.8983 | 0.8962 | C=10 |
| Random Forest | 0.9146 | 0.9164 | n_estimators=200, max_depth=10 |
| Gradient Boosting | 0.9209 | 0.9233 | n_estimators=100, lr=0.05, depth=5 |
| **XGBoost ★** | **0.9223** | **0.9245** | n_estimators=200, lr=0.1, depth=3 |

### Hallazgos principales

- **La Riqueza (PCA) es el predictor dominante** en los 3 modelos de árbol (RF: 59.7%, GB: 84.8%, XGB: 50.8%) — la acumulación patrimonial es el principal determinante de la trayectoria del hogar
- **El 77.5% de los hogares no cambia de estado** entre períodos — evidencia de trampas de privación estructurales
- **Excelente predicción para estados persistentes** (Estable: 97.8%, Crónico: 87.7%), pero limitada para transiciones dinámicas (Vulnerable: 12.9%) porque dependen de eventos imprevistos
- **Brecha de resiliencia rural:** índice de 0.24 vs 0.63 urbano (brecha de 2.6×) — solo el 10.9% tiene acceso a crédito formal
- **Los crónicos reciben más subsidios** (64.5%) pero siguen en privación — la focalización llega pero no alcanza

### Recomendaciones de política pública

1. **Focalización preventiva:** dirigir Familias en Acción y Red Juntos hacia hogares con baja riqueza y alta exposición a choques climáticos — antes de que caigan, no después
2. **Crédito formal rural:** solo el 10.9% tiene acceso; la banca de proximidad y fondos de garantía son la intervención más directa
3. **Infraestructura habitacional:** la privación en vivienda (41.3%) es la más extendida — invertir en acueducto, alcantarillado y pisos
