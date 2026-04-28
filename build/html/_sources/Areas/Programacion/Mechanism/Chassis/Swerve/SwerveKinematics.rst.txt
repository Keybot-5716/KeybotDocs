###################
Swerve Kinematics
###################

¿Cómo funciona el código de swerve?
************************************

Para entender como funciona el código de la swerve, primero debemos entender el cómo funcionan
las llantas, porque, como ya sabemos, el chassis es omnidireccional, pero para poder llegar a eso,
cada módulo tiene que saber a qué angulo específico debe de moverse (incluyendo la dirección o heading).
Para ello, utilizamos un término que se llama kinematics, o más específico, las ``SwerveDriveKinematics``
Las ``SwerveDriveKinematics`` prácticamente utilizan las localizaciones de cada módulo para determinar el ángulo
de rotación y velocidad (en formato de ChassisSpeeds obj) y regresa un ``SwerveModuleState``.
El ``SwerveModuleState`` se utiliza para que el ángulo del giroscopio y la velocidad sean las correspondientes para 
avanzar a la dirección deseada mirando al heading deseado.

Objeto Kinematics
******************

La clase de ``SwerveDriveKinematics`` necesita necesariamente las localizaciones de cada módulo en ``Translation2d``, eso sí,
parece chistoso pero se puede hacer una swerve programada de menos de 4 o mas de 4 llantas, pero para esta documentación vamos
a ser personas cuerdas y vamos a utilizar solo 4 llantas. (FL, FR, BL, BR)

.. tabs::

      .. code-block:: java

         // Estas son relativas al centro del robot
         Translation2d m_frontLeftLocation = new Translation2d(0.381, 0.381);
         Translation2d m_frontRightLocation = new Translation2d(0.381, -0.381);
         Translation2d m_backLeftLocation = new Translation2d(-0.381, 0.381);
         Translation2d m_backRightLocation = new Translation2d(-0.381, -0.381);

         // Aqui creamos el objeto usando las localizaciones
         SwerveDriveKinematics m_kinematics = new SwerveDriveKinematics(
            m_frontLeftLocation, m_frontRightLocation, m_backLeftLocation, m_backRightLocation
         );
