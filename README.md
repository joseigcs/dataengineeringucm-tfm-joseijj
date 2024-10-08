# Proyecto de Predicción de Activos Financieros con Azure

## Descripción

Este proyecto tiene como objetivo predecir el valor futuro de algunos activos financieros utilizando un modelo de machine learning implementado en Microsoft Azure. Se construyó un pipeline completo para la ingesta, almacenamiento, procesamiento, y análisis de datos financieros. Además, se desarrolló un modelo de regresión Ridge para la predicción de precios ajustados, con mejoras como la regularización y la inclusión de variables lagged.

# Estructura del Proyecto

1. Recolección e Ingesta de Datos
Los datos financieros son extraídos de Yahoo Finance utilizando la API yfinance.
Se almacenan en un Data Lake de Azure en la capa Bronze como datos crudos.
2. Almacenamiento de Datos

Los datos se organizan en tres capas en el contenedor del Data Lake:

- Bronze: Datos crudos y sin procesar.
- Silver: Datos limpios con columnas adicionales como year, month, y day_of_week.
- Gold: Datos finales con métricas calculadas como medias móviles y retornos diarios.

3. Procesamiento y Transformación

Se desarrollaron notebooks en Azure Synapse para procesar los datos en las diferentes capas:

BronzeToSilver: Limpieza y estructuración de datos.
SilverToGold: Cálculo de métricas avanzadas, como medias móviles de 7 y 30 días, retornos diarios y agregaciones mensuales. También se añadió información externa sobre la incertidumbre de los Estados Unidos.

4. Entrenamiento del Modelo

El modelo utiliza regresión Ridge para predecir el precio ajustado de cierre de los activos.
Se incluyeron transformaciones logarítmicas y variables lagged para capturar la dependencia temporal.
Se probó con diversos valores de regularización, siendo alpha = 0.0001 el valor óptimo.

5. Despliegue y Monitoreo

El modelo fue versionado utilizando MLFlow para facilitar su monitoreo y actualización.
Se creó un microservicio con Flask para realizar predicciones en tiempo real.
Se integraron notificaciones automáticas con Logic Apps, que envían correos electrónicos en caso de error o éxito.

6. Visualización

Se desarrolló un dashboard en Power BI conectado al Data Lake, mostrando las métricas calculadas en la capa Gold.

Cómo Usar

1. Requisitos
Microsoft Azure: Synapse Analytics, Data Lake, Logic Apps, y Power BI.
Python con librerías: yfinance, pandas, numpy, scikit-learn, flask. Realizado con python 3.8.19.
2. Ejecución del Pipeline
El pipeline está configurado para ejecutarse semanalmente los sábados, cuando la bolsa está cerrada.
Los datos se procesan y almacenan automáticamente en el Data Lake de Azure, con notificaciones configuradas para los resultados.
3. Predicción con el Modelo
Utiliza el microservicio creado en Flask para enviar datos en formato JSON y obtener predicciones en tiempo real.
4. Visualización de Resultados
Carga los datos en PowerBI de forma automática.