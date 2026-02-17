################
Rodillos en FRC
################

¿Qué son los rollers?
######################
Los rollers son mecanismos utilizados en los robots de FRC para manipular y transportar piezas de juego (*game pieces*) dentro del campo de juego. 
Estos mecanismos suelen consistir en una serie de rodillos motorizados que giran para agarrar, mover o lanzar las piezas de juego.

¿Cómo funcionan los rollers?
################################
Funcionan mediante uno o dos motores que hacen girar a los rodillos muy rápido,
esto permite que las piezas de juego se agarren perfectamente y se puedan transportar o lanzar con precisión.

Código
#######

.. important:: La siguiente explicación del código es usando la plantilla de Advantage Kit.

Interfaz básica de un roller
-----------------------------

En la interfaz primeramente hay un constructor que está siendo cargado con ``@AutoLog``.

.. code-block:: java

    package frc.robot.subsystems.rollers;

    import org.littletonrobotics.junction.AutoLog;

    public interface RollerIO {

      @AutoLog
      public static class RollerIOInputs {
        public boolean motorConnected;
        public double acceleration;
        public double appliedVolts;
        public double tempCelcius;
      }

      void updateInputs(RollerIOInputs inputs);

      void setVoltage(double voltage);

      void stopRoller();
    }

Susbsistema de un Roller
########################

¿Qué es un subsistema?
-----------------------
Los subsistemas representan las partes físicas del robot (motores, sensores y mecanismos), además de permitirnos
organizar la lógica de su control.

.. code-block:: java

    package frc.robot.subsystems.rollers;

    import edu.wpi.first.wpilibj2.command.SubsystemBase;
    import org.littletonrobotics.junction.Logger;

    public class RollerSubsystem extends SubsystemBase {
    }

Este subsistema se encarga de controlar los rollers del robot.
Centraliza el movimiento del motor y evita que otros
archivos manipulen directamente el hardware.

.. code-block:: java

    private final RollerIO io;
    private final RollerIOInputsAutoLogged inputs = new RollerIOInputsAutoLogged();
    private DesiredState desiredState = DesiredState.STOPPED;
    private RollersState rollersState = RollersState.STOPPING;
    private double desiredVoltage;

Aquí definimos las variables internas del subsistema roller que registran la comunicación con la interfaz (``io``).
Además, se registran las entradas (``inputs``), los estados del robot o los deseados (``desiredState`` y ``rollersState``), 
y por último el voltaje para controlar su movimiento.

.. code-block:: java

    public enum DesiredState {
        STOPPED,
        FORWARD,
        REVERSE
    }

    private enum RollersState {
        STOPPING,
        FORWARDING,
        REVERSING
    }

    public RollerSubsystem(RollerIO io) {
        this.io = io;
    }

Establecemos las enumeraciones de estados, en el que ``DesiredState`` puede ser solicitado por otros comandos (detener, avanzar y retroceder).
A su vez ``RollersState`` es el estado actual. Finalmente se incluye al constructor del subsistema recibiendo el objeto ``RollerIO``
y enlazando el código con el hardware físico.

.. code-block:: java

    @Override
    public void periodic() {
        Logger.processInputs("RollerInputs", inputs);
        io.updateInputs(inputs);

        rollersState = setStateTransition();
        applyStates();
    }

    public void stop() {
        io.stopRoller();
    }

    public void setVoltage(double voltage) {
        io.setVoltage(voltage);
    }

Las funciones de control y actualización del subsistema del roller:  
``periodic()`` será ejecutado constantemente mientras el robot esté activo, actualizará las entradas de hardware, registrará y aplicará la información del estado correspondiente.  
``stop()`` detiene el roller por completo.  
``setVoltage(double voltage)`` aplica un voltaje al roller para controlar su movimiento.

.. code-block:: java

    private RollersState setStateTransition() {
        return switch (desiredState) {
            case STOPPED -> RollersState.STOPPING;
            case FORWARD -> RollersState.FORWARDING;
            case REVERSE -> RollersState.REVERSING;
        };
    }

    private void applyStates() {
        switch (rollersState) {
            case FORWARDING -> setVoltage(desiredVoltage);
            case REVERSING -> setVoltage(-desiredVoltage);
            case STOPPING -> setVoltage(0);
        }
    }

    public void setDesiredState(DesiredState desiredState) {
        this.desiredState = desiredState;
    }

    public void setDesiredStateWithVoltage(DesiredState desiredState, double desiredVoltage) {
        this.desiredState = desiredState;
        this.desiredVoltage = desiredVoltage;
    }

En este fragmento:  
- ``setStateTransition()`` pasa de un estado deseado a uno actual.  
- ``applyStates()`` aplica el comportamiento del motor según el estado actual.  
- Los métodos ``setDesiredState()`` y ``setDesiredStateWithVoltage()`` permiten cambiar el estado del roller con o sin especificar el voltaje.
