# Análisis de Datos de  Animales Abatidos Suinos en el Estado de Pará

**Autor(es):** Sergio Velasquez, Juan Jose Torres, Nelson Serrano, Juan Cruz

Este proyecto es una implementación en Python para el procesamiento y análisis de datos de abate animal, con un enfoque específico en la información del estado de Pará. El objetivo principal es transformar datos crudos de archivos Excel en un resumen anual consolidado, facilitando el monitoreo y la toma de decisiones.

## Tabla de Contenidos

- [Descripción del Proyecto](#descripción-del-proyecto)
- [Flujo de Trabajo del Proceso](#flujo-de-trabajo-del-proceso)
- [Funcionalidades Implementadas](#funcionalidades-implementadas)
- [Requisitos del Sistema](#requisitos-del-sistema)

## Descripción del Proyecto

El sector agropecuario genera vastas cantidades de datos, cuya correcta interpretación es crucial para entender tendencias y apoyar políticas públicas o decisiones empresariales. Este proyecto aborda el desafío de procesar datos heterogéneos de abate animal, provenientes de reportes en formato Excel. Se ha desarrollado un script en Python que realiza una serie de operaciones de limpieza, filtrado y agregación para extraer únicamente la información relevante del estado de Pará y presentarla de forma anualizada.

El pipeline de procesamiento está diseñado para ser robusto y autónomo, minimizando la intervención manual y asegurando la integridad de los datos desde la ingestión hasta la generación del informe final.

## Flujo de Trabajo del Proceso

El siguiente diagrama de flujo describe los pasos lógicos y secuenciales que ejecuta el algoritmo principal (`procesar_archivo_excel`) para llevar a cabo el análisis:

```mermaid
graph TD
    A[INICIO DEL PROCESO] --> B(Leer archivo Excel: cabecera en fila 3, saltar pie de página)
    B --> C(Renombrar 'Unidade da Federação' a 'Estado')
    C --> D(Rellenar celdas combinadas en 'Estado' (fill))
    D --> E(Eliminar columnas de inspección ('Unnamed: 1', 'Tipo de inspeção'))
    E --> F{¿El 'Estado' es 'Pará'?}
    F -- SI --> G(Convertir columnas de datos a numérico, errores a 0)
    G --> H(Extraer año de los nombres de las columnas de datos)
    H --> I(Sumar los totales de abate por cada año)
    I --> J(Crear DataFrame final: 'Año', 'Total Abatidos en Pará')
    J --> K(Exportar DataFrame final a 'resultados_pará.xlsx')
    K --> L[FIN DEL PROCESO]
    F -- NO --> M(Registro: Fila no pertenece a Pará, omitir)
    M --> E
    B -- ERROR (FileNotFoundError/Otros) --> N(Manejo de Error: Notificar y finalizar)
    N --> L
```

## Funcionalidades Implementadas

El script `procesar_archivo_excel` incorpora las siguientes características clave:

*   **Ingesta Flexible de Datos:** Capacidad para leer archivos Excel omitiendo filas iniciales y finales irrelevantes (`header=3`, `skipfooter=1`), adaptándose a formatos de reporte específicos.
*   **Normalización de Columnas:** Estandarización del nombre de la columna principal de identificación geográfica a 'Estado' y aplicación de un relleno hacia adelante (`ffill()`) para manejar celdas combinadas y garantizar la continuidad de la información del estado.
*   **Limpieza Estructurada:** Identificación y eliminación programática de columnas no esenciales relacionadas con "Unnamed: 1" o "Tipo de inspeção", simplificando el conjunto de datos.
*   **Filtro Geográfico Preciso:** Filtrado riguroso del DataFrame para procesar exclusivamente los registros correspondientes al estado de Pará, asegurando la relevancia geográfica del análisis.
*   **Robustez Numérica:** Conversión de todas las columnas de datos de abate a formato numérico, reemplazando automáticamente cualquier valor no convertible con cero, lo que previene errores en las operaciones matemáticas.
*   **Agregación Temporal:** Extracción de años a partir de los encabezados de columna y consolidación de los datos para sumar el total de animales abatidos por cada año, proporcionando una visión anualizada.
*   **Salida Consolidada:** Generación de un archivo Excel (`resultados_pará.xlsx`) que contiene el resumen anual de abates, facilitando la integración con otras herramientas de análisis o reporte.
*   **Gestión de Excepciones:** Inclusión de manejo de errores (`try/except`) para abordar situaciones comunes como la ausencia del archivo de entrada (`FileNotFoundError`) o errores inesperados durante el procesamiento, mejorando la robustez del script.

## Requisitos del Sistema

Para ejecutar este proyecto, se requiere una instalación de Python (versión 3.x recomendada) y las siguientes librerías:

*   `pandas`
*   `openpyxl` (Es una dependencia para que pandas pueda leer y escribir archivos `.xlsx`)

Puedes instalar estas librerías utilizando `pip`:

```bash
pip install pandas openpyxl
```



---
