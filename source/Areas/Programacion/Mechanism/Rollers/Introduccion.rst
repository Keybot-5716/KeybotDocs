################
Rodillos en FRC
################

¿Qué son los rollers?
######################
Los rollers son mecanismos utilizados en los robots de FRC para manipular y transportar piezas de juego (game pieces) dentro del campo de juego. 
Estos mecanismos suelen consistir en una serie de rodillos motorizados que giran para agarrar, mover o lanzar las piezas de juego.

¿Cómo funcionan los rollers?
################################
Estos funcionan mediante uno dos motores que hacen girar a los rodillos muy rápido,
esto permite que las piezas de juego se agarren perfectamente y se puedan transportar o lanzar con precisión.

Código
#######

.. important:: La siguiente explicación del código es usando la plantilla de Advantage Kit.

Interfaz básica de un roller
-----------------------------

En la interfaz primeramente hay un constructor que está siendo cargado con @AutoLog


.. code-block:: java

    package frc.robot.subsystems.rollers;

    import org.littletonrobotics.junction.AutoLog;

    public interface RollerIO {

      @AutoLog
      public class RollerIOInputs {
        public boolean motorConnected;
        public double acceleration;
        public double appliedVolts;
        public double tempCelcius;
      }

      void updateInputs(RollerIOInputs inputs);

      void setVoltage(double voltage);

      void stopRoller();
    }





