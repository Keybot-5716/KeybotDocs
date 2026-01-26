Area de Electrónica (Keybot 5716)
=================================

La electrónica es una área fundamental dentro del robot, 
ya que sin electricidad el robot simplemente no funciona.

Motor Neo v1.1
###############

Es un motor que podemos utilizar en distintos subsistemas cómo (rollers, intake, conveyors, etc).  Este motor funciona con el controlador SPARK MARX.
Su voltaje de funcionamiento es de 12V
Costo: 50 dls
Peso: 0.424 kg
Potencia máxima: 406W

Spark MAX
###########

Es un controlador de motor avanzado en el que se puede modificar la velocidad, posición, corriente, PID interno, hasta rampas de aceleración. Es compatible con controladores motores DC (con y sin escobillas). 
Tiene un voltaje de funcionamiento de 12V y sus conexiones son mediante USB, CAN y PWM.
El precio aproximado de este controlador ronda entre los $100 USD, basado en REV ROBOTICS.

Modos de operación principales
###############################

Duty Cycle (Percent Output): Permite el control directo de la potencia (ej. set(0.5) = 50%).
Velocity Control: Mantiene la velocidad objetivo usando el encoder interno.
Position Control: Mueve el motor a una posición específica.
Smart Motion: Control avanzado con límites de aceleración y velocidad.

Configuración inicial recomendada:
###################################

Restaurar los valores de fábrica: Evita las configuraciones previas mediante la siguiente función restoreFactoryDefaults().
Idle Mode: setIdleMode(IdleMode.kBrake) o IdleMode.kCoast según se requiera que el motor se frene o quede libre.
Inversión de giro: setInverted(true/false) según el montaje físico
Limitar corriente: setSmartCurrentLimit(40) para proteger el motor y el controlador.
Encoder: Acceso con getEncoder() para leer posición y velocidad.

Motor Kraken X60
##################
Motor eléctrico de alto rendimiento sin escobillas (brushless).
Ejerce una potencia máxima de 1,108 W, causando una eficiencia máxima del 87% y un par de pérdida de 7,09 Nm. Además de que integra el controlador avanzado de Talon FX de CTR para tener un control de movimiento preciso y a diferencia de los Spark MAX este motor te da más feedback a la hora de usarlos y querer ver su estado.

El precio aproximado es de 400 USD, aunque para los equipos e instituciones de la competición de robótica FIRST califican para un precio de 218 USD.

Código
#######


Imports para Kraken X60 (Talon FX)
----------------------------------

En el primer conjunto de código ponemos las importaciones que van a servir para poder mover el motor de cierta manera.
En el segundo conjunto nos servira para poner cierta configuración al motor, como por ejemplo invertir el sentido al que está girando, entre otras.
En el último conjunto va a servir para poner las correspondientes configuraciones para el manejo del voltaje.

.. code-block:: java

   // TalonFX (Kraken X60) y control básico por duty cycle/voltaje
   import com.ctre.phoenix6.hardware.TalonFX;
   import com.ctre.phoenix6.controls.DutyCycleOut;   // salida tipo % (abierto)
   import com.ctre.phoenix6.controls.VoltageOut;     // salida en volts (abierto)

   // Configuraciones (neutral mode, invert, límites, etc.)
   import com.ctre.phoenix6.configs.TalonFXConfiguration;
   import com.ctre.phoenix6.signals.NeutralModeValue; // Brake / Coast
   import com.ctre.phoenix6.signals.InvertedValue;

   // Límites de corriente
   import com.ctre.phoenix6.configs.CurrentLimitsConfigs;


