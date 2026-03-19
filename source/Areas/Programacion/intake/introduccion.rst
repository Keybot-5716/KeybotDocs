################
Intake en FRC
################

¿Qué es el intake?
######################
Es un mecanismo activo de recolección impulsado por un motor, diseñado para recoger elementos del juego y pensado para ser rápido confiable y tolerante a los errores de alineación. 
Su éxito en la competencia depende de su geometría, materiales y su velocidad.


¿Cómo funcionan el intake?
################################
El intake funciona de manera continua succionando o atrayendo con diferentes medios, como rodillos, ruedas, correas o tubos.
Se debe considerar diseñar un margen de error, la orientación de las piezas del juego y la alineación del robot para tener una buena resistencia en la competencia.
Además de que no sea necesario, pero se recomienda dedicarle un motor al intake.


Código
#######

.. important:: La siguiente explicación del código es usando la plantilla de Advantage Kit.

Intake del Pivot
----------------

Este subsistema controla el ángulo del intake mediante un motor (TalonFX en hardware real, simulación en `IntakePivotIOSim`).

.. code-block:: java

    public interface IntakePivotIO {
      @AutoLog
      public class IntakePivotIOInputs {
        public boolean motorConnected;
        public double appliedVolts;
        public double tempCelcius;
        public double positionIntake;
      }

      void updateInputs(IntakePivotIOInputs inputs);

      void setVoltage(double voltage);

      void stopMotor();
    }

Definimos la interfaz del **pivot del intake**, que sirve como puente entre el código y el hardware físico.
La clase interna ``IntakePivotIOInputs`` almacena las lecturas principales las cuales son: conexión del motor, voltaje aplicado, temperatura y posición.
Con el método ``updateInputs`` actualizamos estas lecturas periódicamente.
Con ``setVoltage`` aplicamos voltaje al motor, mientras que ``stopMotor`` lo detiene de forma segura.

.. code-block:: java

    public class IntakePivotIOSim implements IntakePivotIO {

      private final IntakeSimulation intakeSimulation;

      /**
       * Este método sirve para configurar la especificaciones del intake que se va a mostrar simulado
       *
       * @param driveTrain Sirve para tener en cuenta en qué base estara este intake
       */
      public IntakePivotIOSim(AbstractDriveTrainSimulation driveTrain) {
        this.intakeSimulation =
            IntakeSimulation.OverTheBumperIntake(
                "Fuel",
                driveTrain,
                Meters.of(0.508), // 20 inches
                Meters.of(0.2),
                IntakeSimulation.IntakeSide.FRONT,
                20);
      }
    }

Esta clase implemente la interfaz del ``IntakePivotIO`` que permite probar el funcionamiento del intake sin necesidad del hardware en un entorno de simulación.
La variable interna ``intakeSimulation`` representa el intake simulado y contiene toda la lógica por parte de la extensión, retracción y la detección de piezas de juego.
En cuanto al método constructor ``IntakePivotIOSim(AbstractDriveTrainSimulation driveTrain)``, tiene parámetros específicos como:
- Tipo de objeto que recogerá: ``Fuel``.
- Base de conducción a la que está conectado: ``driveTrain``.
- Ancho del intake: ``Meters.of(0.508)`` ancho del intake (20 pulgadas).
- Extensión fuera del chasis: ``Meters.of(0.2)``.
- Lado de montaje: ``FRONT`` (parte frontal del robot).
- Capacidad máxima: ``20`` piezas.

.. code-block:: java

    /**
     * Con esto podemos hacer que extienda o que retraiga el intake
     *
     * @param runIntake sirve si es que queremos que se extienda o se retraiga
     */
    public void setRunning(boolean runIntake) {
      if (runIntake)
        intakeSimulation.startIntake();
      else
        intakeSimulation.stopIntake();
    }

    /**
     * @return si hay algun fuel en el intake
     */
    public boolean isFuelInsideIntake() {
      return intakeSimulation.getGamePiecesAmount() != 0;
    }

    /** */
    public void launchFuel() {
      if (intakeSimulation.obtainGamePieceFromIntake()) {}
      // ShooterIOSim.launchFuel(); // futuro: notificar al shooter
    }

Ahora tenemos algunos de los métodos como ``setRunning(boolean runIntake)`` en el que podemos controlar si el intake se enciende o se retrae.
Cuando es true, detecta pieza del juego y se extiende. 
Cuando es false, se retrae dentro del chasis y deja de recolectar.

El siguiente método es ``isFuelInsideIntake`` el cual devuelve un valor booleano para incidcar si hay alguna pieza de juego dentro del intake.
Terminamos con el método ``launchFuel`` simulando el lanzamiento de una pieza de juego desde el intake.

.. code-block:: java

    @Override
    public void updateInputs(IntakePivotIOInputs inputs) {}

    @Override
    public void setVoltage(double voltage) {}

    @Override
    public void stopMotor() {}

