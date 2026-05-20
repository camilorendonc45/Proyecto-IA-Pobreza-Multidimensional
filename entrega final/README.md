# Predicción de Transiciones de Privación Básica en Hogares Colombianos
### Un enfoque longitudinal con la ELCA (2010–2016)

**Autores:** Talia Linares · Camilo Rendón · Nicolás Cubillos · Alejandro Velandia  
**Curso:** Inteligencia Artificial con Aplicaciones en Economía I  
**Universidad Externado de Colombia · 2026**

---

## 🌍 Descripción y Relevancia Económica

Más de seis millones de colombianos presentan privaciones simultáneas en educación, salud y vivienda. Este proyecto construye un **Índice de Privaciones Básicas del Hogar (IPBH)** con datos de la Encuesta Longitudinal Colombiana (ELCA, 2010–2016) y modela las **transiciones entre cuatro estados de privación** (estable, vulnerable, movilidad ascendente y crónico) para 16.957 hogares.

Se incorporan 14 variables nuevas sobre choques climáticos, resiliencia financiera, subsidios del Estado y activos productivos. Se comparan cuatro modelos mediante **GridSearchCV con validación cruzada estratificada (k=5)**: Regresión Logística, Random Forest, Gradient Boosting y **XGBoost**, usando AUC-ROC como métrica principal. El **XGBoost** obtiene el mejor desempeño **(AUC-ROC CV: 0.9223, Test: 0.9245)**. El hallazgo principal es que la **Riqueza (PCA)** es el predictor dominante en los 3 modelos de árbol (RF: 59.7%, GB: 84.8%, XGB: 50.8%), confirmando que la acumulación patrimonial es el principal mecanismo de protección frente a la privación.

---

## 📂 Estructura del Repositorio
