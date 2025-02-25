Control de motores DC
=========================


Objetivo
--------

Se realizará un sistema de control de motores de corriente directa (DC) con ayuda de un driver L298N.

.. note::

    En esta práctica, se utilizará el **RP2040**.

Descripción
-----------

Los sistemas de control son parte integral de nuestra sociedad actual y tienen múltiples aplicaciones, desde mantener una temperatura deseada hasta mantener la estación espacial en órbita. La definición de un sistema de control es: conjunto de procesos que en conjunto nos ayudan a obtener una salida esperada con un desempeño deseado dada una entrada específica.

Un sistema de control de motores DC puede ser fácilmente adecuado para aplicaciones como:

- Propulsión de vehículos
- Robots Móviles
  - Drones
  - Robots con ruedas u orugas
- Sistemas de refrigeración
  - Control de ventiladores
  - Extracción de humos
- Instalaciones de transporte
  - Bandas de transporte
- Sistemas de automatización
  - Persianas/Cortinas inteligentes
- Electrodomésticos y herramientas
  - Aspiradoras
  - Licuadoras
  - Dremel
  - Esmeril 
  - Taladros

Si bien existen muchas más, esta lista da una amplia idea de las aplicaciones de los motores DC.

Esta práctica se enfocará en realizar un sistema para controlar la velocidad y dirección de motores DC de manera precisa utilizando un dispositivo ESP32 o RP2040 con MicroPython.

Requisitos
----------

- 1x `Placa UNIT DualMCU <https://uelectronics.com/producto/unit-dualmcu-esp32-rp2040-tarjeta-de-desarrollo/>`_
- 2x `Motores de corriente continua (DC) <https://uelectronics.com/producto/l298n-modulo-driver-motor-a-pasos/>`_
- 1x `Driver L298N <https://uelectronics.com/producto/cables-dupont-largos-20cm-hh-mh-mm/>`_
- 1x `Alambre 24 AWG <https://uelectronics.com/producto/cable-de-alambre-estanado-24awg-25cm-100-piezas/>`_
- 1x `Cables Dupont: Hembra - Macho <https://uelectronics.com/producto/cables-dupont-largos-20cm-hh-mh-mm/>`_
- Alimentación de 12 V

Opcionalmente, se recomiendan los kits de robótica que incluyen motores y drivers:

- 1x `Kit Carrito 4WD Robot Educacional con Accesorio <https://uelectronics.com/producto/kit-carrito-4wd-robot-educacional-con-accesorios/>`_
- 1x `Kit Carrito Robot Seguidor de Líneas con Accesorios <https://uelectronics.com/producto/kit-carrito-robot-seguidor-lineas-con-accesorios/>`_




Código
------

Una vez realizadas las conexiones para un motor, se puede controlar dicho motor DC con el controlador L298N sin usar PWM, con el siguiente código. Ten en cuenta que en este ejemplo el motor solo podrá encenderse o apagarse, sin control de velocidad.

.. note::
    Código realizado para MicroPython utilizando la DualMCU con el microprocesador RP2040. Recuerda que puedes intercambiar entre microcontroladores con el selector USB.

.. container:: center

    .. figure:: /_static/2-Micropython/images/esp32_or_rasp.jpg
        :width: 50%
        :align: center
        :alt: sections of the code

Download unitRP2040_motors1.py:
-------------------------------

Diagrama para controlar un motor:

.. figure:: /_static/7-Control_de_motores_DC/images/UnMotor_bb.png
   :width: 70%
   :align: center
   :alt: sections of the code

.. `Download unitRP2040_motors1.py <../docs/7-Control_de_motores_DC/code/unitRP2040_motors1.py>`_

.. code-block:: python
    :linenos:

    from machine import Pin
    import time

    # Configura los pines para controlar el L298N
    l298n_enable = Pin(7, Pin.OUT)     # Conecta a EN del L298N
    l298n_input1 = Pin(14, Pin.OUT)     # Conecta a IN1 del L298N
    l298n_input2 = Pin(9, Pin.OUT)      # Conecta a IN2 del L298N

    # Habilita el motor
    l298n_enable.on()
    # Control del motor
    l298n_input1.on()
    l298n_input2.off()

    # Espera 5s
    time.sleep(5)
    # Deshabilita el motor
    l298n_enable.off()

    # Espera 1s
    time.sleep(1)
    # Habilita el motor
    l298n_enable.on()
    # Control del motor, sentido contrario
    l298n_input1.off()
    l298n_input2.on()

    # Espera 5s
    time.sleep(5)
    l298n_enable.off()

El siguiente paso es controlar la velocidad del motor: para ello se utiliza PWM. La velocidad máxima del motor se logra con el valor 65536; se recomiendan pruebas con diferentes valores para encontrar la velocidad adecuada a cada proyecto.

Download unitRP2040_motors2.py:
-------------------------------


Diagrama para controlar dos motores:

