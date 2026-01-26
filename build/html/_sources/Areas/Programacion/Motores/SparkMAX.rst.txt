Codigo de ejepmlo para NEO V1.1
################################

.. code-block:: java

    import com.revrobotics.spark.SparkBase.PersistMode; // Para guardar configuración en el flash
    import com.revrobotics.spark.SparkBase.ResetMode;   // Resetea configuraciones previas
    import com.revrobotics.spark.SparkLowLevel.MotorType; // Si el motor es brushed o brushless
    import com.revrobotics.spark.SparkMax;              // Representa el controlador físico Spark MAX
    import com.revrobotics.spark.config.SparkBaseConfig.IdleMode; // Se define Brake o Coast
    import com.revrobotics.spark.config.SparkMaxConfig; // Objeto para configurar los parámetros

.. code-block:: java

    // Clase para los rollers que implementa la interfaz RollerIO
    public class RollerIOSparkMax implements RollerIO {
        // Aquí iría la implementación de los métodos definidos en RollerIO
    }
