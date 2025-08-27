Configuración de la Driver Station
==================================

¿Qué es?
--------

La **DriverStation** es la aplicación donde se puede prender/apagar los modos del robot.
Hay que saber que el robot tiene los siguientes modos:

1) **Teleoperado:** Es el modo donde los **Drivers** lo pueden manejar.
2) **Autónomo:** Es el modo donde el robot se maneja autónomamente.
3) **Test:** Este modo es especial, ya que en código puedes estables que ciertas funciones sean de puras pruebas, es decir, puedes hacer que cierta parte del código sólamente se pueda ejecutar en este modo.
4) **Práctica:** Este modo se usa para simular una partida real, es decir, tiene cronometrado el **Autonomous mode**, el **Teleop mode** y el **ending** del match.

Ahora, el menú de la **DriverStation** la deben de visualizar algo así:

*imagen*

Y por este lado lateral podremos observar los cambios de modo del robot.

Para que el robot pueda funcionar, debe pasar por los **3** semáforos por así decirlo.

1) La computadora esté conectada a la radio del robot (Solo una computadora)
2) El código del robot se haya subido correctamente a la **roboRIO**
3) Tengan algún **joystick** conectado.

En el caso de que todo lo anterior se cumpla, entonces al presionar **Enable** debería habilitarse el uso del modo del robot:
*imagen*
