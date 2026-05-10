################
Calibración y Configuración de Pipelines
################

En esta sección veremos los principios para calibrar la LimeLight y personalizar los pipelines de visión para mejorar la precisión según lo que se requiera.

Calibración de la cámara
#########################

Este procedimiento es muy importante porque permite asegurar que los datos obtenidos sean consistentes con la posición real del objetivo, reduciendo el margen de error.


Para realizar la calibración, sigue estos pasos:

- Ajustar el campo de visión (FOV) de acuerdo con la posición real de la cámara en el robot.
- Verificar el ángulo en el que se encuentra la cámara, asegurando que esta esté correctamente orientada hacia el punto deseado.
- Definir la altura de la cámara respecto al suelo y su posición en el robot.
- Realizar pruebas con los objetivos y comprobar la precisión obtenida.
Durante este proceso, es importante observar estos siguientes valores de salida:

- ``tx``: desplazamiento horizontal del objetivo.
- ``ty``: desplazamiento vertical del objetivo.
- ``ta``: área del objetivo detectado.


Estos datos te ayudan a ajustar cómo funciona el sistema de visión y aumentar la exactitud de la posición del objeto.

Configuración de los Pipelines
#########################

Los **pipelines** son métodos de procesamiento de datos que permiten a la Limelight detectar objetos específicos mediante visión artificial.

Para configurarlos, realiza estos siguientes pasos: 

- Acceder a la interfaz web de la Limelight.
- Seleccionar un pipeline disponible o crear uno nuevo.
- Elige el tipo de objetivo o modo de visión (**AprilTag, detección de color, objetos retroreflectivos o modelos entrenados de visión artificial**).

- Ajustar parámetros como:

    - Umbrales de color (HSV)
    - Exposición de la cámara
    - Brillo y saturación

- Por último, verifica en tiempo real la detección en el stream de video.

Es bueno ir haciendo ajustes de forma iterativa hasta que la detección sea estable y lo más precisa posible.


