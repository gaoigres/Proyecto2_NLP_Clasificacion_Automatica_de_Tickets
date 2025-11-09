# Proyecto: Clasificación Automática de Tickets (NLP)

## Tabla de Contenidos
1. [Introducción](#1-introducción)
2. [Descripción de los Datos](#2-descripción-de-los-datos)
3. [Limpieza y Preprocesamiento del Texto](#3-limpieza-y-preprocesamiento-del-texto)
4. [Representación Vectorial (TF-IDF)](#4-representación-vectorial-tf-idf)
5. [Entrenamiento de Modelos Supervisados](#5-entrenamiento-de-modelos-supervisados)
6. [Inferencia del Modelo (Nuevos Reclamos)](#6-inferencia-del-modelo-nuevos-reclamos)
7. [Interpretación del Rendimiento del Modelo](#7-interpretación-del-rendimiento-del-modelo)
8. [Conclusiones Finales](#8-conclusiones-finales)

---

## 1. Introducción

Este proyecto utiliza técnicas de Procesamiento de Lenguaje Natural (NLP) para clasificar reclamos financieros escritos por usuarios.  
El objetivo es identificar automáticamente el tema principal del reclamo para mejorar la gestión y priorización de casos.

---

## 2. Descripción de los Datos

- Formato del dataset: `complaints.json.gz` (almacenado con Git LFS por su tamaño).
- Columna analizada: **Complaint** (texto libre).
- Se eliminaron filas vacías y se estandarizó la codificación en UTF-8.

Los reclamos hacen referencia a servicios como tarjetas de crédito, cuentas bancarias, hipotecas y disputas de pagos.

---

## 3. Limpieza y Preprocesamiento del Texto

Pasos aplicados:

| Paso | Descripción |
|------|-------------|
| Minúsculas | Normaliza el texto |
| Eliminación de ruido | URLs, signos, números, caracteres especiales |
| Tokenización | División del texto en palabras |
| Stopwords | Eliminación de palabras sin valor semántico |
| Lematización | Conversión al lema base |
| POS Tagging | Conservación de sustantivos (NN), para priorizar conceptos clave |

Este proceso mejora la claridad y relevancia de los términos para el modelo.

---

## 4. Representación Vectorial (TF-IDF)

Se utilizó un enfoque de dos etapas:

1. **CountVectorizer** → Obtiene la frecuencia de cada palabra.
2. **TfidfTransformer** → Ajusta el peso según importancia en el corpus.

La matriz resultante permitió representar el texto como vectores numéricos listos para utilizar en modelos supervisados.

Las categorías utilizadas son
- 0: "Bank Account services",
- 1: "Credit card / prepaid card",
- 2: "Mortgage / Loan",
- 3: "Theft / Dispute Reporting",
- 4: "Others"

---

## 5. Entrenamiento de Modelos Supervisados

Se entrenaron y compararon los siguientes modelos:

| Modelo               | F1 Macro (aprox.) |
|----------------------|------------------|
| Regresión Logística  | **0.96**             |
| Árbol de Decisión    | 0.85             |
| **Random Forest**    | 0.89         |

**Modelo final seleccionado:** **Regresión logística**  
Motivo: mejor equilibrio entre precisión y generalización.

---

## 6. Inferencia del Modelo (Nuevos Reclamos)

Ejemplo de predicción:

    - "My credit card was charged twice and the bank rejected my dispute." (Categoría Credit card / prepaid card)
    - "Mortgage payment failed and automatic debit was declined." (Others)

---

## 7. Interpretación del Rendimiento del Modelo

- El modelo muestra un rendimiento consistente en categorías con vocabulario distintivo.

- Se observan confusiones cuando dos categorías comparten terminología financiera cercana.

- La matriz de confusión evidencia una correcta separación general entre clases.

De esa manera, el modelo es estable y se desempeña adecuadamente en la clasificación automática de reclamos.

---

## 8. Conclusiones finales

- El preprocesamiento profundo del texto fue clave para mejorar los resultados.

- TF-IDF fue una representación adecuada para este tipo de datos.

- Random Forest superó a otros clasificadores evaluados.

- La solución es viable para integrarse en sistemas de atención al cliente.

