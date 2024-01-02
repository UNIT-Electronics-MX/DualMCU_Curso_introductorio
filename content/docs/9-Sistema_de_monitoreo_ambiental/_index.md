---
title: 9. Sistema de monitoreo ambiental
type: docs
weight: 9
BookToC: false
---

# Prácticas con la DualMCU - MicroPython

## 9. Sistema de monitoreo ambiental 
### 9.1. Objetivo
Utiliza sensores para medir parámetros ambientales como temperatura, humedad, calidad del aire, etc. 
### 9.2. Descripción
Este apartado contiene un conjunto de recursos y código para construir un sistema de control ambiental utilizando un dispositivo ESP32 o RP2040 con MicroPython. Los sistemas de control ambiental son fundamentales para medir y gestionar parámetros como la temperatura, humedad, calidad del aire y otros factores para crear entornos cómodos y seguros.

### 9.5 Requisitos
+  1x Placa de desarrollo [DualMCU](https://uelectronics.com/producto/unit-dualmcu-esp32-rp2040-tarjeta-de-desarrollo/)
+ Sensores de temperatura, humedad, calidad del aire, u otros sensores ambientales según tus necesidades.
+ Conexiones eléctricas y fuente de alimentación adecuadas.


### 9.3 Contenido del Repositorio
En este repositorio, encontrarás el código fuente necesario para monitorear y controlar parámetros ambientales. Se proporcionarán ejemplos de código que demuestran cómo utilizar sensores para medir temperatura, humedad, calidad del aire y otros factores.

Ejemplo de cómo podrías leer datos de un sensor de temperatura y humedad [DHT11](https://uelectronics.com/producto/modulo-ky-015-sensor-de-temperatura-y-humedad/) y un sensor de calidad del aire [MQ135](https://uelectronics.com/producto/mq-135-modulo-detector-de-calidad-de-aire/) con un ESP32 en MicroPython:

```python
'''
Unit Electronics 2024
       (o_
(o_    //\
(/)_   V_/_ 

version: 0.0.1
revision: 0.0.1
context: This code is a basic configuration of a ADC
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
```

Este código lee la temperatura y la humedad del sensor DHT11 y la calidad del aire del sensor MQ135 cada minuto, e imprime los valores leídos.


> **Nota:** Ten en cuenta que este código es un ejemplo y puede que necesites ajustarlo según tu configuración específica y tus necesidades.

Por ejemplo, podrías querer enviar los datos leídos a un servidor o a una base de datos en lugar de simplemente imprimirlos. Además, el sensor MQ135 necesita ser calibrado para proporcionar lecturas precisas de la calidad del aire.

* [Licencia](https://www.gnu.org/licenses/gpl-3.0.html) El código que se presenta en este repositorio está licenciado bajo la Licencia Pública General de GNU (GPL) versión 3.0.
---
⌨️ con ❤️ por [UNIT-Electronics](https://github.com/UNIT-Electronics) 😊