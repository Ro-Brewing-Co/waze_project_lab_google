# waze_project_lab_google
Proyecto finalde "Los entresijos del aprendizaje automático", curso número 6 del certificado profesional de Google: "Google Advanced Data Analytics". El objetivo es desarrollar un modelo de machine learning que permita predecir si un usuario de Waze abandonará o no la aplicación, para ayudar a la empresa a mejorar sus estrategias de retención.

---

## Proyecto de abandono de usuarios – Fase final

Tu equipo está cerca de completar su proyecto sobre el abandono de usuarios. Anteriormente, completaron una propuesta de proyecto y utilizaron Python para explorar y analizar los datos de usuarios de Waze, crear visualizaciones de datos y realizar una prueba de hipótesis. Más recientemente, construyeron un modelo de regresión logística binomial basado en múltiples variables.

La directiva aprecia todo el arduo trabajo que han realizado. Ahora, quieren que el equipo construya un modelo de aprendizaje automático para predecir el abandono de usuarios. Para obtener los mejores resultados, su equipo ha decidido construir y probar dos modelos basados en árboles: **Random Forest** y **XGBoost**.

Su trabajo ayudará a la dirección a tomar decisiones empresariales informadas para prevenir el abandono de usuarios, mejorar la retención y hacer crecer el negocio de Waze.

---
Aquí tienes la traducción completa al español en formato **Markdown**:

---

## Proyecto Final del Curso 6: Construcción de un modelo de aprendizaje automático

En esta actividad, practicarás el uso de técnicas de modelado basadas en árboles para predecir una clase objetivo binaria.

**El propósito de este modelo** es encontrar los factores que impulsan el abandono de usuarios.

**El objetivo del modelo** es predecir si un usuario de Waze será retenido o abandonará la plataforma.

### Esta actividad consta de tres partes:

---

### Parte 1: Consideraciones éticas

* Considera las implicaciones éticas de la solicitud.
* ¿Debería ajustarse el objetivo del modelo?

---

### Parte 2: Ingeniería de características

* Realiza la selección, extracción y transformación de características para preparar los datos para el modelado.

---

### Parte 3: Modelado

* Construye los modelos, evalúalos y asesora sobre los próximos pasos.

---

Sigue las instrucciones y responde a las preguntas que se presentan a continuación para completar la actividad. Luego, deberás completar un **Resumen Ejecutivo** utilizando las preguntas enumeradas en el **Documento de Estrategia PACE**.

Asegúrate de completar esta actividad antes de continuar. El siguiente elemento del curso te proporcionará un ejemplo completo para que lo compares con tu propio trabajo.


---

## Construcción de un modelo de aprendizaje automático

### Etapas PACE

A lo largo de estos cuadernos de proyecto, verás referencias al marco de resolución de problemas **PACE**. Los siguientes componentes del cuaderno están etiquetados con la etapa PACE correspondiente: **Planificar (Plan), Analizar (Analyze), Construir (Construct)** y **Ejecutar (Execute)**.

---

## PACE: Planificar (Plan)

Considera las preguntas de tu **Documento de Estrategia PACE** para reflexionar sobre la etapa de Planificación.

En esta etapa, reflexiona sobre las siguientes preguntas:

* ¿Qué se te está pidiendo que hagas?
* ¿Cuáles son las implicaciones éticas del modelo? ¿Cuáles son las consecuencias si tu modelo comete errores?
* ¿Cuál es el efecto probable del modelo cuando predice un **falso negativo** (es decir, cuando el modelo dice que un usuario de Waze no abandonará, pero en realidad sí lo hará)?
* ¿Cuál es el efecto probable del modelo cuando predice un **falso positivo** (es decir, cuando el modelo dice que un usuario de Waze abandonará, pero en realidad no lo hará)?
* ¿Superan los beneficios de dicho modelo a los posibles problemas?
* ¿Procederías con la solicitud de construir este modelo? ¿Por qué sí o por qué no?


---

## Tarea 1. Importaciones y carga de datos