.. figure:: /_static/7-Control_de_motores_DC/images/DosMotores_bb.png
   :width: 70%
   :align: center
   :alt: sections of the code

.. `Download unitRP2040_motors2.py <../docs/7-Control_de_motores_DC/code/unitRP2040_motors2.py>`_

.. code-block:: python
    :linenos:

    from machine import Pin, PWM
    import time

    # Configura los pines para controlar el L298N
    l298n_enable = Pin(7, Pin.OUT)     # Conecta a EN del L298N
    l298n_input1 = Pin(14, Pin.OUT)     # Conecta a IN1 del L298N
    l298n_input2 = Pin(9, Pin.OUT)      # Conecta a IN2 del L298N

    # Habilita el motor
    l298n_enable.on()

    # Prepara el PWM
    pwm1 = PWM(l298n_input1)
    pwm1.freq(1000)

    pwm2 = PWM(l298n_input2)
    pwm2.freq(1000)

    # Define la velocidad del motor (ajusta el valor según sea necesario)
    motor_speed = 65536  # Velocidad máxima

    pwm1.duty_u16(motor_speed)
    pwm2.duty_u16(0)

    time.sleep(5)

    motor_speed = 40000

    pwm1.duty_u16(motor_speed)
    pwm2.duty_u16(0)

    time.sleep(2)

    motor_speed = 65536  # Velocidad máxima

    pwm1.duty_u16(0)
    pwm2.duty_u16(motor_speed)

    time.sleep(5)

    motor_speed = 40000

    pwm1.duty_u16(0)
    pwm2.duty_u16(motor_speed)

    time.sleep(2)

    l298n_enable.off()

Tomando como base los códigos anteriores, se puede controlar dos motores DC utilizando el driver L298N simultáneamente, controlando velocidad, dirección y encendido/apagado.

Download unitRP2040_motors3.py:
-------------------------------

`Download unitRP2040_motors3.py <../docs/7-Control_de_motores_DC/code/unitRP2040_motors3.py>`_

.. code-block:: python
    :linenos:

    from machine import Pin, PWM
    import time

    # Configura los pines para controlar el L298N
    l298n_enableA = Pin(7, Pin.OUT)    # Conecta a ENA del L298N
    l298n_input1 = Pin(14, Pin.OUT)      # Conecta a IN1 del L298N
    l298n_input2 = Pin(9, Pin.OUT)       # Conecta a IN2 del L298N
    l298n_enableB = Pin(4, Pin.OUT)     # Conecta a ENB del L298N
    l298n_input3 = Pin(8, Pin.OUT)       # Conecta a IN3 del L298N
    l298n_input4 = Pin(11, Pin.OUT)      # Conecta a IN4 del L298N

    # Habilita los motores
    l298n_enableA.on()
    l298n_enableB.on()

    # Prepara el PWM
    pwm1 = PWM(l298n_input1)
    pwm1.freq(1000)
    pwm2 = PWM(l298n_input2)
    pwm2.freq(1000)
    pwm3 = PWM(l298n_input3)
    pwm3.freq(1000)
    pwm4 = PWM(l298n_input4)
    pwm4.freq(1000)

    # Define la velocidad del motor (ajusta el valor según sea necesario)
    motor_speed = 65536  # Velocidad máxima

    pwm1.duty_u16(motor_speed)
    pwm2.duty_u16(0)
    pwm3.duty_u16(motor_speed)
    pwm4.duty_u16(0)

    time.sleep(5)

    motor_speed = 40000

    pwm1.duty_u16(motor_speed)
    pwm2.duty_u16(0)
    pwm3.duty_u16(motor_speed)
    pwm4.duty_u16(0)

    time.sleep(2)

    motor_speed = 65536  # Velocidad máxima

    pwm1.duty_u16(0)
    pwm2.duty_u16(motor_speed)
    pwm3.duty_u16(0)
    pwm4.duty_u16(motor_speed)

    time.sleep(5)

    motor_speed = 40000

    pwm1.duty_u16(0)
    pwm2.duty_u16(motor_speed)
    pwm3.duty_u16(0)
    pwm4.duty_u16(motor_speed)

    time.sleep(2)

    l298n_enableA.off()
    l298n_enableB.off()

.. caution::

    Ten en cuenta que este código es un ejemplo y puede que necesites ajustarlo según tu configuración específica y necesidades.

Resultados
----------

.. only:: html

    .. figure:: /_static/7-Control_de_motores_DC/images/carrito.gif
        :align: center
        :alt: figura-gif
        :width: 90%


Conclusiones
------------

Esta actividad ejemplifica de manera destacada los sistemas de control al haber desarrollado exitosamente un sistema para motores DC. Este logro sienta las bases para diversos proyectos futuros, introduciendo conceptos clave de MicroPython, PWM (Modulación de Ancho de Pulso) y un mejor manejo de la tarjeta de desarrollo Dual MCU.

Referencias
------------

Nise, N. (2019). Control Systems Engineering. Editorial Wiley.

