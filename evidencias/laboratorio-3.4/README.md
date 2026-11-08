# Laboratorio 3.4

En este laboratorio se preparó el conjunto de datos de columna vertebral, se separó en conjuntos de entrenamiento, prueba y validación, y se configuró un trabajo de entrenamiento XGBoost en SageMaker.

## Preparación de los datos

Se cargó el archivo ARFF y se exploraron sus dimensiones y columnas. El conjunto contiene 310 registros, seis características biomecánicas y la variable objetivo `class`.

![Importación y carga de los datos](01-carga-datos.png)

![Dimensiones y columnas del conjunto](02-exploracion-datos.png)

Se colocó `class` como primera columna, formato requerido por el contenedor XGBoost utilizado en el laboratorio.

![Variable objetivo en la primera columna](03-objetivo-primera-columna.png)

Se instaló scikit-learn para usar `train_test_split` en la partición estratificada.

![Instalación de la dependencia](04-instalacion-scikit-learn.png)

La partición reproducible reservó 80 % para entrenamiento y dividió el 20 % restante por igual entre prueba y validación. `stratify` conservó la proporción de clases: entrenamiento `(248, 7)`, prueba `(31, 7)` y validación `(31, 7)`.

![División estratificada y conteos](05-division-estratificada.png)

## Carga a S3 y entrenamiento

Se exportaron los tres conjuntos como CSV sin encabezados ni índice de pandas y se cargaron en las rutas de S3 del laboratorio.

![Exportación y carga de los CSV a S3](06-carga-datos-s3.png)

Se configuró XGBoost para clasificación binaria con `binary:logistic`, métrica AUC y 42 rondas. El estimador usa los canales `train` y `validation` para iniciar el entrenamiento.

![Configuración del estimador XGBoost](07-configuracion-xgboost.png)

![Inicio del entrenamiento en SageMaker](08-entrenamiento-xgboost.png)

La última captura de este laboratorio muestra el trabajo en ejecución. La captura de finalización se conserva en las evidencias del laboratorio 3.5.