Importa los paquetes y bibliotecas necesarios para construir y evaluar modelos de clasificación **Random Forest** y **XGBoost**.

---

## PACE: Analizar (Analyze)

Considera las preguntas en tu **Documento de Estrategia PACE** para reflexionar sobre la etapa de Análisis.

---

## Tarea 2. Ingeniería de características

Ya has preparado gran parte de estos datos y realizaste un análisis exploratorio de datos (EDA) en cursos anteriores. Sabes que algunas características tenían correlaciones más fuertes con el abandono que otras, y también creaste algunas características que podrían ser útiles.

En esta parte del proyecto, **crearás estas características y algunas nuevas** para utilizarlas en el modelado.

Para comenzar, crea una copia de `df0` para preservar el dataframe original. Llama a la copia `df`.


---

## Tarea 3. Eliminar valores faltantes

Dado que sabes, a partir del análisis exploratorio de datos (EDA) previo, que no hay evidencia de una causa no aleatoria para los **700 valores faltantes** en la columna de la etiqueta, y porque estas observaciones comprenden menos del **5% de los datos**, utiliza el método `dropna()` para eliminar las filas que tienen datos faltantes en esta columna.

---

## Tarea 4. Valores atípicos (Outliers)

Sabes, a partir del análisis exploratorio de datos (EDA) previo, que muchas de estas columnas tienen **valores atípicos**. Sin embargo, los modelos basados en árboles son **resilientes a los valores atípicos**, por lo que no es necesario realizar ninguna imputación.

---

## Tarea 5. Codificación de variables

### Creación de variables ficticias (Dummying features)

Para utilizar **device** como una variable X, deberás convertirla en binaria, ya que esta variable es **categórica**.

En casos donde los datos contienen muchas variables categóricas, puedes utilizar:

* La función incorporada de pandas: `pd.get_dummies()`, o
* La función `OneHotEncoder()` de **scikit-learn**.

> **Nota:** Cada categoría posible de cada característica resultará en una nueva característica para tu modelo, lo que podría conducir a una proporción inadecuada de características frente a observaciones y/o dificultad para comprender las predicciones del modelo.

Dado que este conjunto de datos solo tiene **una característica categórica restante** (**device**), no es necesario utilizar ninguna de estas funciones especiales. Puedes realizar la transformación directamente.

### Instrucciones:

Crea una nueva columna binaria llamada **device2** que codifique los dispositivos de usuario de la siguiente manera:

* **Android** → `0`
* **iPhone** → `1`

---

## Tarea 6. Selección de características (Feature Selection)

Los modelos basados en árboles pueden manejar bien la **multicolinealidad**, por lo tanto, la única característica que puede ser eliminada es **ID**, ya que no contiene ninguna información relevante para predecir el abandono de usuarios (**churn**).

> **Nota:** La variable `device` no se utilizará, simplemente porque ya se ha creado una copia codificada binariamente llamada `device2`.

### Instrucción:

Elimina la columna **ID** del dataframe `df`.

---

## Tarea 7. Métrica de evaluación

Antes de realizar el modelado, debes decidir cuál será la **métrica de evaluación**. Esta decisión dependerá del **balance de clases** de la variable objetivo y del caso de uso del modelo.

### Primer paso:

Examina el **balance de clases** de tu variable objetivo.

---

## PACE: Construir (Construct)

Considera las preguntas en tu **Documento de Estrategia PACE** para reflexionar sobre la etapa de Construcción.

---

## Tarea 8. Flujo de trabajo de modelado y proceso de selección del modelo

El conjunto de datos final para el modelado contiene **14,299 muestras**. Esto se encuentra en el límite inferior de lo que podría considerarse suficiente para llevar a cabo un proceso de selección de modelo robusto, pero sigue siendo factible.

---

### Pasos:

