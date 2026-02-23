<img width="886" height="935" alt="image" src="https://github.com/user-attachments/assets/706760f5-13b3-4ed4-b9d4-a499f4f886df" />
EDA:
Daniel Carrasco
Alejandro Cantero

📊 Análisis Exploratorio de Datos (EDA) 
1. Análisis de la Variable Objetivo (Balance de Clases)
El primer paso crítico es evaluar la viabilidad del aprendizaje supervisado mediante el balance de clases.
•	Estado del Dataset: Se observa un equilibrio casi óptimo, con una distribución de 51.8% (comestibles) frente a 48.2% (venenosas).
•	Implicación Técnica: Al no existir un sesgo significativo, no se requieren técnicas de remuestreo como SMOTE ni ajustes en la función de pérdida del modelo. Los algoritmos podrán aprender las características de ambas clases con la misma prioridad.

(¿Qué es SMOTE?
SMOTE (Synthetic Minority Over-sampling Technique) es una técnica que se usa cuando tienes un dataset muy "cojo" o desbalanceado.
Imagina que quieres entrenar una IA para detectar incendios:
•	Tienes 1,000 fotos de bosques normales.
•	Solo tienes 10 fotos de bosques quemándose.
Si entrenas al modelo así, la IA se volverá "perezosa" y dirá que nunca hay fuego, porque acertará el 99% de las veces por pura estadística. SMOTE soluciona esto creando datos falsos (sintéticos) de la clase minoritaria (el fuego) basándose en los que ya existen para igualar las cantidades.)


<img width="721" height="513" alt="image" src="https://github.com/user-attachments/assets/7477ea33-9689-46b5-8e29-b3010b2aed7e" />


 
Conclusión: El dataset presenta un equilibrio casi perfecto (51.8% vs 48.2%). No se requieren técnicas de sobremuestreo (SMOTE) ni ajustes por desbalanceo. Los modelos de clasificación podrán aprender ambas clases por igual.








2. Análisis de Predictores Clave: El Factor "Olor"
Científicamente, la variable odor se identifica como el descriptor con mayor poder discriminatorio del conjunto de datos.
•	Segmentación Determinista:
o	Toxicidad Absoluta (100%): Olores fétido, acre, pescado y picante.
o	Inocuidad Absoluta (100%): Olores almendra y anís.
•	Punto de Incertidumbre: La categoría "Ninguno" es la única que presenta una distribución mixta, lo que justifica la necesidad de modelos de mayor complejidad o técnicas de Clustering para resolver la ambigüedad en estos ejemplares

<img width="886" height="479" alt="image" src="https://github.com/user-attachments/assets/49ab8776-956a-412a-b470-2473b08bd513" />

 
Conclusión: Se observa una segmentación casi perfecta. Los olores fétido, acre, pescado y picante son 100% asociados a veneno. Los olores almendra y anís son 100% seguros. El olor "Ninguno" es el único que presenta ambigüedad, lo que justifica el uso de modelos complejos para desempatar estos casos.







3. Análisis de la Superficie y el Color del Sombrero
Se evaluó el impacto de la apariencia externa (cap-surface y cap-color) en la predicción de toxicidad.
•	Hallazgo: Contrario al conocimiento popular, los colores vibrantes (amarillo, rojo) no son indicadores fiables de veneno, pues abundan setas comestibles con estas características.
•	Estrategia Sugerida: Dado que ninguna característica visual es determinante por sí sola, se recomienda el uso de PCA (Análisis de Componentes Principales) para sintetizar múltiples variables en dimensiones de mayor valor explicativo.

¿Influye el aspecto visual en la seguridad de la seta?

 <img width="886" height="329" alt="image" src="https://github.com/user-attachments/assets/6e0843bf-2229-44a6-9bf8-638f0505bccc" />


Conclusión: Contrario al mito popular, colores brillantes como el rojo o amarillo no garantizan que la seta sea venenosa (hay muchas comestibles con esos colores). La superficie tampoco es un indicador definitivo por sí sola, lo que refuerza la necesidad de técnicas de PCA para combinar múltiples variables en un solo análisis.








4. Análisis de Correlación de Variables (Heatmap de Contingencia)
Debido a la naturaleza cualitativa de los datos, se emplearon matrices de asociación (V de Cramér) para identificar dependencias.
•	Variables de Alto Impacto: Además del olor, las características relacionadas con el anillo (ring-type) y las láminas (gill-size) presentan las correlaciones más robustas con la variable objetivo.
•	Selección de Modelo: Esta estructura de datos sugiere que modelos basados en árboles, como Random Forest, otorgarán un peso jerárquico prioritario a estas variables, facilitando una clasificación precisa.

<img width="886" height="329" alt="image" src="https://github.com/user-attachments/assets/67c8a20a-80e2-459e-9d15-8047ed6a7455" />

 
Conclusión: Las variables relacionadas con el anillo (ring-type) y las laminas (gill-size) muestran las correlaciones más altas con la toxicidad. Esto sugiere que el modelo de Random Forest les dará un peso prioritario.


5. Resumen Ejecutivo del EDA 
Calidad del Dato: No hay valores nulos convencionales, pero el 30% de la columna stalk-root está oculto tras el carácter ?. Se ha decidido mantenerlo como categoría 'unknown' para no perder volumen de datos.
1.	Redundancia: La variable veil-type es constante; se elimina para evitar ruido en el modelo y problemas de división por cero en algunos algoritmos.
2.	Predictibilidad: El dataset es altamente separable. La fuerte correlación de la variable odor sugiere que incluso modelos simples podrían tener un accuracy alto, pero el Clustering será vital para entender la estructura de las setas sin olor definido.

<img width="422" height="293" alt="image" src="https://github.com/user-attachments/assets/9a310860-4e09-46d7-a297-02bfc5118255" />











Como análisis final hicimos una matris de asociación en la V de Cramer.
Basado en los valores de la matriz, se extraen los siguientes hallazgos estratégicos:
•	Identificación del Predictor Maestro: La variable spore-print-color (color de la espora) muestra la asociación más alta con la toxicidad (poisonous) con un valor de 0.75. Esto indica que es una de las características más determinantes para clasificar una seta como venenosa o comestible.
•	Relevancia de las Láminas: La variable gill-size (tamaño de la lámina) presenta una correlación significativa de 0.54 con la toxicidad. Esto sugiere que el modelo de Machine Learning (como un Random Forest) le asignará una importancia prioritaria.
•	Redundancia Detectada: Existe una asociación casi perfecta (0.95) entre veil-color y gill-attachment. Esto señala una redundancia de información; en un modelo avanzado, se podría considerar eliminar una de las dos para simplificar el algoritmo sin perder precisión.
•	Variables de Bajo Impacto: Características como cap-shape (0.24) o cap-surface (0.20) tienen asociaciones débiles con la toxicidad por sí solas. Esto refuerza la idea de que la apariencia externa no es un indicador definitivo y se requiere un análisis multivariable.
Nota Estratégica: La alta separabilidad que muestran estas variables sugiere que el dataset es ideal para modelos de clasificación, permitiendo alcanzar métricas de precisión muy elevadas rápidamen te

<img width="826" height="643" alt="image" src="https://github.com/user-attachments/assets/0a65bccb-3abd-4b99-af2b-d72a7257dc73" />
