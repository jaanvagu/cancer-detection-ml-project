# Competición **Child Mind Institute — Problematic Internet Use**

## 1. Objetivo y contexto

La competición **Child Mind Institute — Problematic Internet Use** (PIU) fue organizada en Kaggle por el **Child Mind Institute** (CMI) con el apoyo de Dell Technologies y el Healthy Brain Network (HBN).  El reto planteaba predecir la **Severity Impairment Index (SII)** de niños y jóvenes a partir de datos de actividad física, mediciones de condición física y cuestionarios sobre uso de Internet.  La SII proviene de la **Parent‑Child Internet Addiction Test (PCIAT)** y se clasifica en cuatro niveles: **0 (None)**, **1 (Mild)**, **2 (Moderate)** y **3 (Severe)**【988202382092312†L680-L687】.  La competencia buscaba modelos capaces de **identificar signos tempranos de uso problemático de Internet** y ofrecía **datos etiquetados y sin etiquetar** para fomentar métodos supervisados y semi‑supervisados.

## 2. Conjunto de datos

La competencia utilizó datos procedentes del estudio **Healthy Brain Network (HBN)** de CMI.  Este proyecto reclutó alrededor de 5 000 participantes de **5 a 22 años** que se sometieron a evaluaciones clínicas, pruebas de fitness y cuestionarios.  Para la competición PIU se seleccionaron **3 960** registros (82 atributos tabulares) y **996** conjuntos de datos de actigrafía (series temporales)【988202382092312†L680-L695】.  Algunos detalles clave del conjunto de datos se resumen a continuación:

### 2.1 Variables tabulares (82 atributos)

Las variables estaban divididas en varias categorías (solo se muestran ejemplos representativos).  La lista completa aparece en la referencia【988202382092312†L702-L769】.

| Categoría | Ejemplos de variables (interpretación) | 
| --- | --- | 
| **Identificación y demografía** | `id` (identificador único), `Basic_Demos-Enroll_Season` (estación de enrolamiento), `Basic_Demos-Age` (edad) y `Basic_Demos-Sex` (sexo, 0 = varón, 1 = mujer)【988202382092312†L702-L706】. |
| **Uso de Internet** | `PreInt_EduHx-computerinternet_hoursday`, horas diarias de uso de ordenador/internet codificadas como: 0 = < 1 h/día, 1 = ≈1 h/día, 2 = ≈2 h/día, 3 = > 3 h/día【988202382092312†L708-L710】. |
| **Escalas de salud mental** | `CGAS-CGAS_Score`: escala global de evaluación infantil【988202382092312†L710-L712】; `SDS-SDS_Total_Raw` y `SDS-SDS_Total_T`: puntuaciones de la *Sleep Disturbance Scale*【988202382092312†L763-L765】. |
| **Medidas físicas** | `Physical-BMI` (índice de masa corporal), `Physical-Height` (altura), `Physical-Weight` (peso), `Physical-Waist_Circumference` (circunferencia de cintura), presión arterial diastólica/sistólica y pulso【988202382092312†L714-L721】. |
| **Pruebas de condición física (FitnessGram)** | Número de abdominales (`FGC-FGC_CU`), flexiones (`FGC-FGC_PU`), fuerza de agarre de la mano (dominante y no dominante) y zonas de condición física correspondientes, así como tiempo y etapa máxima en la prueba de resistencia en cinta (`Fitness_Endurance-Max_Stage`, `Time_Mins`, `Time_Sec`)【988202382092312†L722-L741】. |
| **Bioimpedancia (BIA)** | Medidas de composición corporal obtenidas mediante análisis de bioimpedancia: `BIA-BIA_BMC` (contenido mineral óseo), `BIA-BIA_BMR` (tasa metabólica basal), `BIA-BIA_DEE` (gasto energético diario), porcentaje de grasa corporal (`BIA-BIA_Fat`), masa magra (`BIA-BIA_FFM`) y agua corporal total (`BIA-BIA_TBW`)【988202382092312†L742-L758】. |
| **Cuestionarios de actividad física** | `PAQ_A-PAQ_A_Total` (puntuación de la Physical Activity Questionnaire para adolescentes) y `PAQ_C-PAQ_C_Total` (versión para niños)【988202382092312†L759-L762】. |
| **Cuestionario de adicción a Internet (PCIAT)** | Veinte ítems (`PCIAT-01 – PCIAT-20`) que miden compulsividad, evitación y dependencia del uso de Internet, así como el total de la escala (`PCIAT-PCIAT_Total`)【988202382092312†L766-L769】.  El total se convierte en la **SII** (0 – None, 1 – Mild, 2 – Moderate, 3 – Severe)【988202382092312†L680-L687】. |