1. **Divide los datos** en conjuntos de entrenamiento/validación/prueba con una proporción de **60/20/20**.

   > Nota: Al decidir la proporción de división y si se debe usar o no un conjunto de validación para seleccionar el modelo campeón, considera cuántas muestras habrá en cada partición de datos y cuántos ejemplos de la clase minoritaria contendrá cada una.
   >
   > En este caso, una división **60/20/20** resultaría en aproximadamente **2,860 muestras** en el conjunto de validación y la misma cantidad en el conjunto de prueba, de las cuales aproximadamente el **18%**—es decir, unas **515 muestras**—representarían usuarios que abandonan (**churn**).

---

2. **Ajusta los modelos** y realiza la **tuning de hiperparámetros** utilizando el conjunto de entrenamiento.

3. **Realiza la selección final del modelo** utilizando el conjunto de validación.

4. **Evalúa el rendimiento** del modelo campeón en el conjunto de prueba.

---

## Tarea 9. Dividir los datos

Ahora estás listo para comenzar el modelado. El único paso restante es dividir los datos en **características / variable objetivo** y en conjuntos de **entrenamiento / validación / prueba**.

---

### Instrucciones:

1. **Define una variable `X`** que aísle las características (**features**).

   > Recuerda **no incluir la variable `device`**.

2. **Define una variable `y`** que aísle la variable objetivo (**label2**).

3. **Divide los datos en un conjunto provisional de entrenamiento y un conjunto de prueba** utilizando una proporción **80/20**.

   * No olvides **estratificar** las divisiones para mantener el balance de clases.
   * Establece el **random state en 42**.

4. **Divide el conjunto provisional de entrenamiento en conjuntos de entrenamiento y validación** con una proporción **75/25**.

   * Esto dará como resultado una proporción final de **60/20/20** para los conjuntos de entrenamiento, validación y prueba.
   * Nuevamente, **estratifica las divisiones** y establece el **random state en 42**.

---

## Tarea 10. Modelado

### Random Forest

Comienza utilizando **GridSearchCV** para ajustar un modelo de **Random Forest**.

---

### Instrucciones:

1. **Instancia el clasificador Random Forest** `rf` y establece el **random state**.

2. Crea un diccionario **`cv_params`** con cualquiera de los siguientes hiperparámetros y sus valores correspondientes para ajustar (**tuning**).

   > Cuantos más hiperparámetros ajustes, mejor se ajustará tu modelo a los datos, pero tomará más tiempo.

   * `max_depth`
   * `max_features`
   * `max_samples`
   * `min_samples_leaf`
   * `min_samples_split`
   * `n_estimators`

3. Define una lista **`scoring`** de métricas de evaluación para que **GridSearch** las capture:

   * Precisión (**precision**)
   * Recall (**recall**)
   * Puntaje F1 (**F1 score**)
   * Exactitud (**accuracy**)

4. **Instancia el objeto `GridSearchCV`** llamado `rf_cv`. Pásale como argumentos:

   * `estimator = rf`

   * `param_grid = cv_params`

   * `scoring = scoring`

   * `cv`: define el número de pliegues de validación cruzada (**cv = \_**)

   * `refit`: indica qué métrica de evaluación quieres usar para seleccionar el modelo (**refit = \_**)

   > El parámetro **refit debe establecerse en 'recall'**.

---

## Tarea 11. Selección del modelo

Ahora, utiliza el **mejor modelo de Random Forest** y el **mejor modelo de XGBoost** para hacer predicciones sobre los datos de **validación**.

---

### Objetivo:

Comparar el rendimiento de ambos modelos sobre el conjunto de validación y seleccionar el que **mejor desempeño tenga** como el **modelo campeón** (**champion model**).

> La métrica principal para comparar debe ser **recall**, ya que fue definida como prioridad en la tarea anterior (`refit='recall'`).


---

## PACE: Ejecutar (Execute)

Considera las preguntas en tu **Documento de Estrategia PACE** para reflexionar sobre la etapa de **Ejecución**.

---

## Tarea 12. Usar el modelo campeón para predecir en el conjunto de prueba

Ahora, utiliza el **modelo campeón** (el mejor entre Random Forest y XGBoost) para hacer **predicciones sobre el conjunto de prueba**.

---

### Objetivo:

