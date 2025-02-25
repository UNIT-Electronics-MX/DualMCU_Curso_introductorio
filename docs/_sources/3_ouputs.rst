
Led intermitente (OUTPUT)
==========================

Objetivo
^^^^^^^^

El objetivo principal de esta sección es desarrollar las habilidades necesarias para programar un efecto de parpadeo intermitente, comúnmente conocido como "Blink", a una frecuencia específica.

.. note::
    En esta práctica, se emplearán ambos microcontroladores para fortalecer los fundamentos básicos.

Descripción
^^^^^^^^^^^

La realización de un programa sencillo del parpadeo de un LED se respalda con diversos propósitos beneficiosos: la verificación del funcionamiento inicial de la DUALMCU, el entendimiento de la estructura del programa para cada microcontrolador, la familiarización con el entorno de programación y el hardware asociado a la DUALMCU. Esta práctica inicial sienta las bases para abordar con éxito las siguientes actividades.

Se utilizarán los dos LEDs RGB incorporados en la placa de desarrollo UNIT DUALMCU. La configuración y programación de ambos microcontroladores se realizará de manera coordinada para lograr el efecto deseado. Esta práctica provee una introducción práctica al manejo de LEDs y a la programación, estableciendo además una base sólida para futuras actividades en este entorno de desarrollo.

Requisitos
^^^^^^^^^^

Los componentes electrónicos ya están integrados en la placa. Se utilizarán dos LEDs RGB. Los materiales específicos son:

- `Placa UNIT DualMCU <https://uelectronics.com/producto/unit-dualmcu-esp32-rp2040-tarjeta-de-desarrollo/>`_
- `Cable USB Tipo C <https://uelectronics.com/producto/cable-usb-tipo-c-3a-6a/>`_

Diagrama de Conexión
^^^^^^^^^^^^^^^^^^^^^

Conecta la UNIT DUALMCU a tu laptop o PC mediante un cable USB Tipo C.

.. figure:: /_static/3-Led_intermitente/images/pc_dual.jpg
    :alt: pc

Software
^^^^^^^^

Después de conectar la UNIT DUALMCU, enciéndela y selecciona el microcontrolador (MCU) deseado.

.. .. figure:: /_static/2-Micropython/images/esp32_or_rasp.jpg
..     :alt: Block Diagram
..     :width: 300px


Código
^^^^^^

Usa el siguiente código para comprobar la instalación del firmware de MicroPython en el ESP32. Asegúrate de que esté seleccionado el MCU ESP32.

Download: `Download blink.py <https://github.com/UNIT-Electronics-MX/DualMCU_Curso_introductorio/releases/download/v0.0.1/blink.py>`_

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
    '''
    import machine
    import time

    led_pin = machine.Pin(4, machine.Pin.OUT)
    led_pin2 = machine.Pin(26, machine.Pin.OUT)
    led_pin3 = machine.Pin(25, machine.Pin.OUT)


    def loop():
         while True:
              led_pin.on()
              led_pin2.on()
              led_pin3.on()
              time.sleep(1)
              led_pin.off()
              led_pin2.off()
              led_pin3.off()
              time.sleep(1)

    loop()

El código prueba la instalación de MicroPython en el ESP32: enciende tres LEDs y luego los apaga en intervalos de 1 segundo. Puedes ajustar el parpadeo modificando el valor en time.sleep, por ejemplo, a 0.5 para un parpadeo cada 0.5 segundos.

Resultados
^^^^^^^^^^

Visualiza el funcionamiento en el siguiente GIF:




.. only:: html

    .. figure:: /_static/3-Led_intermitente/images/blink_led2.gif
        :align: center
        :alt: figura-gif
        :width: 60%



Interactúa con el RP2040
^^^^^^^^^^^^^^^^^^^^^^^^^

1. Configura el selector de posición para el RP2040 en la UNIT DUALMCU.
2. Actualiza el puerto serial COM según la configuración de tu sistema operativo.
3. Abre Thonny y copia el siguiente código.
4. Pega y ejecuta el código en Thonny para observar el comportamiento del LED del RP2040.

.. code-block:: python
    :linenos:

    '''
    Unit Electronics 2023
             (o_
    (o_    //\
    (/)_   V_/_
    '''
    import machine
    import utime

    led = machine.Pin(25, machine.Pin.OUT)  # Configura el pin GPIO25 como salida

    while True:
         led.value(not led.value())  # Invierte el estado del LED (encendido/apagado)
         utime.sleep(1)  # Espera 1 segundo

Conclusiones
^^^^^^^^^^^^

Esta práctica del Blink en la UNIT DUALMCU no solo es una introducción para programar los MCUs RP2040 y ESP32, sino también para el manejo de la placa con MicroPython. Sienta las bases para explorar y expandir conocimientos en futuras prácticas y proyectos con la placa DUALMCU.

.. note::
    Ten en cuenta que este código es un ejemplo y puede requerir ajustes según tu configuración y necesidades.