### 2.2 Datos de actigrafía

Los datos de actigrafía proceden de un dispositivo **ActiGraph wGT3X‑BT** que los participantes llevaron en la muñeca durante hasta **30 días**.  Estos dispositivos registran aceleraciones en los ejes **X, Y y Z**, la magnitud del movimiento (**enmo**), el ángulo del brazo respecto al plano horizontal (**anglez**), una señal de luz ambiental, voltaje de batería, hora del día, día de la semana y un indicador de uso del dispositivo【988202382092312†L778-L797】.  Las muestras se obtienen cada **5 segundos** y, para algunas variables secundarias (luz y voltaje), se aplicó un *upsampling* para sincronizarlas con la frecuencia de muestreo【988202382092312†L978-L987】.

### 2.3 Problemas de faltantes y desbalanceo

El conjunto de datos presenta desafíos importantes:

- **Valores ausentes**: La SII está ausente en alrededor del **30 %** de los registros【988202382092312†L680-L687】.  Muchas variables tienen tasas de pérdida elevadas, de modo que el estudio “Risk level prediction for problematic internet use” reporta que dos tercios de los atributos presentan entre **30 % y 50 %** de valores faltantes【988202382092312†L880-L893】.  Para el procesamiento se proponen estrategias de imputación (media/mediana, *KNN*, *Iterative Imputation* o imputación múltiple)【988202382092312†L904-L913】.
- **Clases desbalanceadas**: La distribución de la SII en el conjunto completo es **0 (None): 58,26 %**, **1 (Mild): 26,28 %**, **2 (Moderate): 13,82 %** y **3 (Severe): 1,24 %**【988202382092312†L680-L687】, lo que dificulta la detección de casos severos.
- **Actigrafía parcial**: Solo **996** de las **3 960** personas cuentan con datos de actigrafía (≈33 % de la muestra)【988202382092312†L880-L892】.  Esto obliga a usar modelos capaces de integrar series temporales y datos tabulares o a generar características estadísticas de la serie【988202382092312†L990-L1000】.

## 3. Tarea de predicción

El objetivo era construir un modelo que, a partir de los **82 atributos tabulares** y/o las **series de actigrafía**, predijera el valor de la **SII**.  Esta tarea es un **problema de clasificación ordinal de cuatro clases** (0–3).  La competición permitía utilizar técnicas de **aprendizaje supervisado** con el conjunto etiquetado y **aprendizaje semi‑supervisado** con los registros sin SII.  El conjunto de test oculto contenía alrededor de **3 800** instancias sin etiquetas visibles para los participantes.

### 3.1 Métrica de evaluación

Las soluciones se evaluaron mediante la **Cohen’s Quadratic Weighted Kappa (QWK)**, una métrica que mide el grado de acuerdo entre las predicciones y las etiquetas reales en problemas de clasificación ordinal.  La QWK penaliza de forma más severa las discrepancias mayores entre categorías (por ejemplo, confundir la clase 0 con la 3 se penaliza más que confundir la 0 con la 1)【988202382092312†L1700-L1709】.  El artículo de Youngjung Suh et al. señala que la QWK es eficaz para medir el rendimiento en conjuntos desbalanceados, pues considera la distancia entre clases【988202382092312†L1700-L1709】.  No obstante, también advierten que el QWK no muestra información detallada sobre qué clases se predicen mal【988202382092312†L1518-L1524】; por ello algunas propuestas analizan tasas de error por clase y combinan QWK con otras métricas como F1 score.

## 4. Procesamiento y retos

Los competidores debían enfrentar varios retos:

1. **Integración de datos heterogéneos**: Los datos combinan series temporales (actigrafía) y variables tabulares.  Estudios como el de Suh et al. describen dos enfoques: (a) crear **características manuales** de la actigrafía (media, mediana, desviación típica, kurtosis, patrones de actividad/inactividad, periodicidad, etc.)【988202382092312†L990-L1002】, y (b) utilizar **redes auto‑codificadoras** para obtener vectores representativos a partir de la serie【988202382092312†L990-L1002】.
2. **Manejo de valores faltantes**: La alta proporción de datos faltantes obligó a aplicar métodos de imputación para preservar la mayor cantidad de muestras【988202382092312†L880-L893】.  También se eliminaron trece variables con demasiados vacíos y se redujo el número total de atributos a 67 en el preprocesado de algunos trabajos de investigación【498671014907858†L440-L471】.
3. **Desbalanceo de clases**: Al haber muy pocos casos severos, se exploraron técnicas de **ponderación de clases**, **SMOTE/ADASYN** y **pseudolabeling**.  El estudio de Suh et al. muestra que asignar pesos a las clases mejoró el QWK de 0,53 a 0,5515【988202382092312†L1596-L1603】, mientras que el sobresampling sintético tuvo impacto limitado【988202382092312†L1604-L1617】.
4. **Escalabilidad**: El archivo de actigrafía (`series_train.parquet`) es voluminoso, por lo que se requieren técnicas eficientes de lectura y procesamiento.  Muchos participantes utilizaron métodos de reducción de dimensionalidad (PCA, autoencoders) para reducir la dimensionalidad de la serie antes de combinarla con los datos tabulares.

