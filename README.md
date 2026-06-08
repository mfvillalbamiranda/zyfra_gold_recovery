<!--
# Descripción del proyecto

Zyfra es una empresa que desarrolla soluciones de eficiencia y seguridad para las industrias minera, petrolera, química y de la ingeniería. Una de las formas en las que se puede ayudar al mejoramiento de las industrias pesadas, es a través de la implementación de soluciones digitales, que permitan la optimización de la producción y eliminen parámetros no rentables. Es así que Zyfra requiere de la creación de un prototipo de modelo de machine learning que ayude a predecir la cantidad de oro extraído del mineral de oro.

Dentro del proceso de extracción de oro, el mineral extraído se somete a un tratamiento primario para obtener la materia prima o mezcla mineral que atravesará los procesos de flotación y purificación. Durante la flotación o proceso rougher, la mezcla mineral de oro se introduce en plantas de flotación, obteniendo dos productos importantes: el concentrado rougher (oro) y la cola rougher (residuos del producto). Posteriormente, el concentrado de oro se somete a dos etapas de purificación, a partir de la cual se obtendrá el concentrado final de oro y nuevas colas de residuos.

Considerando esto, se requerirá construir un modelo que pueda predecir los valores del concentrado de oro rougher y el concentrado final. Para lo cual se contará con la información sobre los residuos y concentrados producidos en cada etapa de extracción y sus diferentes parámetros.

## Objetivos
- Desarrollar un modelo predictivo que permita establecer la cantidad de oro extraído a partir del mineral oro, considerando los residuos y concentrados que se producen en cada etapa del proceso.
- Realizar el preprocesamiento de datos y comprobar que los cálculos de recuperación sean correctos.
- Analizar los datos a través de la exploración de distribuciones de metales y el tamaño de las particulas de alimentación.
- Entrenar diferentes modelos y evaluarlos a través de la métrica sMAPE (error medio absoluto porcentual simétrico) y la validación cruzada.
-->

## Objetivo de negocio
Zyfra, empresa desarrolladora de soluciones de eficiencia para industrias pesadas, requiere optimizar la producción minera eliminando parámetros no rentables en el procesamiento de oro. Su meta es crear un prototipo de modelo de aprendizaje automático (Machine Learning) capaz de predecir con alta fidelidad la cantidad de oro extraído del mineral original. Con este desarrollo se busca predecir con precisión los valores del concentrado de oro en la etapa bruta (rougher) y el concentrado final. Esto permitirá simulaciones operativas para optimizar los flujos de purificación, regular reactivos químicos y maximizar la recuperación económica sin desperdiciar recursos.

## Descripción del dataset
Se utilizan tres archivos CSV de registros mineros indexados por fecha y hora (date):
- gold_recovery_train.csv: Conjunto de entrenamiento (16,860 filas y 87 columnas).
- gold_recovery_test.csv: Conjunto de prueba (5,856 filas y 53 columnas).
- gold_recovery_full.csv: Conjunto de datos completo original (22,716 filas y 87 columnas).

Las columnas describen concentraciones porcentuales de metales (Au - oro, Ag - plata, Pb - plomo) y sólidos (sol), tamaños de partículas (feed_size), ritmos de alimentación (feed_rate), y adición de químicos (sulfatos, xantatos, depresores) distribuidos a lo largo del flujo técnico.

## Metodología
- Auditoría de Recuperación: Verificación analítica de los cálculos de laboratorio para la recuperación del oro bruto (rougher.output.recovery) utilizando la fórmula matemática estándar. El Error Absoluto Medio (EAM) obtenido entre los datos reales y el cálculo fue de $9.303 \times 10^{-15}$ (cercano a cero), ratificando que los registros históricos son consistentes y correctos.
- Preprocesamiento: Eliminación de filas con valores ausentes en las variables objetivo de entrenamiento y aplicación de la técnica Forward-fill (ffill()) para rellenar vacíos en el set de prueba debido a la contigüidad cronológica de las mediciones mineras.
- Análisis Exploratorio de Metales (EDA):
  - Concentración: Se analiza el comportamiento de los metales en cada fase (Mezcla $\rightarrow$ Rougher $\rightarrow$ Final). La concentración promedio de Oro (Au) aumenta exponencialmente un 544% al pasar de una media de 8.1% inicial a 44.1% en la purificación final. El plomo aumenta de 3.6% a 10.2%, mientras que la plata se reduce de forma variable hasta el 41% de su valor inicial.
  - Partículas: Se verifica que las distribuciones del tamaño de partícula de alimentación (feed_size) en el set de entrenamiento y prueba sean homólogas (baja variación), garantizando que el modelo sea evaluado bajo condiciones físicas estables.
  - Concentraciones totales: Se descartan anomalías y valores atípicos (como registros en 0% que representan fallas de sensores) para limpiar el entrenamiento.

## Modelos utilizados
Dado que el problema requiere predecir dos variables continuas e independientes de manera simultánea (rougher.output.recovery y final.output.recovery), se implementaron algoritmos de Regresión automatizada utilizando validación cruzada (cross_val_score):
- Regresión Lineal (LinearRegression)
- Árbol de Decisión para Regresión (DecisionTreeRegressor)
- Bosque Aleatorio para Regresión (RandomForestRegressor)

## Métricas
La efectividad del prototipo industrial se rige mediante el sMAPE (Symmetric Mean Absolute Percentage Error / Error Medio Absoluto Porcentual Simétrico). Evalúa la desviación porcentual entre los valores reales ($y$) y las predicciones ($\hat{y}$).La métrica final se pondera asignando el 25% del peso al error en la etapa de flotación (rougher) y el 75% al error de la etapa final de purificación:

$$ \mathrm{sMAPE}_{Final} = 0.25 \times \mathrm{sMAPE}_{rougher} + 0.75 \times \mathrm{sMAPE}_{final}
$$


## Resultados
- Durante el ciclo de experimentación y validación cruzada con la función personalizada de pérdida make_scorer, los algoritmos de ensamble (especialmente Bosque Aleatorio / RandomForest) demostraron la mejor capacidad de generalización frente a patrones complejos no lineales presentes en las mezclas químicas.
- El preprocesamiento avanzado (como el manejo idóneo de los nulos y la homologación de columnas entre el set de entrenamiento y prueba) permitió estabilizar el error absoluto porcentual en rangos altamente competitivos para la industria metalúrgica.

## Conclusiones
- El análisis exploratorio confirmó que el proceso físico-químico de las plantas de flotación de Zyfra funciona según la teoría metalúrgica: el contenido de oro se enriquece de etapa en etapa de forma progresiva mientras se descartan subproductos residuales (colas).
- El bajo valor de EAM en la etapa de control demostró que el pipeline de ingeniería de datos no arrastra errores de cálculo sistémicos desde el origen.
- Al desplegar este prototipo de Machine Learning entrenado, Zyfra puede predecir con antelación el rendimiento de la extracción y alertar a los operadores de planta sobre lotes de mineral cuya combinación de parámetros químicos (sulfatos, xantatos o tasas de alimentación) no sean rentables, permitiendo realizar ajustes en tiempo real y maximizar la recuperación final del oro.