Obtener una **evaluación final del rendimiento del modelo** sobre datos completamente nuevos (no utilizados durante el entrenamiento ni validación). Esto te dará una **estimación realista de cómo funcionará el modelo en el mundo real** si decides implementarlo.

> Recuerda evaluar las predicciones utilizando las mismas métricas clave (por ejemplo: *recall*, *accuracy*, *F1 score*, etc.) para asegurar la consistencia del análisis.

---

## Tarea 13. Matriz de confusión

Grafica una **matriz de confusión** con las predicciones del **modelo campeón** sobre el conjunto de prueba.

---

### Objetivo:

Visualizar el desempeño del modelo campeón mostrando:

* **Verdaderos positivos (VP)**
* **Falsos positivos (FP)**
* **Verdaderos negativos (VN)**
* **Falsos negativos (FN)**

Esto te permitirá:

* Evaluar la cantidad de errores de clasificación.
* Identificar posibles sesgos o debilidades del modelo (por ejemplo, si tiende a predecir falsos negativos o falsos positivos).

---

## Tarea 14. Importancia de las características

Utiliza la función **`plot_importance`** para inspeccionar las **características más importantes** de tu **modelo final**.

---

### Objetivo:

* Visualizar qué **variables** tuvieron más peso en las decisiones del modelo.
* Comprender qué factores influyen más en la predicción del abandono de usuarios (**churn**).
* Comunicar a los equipos técnicos y de negocio **qué variables deben monitorearse o priorizarse**.

> Nota: Esta función es especialmente útil con modelos de tipo **XGBoost**, ya que proporciona una representación clara de la importancia relativa de cada variable.

---
### **Task 15. Conclusion**

Now that you've built and tested your machine learning models, the next step is to share your findings with the Waze leadership team. Consider the following questions as you prepare to write your executive summary. Think about key points you may want to share with the team, and what information is most relevant to the user churn project.

**Questions:**

# 1. Would you recommend using this model for churn prediction? Why or why not?

---

### **Sí, recomendaría usar el modelo XGBoost para predecir la tasa de abandono (churn), pero con ciertas observaciones.**

---

### **¿Por qué SÍ recomendar el modelo XGBoost?**

1. **Mejor balance entre precisión y recall:**  
   - Aunque XGBoost no tiene la mejor precisión absoluta, ofrece **mejores valores de recall y F1 score** en comparación con Random Forest.  
   - Esto es crucial si tu objetivo es **detectar usuarios que probablemente abandonen**, incluso si implica cometer algunos errores.

2. **Buen rendimiento general:**  
   - La **accuracy** se mantiene alta (~81%), pero mejora la **detección de abandonos reales**, que es el objetivo principal del negocio.

3. **Matriz de confusión más equilibrada:**  
   - Aunque la cantidad de verdaderos positivos (TP = 90) es aún baja, es **mayor que con Random Forest**, lo que indica que XGBoost **aprende mejor los patrones de abandono**.

4. **XGBoost permite ajustes más precisos:**  
   - Este modelo ofrece muchas opciones para **mejorar su rendimiento**, como:
     - Ajuste del umbral de clasificación.
     - Penalización de clases desbalanceadas.
     - Personalización de la función de pérdida.
     - Optimización con técnicas como `early_stopping`.

---

###  **Pero también hay que tener en cuenta...**

- **Recall aún bajo (~18%)**:  
  Aunque XGBoost lo hace mejor que otros modelos, todavía **no está capturando suficientes abandonos reales** (FN = 417 es alto).  
  Esto puede significar que se necesitan **mejoras adicionales**, como:
  - Rebalancear las clases (`scale_pos_weight` en XGBoost).
  - Ajustar el umbral de probabilidad predicha (por defecto es 0.5).
  - Ingeniería de características más profunda.

- **Costo de falsos negativos**:  
  En un contexto como el abandono de usuarios, los **falsos negativos son costosos**, porque **no se tomarían acciones para retener a esos usuarios**.

---

