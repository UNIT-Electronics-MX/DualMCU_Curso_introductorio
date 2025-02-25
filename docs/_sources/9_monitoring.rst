Sistema de monitoreo ambiental
==============================

Objetivo
--------
Utilizar los sensores DHT11 y MQ135 para medir parámetros ambientales tales como Humedad, temperatura y calidad de aire cada minuto y visualizar las lecturas en el Monitor Serial de Thonny.

.. note::
    
    En esta práctica, se utilizará el **ESP32**.

Descripción
-----------
Los sistemas de control ambiental son fundamentales para medir y gestionar parámetros como la temperatura, humedad, calidad del aire y otros factores para crear entornos cómodos y seguros. A continuación te compartimos recursos y código para construir un sistema de control ambiental utilizando los sensores DHT11 y MQ135 así como la configuración ESP32 con MicroPython.

Requisitos
----------
- 1x `Placa UNIT  DualMCU <https://uelectronics.com/producto/unit-dualmcu-esp32-rp2040-tarjeta-de-desarrollo/>`_
- 1x `Detector de Calidad de Aire MQ135 <https://uelectronics.com/producto/modulo-ky-015-sensor-de-temperatura-y-humedad/>`_
- 1x `Detector de Calidad de Aire MQ135 <https://uelectronics.com/producto/mq-135-modulo-detector-de-calidad-de-aire/>`_
- 1x `Cables Dupont : Hembra - Macho <https://uelectronics.com/producto/cables-dupont-largos-20cm-hh-mh-mm/>`_

Diagrama de conexión
--------------------

A continuación se presenta el diagrama de conexión entre los sensores DHT11 y MQ135 y la tarjeta de desarrollo Dual MCU utilizando el microcontrolador ESP32.

.. figure:: /_static/9-Sistema_de_monitoreo/images/AR3578Diagrama.jpg
    :alt: Block Diagram
    :align: center

.. note::
    Para la realización de la práctica la Dual MCU tendrá que encontrarse en la configuración de uso en ESP32, dependiendo del código que quieras utilizar.

.. figure:: /_static/2-Micropython/images/esp32_or_rasp.jpg
    :alt: Block Diagram
    :align: center
    :width: 300px

Código
------

.. raw:: html

    <div style="text-align: right;">
         <a href="/docs/9-Sistema_de_monitoreo/code/unitESP32_dth11.py" download="unitESP32_dth11.py">
              <button style="background-color: #4CAF50; color: white; padding: 10px 20px; border: none; border-radius: 4px; cursor: pointer;">
                    Download unitESP32_dth11.py
              </button>
         </a>
    </div>

.. code-block:: python

    '''
    Unit Electronics 2024
             (o_
    (o_    //\
    (/)_   V_/_ 

    version: 0.0.1
    revision: 0.0.1
    compiler: MicroPython v1.22.0 on 2023-12-27; Generic ESP32 module with ESP32
    '''
    from dht import DHT11
    from machine import Pin, ADC
    from time import sleep

    sensor = DHT11 (Pin(4))

    # configura entrada de adc en el pin 36

    adc = ADC(Pin(36))
    # configura el rango de lectura de 0 a 3.3v
    adc.atten(ADC.ATTN_11DB)
    # configura el rango de lectura de 0 a 4095
    adc.width(ADC.WIDTH_12BIT)

    while True:
      try:
         sleep(2)
         valor = adc.read()
         #imprime el valor del adc
         print(valor)
         sensor.measure()
         temp = sensor.temperature()
         hum = sensor.humidity()
         temp_f = temp * (9/5) + 32.0
         print('Temperature: %3.1f C' %temp)
         print('Temperature: %3.1f F' %temp_f)
         print('Humidity: %3.1f %%' %hum)
      except OSError as e:
         print('Failed to read sensor.')

Resultados
----------
Este código lee la temperatura y la humedad del sensor DHT11 y la calidad del aire del sensor MQ135 cada minuto, e imprime los valores leídos.

Conclusiones
------------

Durante esta práctica, se adquirió conocimiento sobre el funcionamiento de dos sensores, los cuales posibilitan el monitoreo de la temperatura, humedad y calidad del aire. Es imperativo destacar que, además de proporcionar el código y el diagrama de conexiones, se hace necesario llevar a cabo la calibración de los sensores para optimizar su utilidad en una aplicación específica. También es importante señalar la viabilidad de conectar el ESP32 a una red, permitiendo así el envío de las lecturas adquiridas a un servidor o una base de datos.

.. caution::

    Ten en cuenta que este código es un ejemplo y puede que necesites ajustarlo según tu configuración específica y tus necesidades.
