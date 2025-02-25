
Control Servo (PWM)
=======================


Objetivo
........
Utilizar la tarjeta DualMCU con el RP2040 para controlar un servo motor, logrando movimientos precisos a ángulos específicos. Esto incluye la capacidad de dirigir el servo en un rango definido o seguir una secuencia predefinida de movimientos.

.. note::
    En esta práctica, se utilizará el **RP2040**.

Descripción
.............
Esta sección proporciona recursos y código para el control de servomotores mediante MicroPython. Los servomotores, comunes en robótica y automatización, permiten gestionar la posición angular de un eje. Con MicroPython se dispone de una interfaz sencilla y eficiente para controlar los servomotores.

Requisitos
...........
- 1x `Placa UNIT DualMCU <https://uelectronics.com/producto/unit-dualmcu-esp32-rp2040-tarjeta-de-desarrollo/>`_
- 1x `Servomotor compatible <https://uelectronics.com/producto/servomotor-sg90-rc-9g/>`_
- 1x `Cables Dupont: Hembra - Macho <https://uelectronics.com/producto/cables-dupont-largos-20cm-hh-mh-mm/>`_

Diagrama de conexión
......................
Conecta la UNIT DUALMCU al servomotor según el siguiente diagrama:

.. figure:: /_static/5-Control_Servo/images/Diagrama.jpg
    :alt: Block Diagram
    :align: center
    :width: 600px

.. note::
    Recuerda que en la DualMCU puedes intercambiar entre microcontroladores mediante un interruptor. Para esta práctica, usa el microcontrolador RP2040 cambiando el interruptor a la posición “A”.

.. figure:: /_static/2-Micropython/images/selector.png
    :alt: Selector DualMCU
    :align: center
    :width: 600px

Código Fuente: PWM (unitRP2040_pwm.py)
---------------------------------------
El siguiente código configura una salida para el servomotor en el GPIO 0:

.. raw:: html

     <div style="text-align: right;">
          <a href="/_static/5-Control_Servo/code/unitRP2040_pwm.py" download="unitRP2040_pwm.py">
                <button style="background-color: #4CAF50; color: white; padding: 10px 20px; border: none; border-radius: 4px; cursor: pointer;">
                     Download unitRP2040_pwm.py
                </button>
          </a>
     </div>

.. code-block:: python
     :linenos:

     '''
     Unit Electronics 2023
                  (o_
         (o_    //\
         (/)_   V_/_ 
     tested code mark
         version: 0.0.1
         revision: 0.0.1

     Código de prueba
     '''
     import machine
     import utime

     # Configuración del pin PWM
     pwm_pin = machine.Pin(0)  # Cambia a machine.Pin(1) si usas GPIO 1
     pwm = machine.PWM(pwm_pin)

     # Frecuencia del PWM en Hz (ajusta según tus necesidades)
     pwm.freq(1000)

     try:
          while True:
                # Ciclo de trabajo del PWM (0-65535: 0 apagado, 65535 encendido)
                for duty_cycle in range(0, 65536, 5000):
                     pwm.duty_u16(duty_cycle)
                     utime.sleep(0.1)

                # Efecto de atenuación inversa
                for duty_cycle in range(65535, -1, -5000):
                     pwm.duty_u16(duty_cycle)
                     utime.sleep(0.1)

     except KeyboardInterrupt:
          pwm.deinit()
          print("\nPWM detenido. Recursos liberados.")


.. only:: html

    .. figure::  /_static/5-Control_Servo/images/pwm_osc.gif
        :align: center
        :alt: figura-gif
        :width: 60%

Código Fuente: Servo (unitRP2040_servo.py)
-------------------------------------------
El siguiente ejemplo controla un servomotor utilizando PWM a 50 Hz:

.. raw:: html

     <div style="text-align: right;">
          <a href="/_static/5-Control_Servo/code/unitRP2040_servo.py" download="unitRP2040_servo.py">
                <button style="background-color: #4CAF50; color: white; padding: 10px 20px; border: none; border-radius: 4px; cursor: pointer;">
                     Download unitRP2040_servo.py
                </button>
          </a>
     </div>

.. code-block:: python
     :linenos:

     import machine
     import utime

     # Configuración del pin de control del servomotor
     servo_pin = machine.Pin(0)  # Cambia según tu conexión

     # Crea un objeto PWM para el servomotor
     pwm_servo = machine.PWM(servo_pin)
     pwm_servo.freq(50)  # Frecuencia para servomotores (~50 Hz)

     def set_servo_angle(angle):
          # Convierte el ángulo (0-180°) a un ciclo de trabajo
          duty_cycle = int(1024 + (angle / 180) * 3072)
          pwm_servo.duty_u16(duty_cycle)

     try:
          while True:
                # Mueve el servomotor de 0 a 180°
                for angle in range(0, 181, 10):
                     set_servo_angle(angle)
                     utime.sleep(0.1)

                # Mueve el servomotor de 180 a 0°
                for angle in range(180, -1, -10):
                     set_servo_angle(angle)
                     utime.sleep(0.1)

     except KeyboardInterrupt:
          pwm_servo.deinit()
          print("\nPWM detenido. Recursos liberados.")





.. only:: html

    .. figure:: /_static/5-Control_Servo/images/pwm_servo.gif
        :align: center
        :alt: figura-gif
        :width: 60%


Resultados
----------
El código demuestra la capacidad del RP2040 para controlar un servomotor mediante PWM, utilizando el pin GPIO 0 (ajustable) a 50 Hz, la frecuencia recomendada para servos.

Conclusiones
------------
La práctica con la tarjeta DualMCU - RP2040 y un servomotor introduce conceptos clave en el control de hardware a través de microcontroladores. Se cubren la configuración de pines, la generación de PWM y la conversión de ángulos a valores de ciclo de trabajo, proporcionando una base para proyectos avanzados de robótica y automatización.

Para profundizar en el control PWM, se sugiere experimentar con ejemplos del `repositorio de la DualMCU <https://github.com/UNIT-Electronics/DualMCU/tree/main/Examples/Micropython%20Basics/RP2040/02.PWM>`_.

