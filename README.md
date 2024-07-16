CLASIFICACIÓN DE RESONANCIAS MAGNÉTICA DEL CEREBRO PARA DETERMINAR LA EXISTENCIA (O NO) DE UN TUMOR CEREBRAL

El programa presentado presenta 2 aproximaciones para clasificar resonancias magnéticas (MRI) del cerebro de diferentes pacientes, determinando así si hay presencia de un tumor cerebral o no. Para ello se utilizaron 662 fotos de MRIs etiquetadas: 334 de ellas correspondían a pacientes sin tumores cerebrales (etiquetadas como "no") y 328 a pacientes con tumores cerebrales (etiquetadas como "yes"). Estas imágenes fueron rescatadas de dos bases de datos presentes en Kaggle:
    -https://www.kaggle.com/datasets/navoneel/brain-mri-images-for-brain-tumor-detection/data
    -https://www.kaggle.com/datasets/masoudnickparvar/brain-tumor-mri-dataset
Posteriormente también se utilizaron 20 imágenes más para graficar la funcionalidad y efectividad de ambos modelos


*1er aproximación - Clasificador propio:
    -Se utilizó un generador de datos para aumentar el volumen de datos a utilizar dado que los resultados no estaban siendo del todo óptimos
    -Se utilizaron capas convolucionales, de agrupación, densas y de drop_out para mejorar el funcionamiento del modelo
    -Se utilizó un optimiador de adam
    -Se fue variando el valor de learning_rate, hasta concluir que los mejores resultados eran entregados con un valor de 0,001
    -Se fue variando el valor de batch_size, hasta concluir que los mejores resultados eran entregados con un valor de 32
    -Se fue variando el valor de epocas, para no caer en un sobreajuste, hasta concluir que con un valor de 30 ya era suficiente

    Cabe aclarar que fue un modelo bastante tedioso para entrenar dado que consumía muchos recursos de la PC y era extremadamente lento; los resultado finales no fueron del todo exactos, sin embargo teniendo en cuenta la complejidad del problema fueron aceptables. Es por ello que luego se implementó un modelo utilizando transfer learning

    Para la última época de entrenamiento se obtuvo:
            accuracy: 0.8064 - loss: 0.4243 - val_accuracy: 0.7071 - val_loss: 0.4985


*2da aproximación - Clasificador utilizando transfer learning:
    -No se utilizó un generador de datos dado que ya no eran necesarios tantos datos ya que el modelo estaba preentrenado, y ademas me dificultaba el entrenamiento final del modelo ya que tomaba una eternidad
    -Se utilizó un modelo pre-entrenado de movilenet_v2, rescatado del siguiente link:
        "https://tfhub.dev/google/tf2-preview/mobilenet_v2/feature_vector/4"
    -Se utilizó tf_keras en lugar de tf.keras dado que este último me generaba problemas de compatibilidad con el transfer learning

    En este caso, las 2 grandes complicaciones que tuve fueron: con el uso de tf.keras (que me resultaba incompatible por lo que tuve que buscar otras soluciones en internet), y con el entrenamiento (dado que me era imposible al usar un generador de imágenes por el tiempo que demoraba, por lo que procedí a desestimarlo)

    Para la última época de entrenamiento se obtuvo:
            loss: 0.0365 - accuracy: 1.0000 - val_loss: 0.1533 - val_accuracy: 0.9397   
