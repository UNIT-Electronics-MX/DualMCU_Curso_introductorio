Comunicación entre microcontrolador ESP32 y RP2040
======================================================

Objetivo
--------

Establecer una comunicación efectiva entre dos microcontroladores de la DualMCU, con el fin de unificar recursos y potenciar el poder de procesamiento en aplicaciones que requieran mayores capacidades.

.. note::
    En esta práctica, se emplearán ambos microcontroladores para finalizar básicos.

Descripción
-----------

Esta práctica proporciona una solución para lograr una comunicación eficiente entre dos microcontroladores, específicamente el ESP32 y el RP2040. La implementación está diseñada para optimizar el rendimiento en aplicaciones que demandan mayores recursos computacionales.

Requisitos
----------

- `Placa UNIT DualMCU <https://uelectronics.com/producto/unit-dualmcu-esp32-rp2040-tarjeta-de-desarrollo/>`_
- `Cable USB Tipo C <https://uelectronics.com/producto/cable-usb-tipo-c-3a-6a/>`_

Diagrama de Conexión
--------------------

A continuación, se muestra el diagrama de conexión, el cual es muy sencillo: solo necesitas conectar la UNIT DUALMCU a tu laptop o computadora de escritorio mediante un cable USB Tipo C.

.. figure:: /_static/3-Led_intermitente/images/pc_dual.jpg
   :alt: pc

Cambia el Interruptor DIP UART a "ON" para esta configuración.


.. figure:: /_static/12-Comunicacion_esp32_rp2040/images/SEL.png
   :alt: Block Diagram
   :align: center

Para esta práctica, necesitarás cambiar entre microcontroladores. Se te recuerda que a través del selector puedes intercambiar entre microcontroladores. Después de conectar la UNIT DUALMCU al ordenador, procede a encender el dispositivo y seleccionar el microcontrolador (MCU) deseado.

.. note::
    En esta parte se utilizará el microcontrolador RP2040 por lo que debes cambiar el interruptor a la posición “A”.


.. figure:: /_static/2-Micropython/images/selector.png
   :alt: Block Diagram
   :align: center
   :width: 600px

Código
------

El proceso se lleva a cabo en dos partes. La primera etapa implica la carga del código en el RP2040, lo cual debe realizarse de la siguiente manera: selecciona la placa en el COM en la parte inferior derecha.


.. figure:: /_static/12-Comunicacion_esp32_rp2040/images/RP2040_COM.png
   :alt: Block Diagram
   :align: center
   :width: 600px

Copia el siguiente código:

.. raw:: html

    <div style="text-align: right;">
        <a href="/docs/12-Comunicacion_esp32_rp2040/codes/unitR2040_send.py" download="unitR2040_send.py">
            <button style="background-color: #4CAF50; color: white; padding: 10px 20px; border: none; border-radius: 4px; cursor: pointer;">
                Download unitR2040_send.py
            </button>
        </a>
    </div>

.. code-block:: python

    '''
    rp2040
    '''

    import time
    from machine import UART, Pin
    import ujson

    uart1 = UART(0, baudrate=115000, tx=Pin(0, Pin.OUT), rx=Pin(1, Pin.IN))

    led_sequence = ["rojo", "verde", "azul"]  # Lista que define la secuencia de LEDs

    while True:
        time.sleep(0.1)

        # Obtén el siguiente LED en la secuencia
        led_actual = led_sequence.pop(0)
        
        # Añade el estado del LED actual al JSON
        datos = {
            "led_actual": led_actual,
            "accion": "encender"
        }
        txData = ujson.dumps(datos)
        uart1.write(txData + '\\n\\r')
        print(txData)

        time.sleep(1)  # Espera 1 segundo antes de enviar el siguiente conjunto de datos

        # Añade el estado del LED actual al JSON
        datos = {
            "led_actual": led_actual,
            "accion": "apagar"
        }
        txData = ujson.dumps(datos)
        uart1.write(txData + '\\n\\r')
        print(txData)

        # Mueve el LED actual al final de la secuencia
        led_sequence.append(led_actual)

        time.sleep(1)  # Espera 1 segundo antes de enviar el siguiente conjunto de datos

Guarda el código en el RP2040, seleccionando **Raspberry Pi Pico**.


