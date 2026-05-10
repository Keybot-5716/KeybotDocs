################
Visión en FRC
################

¿Qué es una Limelight?
######################
La Limelight es una cámara de visión artificial  diseñada específicamente para robots de la competencia FIRST Robotics Competition (FRC).
 
¿Cómo funciona la Limelight?
################################

1.  Captura de imagen
---------------------------

La Limelight captura imágenes continuamente mediante su sensor integrado y las procesa a alta tasa de refresco (hasta 90 FPS dependiendo de la configuración). Estas imágenes constituyen la entrada del pipeline de visión.

2. Pipeline de procesamiento interno
---------------------------------------

La Limelight ejecuta un pipeline de visión que procesa cada fotograma sin intervención de la roboRIO. Este pipeline incluye varias etapas configurables desde la interfaz web:

- **Preprocesamiento (Input)**:
  
  Ajuste de parámetros de la imagen como exposición, balance de blancos y resolución antes del procesamiento.

- **Thresholding (Segmentación/umbralización)**:
  
  Aplicación de filtros (por ejemplo en espacio de color HSV) para aislar regiones de interés según características visuales específicas.

- **Detección de contornos (Contour Extraction)**:
  
  Identificación de blobs o formas candidatas que coinciden con el target.

- **Filtrado de contornos (Contour Filtering)**:
  
  Eliminación de detecciones no válidas mediante criterios geométricos (área, relación de aspecto, etc.).

- **Estimación geométrica**:
  
  Cálculo de métricas del target como:
  
  - Offset horizontal (**tx**) y vertical (**ty**) en grados respecto al eje óptico (0,0)
  - Área proyectada (**ta**) como porcentaje de la imagen
  - Rotación o skew (**ts**)
  - Parámetros 3D (posición y orientación) en pipelines avanzados

Este procesamiento ocurre completamente dentro del dispositivo, eliminando la necesidad de ejecutar algoritmos de visión en el roboRIO. 

3. Publicación de datos (NetworkTables)
------------------------------------------

Los resultados del pipeline se publican en la tabla `"limelight"` usando las **NetworkTables** para el  intercambio de datos a alta frecuencia. 

Variables clave:

- ``tv`` → Indicador de target válido (0 o 1)
- ``tx`` → Desviación horizontal 
- ``ty`` → Desviación vertical 
- ``ta`` → Área del target (0–100%)
- ``ts`` → Rotación del target
- ``tl`` → Latencia del pipeline (ms)

Estos datos son leídos por el código del robot y sirven para tener control en tiempo real.

4. Integración con el control del robot
------------------------------------------

El roboRIO toma los valores de NetworkTables para la parte de algoritmos de control (por ejemplo PID). Un caso típico es la alineación automática:

- El error del ángulo se obtiene de ``tx``
- El controlador ajusta la velocidad angular del robot hasta que ``tx ≈ 0``
- ``ta`` puede utilizarse como estimación aproximada de distancia

Debido a que la Limelight realiza todo el procesamiento de imágenes, la roboRIO se limita a la parte de lógica de control y gracias a esta organización se pueden optimizar los recursos del sistema de mejor forma.


.. important:: Es importante mencionar que la LimeLight se puede llegar a calentar mucho después de unos minutos de uso debido a la alta cantidad de datos que procesa, por lo que lo recomendable limitar su uso a la duración de un match (2 min aprox.) y dejarla reposar unos 15 minutos