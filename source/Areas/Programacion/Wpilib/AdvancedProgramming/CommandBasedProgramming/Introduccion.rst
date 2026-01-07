########################################
Introducción al framework Command based
########################################

.. note:: Esta información NO es propia, si quieres consultar la original, puedes verlo en `Command Based Programming <https://docs.wpilib.org/en/latest/docs/software/commandbased/index.html>`_

Empezando en Command based
***************************

.. note:: La información sacada en este bloque proviene de: `Command Based Programming <https://docs.wpilib.org/en/latest/docs/software/commandbased/what-is-command-based.html>`_

.. warning:: Esta sección está en Java

El framework Command based es la continuación del modelo normal de TimedRobot. Es importante saber que este
framework está diseñado para evitar iterar de manera innecesaria, es decir, en lugar de:

.. tabs::

    .. tab:: **Timed** 

        .. code-block:: java
    
            public class Robot extends TimedRobot {
                @Override
                public void teleopPeriodic() {
                    if(condition.isTrue()) {
                        doSomething();
                    } else {
                        doSomethingElse();
                    }
                }
            }

    .. tab:: **Command-based**

        .. code-block:: java
    
            public class Robot extends TimedRobot {
                private final Command doSomethingCommand = new DoSomethingCommand();
                private final Command doSomethingElseCommand = new DoSomethingElseCommand();

                @Override
                public void teleopInit() {
                    if(condition.isTrue()) {
                        doSomethingCommand.schedule();
                    } else {
                        doSomethingElseCommand.schedule();
                    }
                }
            }

a