Micropython y la DualMCU
============================


Este apartado se presenta como un ejemplo básico de cómo instalar MicroPython en la DualMCU utilizando el microcontrolador ESP32 y el RP2040. Nuestra meta principal es que encuentres este recurso útil, permitiéndote integrar partes de esta implementación directamente en tus proyectos.


A continuación se proporciona un diagrama de conexión extremadamente sencillo: simplemente conecta la UNIT DUALMCU a tu laptop o computadora de escritorio mediante un cable USB Tipo C. Este paso inicial te permitirá explorar y aprovechar las funcionalidades de MicroPython de manera rápida y eficiente. 

.. tip::

  ¡Esperamos que esta guía simplificada te sea de gran utilidad en tus futuros proyectos!

.. figure:: /_static/2-Micropython/images/pc_dual.jpg
   :width: 100%
   :align: center
   :alt: sections of the code



.. note::

  Recuerda que al trabajar con la DualMCU puedes intercambiar entre microcontroladores mediante el interruptor de cambios.


  .. figure:: /_static/2-Micropython/images/selector.png
    :width: 70%
    :align: center
    :alt: sections of the code



Configuración del entorno
-------------------------

Antes de comenzar, se recomienda realizar la siguiente configuración:

Instalación de Thonny
~~~~~~~~~~~~~~~~~~~~~

Para instalar Thonny, visita the `sitio oficial de Thonny <https://thonny.org/>`_. Esto te permitirá descargar el firmware para la DualMCU ESP32.

Dirígete a *"Ejecutar"* -> *"Configurar intérprete"* para completar la configuración.



.. figure:: /_static/2-Micropython/images/find.png
   :width: 50%
   :align: center
   :alt: sections of the code

.. note::

  En caso de que la `UNIT DUALMCU ESP32` no sea reconocida, será necesario instalar el `controlador CH340 <https://www.wch-ic.com/downloads/CH341SER_EXE.html>`_.

Al hacerlo, se abrirá la siguiente ventana:


.. figure:: /_static/2-Micropython/images/config_intepeter.png
   :width: 70%
   :align: center
   :alt: sections of the code

.. _actualizacion-de-firmware-esp32:

Actualización de firmware ESP32
-------------------------------

Inicia la UNIT DualMCU con el microcontrolador ESP32 en **Posición B**, presionando el botón de FLASH y conectando el dispositivo a la PC.


.. figure:: /_static/2-Micropython/images/flash.png
   :width: 50%
   :align: center
   :alt: sections of the code

1. Da clic en **Instalar o Actualizar MicroPython**.
2. Se abrirá una nueva ventana y se recomienda utilizar la siguiente configuración:
  
.. code-block:: bash

    Variant: Espessif ESP32/WROOM
    Version: 1.20.0


.. figure:: /_static/2-Micropython/images/instalador.png
   :width: 70%
   :align: center
   :alt: sections of the code

3. Presiona instalar y espera a que finalice la instalación.

Selecciona la tarjeta con la que deseas trabajar en la parte inferior de Thonny. La opción se verá similar a la siguiente imagen para el ESP32. Ten en cuenta que el puerto COM asignado a la máquina puede variar.


.. figure:: /_static/2-Micropython/images/target.png
   :width: 50%
   :align: center
   :alt: sections of the code


Actualización de firmware RP2040
-------------------------------

Inicia la UNIT DualMCU con el microcontrolador RP2040 en **Posición A**, presionando el botón de `BOOT` y conectando el dispositivo a la PC.


.. figure:: /_static/2-Micropython/images/RP2040-Boot_button.jpg
   :width: 50%
   :align: center
   :alt: sections of the code

1. Da clic en **Instalar o Actualizar MicroPython**.
2. Se abrirá una nueva ventana y se recomienda utilizar la siguiente configuración:

.. code-block:: bash

  Variant: Raspberry Pi Pico/Pico H
  Version: 1.22.2

.. figure:: /_static/2-Micropython/images/config_intepeter_rp.png
   :width: 90%
   :align: center
   :alt: sections of the code

3. Presiona instalar y espera a que finalice la instalación.

Selecciona la tarjeta con la que deseas trabajar en la parte inferior de Thonny. La opción se verá similar a la siguiente imagen para el RP2040. Ten en cuenta que el puerto COM asignado a la máquina puede variar.


.. figure:: /_static/2-Micropython/images/target_rp.png
   :width: 50%
   :align: center
   :alt: sections of the code


Carga tu primer **Hola Mundo**
-------------------------------


Copia el código proporcionado:

.. code-block:: python

  print("Hola, mundo!")

Ejecuta el código. Puedes encontrar un botón verde en la parte superior:


.. figure:: /_static/2-Micropython/images/buton.png
   :width: 50%
   :align: center
   :alt: sections of the code

Visualiza el resultado en la terminal serial:

.. figure:: /_static/2-Micropython/images/result.png
   :width: 50%
   :align: center
   :alt: sections of the code

Este sencillo paso te introduce en el entorno MicroPython y establece las bases para las prácticas siguientes. ¡Explora y disfruta tu experiencia de programación en MicroPython!

.. figure:: /_static/2-Micropython/images/explorando.png
  :alt: Exploración de MicroPython
  :align: center
  :width: 800px