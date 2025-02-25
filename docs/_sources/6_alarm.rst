Sistema de alarma (INPUT)
=============================


Objetivo
--------

Se implementará un sistema capaz de generar una alerta sonora ante la detección de movimiento.

.. note::

    En esta práctica, se utilizará el **ESP32**.


Descripción
-----------------

Los sistemas de alarma son fundamentales para mantener seguro un espacio o propiedad. A continuación, se comparten recursos y código para construir un sistema de alarma personalizado que se adapte a tus necesidades específicas utilizando un dispositivo ESP32 con MicroPython.

Requisitos
----------

- 1x `Placa UNIT DualMCU <https://uelectronics.com/producto/unit-dualmcu-esp32-rp2040-tarjeta-de-desarrollo/>`_
- 1x `Sensores de Movimiento PIR (HC-SR505) <https://uelectronics.com/producto/sensores-de-movimiento-pir-hc-sr501-hc-sr505-hy3612-am312/>`_
- 1x `Buzzer Activo 3V <https://uelectronics.com/producto/buzzer-activo-3v-5v-12v-zumbador/>`_
- 1x `Cables Dupont: Hembra - Macho <https://uelectronics.com/producto/cables-dupont-largos-20cm-hh-mh-mm/>`_

Diagrama de conexión
--------------------

A continuación se presenta el diagrama de conexión entre el sensor de movimiento AM312 y la tarjeta de desarrollo.

.. figure:: /_static/6-Sistema_de_Alarma/images/DIAGRAMA.jpg
    :alt: Diagrama de conexión
    :align: center
    :width: 600px

Adicionalmente, para la realización de la programación de DualMCU debe seleccionarse la configuración de uso en ESP32:

.. container:: center

    .. figure:: /_static/2-Micropython/images/esp32_or_rasp.jpg
        :alt: Block Diagram
        :title: Block Diagram
        :width: 300px

Código
------

A continuación se presenta el programa para manejar el sensor de movimiento AM312 y activar una alarma sonora mediante un buzzer activo. El código se puede utilizar como punto de partida para crear un sistema de alarma personalizado.

.. raw:: html

    <div style="text-align: right;">
         <a href="/_static/6-Sistema_de_Alarma/code/unitRP2040_pir.py" download="unitESP32_pir.py">
              <button style="background-color: #4CAF50; color: white; padding: 10px 20px; border: none; border-radius: 4px; cursor: pointer;">
                    Download unitESP32_pir.py
              </button>
         </a>
    </div>

.. code-block:: python
    :linenos:
    


    from machine import Pin
    import time

    # Configura el pin del sensor PIR y el buzzer
    pir_pin = Pin(16, Pin.IN)  # Reemplaza el número de pin según tu conexión
    buzzer_pin = Pin(15, Pin.OUT)  # Reemplaza el número de pin según tu conexión

    # Función para activar la alarma
    def activate_alarm():
        print("¡Movimiento detectado! Activando alarma...")
        buzzer_pin.on()
        time.sleep(5)  # La alarma suena durante 5 segundos
        buzzer_pin.off()

    print("Sistema de alarma PIR activado")

    while True:
        if pir_pin.value() == 1:  # El sensor PIR detecta movimiento
            activate_alarm()
        time.sleep(0.5)  # Espera 0.5 segundos antes de volver a verificar el sensor PIR

Resultados
----------

Al ejecutar el script se mostrará primero un mensaje indicando que el sistema está listo para funcionar. Posteriormente, se visualizará un mensaje de detección cuando el buzzer activo emita la alerta de movimiento.

.. figure:: /_static/6-Sistema_de_Alarma/images/cap.png
    :alt: Resultados
    :title: Resultados
    :width: 600px
    :align: center

Conclusiones
------------

Con este sistema se identifica la ubicación de las terminales de I/O de la tarjeta de desarrollo DualMCU en su configuración con ESP32. El sistema detecta una señal de entrada mediante el sensor PIR y, a partir de ella, activa un buzzer activo.

.. note::

    Este código es un ejemplo y puede requerir ajustes según la configuración específica y tus necesidades.