.. figure:: /_static/12-Comunicacion_esp32_rp2040/images/SELECT_SAVE.png
   :alt: Block Diagram
   :align: center
   :width: 200px

Te aparecerá una ventana en la que deberás escribir el nombre **main.py** y finalmente presionar **ok**.


.. figure:: /_static/12-Comunicacion_esp32_rp2040/images/SAVE_MAIN.png
   :alt: Block Diagram
   :align: center
   :width: 500px

.. note::
    En esta parte se utilizará el microcontrolador ESP32 por lo que debes cambiar el interruptor a la posición “B”.


.. figure:: /_static/2-Micropython/images/selector.png
   :alt: Block Diagram
   :align: center
   :width: 600px

Copia el siguiente código:

.. raw:: html

    <div style="text-align: right;">
        <a href="/docs/12-Comunicacion_esp32_rp2040/codes/unitESP32.py" download="unitESP32.py">
            <button style="background-color: #4CAF50; color: white; padding: 10px 20px; border: none; border-radius: 4px; cursor: pointer;">
                Download unitESP32.py
            </button>
        </a>
    </div>

.. code-block:: python

    '''
    ESP32
    '''
    import ujson
    from machine import UART, Pin

    uart0 = UART(1, baudrate=115000, tx=Pin(17, Pin.OUT), rx=Pin(16, Pin.IN))
    led_rojo = Pin(4, Pin.OUT)  # Configura el pin GPIO5 como salida para el LED rojo
    led_verde = Pin(26, Pin.OUT)  # Configura el pin GPIO18 como salida para el LED verde
    led_azul = Pin(25, Pin.OUT)  # Configura el pin GPIO19 como salida para el LED azul

    def ejecutar_accion(accion, pin_led):
        if accion == "encender":
            pin_led.on()  # Enciende el LED
        elif accion == "apagar":
            pin_led.off()  # Apaga el LED

    def recibir_json():
        rx_data = b''  # Inicializa una cadena de bytes vacía

        while True:
            if uart0.any():
                byte_received = uart0.read(1)  # Lee un byte desde el UART
                rx_data += byte_received

                # Verifica si el carácter de nueva línea indica el final del JSON
                if byte_received == b'\\n':
                    try:
                        # Intenta cargar el JSON
                        json_data = ujson.loads(rx_data.decode('utf-8'))
                        print("JSON recibido:", json_data)
                        
                        # Extrae los valores de 'accion' y 'led_actual' del JSON
                        accion = json_data.get('accion', '')
                        led_actual = json_data.get('led_actual', '')

                        # Ejecuta la acción indicada en el JSON para cada LED
                        if led_actual == "rojo":
                            ejecutar_accion(accion, led_rojo)
                        elif led_actual == "verde":
                            ejecutar_accion(accion, led_verde)
                        elif led_actual == "azul":
                            ejecutar_accion(accion, led_azul)
                        print("--led recibido:", led_actual, "accion:", accion)
                        
                        return json_data
                    except ValueError as e:
                        print("Error al parsear JSON:", e)
                        rx_data = b''  # Reinicia la cadena si hay un error en el JSON

    # Ejemplo de uso
    while True:
        data = recibir_json()
        # Realiza acciones con el JSON recibido

Corre el código del ESP32 debe aparecer el los datos enviados por el RP2040.


.. figure:: /_static/12-Comunicacion_esp32_rp2040/images/shell1.png
   :alt: Block Diagram
   :align: center

Resultados
----------

Con unos breves resultados, el control de comunicación por JSON es una práctica que beneficia la comunicación en el aspecto de que los microcontroladores permiten su uso sin componentes de software externos, por lo que su implementación es práctica. Los resultados de esta comunicación permiten conocer las posibilidades de la DUALMCU.


.. only:: html

    .. figure:: /_static/12-Comunicacion_esp32_rp2040/images/dual.gif
        :align: center
        :alt: figura-gif
        :width: 60%

Conclusiones
------------

En conclusión, el objetivo de la práctica es lograr una comunicación efectiva entre dos microcontroladores de la DualMCU, el ESP32 y el RP2040, con el propósito de consolidar recursos y potenciar el poder de procesamiento. La implementación busca ofrecer una solución que optimice el rendimiento en aplicaciones que requieren mayores capacidades computacionales, brindando así una solución eficiente para proyectos que demandan un mayor nivel de procesamiento y coordinación entre microcontroladores.
