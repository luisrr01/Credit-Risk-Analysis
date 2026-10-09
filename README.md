# 🏦 Scoring de Riesgo Crediticio: Predicción de Incumplimiento de Pago (Basel-Compliant)

## 📌 Contexto
Este proyecto desarrolla un **pipeline end-to-end de Scoring de Riesgo Crediticio** para predecir la probabilidad de incumplimiento de pago ($\text{PD}$) en préstamos personales. El objetivo principal es optimizar la toma de decisiones en la colocación de crédito, automatizando la aprobación/rechazo de solicitudes mediante reglas de negocio calibradas y minimizando las pérdidas por morosidad.

Dataset utilizado: [Credit Risk Dataset (Kaggle)](https://www.kaggle.com/datasets/laotse/credit-risk-dataset)

---

## 🧠 Enfoque Analítico y Metodología
Se implementó un pipeline robusto de Machine Learning diseñado bajo los estándares normativos de la banca (Basilea):
1. **EDA & Data Cleaning:** Tratamiento de inconsistencias de registros (outliers severos en edad y antigüedad laboral) y análisis discriminatorio de características.
2. **Benchmark Base (Fase 1):** Evaluación de modelos lineales (Regresión Logística) frente a algoritmos basados en ensambles de árboles (XGBoost y LightGBM).
3. **Optimización de Hiperparámetros (Fase 2):** Control de *overfitting* mediante `StratifiedKFold` y `GridSearchCV`, logrando alta generalización ($\Delta\text{Gini} \le 2.36\%$).
4. **Calibración Financiera (Fase 3):** Aplicación de Regresión Isotónica sobre las probabilidades brutas para obtener probabilidades reales e insumos válidos de Pérdida Esperada ($\text{EL}$).
5. **Política de Crédito & Cut-off:** Selección del umbral óptimo de decisión basado en el estadístico Kolmogorov-Smirnov ($KS$).
6. **Interpretabilidad & Gobernanza (SHAP):** Explicabilidad global del modelo y generación local de *Reason Codes* (razones de rechazo) para protección al consumidor financiero.

---

## 🛠️ Herramientas y Técnicas
- **Lenguaje & Entorno:** Python, Jupyter Notebook, VS Code
- **Manipulación & Análisis:** Pandas, NumPy
- **Visualización:** Matplotlib, Seaborn
- **Machine Learning & Calibración:** Scikit-Learn, XGBoost, LightGBM, `CalibratedClassifierCV` (Isotonic Regression)
- **Explicabilidad & Gobernanza:** SHAP (`TreeExplainer`, `SummaryPlot`, `WaterfallPlot`)
- **Métricas Bancarias:** Kolmogorov-Smirnov ($KS$), Gini Index ($2 \times \text{AUC} - 1$), Brier Score, Matriz de Confusión Operativa
- **Despliegue & Producción:** Serialización con `Joblib` (.joblib), simulación de API de scoring en tiempo real

---

## 📊 Principales Hallazgos del EDA
- **Nivel de Endeudamiento:** Un porcentaje de ingreso comprometido (`loan_percent_income`) superior al **20%** representa el umbral crítico donde el riesgo de mora se dispara exponencialmente.
- **Respaldo Inmobiliario:** Poseer vivienda propia (`person_home_ownership_OWN`) o crédito hipotecario activo actúa como el factor protector más potente contra el default.
- **Intención del Crédito:** Solicitudes para consolidación de deudas (`DEBT_CONSOLIDATION`) concentran las tasas de impago más altas, mientras que los créditos productivos (`VENTURE`) muestran la mejor tasa de cumplimiento.

---

## 📈 Resultados y Métricas del Modelo
El modelo final seleccionado fue **XGBoost Calibrado**, demostrando una capacidad de discriminación excepcional y alta estabilidad:

- **Estadístico $KS$ Máximo:** **$72.49\%$** *(Estatus de precisión de élite en banca)*
- **Estabilidad del Modelo ($\Delta\text{Gini}$):** **$2.36\%$** entre Train ($89.49\%$) y Test ($87.13\%$)
- **Calibración de Probabilidad ($\text{Brier Score}$):** **$0.0583$** *(Ajuste casi perfecto a la diagonal ideal)*
- **Cut-off Óptimo ($\text{PD}^*$):** **$26.83\%$**

### Matriz de Confusión Operativa (En Cartera de Test)
- **Tasa de Aprobación Proyectada:** **$78.47\%$** de las solicitudes
- **Tasa de Rechazo Proyectada:** **$21.53\%$** de las solicitudes
- **Captura de Morosos:** Intercepta y rechaza al **$78.2\%$** de los morosos reales ($1,112$ de $1,422$)
- **Reducción de Mora:** Reduce el *Bad Rate* natural del mercado del **$21.80\%$** a solo un **$6.06\%$** en la cartera de créditos colocados

---

## 🚀 Aplicación al Negocio e Impacto Financiero
El pipeline desarrollado permite:
- **Automatización de Decisiones de Crédito:** Regla directa de decisión ($\text{Si } \text{PD} \le 26.83\% \implies \text{Aprobar}$).
- **Mitigación del Riesgo de Crédito:** Mantiene una alta colocación comercial ($78.47\%$) blindando el balance del banco con una mora residual controlada de solo $6.06\%$.
- **Explicabilidad Transparente (*Reason Codes*):** Generación automática de razones normativas de rechazo mediante valores SHAP individuales para cada solicitud denegada.
- **Despliegue Listo para Producción:** Módulo empaquetado (`credit_risk_pipeline_xgb.joblib`) con función de scoring en tiempo real para integración vía API REST o microservicios.

---

## 📚 Aprendizajes Clave
- Importancia de la calibración de probabilidades en modelos de Machine Learning para su uso en modelos de Pérdida Esperada ($\text{EL}$).
- Optimización de modelos basados en árboles para cumplir estándares de estabilidad y evitar *overfitting* en entorno bancario.
- Uso del estadístico $KS$ para la fijación estratégica del Cut-off según el apetito de riesgo de la institución.
- Aplicación práctica de SHAP Value para cumplir normativas de auditabilidad, ética e interpretabilidad en modelos de caja negra.