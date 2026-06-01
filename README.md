# Descripción del proyecto

Zyfra es una empresa que desarrolla soluciones de eficiencia y seguridad para las industrias minera, petrolera, química y de la ingeniería. Una de las formas en las que se puede ayudar al mejoramiento de las industrias pesadas, es a través de la implementación de soluciones digitales, que permitan la optimización de la producción y eliminen parámetros no rentables. Es así que Zyfra requiere de la creación de un prototipo de modelo de machine learning que ayude a predecir la cantidad de oro extraído del mineral de oro.

Dentro del proceso de extracción de oro, el mineral extraído se somete a un tratamiento primario para obtener la materia prima o mezcla mineral que atravesará los procesos de flotación y purificación. Durante la flotación o proceso rougher, la mezcla mineral de oro se introduce en plantas de flotación, obteniendo dos productos importantes: el concentrado rougher (oro) y la cola rougher (residuos del producto). Posteriormente, el concentrado de oro se somete a dos etapas de purificación, a partir de la cual se obtendrá el concentrado final de oro y nuevas colas de residuos.

Considerando esto, se requerirá construir un modelo que pueda predecir los valores del concentrado de oro rougher y el concentrado final. Para lo cual se contará con la información sobre los residuos y concentrados producidos en cada etapa de extracción y sus diferentes parámetros.

## Objetivos
- Desarrollar un modelo predictivo que permita establecer la cantidad de oro extraído a partir del mineral oro, considerando los residuos y concentrados que se producen en cada etapa del proceso.
- Realizar el preprocesamiento de datos y comprobar que los cálculos de recuperación sean correctos.
- Analizar los datos a través de la exploración de distribuciones de metales y el tamaño de las particulas de alimentación.
- Entrenar diferentes modelos y evaluarlos a través de la métrica sMAPE (error medio absoluto porcentual simétrico) y la validación cruzada.
