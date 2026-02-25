# 📉 Modelamiento Predictivo de la Vulnerabilidad Multidimensional para la Optimización de la Política Social en Colombia

**👥 Integrantes:** Camilo Rendon, Nicolás cubillos y Talia Linares

---

## 📌 Descripción del Problema y Objetivos

* **🎯 Objetivo:** Desarrollar un modelo predictivo sobre la pobreza multidimensional en el cual se busca identificar y estimar la probabilidad de que un hogar caiga o salga de la situación de pobreza multidimensional en Colombia a partir de variables socioeconómicas, demográficas y de acceso a servicios básicos.
* Este modelo busca anticipar escenarios de vulnerabilidad social, mejorar la focalización de políticas públicas y programas sociales, y apoyar la toma de decisiones basada en evidencia por parte de las instituciones del Estado.
* Asimismo, el modelo permite analizar la importancia relativa de los distintos determinantes de la pobreza multidimensional, contribuyendo a una comprensión más profunda de sus causas estructurales y facilitando el diseño de estrategias de intervención más eficientes y equitativas.

### 🔍 Descripción del Problema
* En Colombia existen brechas estructurales profundas; para el año 2024, más de seis millones de personas permanecen en privaciones simultáneas en salud, educación y vivienda.
* Los sistemas actuales tienen fallas críticas; por ejemplo, el **94% de los hogares no pobres están en estratos subsidiados**.
* Constituye una problemática persistente que refleja privaciones simultáneas en dimensiones esenciales del bienestar, como educación, salud, empleo y condiciones de vivienda.
* Estas carencias se concentran especialmente en poblaciones rurales y territorios con rezago histórico, lo que evidencia profundas desigualdades sociales y regionales.
* La ausencia de herramientas predictivas limita la capacidad de anticipar qué hogares enfrentan mayor riesgo de pobreza multidimensional, reduciendo la efectividad de la focalización de políticas públicas.
* Por ello, resulta fundamental desarrollar modelos predictivos basados en datos que permitan identificar oportunamente situaciones de vulnerabilidad y apoyar el diseño de intervenciones más eficientes y equitativas.

---

## 🛠️ Desafíos Técnicos

### ⚖️ Desbalance Asimétrico de Clases y Altos Costos Sociales de los Falsos Negativos
* La vulnerabilidad multidimensional presenta una distribución altamente asimétrica dentro del universo de hogares colombianos.
* La clase “vulnerable” representa una proporción significativamente menor frente a la clase “no vulnerable”, lo que genera un problema clásico de desbalance de clases.
* El costo social de un falso negativo es considerablemente superior al de un falso positivo, ya que implica la exclusión de programas sociales y el aumento de la desigualdad territorial.
* Se requiere rediseñar la estrategia de entrenamiento mediante:
    * Ajuste de pesos en la función de pérdida (*cost-sensitive learning*).
    * Uso de parámetros de penalización diferenciada en **XGBoost** (*scale_pos_weight*).
    * Implementación de técnicas de re-muestreo controlado cuando sea pertinente.
    * Priorización de métricas como **Recall y F2-score** sobre la simple exactitud (accuracy).

### 📡 Ruido y Baja Calidad en la Interoperabilidad de Registros Masivos
* La integración de fuentes oficiales heterogéneas (ECV, GEIH, RSH y datos espaciales) introduce complejidades estructurales.
* Los desafíos incluyen diferencias en periodicidad, inconsistencias operativas y datos faltantes no aleatorios.
* El proceso de desarrollo incluirá armonización conceptual de variables, técnicas de imputación múltiple y validación cruzada entre fuentes.

### 📍 Integración de Factores de Expansión y Heterogeneidad Espacial
* Las encuestas del DANE incorporan factores de expansión para extrapolar resultados a nivel poblacional.
* Se implementará entrenamiento ponderado utilizando parámetros *sample_weight* y validación cruzada estratificada respetando el diseño muestral para capturar el contexto territorial.

---

## 🚀 Entrega de Valor
* Este modelo permite proteger a los hogares más vulnerables en Colombia, evitando que las privaciones en educación, salud, empleo y condiciones de vivienda se agraven.
* Facilita que las intervenciones lleguen antes a niños, jóvenes y adultos en contextos de alta vulnerabilidad, reduciendo la transmisión intergeneracional de la pobreza.
* Permite que las entidades actúen de manera proactiva, priorizando intervenciones antes de que se consoliden privaciones múltiples.

---