### **Recomendación final:**

 **Sí se recomienda usar XGBoost** como modelo base para predecir churn.  
 **Pero también se recomienda**:  
- Continuar optimizándolo (balanceo, hiperparámetros, umbral).  
- Acompañarlo de **monitoreo post-implementación** para ajustar estrategias en producción.  
- Considerar un **enfoque híbrido** donde se combinen modelos o se priorice recall sobre otras métricas si el costo del abandono es alto.

# 2. What tradeoff was made by splitting the data into training, validation, and test sets as opposed to just training and test sets?


###  **Compensación principal al usar tres conjuntos:**

#### **Menor cantidad de datos para entrenamiento final**
- Al reservar una parte del conjunto original para validación, **dispones de menos datos para entrenar el modelo**.
- Esto **puede afectar ligeramente el rendimiento del modelo**, especialmente si el dataset no es muy grande.

---

### **Pero, ¿por qué vale la pena hacer esa división adicional?**

####  **1. Mejor ajuste de hiperparámetros**
- El conjunto de **validación** permite afinar hiperparámetros (por ejemplo, `max_depth`, `learning_rate`, `n_estimators`, etc.) de forma **objetiva** y sin mirar el conjunto de prueba.
- Esto reduce el riesgo de **overfitting al conjunto de prueba**.

####  **2. Prevención del sobreajuste en la evaluación**
- Si ajustaras el modelo directamente en el conjunto de prueba, estarías haciendo que el modelo "aprenda" sobre ese conjunto, lo que **viola la idea de prueba ciega**.
- El conjunto de prueba debe ser usado **solo una vez al final** para medir el rendimiento **real y generalizable** del modelo.

####  **3. Mayor control en el ciclo de desarrollo**
- Te permite experimentar libremente con arquitecturas, features y configuraciones sin comprometer la validez de tu evaluación final.

---

### **Resumen de la compensación:**

| Con dos conjuntos (`train/test`) | Con tres conjuntos (`train/val/test`) |
|------------------------------|-------------------------------------|
| Más datos para entrenamiento | Menos datos para entrenamiento      |
| Riesgo de sobreajuste al test | Menor riesgo: el test permanece virgen |
| Hiperparámetros pueden sobreajustarse al test | Se ajustan sobre validación, test se mantiene puro |
| Evaluación final puede estar sesgada | Evaluación final más objetiva y realista |


# 3. What is the benefit of using a logistic regression model over an ensemble of tree-based models (like random forest or XGBoost) for classification tasks?

###  **Beneficios de usar regresión logística en tareas de clasificación:**

| Beneficio | Descripción |
|----------|-------------|
| **Interpretabilidad** | Puedes entender fácilmente **cómo cada variable afecta la predicción**. Esto es muy útil cuando necesitas explicar el modelo a equipos no técnicos o para justificar decisiones (por ejemplo, en salud, finanzas, etc.). |
| **Modelos lineales y simples** | Es ideal cuando la **relación entre variables es lineal** o aproximadamente lineal. No necesitas grandes volúmenes de datos para obtener resultados aceptables. |
| **Velocidad y eficiencia computacional** | Se entrena **muy rápido**, incluso con grandes conjuntos de datos. Además, consume **menos memoria y recursos**. |
| **Menor riesgo de sobreajuste** | Tiene **menos capacidad de modelar ruido**, lo que en ciertos contextos puede ser una ventaja si los datos son ruidosos o limitados. |
| **Probabilidades calibradas** | Devuelve **probabilidades bien calibradas**, lo que puede ser muy útil si necesitas decisiones basadas en riesgo (por ejemplo, "usuario tiene 83% de probabilidad de abandonar"). |
| **Facilidad de regularización** | Soporta fácilmente **L1 y L2 regularization**, lo que ayuda a evitar el sobreajuste y hacer selección de variables. |

---

###  **¿Cuándo no conviene usar regresión logística sola?**
- Cuando las relaciones entre variables y la clase **no son lineales**.
- Cuando hay **interacciones complejas** entre variables.
- Cuando necesitas **alta precisión predictiva** en contextos muy competitivos (por ejemplo, clasificación de imágenes, predicción de churn con muchos factores).

