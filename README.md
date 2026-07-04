# Análisis de Riesgo Crediticio: Predicción de Incumplimiento de Pago

Este proyecto desarrolla un modelo de Machine Learning para predecir el incumplimiento de pago de préstamos utilizando información demográfica, financiera y crediticia de los solicitantes. Se realiza un análisis exploratorio, preprocesamiento de datos, entrenamiento y evaluación de modelos de clasificación.

El objetivo del negocio es construir un modelo de clasificación capaz de estimar si un solicitante incumplirá el pago de un préstamo, proporcionando una herramienta de apoyo para la gestión del riesgo crediticio.

Dataset: https://www.kaggle.com/datasets/laotse/credit-risk-dataset

### EDA

Durante esta fase, se analizaron variables demográficas, financieras y de comportamiento crediticio para entender su impacto directo en la variable objetivo (`loan_status`). Los hallazgos más relevantes se dividen en tres pilares:

#### 1. Psicología e Intención del Cliente (`loan_intent` y `person_home_ownership`)
* **Proyectos Productivos vs. Deuda Previa:** La mayor tasa proporcional de default se concentra en clientes que solicitan el préstamo para **Consolidación de Deudas (`DEBT_CONSOLIDATION`)**, confirmando el riesgo de financiar perfiles previamente ahogados. En contraste, los créditos destinados a **Emprendimientos (`VENTURE`)** presentan la morosidad más baja, validando que el financiamiento productivo facilita el cumplimiento.
* **El Respaldo del Inmueble:** Los clientes con créditos hipotecarios activos (`MORTGAGE`) concentran sus calificaciones en los grados de mayor confianza del banco (Grado A), demostrando que poseer un activo (aunque esté financiado) es interpretado institucionalmente como un factor de estabilidad.

#### 2. Comportamiento Financiero y Umbrales de Riesgo (`loan_amnt`, `loan_int_rate` y `loan_percent_income`)
* **La Barrera de los \$22,500:** A mayor monto solicitado, el riesgo de impago escala drásticamente. El rango entre \$22,500 y \$30,000 representa un comportamiento normal para el grupo en default, mientras que para los clientes cumplidores ya es una anomalía estadística.
* **El Efecto de las Tasas Altas:** Los impagos se concentran fuertemente en tasas de interés superiores al `10.5%`. Esto advierte sobre el riesgo de generar "defaults inducidos" debido al propio costo y asfixia financiera del crédito.
* **Compromiso del Ingreso:** Al evaluar la variable `loan_percent_income`, el 50% central de los morosos compromete una porción de su sueldo muy superior a la mediana de los cumplidores. Se identificó el **20% del ingreso** como el límite crítico a partir del cual el riesgo de default se dispara.

#### 3. Calidad del Dato y Variables No Predictivas
* **Limpieza de Ruido (Outliers):** Se detectaron errores severos de registro en la base de datos que deben ser tratados antes del modelado, incluyendo clientes con edades de hasta 144 años y antigüedades laborales de 123 años.
* **Variables Clones:** Se identificó que la longitud del historial crediticio (`cb_person_cred_hist_length`) y la edad (`person_age`) presentan distribuciones idénticas tanto en clientes morosos como cumplidores, por lo que **no aportan valor predictivo** y son candidatas a exclusión para evitar ruido en el algoritmo.

### Modelado

    x

### Resultados

    x

### Conclusiones

    x