## 5. Aspectos destacados de investigaciones y soluciones

- **Variables más predictivas**: La investigación de Suh et al. identificó que variables como la edad (`Basic_Demos-Age`), el sexo (`Basic_Demos-Sex`), las puntuaciones de sueño (`SDS-SDS_Total_T` y `SDS-SDS_Total_Raw`), el tiempo medio por etapa en la prueba de resistencia y las puntuaciones de fuerza abdominal (`FGC-FGC_CU`) tienen alta importancia en los modelos de LightGBM, XGBoost y CatBoost【988202382092312†L1787-L1813】.  La influencia de variables de sueño y algunos aspectos de la condición física indica que problemas de descanso y menor forma física podrían asociarse con un mayor uso problemático de Internet.
- **Participación de datos sin etiquetar**: Muchas soluciones aplicaron **aprendizaje semi‑supervisado** generando pseudo‑etiquetas para los registros sin SII y entrenando modelos con un conjunto expandido.  Los equipos que obtuvieron medallas en Kaggle (según reseñas de OpenReview) emplearon redes neuronales híbridas que combinaban **LSTM/GRU** para la serie de actigrafía y **redes densas** para datos tabulares, y optimizaron la función de pérdida en función de la QWK【514196895694307†L185-L199】.
- **Rendimiento en investigación académica**: En un estudio independiente sobre el mismo conjunto de datos, Malallah & Iqbal reportaron que tras limpiar los datos (reduciendo a 2 736 registros con 67 características), métodos de *Gradient Boosting Regressor* (GBR) obtenían **100 %** de precisión al evaluar la SII en un conjunto de prueba de 821 participantes【498671014907858†L440-L517】.  Otras técnicas como CNN 1D y LSTM lograron precisiones superiores al 94 %【498671014907858†L544-L576】.  Estos trabajos, sin embargo, no utilizan el QWK como métrica y pueden ser optimistas al evaluar sobre conjuntos reducidos.
- **Énfasis en la ética y salud mental**: Los organizadores del CMI subrayaron que el objetivo no era etiquetar o estigmatizar a los menores sino **desarrollar herramientas de cribado** basadas en datos fácilmente disponibles (actividad física y fitness), que puedan complementar el diagnóstico clínico en contextos donde no existe acceso a especialistas【498671014907858†L69-L74】.  De esta forma, la competición promueve tecnologías de salud digital que podrían integrarse en escuelas y programas comunitarios.

## 6. Conclusiones

La competición **Child Mind Institute — Problematic Internet Use** es un reto de **predicción ordinal** que combina datos de actividad física, composición corporal, cuestionarios de comportamiento y series temporales de acelerometría.  El propósito es predecir la severidad del uso problemático de Internet (SII) con técnicas que puedan ser aplicables en un entorno de salud digital.  Las principales dificultades radican en el **alto porcentaje de datos faltantes**, el **desbalanceo de clases** y la **integración de datos heterogéneos**.  La métrica de evaluación escogida, la **Cohen’s Quadratic Weighted Kappa**, mide el acuerdo teniendo en cuenta la gravedad de los errores【988202382092312†L1700-L1709】 y está alineada con la naturaleza ordinal del problema.  

Los trabajos de investigación y las soluciones de los participantes destacan la utilidad de 
- técnicas de **imputación avanzada** para manejar valores faltantes, 
- **reducción de dimensionalidad** y **auto‑codificadores** para procesar la actigrafía, 
- **modelos híbridos** que combinan redes neuronales recurrentes para series temporales y árboles de gradiente para datos tabulares, y 
- estrategias de **balanceo de clases** mediante ponderación o generación sintética de ejemplos.  

A pesar de las limitaciones de acceso a la página de Kaggle por cuestiones técnicas, los documentos científicos disponibles proporcionan una descripción detallada del conjunto de datos, los desafíos y las aproximaciones de modelado para este problema.
