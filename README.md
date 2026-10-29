🚀 Pipeline de Datos E2E y Predicción de Demanda (E-commerce)

📖 Resumen del Proyecto

Este proyecto es una solución integral (End-to-End) de Ciencia e Ingeniería de Datos diseñada para el dataset público de Olist, el mayor marketplace de Brasil. El objetivo principal es analizar el comportamiento histórico de ventas, identificar los cuellos de botella logísticos y establecer una línea base de Machine Learning para predecir la demanda futura, optimizando así la gestión de inventario.

🏗️ Arquitectura del Proyecto

El proyecto está diseñado bajo dos enfoques:

MVP Local: Extracción con Python (Pandas), modelado relacional en MySQL y visualización en Power BI.

Diseño Cloud (AWS): Arquitectura Serverless propuesta para producción.

Amazon S3: Data Lake para archivos CSV crudos.

AWS Lambda: Ejecución serverless de scripts ETL en Python.

Amazon RDS (MySQL): Data Warehouse transaccional.

🛠️ Tecnologías y Herramientas

Lenguajes: Python, SQL, DAX.

Librerías: pandas, scikit-learn, SQLAlchemy, matplotlib.

Base de Datos: MySQL (Modelado Relacional).

Visualización: Power BI.

⚙️ Fases del Desarrollo

1. Extracción y Transformación (ETL)

Se procesaron múltiples fuentes de datos transaccionales, realizando limpieza de nulos, estandarización de fechas y validación de tipos de datos.

# Ejemplo: Limpieza y consolidación de datos en Pandas
import pandas as pd

# Lectura de datos
orders = pd.read_csv('data/raw/olist_orders_dataset.csv')
order_items = pd.read_csv('data/raw/olist_order_items_dataset.csv')

# Transformación de fechas
orders['order_purchase_timestamp'] = pd.to_datetime(orders['order_purchase_timestamp'])

# Consolidación (Merge)
df_ventas = pd.merge(orders, order_items, on='order_id', how='inner')


2. Modelado de Datos (MySQL)

Se diseñó un esquema relacional para soportar las consultas analíticas del negocio. Se automatizó la ingesta de datos desde Python hacia MySQL usando SQLAlchemy.

-- Creación de la estructura base
CREATE DATABASE ecommerce_olist;
USE ecommerce_olist;

CREATE TABLE productos (
    product_id VARCHAR(50) PRIMARY KEY,
    product_category_name VARCHAR(100),
    product_weight_g FLOAT
);


3. Visualización y Business Intelligence (Power BI)

Se desarrolló un panel interactivo para el monitoreo de KPIs operativos y comerciales, sorteando problemas de tipado y configuraciones regionales mediante Power Query.

Medidas DAX Clave implementadas:

-- Cálculo de Ingresos Totales
Total Ingresos = SUM(ventas_consolidadas[price])

-- Cálculo del Ticket Promedio (AOV)
Ticket Promedio = DIVIDE(
    [Total Ingresos], 
    DISTINCTCOUNT(ventas_consolidadas[order_id]), 
    0
)


4. Machine Learning (Línea Base Predictiva)

Se implementó un modelo base utilizando RandomForestRegressor para predecir las ventas diarias.

from sklearn.ensemble import RandomForestRegressor
from sklearn.metrics import mean_absolute_error

# Entrenamiento del modelo base
rf_model = RandomForestRegressor(n_estimators=100, random_state=42)
rf_model.fit(X_train, y_train)

# Evaluación
predicciones = rf_model.predict(X_test)
mae = mean_absolute_error(y_test, predicciones)
print(f"Error Absoluto Medio (MAE): {mae}")


Resultado: El modelo base arrojó un MAE de 14,487, lo que demuestra la necesidad de implementar Feature Engineering avanzado en la siguiente iteración.
