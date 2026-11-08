# Laboratorio 3.5

En este laboratorio se cargó de nuevo el conjunto de datos para el entorno del laboratorio, se entrenó un estimador XGBoost y se probaron la inferencia en tiempo real y la transformación por lotes.

## Preparación y despliegue

Se instalaron las dependencias que faltaban en el kernel y se importaron las librerías necesarias para los datos, SageMaker y la partición estratificada.

![Instalación de dependencias](01-dependencias-lab-35.png)

![Importaciones y descarga del conjunto](02-importacion-y-descarga.png)

Se prepararon los conjuntos de entrenamiento, prueba y validación y se cargaron los CSV al bucket asignado a este laboratorio.

![Preparación y carga de datos a S3](03-preparacion-y-carga-s3.png)

El trabajo de entrenamiento terminó correctamente y el notebook mostró `ready for hosting!`.

![Entrenamiento completado](04-entrenamiento-completado.png)

Se desplegó el modelo en un endpoint de tiempo real para consultar predicciones.

![Creación del endpoint](05-despliegue-endpoint.png)

## Predicción en tiempo real

Se revisó el conjunto de prueba, que contiene 31 registros. Para predecir, se omitió la columna `class` y se enviaron únicamente las seis características.

![Revisión del conjunto de prueba](06-revision-datos-prueba.png)

La captura muestra la primera fila de prueba (índice 136) convertida a CSV y una puntuación devuelta por el endpoint: `0.9966071844100952`. Como `class=1` representa **anormal** y `class=0` representa **normal**, esa puntuación indica una probabilidad alta de clase anormal para esa fila.

![Predicción de una fila del conjunto de prueba](07-prediccion-fila-prueba.png)

![Muestras del conjunto de prueba y sus etiquetas](08-muestras-prueba.png)

La actividad también solicita consultar la entrada `63.0,12.5,45.0,50.5,125.0,3.0`. Su salida exacta aún no está en estas capturas; se añadirá después de ejecutar esa validación. La puntuación de la fila 136 anterior no corresponde a esa entrada.

## Cierre del endpoint y transformación por lotes

Tras las consultas en tiempo real, se solicitó eliminar el endpoint y su configuración para liberar los recursos de alojamiento.

![Eliminación del endpoint](09-eliminacion-endpoint.png)

Para la inferencia por lotes, se separaron las seis características de `test` y se cargó el CSV de entrada a S3 sin incluir la etiqueta objetivo.

![Archivo de entrada para la transformación por lotes](10-entrada-transformacion-lote.png)

Se configuró un transformador de SageMaker con entrada y salida en S3 y se inició el trabajo por lotes. La captura también muestra el código para descargar su resultado.

![Configuración e inicio de la transformación por lotes](11-transformacion-lote-s3.png)

Se definió una regla con umbral `0.65` para convertir puntuaciones de probabilidad en etiquetas binarias: mayor que 0.65 se convierte en clase 1; en caso contrario, clase 0. La captura contiene el código de conversión, pero no la salida final de la tabla.

![Conversión de puntuaciones a clases binarias](12-clasificacion-binaria.png)
