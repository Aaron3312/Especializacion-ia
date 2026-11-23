# Guía para el Quiz 4 · Integración módulo 4 (TC5053)

10 preguntas, 10 minutos, 2 intentos, 4 puntos. Cerró el 15 de noviembre; esta guía es para cuando el profe lo reabra.
Temas: 7.1 aprendizaje no supervisado y PCA, 8.1 aprendizaje supervisado y regresión, 9.1 métricas de evaluación.

## 1. Dónde estamos: CRISP-DM
- El módulo es la fase de **modelado** de CRISP-DM; después viene la **evaluación**. Si los resultados no sirven, se regresa a la preparación de datos.
- Los datos se dividen en **entrenamiento** y **prueba** (normalmente ~20 % para prueba, con `train_test_split`).

## 2. Supervisado vs. no supervisado
| | Supervisado | No supervisado |
|---|---|---|
| Datos | Etiquetados (se conoce la salida) | Sin etiquetas |
| Objetivo | Aprender una función entrada → salida | Encontrar la estructura natural de los datos |
| Tareas | Regresión (salida numérica continua) y clasificación (salida categórica) | Agrupamiento, asociación y reducción de dimensionalidad |
| Ejemplos | Precio de un diamante, spam / no spam | Segmentar clientes, canasta de compra, PCA |

- Los modelos supervisados suelen ser más precisos, pero alguien tiene que etiquetar los datos.
- Los no supervisados también necesitan que una persona valide lo que salió.

## 3. No supervisado
- **Agrupamiento (clustering):** junta datos parecidos; sirve para segmentar mercados (k-means).
- **Asociación:** reglas del tipo "quien compra A compra B"; se usa en canasta de compra y recomendaciones.
- **Reducción de dimensionalidad:** baja el número de variables sin perder lo importante. Se usa en el preprocesamiento: entrena más rápido, ocupa menos memoria y facilita graficar.
- **PCA:**
  - crea **componentes principales**, que son combinaciones lineales de las variables, ordenadas por cuánta **varianza** explican;
  - hay que **escalar** antes (StandardScaler), porque PCA es sensible a las unidades;
  - para elegir cuántos componentes dejar, mira la varianza explicada acumulada (por ejemplo, llegar al 90–95 %).

## 4. Supervisado: regresión
- **Regresión lineal:** ŷ = b₀ + b₁x₁ + … + bₙxₙ.
  - b₀ es la **intersección** (*bias*) y b₁…bₙ son los **coeficientes**;
  - se ajusta con **mínimos cuadrados**.
- **Residuo** = valor observado − valor predicho.
  - Si es positivo, el modelo predijo de menos; si es negativo, predijo de más.
- **MSE** = promedio de los residuos al cuadrado. Se elevan al cuadrado para que no se cancelen los positivos con los negativos.
- **RMSE** = √MSE. Queda en las mismas unidades que la variable de respuesta, por eso es el que más se reporta.
- **Regresión polinomial:** agrega términos como x², que le dan curvatura a la recta.
  - Para el algoritmo, x² es solo otra variable predictora.
  - Multiplicar variables distintas da **términos de interacción**.
- **R²:** proporción de la varianza de y que explica el modelo (de 0 a 1).
  - **R² ajustado** castiga agregar variables que no aportan, así que sirve para comparar modelos con distinto número de predictores.
  - El R² normal nunca baja aunque metas variables que no sirven.
- **Pipeline (canalización):** encadena los pasos (imputar, escalar, codificar y modelo) para aplicarlos igual en entrenamiento y en prueba.

## 5. Clasificación y métricas
- **Regresión logística:**
  - es para una respuesta **binaria** (sí/no);
  - en vez de una recta usa una curva **sigmoide** que da una probabilidad entre 0 y 1;
  - un **umbral** (normalmente 0.5) convierte esa probabilidad en 0 o 1;
  - entrena rápido, así que es un buen modelo base para comparar.
- **Matriz de confusión:** las filas son los valores reales y las columnas los predichos.
  - VP: era positivo y dijo positivo.
  - FN: era positivo y dijo negativo.
  - VN: era negativo y dijo negativo.
  - FP: era negativo y dijo positivo.
- **Recall (sensibilidad)** = VP / (VP + FN): de los positivos reales, cuántos encontró.
- **Precision (precisión)** = VP / (VP + FP): de los que dijo positivos, cuántos sí lo eran.
- **Accuracy (exactitud)** = (VP + VN) / total: qué tan seguido acierta en general.
  - Engaña cuando las clases están desbalanceadas.
- Ejemplo de la lectura: 11 positivos y 9 negativos, con VP = 8, FN = 3, VN = 7 y FP = 2.
  - Recall = 8/11 = 0.73
  - Precision = 8/10 = 0.80
  - Accuracy = 15/20 = 0.75
- **Subir o bajar el umbral:**
  - si lo subes, sube la precisión y baja el recall;
  - si lo bajas, pasa lo contrario.

## 6. Autoevaluación (respuestas abajo)
1. Quieres agrupar clientes sin etiquetas. ¿Qué tipo de aprendizaje y qué tarea usarías?
2. ¿Por qué se escalan los datos antes de PCA?
3. Un modelo tiene MSE = 400 (pesos²). ¿Cuánto vale el RMSE y en qué unidades está?
4. Agregas 5 variables aleatorias a una regresión. ¿Qué pasa con R² y con R² ajustado?
5. Un detector de fraude tiene 99 % de accuracy, pero solo el 1 % de las transacciones son fraude. ¿Por qué no basta esa métrica?
6. En un diagnóstico médico es peor no detectar a un enfermo. ¿Qué métrica cuidas más?
7. Con VP = 30, FP = 10, FN = 20 y VN = 140, calcula recall, precision y accuracy.

<details><summary>Respuestas</summary>

1. No supervisado; agrupamiento (por ejemplo, k-means).
2. Porque PCA busca las direcciones de mayor varianza: una variable con unidades grandes dominaría aunque no sea la más informativa.
3. RMSE = 20, en pesos, las mismas unidades que la respuesta.
4. R² sube o se queda igual; R² ajustado baja porque penaliza las variables que no aportan.
5. Un modelo que diga "nunca hay fraude" tendría 99 % de accuracy y no detectaría nada. Hay que ver recall y precision de la clase fraude.
6. El recall: lo que se quiere evitar son los falsos negativos.
7. Recall = 30/50 = 0.60; precision = 30/40 = 0.75; accuracy = 170/200 = 0.85.
</details>