## 👥 Grupos de Interés (Stakeholders)
* **🏢 Entidades Nacionales:** Departamento Nacional de Planeación (DNP), DANE y Departamento para la Prosperidad Social.
* **📍 Gobiernos Territoriales:** Para la asignación eficiente de recursos en las regiones.
* **🌍 Organismos Internacionales:** Interesados en modelos predictivos de movilidad social (Banco Mundial, PNUD).

---

## 📊 Técnicas a Utilizar
* **📈 Regresión Logística:** Para interpretación económica y análisis de incidencia de variables.
* **🌲 Random Forest:** Para capturar relaciones no lineales e interacciones más robustas entre las variables explicativas.

---

## 📂 Datos y Variables

### 📋 Fuentes de Datos Oficiales
* Encuesta Nacional de Calidad de Vida (ECV) y Gran Encuesta Integrada de Hogares (GEIH) del DANE.
* Registro Social de Hogares (RSH) del DNP y variables espaciales de observación terrestre.
* Universidad de los Andes. (2021). Encuesta Longitudinal Colombiana (ELCA)

### 🗂️ Variables Seleccionadas

# Estructura de Variables: Modelo Predictivo de Transiciones de Pobreza

## 1. Variable de Respuesta ($y$)
Es el objetivo de predicción del modelo (Target). Define la trayectoria del hogar entre dos rondas de la ELCA (Ronda $T$ y Ronda $T+1$).

| Categoría | Nombre Técnico | Condición (Umbral IPM: 0.33) | Descripción Social |
| :--- | :--- | :--- | :--- |
| **0** | **Resiliente** | $IPM_T < 0.33$ y $IPM_{T+1} < 0.33$ | Hogares que se mantienen fuera de la pobreza. |
| **1** | **Vulnerable** | $IPM_T < 0.33$ y $IPM_{T+1} \geq 0.33$ | **Entrada:** Hogares que caen en pobreza (Foco del modelo). |
| **2** | **Movilidad** | $IPM_T \geq 0.33$ y $IPM_{T+1} < 0.33$ | **Salida:** Hogares que logran superar la pobreza. |
| **3** | **Persistente** | $IPM_T \geq 0.33$ y $IPM_{T+1} \geq 0.33$ | **Crónica:** Hogares atrapados en la pobreza. |

---

## 2. Variables de Control ($X_{control}$)
Características estructurales utilizadas para garantizar que el modelo compare perfiles similares y aísle efectos externos.

### Geográficas
* **Zona:** Urbana o Rural (Cabecera vs. Resto).

### Demográficas (Jefe de Hogar)
* **Sexo:** Masculino o Femenino (Control de brecha de género).
* **Edad:** Rango etario (18-28, 29-45, 46-60, 60+).
* **Etnia:** Identificación étnica (Indígena, Afro, Rrom, Ninguna).

### Estructura del Hogar
* **Tamaño del Hogar:** Número total de integrantes.
* **Tasa de Dependencia:** Relación entre niños/ancianos y adultos en edad de trabajar.

---

## 3. Variables Independientes

### A. Indicadores Base de la ELCA
* **Educación:** Bajo logro educativo del jefe y analfabetismo.
* **Salud:** Falta de aseguramiento (EPS) y barreras de acceso al servicio.
* **Trabajo:** Empleo informal y desempleo de larga duración.
* **Vivienda:** Hacinamiento crítico, materiales de pisos/paredes.
* **Servicios Públicos:** Falta de acceso a agua potable y alcantarillado.
* **Niñez/Juventud:** Inasistencia escolar, rezago y trabajo infantil.

### B. Factores de Vulnerabilidad
* **Acceso a Internet:** Dicotómica (0/1). Predictor de capital informativo.
* **Ingreso Monetario:** Ingreso neto per cápita del hogar.
* **Tenencia de Vivienda:** Propia, arriendo o usufructo (Estabilidad de activos).
* **Posesión de Activos:** Conteo de bienes (Computador, moto, cuenta de ahorros).
* **Seguridad Alimentaria:** Indicador de choque extremo por falta de recursos.

---


## 📚 Fuentes Bibliográficas
* [Colombia MPI - OPHI](https://ophi.org.uk/national-mpi-directory/colombia-mpi)
* [DANE - Índice de Pobreza Multidimensional 2022](https://microdatos.dane.gov.co/index.php/catalog/792)
* [Visor Geográfico de Pobreza Multidimensional](https://centralpdet.renovacionterritorio.gov.co/visor-geografico-de-pobreza-multidimensional/)
* Universidad de los Andes. (2021). Encuesta Longitudinal Colombiana (ELCA) [Conjunto de datos]. Facultad de Economía, Centro de Datos. https://economia.uniandes.edu.co/elca