Finalmente algunos de los métodos sobreescritos, con ``updateInputs`` actualizamos las lecturas del motor (voltaje,temperatura,posición), mientras que en simulación no se modifican valores.
Con ``setVoltage`` es el voltaje que vamos a establecer y se aplica al motor para mover el pivot.
El último metódo ``stopMotor`` detiene el motor de forma segura.


Subsistema del intake
---------------------

El subsistema del Intake coordina el funcionamiento del pivot y los rollers del intake.
Se construye en la clase ``SubsystemBase`` y es la conexión entre las interfaces de hardware/simulación y lógica del robot.

.. code-block:: java

    package frc.robot.subsystems.intake;

    import edu.wpi.first.math.controller.ProfiledPIDController;
    import edu.wpi.first.math.filter.Debouncer;
    import edu.wpi.first.math.filter.Debouncer.DebounceType;
    import edu.wpi.first.math.trajectory.TrapezoidProfile;
    import edu.wpi.first.wpilibj2.command.SubsystemBase;
    import frc.lib.team6328.LoggedTunableNumber;
    import frc.robot.subsystems.intake.pivot.IntakePivotIO;
    import frc.robot.subsystems.intake.pivot.IntakePivotIOInputsAutoLogged;
    import frc.robot.subsystems.intake.rollers.IntakeRollersIO;
    import frc.robot.subsystems.intake.rollers.IntakeRollersIOInputsAutoLogged;
    import org.littletonrobotics.junction.Logger;

    public class IntakeSubsystem extends SubsystemBase {
      private final IntakePivotIO pivotIO;
      private final IntakeRollersIO rollersIO;

      // private final IntakeIOInputsAutoLogged inputs = new IntakeIOInputsAutoLogged();
      private final IntakePivotIOInputsAutoLogged pivotInputs = new IntakePivotIOInputsAutoLogged();
      private final IntakeRollersIOInputsAutoLogged rollersInputs =
          new IntakeRollersIOInputsAutoLogged();

      // Creamos un objeto TrapezoidProfile que tenga velocidad y aceleración máximas
      private TrapezoidProfile.Constraints profile = new TrapezoidProfile.Constraints(2.0, 2.0);
      private ProfiledPIDController controller = new ProfiledPIDController(4, 0, 0.08, profile);
      private static TrapezoidProfile.State goal = new TrapezoidProfile.State(0, 0);

      private static final LoggedTunableNumber rollerVolts =
          new LoggedTunableNumber("Intake/Rollers/RollerVoltsIntake", 5.0);

      private static final LoggedTunableNumber pivotVolts =
          new LoggedTunableNumber("Intake/Pivot/PivotVolts", 2.0);

En este bloque de código comenzamos por la conexión de las interfaces `IntakePivotIO` y los rollers `IntakeRollersIO`, se crean objetos para registrar sus lecturas con ``pivotInputs``, ``rollersInputs``.
Se configura un controlador PID con perfil trapezoidal para mover el pivot suavemente. Además de definir los voltajes ajustables ``rollerVolts`` y ``pivotVolts`` los cuales nos permiten controlar la 
potencia en tiempo real.

.. code-block:: java

    private final Debouncer pivotDebouncer = new Debouncer(0.5, DebounceType.kFalling);
    // private final Alert pivotDisconnected = new Alert("Pivot disconnected :(",
    // Alert.AlertType.kWarning);

    private DesiredState desiredState = DesiredState.STOPPED;
    private IntakeState intakeState = IntakeState.STOPPING;

    private double desiredRollersVoltage;
    private double desiredPivotVoltageForOpenLoop;

    public enum DesiredState {
      STOPPED,
      FORWARD_ROLLERS,
      REVERSE_ROLLERS,
      FORWARD_PIVOT,
      REVERSE_PIVOT,
      STOPPPED_PIVOT,
      IN,
      OUT
    }

    private enum IntakeState {
      STOPPING,
      FORWARDING_ROLLERS,
      REVERSING_ROLLERS,
      FORWARDING_PIVOT,
      REVERSING_PIVOT,
      STOPPING_PIVOT,
      INING,
      OUTING
    }

    public IntakeSubsystem(IntakeRollersIO rollersIO, IntakePivotIO pivotIO) {
      this.rollersIO = rollersIO;
      this.pivotIO = pivotIO;
    }

Aqui definimos la lógica de estados del intake.
Ocupamos un Debouncer para filtrar las señales del pivot, y se inicializan dos enums ``DesiredState`` (lo que queremos que haga el intake) y ``IntakeState`` (Acción que se ejecuta).
También se guardan voltajes deseados para rollers y pivot en modo abierto. Por último, el constructor conecta el subsistema con las interfaces de hardware ``rollersIO`` y ``pivotIO``.


