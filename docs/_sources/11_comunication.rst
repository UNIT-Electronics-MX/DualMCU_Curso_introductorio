Comunicación inalámbrica
============================

Objetivo
--------

Esta práctica proporciona recursos y ejemplos para la implementación de un sistema de servidor y cliente en una red local utilizando la tarjeta de desarrollo DualMCU aprovechando las capacidades del ESP32.

.. note::
    En esta práctica, se utilizará el **ESP32**.

Descripción
-----------

La DualMCU no solo se distingue por su capacidad de dos microcontroladores, sino que también aprovecha la funcionalidad del ESP32 como servidor y cliente al conectarse a una red local. Este proyecto requiere una sólida base en electrónica y programación de diversas áreas, ya que, a pesar de su aparente simplicidad, exige experiencia en ambas disciplinas.

Requisitos
----------

- 1x `Placa UNIT  DualMCU <https://uelectronics.com/producto/unit-dualmcu-esp32-rp2040-tarjeta-de-desarrollo/>`_
- 1x `Potenciómetro de 10K ohm <https://uelectronics.com/producto/potenciometro-3-pines-15mm-wh148/>`_
- 1x `Cables Dupont : Hembra - Macho <https://uelectronics.com/producto/cables-dupont-largos-20cm-hh-mh-mm/>`_

Diagrama de conexión
--------------------

.. figure:: /_static/11-Comunicacion_inalambrica/images/diagrama.jpg
    :alt: Block Diagram
    :align: center

Adicional, para la realización de la programación nuestra Dual MCU tendrá que encontrarse en la configuración de uso en ESP32.

.. figure:: /_static/2-Micropython/images/esp32_or_rasp.jpg
    :alt: Block Diagram
    :align: center
    :width: 300px

Software
--------

Para la ejecución de esta práctica la dividiremos en las siguientes etapas de configuración que tendrán que seguirse en ese orden.

Configuración de un servidor Local
----------------------------------

La configuración del entorno es un paso esencial que abarca la instalación de los componentes necesarios para desplegar el proyecto como recurso principal. Se requiere un servidor local para poder desplegar un servicio web en tu red local.

Instalación de Node.js
~~~~~~~~~~~~~~~~~~~~~~

Como requisito inicial, necesitarás un servidor web, por lo que este proyecto se despliega en Node.js. Puedes descargar Node.js desde la siguiente página:

`Descargar Node.js <https://nodejs.org/en/download/>`_

- Una vez completada la descarga, ejecuta el programa y selecciona "Instalar".
- Aparecerá una ventana de bienvenida. Haz clic en "Siguiente".
- Luego, se mostrarán los términos y condiciones en pantalla (se recomienda leerlos). Haz clic en "Siguiente".
- Se te recomienda dejar la configuración de localización por defecto.
- Finalmente, haz clic en "Instalar". Cuando termine la instalación, selecciona "Cerrar" o "Finalizar".

Verifica tu Instalación
~~~~~~~~~~~~~~~~~~~~~~~

Para verificar que Node.js y NPM (Node Package Manager) se han instalado correctamente, abre el Command Prompt o PowerShell y escribe los siguientes comandos, luego presiona Enter:

.. code-block:: shell

    node -v

.. figure:: /_static/11-Comunicacion_inalambrica/images/node_version.png
    :alt: Block Diagram
    :align: center

Deberías ver la versión de Node.js instalada. Luego, verifica la versión de NPM con este comando:

.. code-block:: shell

    npm -v

.. figure:: /_static/11-Comunicacion_inalambrica/images/npm_versiom.png
    :alt: Block Diagram
    :align: center

Uso Básico
~~~~~~~~~~

Node.js es un framework que interpreta comandos que le envías. Para probar tu instalación, puedes crear un script de prueba siguiendo estos pasos:

- Abre tu editor de preferencia.
- Copia y pega este `código <https://github.com/UNIT-Electronics-MX/DualMCU_Curso_introductorio/releases/download/v0.0.1/app.js>`_:

.. raw:: html

    <div style="text-align: right;">
         <a href="/docs/11-Comunicacion_inalambrica/code/app.js" download="app.js">
              <button style="background-color: #4CAF50; color: white; padding: 10px 20px; border: none; border-radius: 4px; cursor: pointer;">
                    Download app.js
              </button>
         </a>
    </div>

.. code-block:: javascript

    var http = require('http');
    http.createServer(function (req, res) {
    res.writeHead(200, {'Content-Type': 'text/html'});
    res.end('Hello World!');
    }).listen(8080);

