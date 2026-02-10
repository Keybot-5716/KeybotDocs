Codigo de ejepmlo para NEO V1.1
################################

.. code-block:: java

    import com.revrobotics.spark.SparkBase.PersistMode; // Para guardar configuración en el flash
    import com.revrobotics.spark.SparkBase.ResetMode;   // Resetea configuraciones previas
    import com.revrobotics.spark.SparkLowLevel.MotorType; // Si el motor es brushed o brushless
    import com.revrobotics.spark.SparkMax;              // Representa el controlador físico Spark MAX
    import com.revrobotics.spark.config.SparkBaseConfig.IdleMode; // Se define Brake o Coast
    import com.revrobotics.spark.config.SparkMaxConfig; // Objeto para configurar los parámetros


Ahora creamos una clase (en este caso para los rollers), en la cual implementamos la interfaz de RollerIO.

.. code-block:: java

    public class RollerIOSparkMax implements RollerIO {

Ahora creamos una variable controlada por CAN (Controller Area Network) y creamos un objeto de configuración para poder definir límites de corriente, modo break o coast, inversión del motor, etc.

.. code-block:: java

    private SparkMax motor;
    private SparkMaxConfig config = new SparkMaxConfig();

Creamos un constructor para iniciar con la configuración del Spark MAX.
Declaramos que el motor será un objeto de Spark MAX y declaramos el ID CAN tomado de las constantes para Rollers así como el tipo de motor.

.. code-block:: java

    motor = new SparkMax(RollerConstants.ROLLER_MOTOR_ID, MotorType.kBrushless);


IMPORTANTE: el tipo de motor debe ser brushless porque el NEO V1.1 lo es, si se pone Brushed el motor NO funcionará.


Configuración del Spark MAX

.. code-block:: java

    config.smartCurrentLimit(40).idleMode(IdleMode.kBrake);

    // smartCurrentLimit(40) limita el motor a 40 amperes 
    // (protege el motor, el controlador y el breaker del PDP/PDH)
    // con idleMode(IdleMode.kBrake) indicamos que el motor va a frenar 
    // cuando no reciba señal, de manera "inmediata"


