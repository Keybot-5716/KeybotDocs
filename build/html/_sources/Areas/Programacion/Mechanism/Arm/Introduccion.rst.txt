##############
Brazos en FRC
##############

¿Qué es el Brazo?
######################
El Brazo es un mecanismo estructural articulado o telescópico, montado sobre el chasis o en la superestructura del robot.
Esta compuesto por elementos rigidos (como tubos o placas de aluminio) unidos por ejes o juntas y es accionado por sistemas de transmisión (motores,engranajes, cadenas o correas), cuya construcción amplia el alcance fisico del robot.



¿Cómo funcionan el Brazo?
################################
El funcionamiento del brazo es la combinación de la motores, transmisión, estructura y sensores.
En otras palabras, es un subsistema mecánico controlado electrónicamente,  en el que gracias a los motores que transmiten movimiento a la estructura articulada y a su vez que esta regulada por sensores, se logra el desplazamiento repetitivo y confiable.


Código
#######

.. important:: La siguiente explicación del código es usando la plantilla de Advantage Kit.



Interfaz del Brazo
------------------

.. code-block:: java

    package frc.robot.subsystems.arm;

    import org.littletonrobotics.junction.AutoLog;

    public interface ArmIO {
      @AutoLog
      public static class ArmIOInputs {
        ArmIOData data = new ArmIOData(false, 0, 0, 0, 0, 0);
      }

      record ArmIOData(
          boolean motorConnected,
          double positionRotations,
          double velocityRotationsPerSec,
          double appliedVolts,
          double supplyCurrentAmps,
          double tempCelsius) {}

      public default void updateInputs(ArmIOInputs inputs) {}

      public default void stop() {}

      public default void runOpenLoop(double output) {}

      public default void setVoltage(double volts) {}

      public default void setPosition(double position) {}

      public default void setNeutralModeBreak(boolean enable) {}

      public default void resetEncoder() {}

      public default void setPID(double kP, double kI, double kD) {}
  
      /** Se utiliza para protocolos SysID */
      default void optimizeForSysID() {}
    }

Comenzamos con ``ArmIOData``, el cual nos mostrara si el motor esta funcionando y los datos acerca de la posición, velocidad, voltaje, corriente y temperatura.
Después tenemos los metodos básicos para controlar el brazo, en los cuales esta el método ``updateInputs`` para actualizar las entradas, el siguiente es ``stop`` para detener, ``runOpenLoop`` mover en modo abierto.
Finalmente, aplicar voltaje ``setVoltage``, fijar posición ``etPosition``, reiniciar el encoder ``resetEncoder`` o ajustar parámetros PID ``setPID``. Además, incluye optimizeForSysID, usado en protocolos de caracterización del motor. 
Toda esta interfaz es la conexión entre el código y el hardware.

Subsistema del Brazo
--------------------