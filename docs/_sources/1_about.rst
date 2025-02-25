
Introducción a la DUALMCU
==========================

Descripción general
----------------------

La tarjeta de desarrollo DualMCU representa una innovadora fusión entre el microcontrolador Raspberry Pi RP2040 y el chip Espressif ESP32 WROOM, consolidados en un único dispositivo. Este diseño aprovecha plenamente los núcleos duales Arm® Cortex®-M0+ de 32 bits, proporcionando una base sólida para implementar proyectos IoT con conectividad Bluetooth® y Wi‑Fi.

En términos de potencia, la DualMCU integra dos microprocesadores de 32 bits: un Cortex M0+ del Raspberry Pi RP2040 operando a 133 MHz y un ESP32 que alcanza hasta 240 MHz. Con un tamaño de PCB de 36 mm x 84 mm y tecnología de montaje superficial, la placa alberga cuatro núcleos programables, ofreciendo funciones inalámbricas avanzadas y un consumo de energía muy bajo.

.. figure:: /_static/1-Descripcion-general/images/EU0002-DualMCU_V7.jpg
   :width: 50%
   :align: center
   :alt: sections of the code

   DualMCU 

Para obtener información adicional, se recomienda visitar el 
`repositorio oficial de la DualMCU <https://github.com/UNIT-Electronics/DualMCU>`_.

Pinout
~~~~~~

El pinout de la DualMCU se presenta a continuación:

.. figure:: /_static/1-Descripcion-general/images/EU0002-DUALMCU_V3.1.2.jpg
   :width: 100%
   :align: center
   :alt: sections of the code

   Pinout DualMCU


Características técnicas
-------------------------

A continuación se destacan sus principales atributos técnicos:


.. list-table:: Descripción DualMCU
   :widths: 50 50
   :header-rows: 0
   
   * - **Fabricante:** UNIT ELECTRONICS
     - **Color de PCB:** Negro

   * - **Dimensiones:** 84 mm x 36 mm x 6.6 mm
     - **Peso:** 22.57 g

   * - **MCUs:** RP2040 Dual Core + ESP32 WROOM-32E
     - **USB a UART:** CH340C

   * - **Conectores:** 2 x I2C JST-SH Pitch 1 mm, 1 MicroSD, USB Tipo C y JST-SH 2p Pitch 2 mm (conexión para batería).
     - **Incluye:** Tira header macho doble 2.54 mm (2×3, 2×20 pines)

   * - **Memoria:** W25Q16JVUXIQ 2 MB NOR Flash, 532 MHz Quad SPI y 66 MB/s transferencia continua.
     - **Alimentación:** 3.3V LDO 600 mA, 3.3V Power/Enable, VUSB Output/VIN de 3.2 a 6 V DC, interfaz para cargar baterías de 200 mA con LED incorporado.

   * - **SWITCH:** Power Switch, Selector de comunicación USB, DIP Switch para UART, Botón de RESET y cargador de arranque para reinicios rápidos (RP2040) y botón de RESET/FLASH/BOOT.
     - **LEDs:** LEDs RGB WS2812B NeoPixel (RP2040), LED RGB de cátodo común (ESP32) y LED en GPIO25 (RP2040).

   * - **MICROSD CARD:** Conexión a ESP32 y comunicación vía VSPI.
     -



Características de la placa
---------------------------

A continuación, se presenta la disposición de elementos de la placa para facilitar su uso.

Vista frontal
~~~~~~~~~~~~

.. figure:: /_static/1-Descripcion-general/images/Front_View_DualMCU_Topology.jpg
   :width: 70%
   :align: center
   :alt: sections of the code

.. list-table:: Componentes de la vista frontal
   :widths: 15 35 15 35
   :header-rows: 1

   * - **Ref.**
     - **Descripción**
     - **Ref.**
     - **Descripción**
   * - U1
     - Microcontrolador Raspberry Pi RP2040
     - U2
     - Módulo Wi‑Fi/Bluetooth® Espressif ESP32 WROOM
   * - U3
     - Circuito integrado de memoria flash de 2 MB W25Q16JVUXIQ
     - U4
     - Circuito integrado de conversión USB CH340C
   * - U5
     - Circuito integrado de gestión de carga de batería MCP73831
     - U6
     - Regulador de voltaje LDO 3.3V AP2112K
   * - L1
     - LED de encendido
     - L2
     - LED de carga
   * - L3
     - LED (GPIO25)
     - L4
     - WS2812B LED
   * - L5
     - LED RGB 2020
     - J1
     - Conector USB tipo C macho
   * - PB1
     - Botón de reinicio RP2040
     - PB2
     - Botón de arranque RP2040
   * - PB3
     - Botón de flasheo ESP32
     - PB4
     - Botón de reinicio ESP32
   * - JP1
     - GPIO Pines de la RP2040
     - JP2
     - ESP32 GPIO Header
   * - JP3
     - RP2040 (SWD) Debug Header
     - JST1
     - Conector JST I2C RP2040
   * - JST2
     - Conector JST I2C ESP32
     - JST3
     - Conector JST para batería de litio (LiPo)
   * - SW2
     - Selector de comunicación USB
     - SW3
     - Interruptor DIP UART


Vista reversa
~~~~~~~~~~~~

.. figure:: /_static/1-Descripcion-general/images/Back_View_DualMCU_Topology.jpg
   :width: 70%
   :align: center
   :alt: sections of the code


.. list-table:: Componentes de la vista reversa
   :widths: 15 85
   :header-rows: 1

   * - **Ref.**
     - **Descripción**
   * - U7
     - Soporte para el circuito integrado criptográfico ATECC608A-MAHDA-T
   * - J2
     - Conector para tarjeta microSD
   * - SW1
     - Interruptor de encendido
   * - SB1
     - Puente de soldadura del LED de carga (desconectado por defecto)
   * - SB2
     - Puente de soldadura del sensor VBUS (desconectado por defecto)
   * - SB3
     - Regulador de voltaje LDO 3.3V AP2112K
   * - SB4
     - Puente de soldadura del reinicio ESP32 (desconectado por defecto)
   * - SB5
     - Puente de soldadura del selector de señal SCL para ATECC608A-MAHDA-T (desconectado por defecto)
   * - SB6
     - Puente de soldadura del selector de señal SDA para ATECC608A-MAHDA-T (desconectado por defecto)
   * - B1
     - Pads de soldadura para batería de litio (LiPo)