- Guarda el archivo como '**app.js**', asegurándote de recordar la ruta de almacenamiento.
- Abre la consola de comandos en la ubicación donde se encuentra el archivo 'app.js' y ejecuta el siguiente comando:

.. code-block:: shell

    node app.js

- Dado que el script se ejecuta en segundo plano, abre un navegador web y entra la siguiente dirección en la barra de navegación:

    http://localhost:8080

- Deberías ver el texto "Hello World!".

.. note::

    En algunos casos, al intentar acceder externamente, es posible que aparezca una ventana emergente que, al dar acceso, permite la conexión a través de Node.js.

.. image:: /_static/11-Comunicacion_inalambrica/images/firewall_promt.png
    :alt: Ventana Emergente del Firewall

Configuración del Host
----------------------

Descarga o clona el repositorio. Encontrarás el archivo de ejemplo en el directorio `Control_web_panel <../Control_web_panel/>`_. Como se mencionó en la configuración del entorno, debes ejecutar el archivo app.js de la siguiente manera:

Abre el Command Prompt o PowerShell y escribe los siguientes comandos, luego presiona Enter:

.. code-block:: shell

    node app.js

El código te mostrará un mensaje breve como el siguiente:

.. code-block:: shell

    Servidor en funcionamiento en 0.0.0.0:3000

Esto significa que el servicio está activo, y la dirección a la que debes dirigirte para visualizar el proyecto es:

    http://localhost:3000/

.. figure:: /_static/11-Comunicacion_inalambrica/images/web_localhost.png
    :alt: Image

Configuración del cliente
-------------------------

La ESP32 debe contar con el firmware de MicroPython.

En el directorio de `esp32micropython <https://github.com/UNIT-Electronics/DualMCU_ESP32_Panel_de_control_Web/blob/main/Control_web_panel/esp32micropython/>`_ encontrarás un archivo:

`esp32_comunication_between_server_client.py <https://github.com/UNIT-Electronics/DualMCU_ESP32_Panel_de_control_Web/blob/main/Control_web_panel/esp32micropython/esp32_comunication_between_server_client.py>`_

Debes realizar algunos ajustes en el código, en particular, en los datos de tu red Wi-Fi:

.. code-block:: python

    ssid = "SSID"  # Reemplaza con el nombre de tu red Wi-Fi
    password = "PASSWORD"  # Reemplaza con la contraseña de tu red Wi-Fi

También debes cambiar el host en la siguiente línea:

.. code-block:: python

    server_url = "http://tu_host:3000/endpoint" # Reemplaza con el nombre de la ip de tu servidor

Para conocer la dirección IP de tu dispositivo, en Windows, puedes abrir una terminal y ejecutar el comando:

    ipconfig

En la sección de "Adaptadores de red inalámbricos", encontrarás una entrada similar a:

.. code-block:: python

    Dirección IPv4. . . . . . . . . . . . . . : 192.168.0.2

Reemplaza tu_host por la dirección IP, por ejemplo:

.. code-block:: python

    server_url = "http://192.168.0.2:3000/endpoint"

Código
------

.. raw:: html

    <div style="text-align: right;">
         <a href="https://github.com/UNIT-Electronics-MX/DualMCU_Curso_introductorio/releases/download/v0.0.1/unitESP32_wireless.py" download="unitESP32_wireless.py">
              <button style="background-color: #4CAF50; color: white; padding: 10px 20px; border: none; border-radius: 4px; cursor: pointer;">
                    Download unitESP32_wireless.py
              </button>
         </a>
    </div>