---

### Comparado con modelos como **Random Forest** o **XGBoost**:

| Aspecto               | Regresión Logística       | Random Forest / XGBoost         |
|------------------------|---------------------------|----------------------------------|
| Interpretación         |  Muy fácil               |  Difícil (caja negra)         |
| Precisión en tareas complejas |  Limitada si no hay linealidad |  Muy alta en relaciones no lineales |
| Velocidad              |  Muy rápida              |  Más lenta, especialmente XGBoost |
| Ajuste a datos no lineales |  No captura interacciones complejas |  Sí captura |
| Requiere ingeniería de features |  Generalmente sí |  No siempre, porque los árboles lo hacen internamente |
| Escalado necesario     |  Necesita escalado       |  No requiere                 |

---

### **Conclusión:**
> Si lo que buscas es **velocidad, transparencia, interpretabilidad** y una **solución inicial rápida**, la **regresión logística** es una excelente opción.

> Pero si el problema involucra **relaciones complejas, interacciones no lineales** o si necesitas **el mejor rendimiento predictivo posible**, entonces un modelo de ensamble como **Random Forest o XGBoost** será más adecuado.


# 4. What is the benefit of using an ensemble of tree-based models like random forest or XGBoost over a logistic regression model for classification tasks?

 
Usar un conjunto de modelos basados en árboles como **Random Forest** o **XGBoost** en lugar de un modelo más simple como **regresión logística** tiene múltiples ventajas, especialmente cuando se trabaja con **problemas complejos y no lineales**.

---

###  **Beneficios clave de usar modelos basados en árboles (Random Forest, XGBoost) en lugar de regresión logística:**

| **Ventaja** | **Explicación** |
|-------------|-----------------|
|  **Captura relaciones no lineales** | A diferencia de la regresión logística (que modela relaciones lineales), los árboles pueden capturar relaciones **no lineales y complejas entre variables** sin necesidad de transformar los datos manualmente. |
|  **Captura interacciones entre variables** | Los modelos de árboles consideran **combinaciones de características** de forma natural en las divisiones (splits), lo cual es muy difícil de hacer manualmente con regresión logística. |
|  **Manejo automático de variables categóricas (en algunos casos)** | Algoritmos como XGBoost o LightGBM pueden trabajar directamente con variables categóricas codificadas, y no requieren una transformación tan estricta como la regresión logística. |
|  **Importancia de características** | Pueden calcular la **importancia de cada variable** en las decisiones del modelo, ayudando a la interpretación y selección de features. |
|  **Mayor poder predictivo** | En la mayoría de los casos, modelos como Random Forest o XGBoost **logran mejor desempeño predictivo**, especialmente en datasets grandes y variados. |
|  **Robustos al ruido y outliers** | Los árboles no se ven tan afectados por valores extremos como la regresión logística. |
|  **Soportan técnicas avanzadas (boosting/bagging)** | Modelos como XGBoost aplican boosting (aprenden de los errores de modelos anteriores), lo que **incrementa la precisión** del modelo final. |

---

###  **Ejemplo de escenarios donde Random Forest/XGBoost superan a regresión logística:**

- Detección de fraude (datos desbalanceados y relaciones complejas).
- Predicción de abandono de clientes (churn).
- Clasificación de imágenes/texto.
- Datos con muchos atributos y relaciones no evidentes.

---

###  Comparación directa:  
| Aspecto                          | Regresión Logística       | Random Forest / XGBoost       |
|----------------------------------|----------------------------|-------------------------------|
| Tipo de modelo                   | Lineal                     | No lineal                     |
| Interacciones entre variables    | Necesitan ser modeladas manualmente | Captura automáticamente |
| Ingeniería de features           | Alta                       | Baja o automática             |
| Precisión en datos complejos     | Limitada                   | Alta                          |
| Interpretabilidad                | Alta                       | Media-baja (más caja negra)   |
| Sensibilidad a outliers          | Alta                       | Baja                          |
| Escalado de variables necesario  | Sí                         | No                            |
| Costo computacional              | Bajo                       | Medio-alto                    |



