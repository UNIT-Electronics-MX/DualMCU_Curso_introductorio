
Sensor de temperatura
=====================

Objetivo
--------

Obtener las lecturas de la temperatura actual mediante un sensor analógico LM35 y visualizar los resultados a través del monitor en serie del IDE de Thonny.

.. note::
    En esta práctica, se utilizará el **RP2040**.

Descripción
-----------

En esta práctica, nos enfocaremos en la utilización del sensor analógico LM35 para la medición de la temperatura actual. El sensor LM35 ha sido seleccionado por su versatilidad y facilidad de integración con diversas placas de desarrollo. Es ideal para proyectos educativos y de electrónica al ser compatible con Arduino, Raspberry Pi, Nodemcu, ESP32, entre otros dispositivos con pines analógicos.

Este enfoque permite familiarizarse con la lectura analógica y la visualización de resultados en el monitor serie del IDE de Thonny. El código de ejemplo funciona tanto en ESP32 como en RP2040 bajo MicroPython, haciendo la práctica versátil y adaptable a distintas plataformas.

Descripción del ADC
--------------------

La clase ADC sirve como interfaz para convertir una señal analógica a digital. Se encuentra en el módulo machine y se puede importar de la siguiente forma:

.. code-block:: python
    :linenos:

    from machine import ADC

    adc = ADC(pin)        # crea un objeto ADC para un pin
    val = adc.read_u16()  # lectura de 0 a 65535
    val = adc.read_uv()   # lectura en microvoltios


Para más información, consulta la `Documentación oficial de MicroPython <https://docs.micropython.org/en/latest/library/machine.ADC.html>`_.

Puedes ver un ejemplo en el `repositorio de la DualMCU <https://github.com/UNIT-Electronics/DualMCU/blob/main/Examples/Micropython%20Basics/RP2040/01.ADC/ADC.py>`_.

Materiales
----------

- `1x  Placa UNIT DualMCU <https://uelectronics.com/producto/unit-dualmcu-esp32-rp2040-tarjeta-de-desarrollo/>`_
- `1x  Sensor de temperatura LM35 <https://uelectronics.com/producto/lm35-sensor-de-temperatura/>`_
- `1x  Cables Dupont: Hembra - Macho <https://uelectronics.com/producto/cables-dupont-largos-20cm-hh-mh-mm/>`_

Para el ESP32, adicionalmente necesitarás:

- `2x LEDs <https://uelectronics.com/producto/led-5mm-difuso-rojo-amarillo-verde-azul-blanco/>`_
- `2x Resistencias 220 ohms <https://uelectronics.com/producto/resistencia-1-4w-presicion/>`_

Diagrama de conexión
--------------------

.. note::
    Recuerda que la DualMCU permite intercambiar entre microcontroladores mediante un interruptor.

.. figure:: /_static/2-Micropython/images/selector.png
    :alt: Block Diagram
    :align: center
    :width: 600px

A continuación, se muestra el diagrama de conexión para el sensor LM35 con el microcontrolador RP2040:

.. figure:: /_static/4-Sensor_de_temperatura/images/AR3578_Diagrama_RP2.jpg
    :alt: Diagrama RP2040

Para el ESP32, utiliza el siguiente diagrama:

.. figure:: /_static/4-Sensor_de_temperatura/images/AR3578_Diagrama_ESP2.jpg
    :alt: Diagrama ESP32

Código
------

Estos ejemplos muestran cómo usar el LM35 en microcontroladores RP2040 y ESP32.

.. raw:: html

    <div style="text-align: right;">
         <a href="/_static/4-Sensor_de_temperatura/code/unitESP32_adc.py" download="unitRP2040_adc.py">
              <button style="background-color: #4CAF50; color: white; padding: 10px 20px; border: none; border-radius: 4px; cursor: pointer;">
                    Download unitRP2040_adc.py
              </button>
         </a>
    </div>

.. code-block:: python
    :linenos:

    '''
    Unit Electronics 2024
              (o_
     (o_    //\
     (/)_   V_/_
     tested code mark
     version: 0.0.1
     revision: 0.0.1
     RP2040 
    '''
    import machine
    import time

    # Configura el pin para leer el sensor LM35
    pin_lm35 = machine.Pin(28, machine.Pin.IN)
    adc = machine.ADC(pin_lm35)

    while True:
        # Lee el valor en milivoltios
        lm35_output_mv = adc.read_u16() * 3.3 / 65535 * 1000
        # Convierte a grados Celsius
        temperatura_celsius = (lm35_output_mv - 500) / 10
        print("Temperatura: {:.2f} °C".format(temperatura_celsius))
        time.sleep(1)

.. raw:: html

    <div style="text-align: right;">
         <a href="/_static/4-Sensor_de_temperatura/code/unitESP32_adc.py" download="unitESP32_adc.py">
              <button style="background-color: #4CAF50; color: white; padding: 10px 20px; border: none; border-radius: 4px; cursor: pointer;">
                    Download unitESP32_adc.py
              </button>
         </a>
    </div>

.. code-block:: python
    :linenos:

    '''
    Unit Electronics 2024
             (o_
    (o_    //\
    (/)_   V_/_

    version: 0.0.1
    revision: 0.0.1
    ESP32
    '''
    import machine
    import time

    # Configura el ADC en el pin 36
    adc = machine.ADC(machine.Pin(36))
    adc.atten(machine.ADC.ATTN_11DB)      # Rango 0 a 3.3v
    adc.width(machine.ADC.WIDTH_12BIT)     # Rango 0 a 4095

    # Configura LEDs en los pines 2 y 25
    led = machine.Pin(2, machine.Pin.OUT)
    led2 = machine.Pin(25, machine.Pin.OUT)

    while True:
        valor = adc.read()
        print(valor)
        if valor > 2000:
             led.value(1)
             led2.value(0)
        else:
             led.value(0)
             led2.value(1)
        time.sleep(1)

Resultados
----------

En el monitor serie verás el valor de la temperatura en grados centígrados.

.. figure:: /_static/4-Sensor_de_temperatura/images/sensor.png
    :alt: Sensor Reading

Conclusiones
------------

Esta práctica permite comprender la lectura de sensores analógicos y la configuración del ADC en microcontroladores. Además, demuestra cómo interactuar con dispositivos externos, como LEDs, para visualizar datos en tiempo real.

.. note::
    El ejemplo de código puede requerir ajustes según tu configuración y necesidades.