.. code-block:: python

    import network
    import ubinascii
    import machine
    import urequests
    import time
    import _thread

    try:
      import usocket as socket
    except:
      import socket

    ssid = "SSID"  # Reemplaza con el nombre de tu red Wi-Fi
    password = "PASSWORD"  # Reemplaza con la contraseña de tu red Wi-Fi

    server_url = "http://tu_host:3000/endpoint" # Reemplaza con el nombre de la ip de tu servidor
    headers = {"Content-Type": "application/json"}

    led = machine.Pin(25, machine.Pin.OUT)
    #led_pin2 = machine.Pin(26, machine.Pin.OUT)
    shared_variable = 0

    # Convierte la dirección MAC del ESP32 en un nombre de host único
    def generate_unique_hostname():
         mac = ubinascii.hexlify(network.WLAN().config('mac'), ':').decode()
         return "esp32-" + mac

    # Conecta a la red Wi-Fi
    def connect_to_wifi():
         wlan = network.WLAN(network.STA_IF)
         if not wlan.isconnected():
              print("Conectando a la red WiFi...")
              wlan.active(True)
              wlan.connect(ssid, password)
              while not wlan.isconnected():
                    pass
         print("Conectado a la red WiFi")
         print("Dirección IP:", wlan.ifconfig()[0])

    def adc_potenciometer():

         potentiometer_pin = machine.Pin(36)
         adc = machine.ADC(potentiometer_pin)
         adc.atten(machine.ADC.ATTN_11DB)
         return adc

    def web_page(adc1):
         led_state = 0
         html = """<html>

         <head>
              <meta name="viewport" content="width=device-width, initial-scale=1">

              <style>
                    html {
                         font-family: Arial;
                         display: inline-block;
                         margin: 0px auto;
                         text-align: center;
                    }

                    .button {
                         background-color: #F146C2;
                         border: none;
                         color: white;
                         padding: 16px 40px;
                         text-align: center;
                         text-decoration: none;
                         display: inline-block;
                         font-size: 16px;
                         margin: 4px 2px;
                         cursor: pointer;
                    }

                    .button1 {
                         background-color: #304169;
                    }
              </style>
         </head>

         <body>
              <h2>Soy el ESP32</h2>
              <p>
                    <a href=\"?led_2_on\"><button class="button">LED ON</button></a>
              </p>
              <p>
                    <a href=\"?led_2_off\"><button class="button button1">LED OFF</button></a>
              </p>
         </body>

         </html>"""
         return html

    def loop1():
         global shared_variable
         while True:
              adc1=adc.read()/4096*100
              data = {"potentiometer_value": str(adc1)}
              response = urequests.post(server_url, json=data, headers=headers)
              response.close()
              time.sleep(0.1)

    def loop2():
         global shared_variable
         s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
         s.bind(('0.0.0.0', 80))
         s.listen(5)

         while True:
            try:
              if gc.mem_free() < 102000:
                 gc.collect()
              conn, addr = s.accept()
              conn.settimeout(3.0)
              print('Got a connection from %s' % str(addr))
              request = conn.recv(1024)
              conn.settimeout(None)
              request = str(request)
              led_on = request.find('/?led_2_on')
              led_off = request.find('/?led_2_off')
              if led_on == 6:
                    led_state = "ON"
                    led.on()
              if led_off == 6:
                    led_state = "OFF"
                    led.off()
              response = web_page(shared_variable)
              conn.send('HTTP/1.1 200 OK\n')
              conn.send('Content-Type: text/html\n')
              conn.send('Connection: close\n\n')
              conn.sendall(response)
              conn.close()
            except OSError as e:
              conn.close()
              print('Connection closed')

    connect_to_wifi()
    adc = adc_potenciometer()

    # Crear y lanzar los hilos
    _thread.start_new_thread(loop1, ())
    _thread.start_new_thread(loop2, ())

    time.sleep(10)

Ejecutando el Programa
----------------------

Una vez que hayas realizado las modificaciones en el código, puedes ejecutarlo. En la consola de Thonny, verás una dirección IP a la que puedes acceder para verificar si el ESP32 está conectado:

.. code-block:: yaml

    MPY: soft reboot
    Conectado a la red WiFi
    Dirección IP: 192.168.0.10
    Puedes acceder a esta dirección IP desde cualquier dispositivo en la misma red.

.. figure:: /_static/11-Comunicacion_inalambrica/images/SOY_EL_esp32.png
    :alt: ESP32

La interfaz que se muestra controla el LED 25 de la ESP32 y permite comprobar la funcionalidad del proyecto.

Finalmente, el enlace con la interfaz integrada con el envío de información por el potenciómetro se verá algo como esto:


.. only:: html

    .. figure:: /_static/11-Comunicacion_inalambrica/images/output.gif
        :align: center
        :alt: figura-gif
        :width: 60%




Conclusión
----------

La práctica realizada con DualMCU como cliente y servidor demuestra la versatilidad y potencial de este dispositivo en el ámbito de la comunicación inalámbrica. La capacidad para intercambiar datos de manera eficiente entre un cliente y un servidor abre un amplio abanico de posibilidades para aplicaciones IoT y sistemas embebidos. El aprendizaje obtenido al configurar y operar ambos roles permite comprender mejor el funcionamiento de las redes y cómo aprovechar al máximo las capacidades de la tarjeta de desarrollo DualMCU en distintos escenarios.