# 5. What could you do to improve this model?

Para mejorar el modelo XGBoost que se ha estado utilizando para predecir la tasa de abandono, se pueden aplicar varias estrategias complementarias. Primero, es recomendable realizar un ajuste más exhaustivo de hiperparámetros clave como learning_rate, n_estimators, max_depth y scale_pos_weight, ya sea mediante GridSearchCV, RandomizedSearchCV o herramientas más avanzadas como Optuna. Además, ajustar el umbral de clasificación por debajo de 0.5 puede aumentar el recall y ayudar a detectar más abandonos, lo cual es crítico si se quiere intervenir a tiempo. También se sugiere enriquecer el modelo con ingeniería de características más profunda, incluyendo tasas, promedios e interacciones entre variables, así como aplicar técnicas de rebalanceo como scale_pos_weight o SMOTE si las clases están desbalanceadas. Para evitar sobreajuste, se puede utilizar validación cruzada estratificada junto con early_stopping. Finalmente, incorporar interpretabilidad con herramientas como SHAP puede aportar transparencia al modelo, permitiendo entender mejor qué factores influyen en el abandono y apoyar decisiones estratégicas de retención. Estas acciones combinadas permitirán no solo mejorar el rendimiento del modelo, sino también hacerlo más útil y accionable para el negocio.


# 6. What additional features would you like to have to help improve the model?


### **1. Comportamiento reciente del usuario**
- **Días desde la última sesión**: mide inactividad.
- **Tendencia de uso**: aumento o disminución del número de sesiones en las últimas semanas.
- **Cambio en la frecuencia de uso**: comparación entre uso histórico vs. uso reciente.
- **Variabilidad de conducción**: ¿es regular o intermitente?

---

###  **2. Variables temporales**
- **Antigüedad del usuario**: tiempo desde su primera sesión (en días o semanas).
- **Hora promedio de conducción**: ¿conduce de día, tarde o noche?
- **Días activos consecutivos o más largos sin usar la app**: patrón de continuidad o abandono.

---

### **3. Variables contextuales del entorno**
- **Condiciones de tráfico promedio en sus rutas** (si está disponible).
- **Tipo de zonas que recorre**: urbana, rural, autopistas, etc.
- **Eventos externos**: promociones, actualizaciones de la app, cambios en el clima.

---

### **4. Interacción con la app**
- **Número de notificaciones abiertas**.
- **Clicks o interacciones en funcionalidades específicas** (por ejemplo: informes, navegación).
- **Errores reportados o cierres forzados**.

---

### **5. Datos demográficos o de perfil (si están disponibles)**
- **Edad** o rango etario.
- **Género**.
- **Ubicación geográfica**.
- **Tipo de vehículo** (si se conoce): taxi, moto, auto particular.

---

### **6. Variables de valor comercial**
- **Historial de pagos o suscripciones premium**.
- **Participación en programas de recompensas** o promociones.
- **Valor estimado del cliente (CLV)**.

---

### **7. Interacciones derivadas**
- **Ratios**: sesiones por km, km por día, etc.
- **Indicadores booleanos**: `usuario_inactivo_7_dias`, `uso_bajo`, etc.
- **Combinaciones de variables**: por ejemplo, `device * uso`, o `clase_de_usuario * km`.

---

### **¿Por qué son valiosas estas variables?**
Estas características permiten capturar señales **anticipadas de desinterés o deserción**, **cambios en el comportamiento** y diferencias entre **usuarios comprometidos vs. inactivos**. Al enriquecer el conjunto de datos con este tipo de información, el modelo puede **aprender patrones más profundos y precisos**, lo que se traduce en un **mejor desempeño en la predicción del abandono**.

---

¿Quieres que te ayude a crear alguna de estas variables paso a paso con código o hacer un análisis exploratorio para ver cuáles serían más útiles según tus datos actuales? 













