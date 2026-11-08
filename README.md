# Proyecto Final – Fundamentos de Redes Neuronales

## Predicción de la intención de compra en sesiones de comercio electrónico

**Integrantes:**  
- Juan Mateo Jaramillo Guerra  
- Ángel Santiago Díaz Toloza  

**Curso:** Fundamentos de Redes Neuronales  
**Universidad:** Universidad de La Sabana  
**Dataset:** Online Shoppers Purchasing Intention – UCI Machine Learning Repository

## Descripción del proyecto

El proyecto busca predecir si una sesión de navegación en una tienda virtual terminará o no en una compra. La variable objetivo es `Revenue`, donde 1 representa una sesión con compra y 0 una sesión sin compra.

A lo largo del curso se trabajó desde la preparación de los datos y la construcción de una línea base con regresión logística hasta el desarrollo, optimización y evaluación de una red neuronal.

Debido al desbalance de la variable objetivo, se utilizaron métricas como PR-AUC, recall y F1 como principales criterios de evaluación.

## Preparación de los datos

El dataset original contiene 12.330 sesiones.

Durante la preparación se realizaron los siguientes pasos:

- eliminación de 125 registros duplicados;
- partición estratificada 70 % entrenamiento, 15 % validación y 15 % prueba;
- transformación `log1p` para variables de conteo y duración;
- estandarización de variables numéricas;
- codificación One-Hot para variables categóricas;
- agrupación de categorías poco frecuentes utilizando únicamente el conjunto de entrenamiento;
- uso de pesos de clase para tratar el desbalance.

La variable `PageValues` se excluyó del modelo final debido al posible riesgo de fuga de información, ya que no se confirmó que estuviera disponible antes de conocer el resultado de compra.

## Modelo neuronal

El modelo final utiliza la siguiente arquitectura:

```text
58 características de entrada
        ↓
Dense 64 - ReLU
        ↓
Dropout 0.15
        ↓
Dense 32 - ReLU
        ↓
Dropout 0.15
        ↓
Dense 1 - Sigmoid
```

Configuración principal:

```text
Optimizador: Adam
Learning rate: 0.001
Batch size: 32
Loss: Binary Crossentropy
Umbral de clasificación: 0.5
```

## Resultados

En validación, el modelo final obtuvo:

| Métrica | Resultado |
|---|---:|
| PR-AUC | 0.3861 |
| F1 | 0.4159 |
| Recall | 0.6958 |
| ROC-AUC | 0.7760 |

Además, se evaluó la estabilidad del modelo utilizando tres semillas diferentes:

| Semilla | PR-AUC |
|---:|---:|
| 17 | 0.3861 |
| 31 | 0.3637 |
| 53 | 0.3780 |

El PR-AUC promedio fue aproximadamente:

```text
0.3760 ± 0.0113
```

## Evaluación final en prueba

| Modelo | Accuracy | Precision | Recall | F1 | ROC-AUC | PR-AUC |
|---|---:|---:|---:|---:|---:|---:|
| Regresión logística | 0.6412 | 0.2594 | 0.6993 | 0.3784 | 0.7354 | 0.3091 |
| Red neuronal final | 0.6827 | 0.2872 | 0.6958 | 0.4065 | 0.7690 | 0.3451 |

La red neuronal mejora PR-AUC, F1, precision, ROC-AUC y accuracy frente a la regresión logística. El recall se mantiene muy cercano, aunque ligeramente por debajo del baseline.

## Contenido del repositorio

```text
proyecto-final-redes-neuronales/
│
├── README.md
├── Proyecto_Final_Redes_Neuronales_Angel_Mateo.ipynb
├── Informe_Final_Actividad_5_Unidad_3.pdf

```

## Ejecución

El notebook fue desarrollado para ejecutarse en Google Colab.

Para reproducir el proyecto:

1. Abrir `Proyecto_Final_Redes_Neuronales_Angel_Mateo.ipynb`.
2. Ejecutar todas las celdas en orden.
3. El dataset se descarga directamente desde UCI mediante `ucimlrepo`.
4. El notebook realiza automáticamente el preprocesamiento, entrenamiento, validación y evaluación final.

Al finalizar también se generan los artefactos necesarios para reutilizar el pipeline:

```text
modelo_final_compra.keras
preprocesador_final.joblib
metadata_modelo.json
```

## Conclusión

El proyecto muestra la evolución desde una línea base interpretable hasta una red neuronal optimizada y evaluada de forma reproducible.

La arquitectura final mejora la discriminación de las sesiones con intención de compra frente a la regresión logística, aunque todavía existe un compromiso entre recuperar compradores y generar falsos positivos.

Por esta razón, el modelo se plantea como una herramienta de apoyo para priorización comercial y no como un sistema automático de decisión.
