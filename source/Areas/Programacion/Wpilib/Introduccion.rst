########################
Introducción a la WPILIB
########################

La **WPILIB** es una librería de software de código abierto diseñada para facilitar la programación 
de robots en la competencia FIRST Robotics Competition (FRC). 
Proporciona una amplia gama de herramientas, clases y funciones que permiten a los equipos de robótica 
desarrollar software para controlar sus robots de manera eficiente y efectiva.

Aqui les dejo el link oficial de la WPILIB (stable): `WPILIB Stable <https://docs.wpilib.org/en/stable/>`_
Y aqui el reciente: `WPILIB Latest <https://docs.wpilib.org/en/latest/>`_

De igual manera, la **WPILIB** tiene su propio glosario para poder entender varia terminología
usada en la librería. Eso sí, tengo que aclarar que a pesar de que nuestra documentación esté en español,
la documentación oficial de la **WPILIB** está en inglés, por lo que para entrar en detalles, es mejor
tener un buen dominio del inglés técnico para poder tener una mejor comprensión.
El glosario oficial de la WPILIB es el siguiente: `WPILIB Glossary <https://docs.wpilib.org/en/latest/docs/software/frc-glossary.html#term-design-pattern>`_
Puede que en un futuro hagamos una traducción de este glosario que tengan una definición traducida exacta y con ejemplos.

Ahora sí, hay que dejar algo claro, la **WPILIB** tiene grandes explicaciones acerca de los temas, pero
a veces, estas explicaciones pueden ser un poco difíciles de entender para los nuevos programadores, por lo que,
vamos a resaltar ciertas explicaciones alargando ciertos puntos y dándoles ejemplos prácticos para que 
puedan tener una mayor comprensión.

Cabe aclarar que toda la información usada en esta documentación es sacada de la documentación oficial
de la **WPILIB**, por lo que, siempre vamos a dejar los links oficiales para que puedan consultar la
información directamente antes de cada explicación dada.

En el tema de la programación, la **WPILIB** está dividida en 2 partes, **bases de la programación** y 
**programación avanzada.** 

Bases de la programación
************************

Esta parte de la programación se refiere a los fundamentos básicos de la programación con la **WPILIB**.
Aquí se abarcan temas como:
- VS Code
- Dashboards
- Telemetría

Programación avanzada
*********************

Esta parte de la programación se refiere a temas más avanzados de la programación con la **WPILIB**.
Aquí se abarcan temas como:
- Command based programming
- Cinemática y odometría
- Controles avanzados
- NetworkTables
- Visión

.. toctree::
  :maxdepth: 1

    Framework Command Based Programming <AdvancedProgramming/CommandBasedProgramming/Introduccion>
    Cinemática <AdvancedProgramming/Kinematics/Introduccion>
    Odometría <AdvancedProgramming/Odometry/Introduccion>