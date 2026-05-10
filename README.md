<[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/MCJunYEq)
[![Open in Visual Studio Code](https://classroom.github.com/assets/open-in-vscode-2e0aaae1b6195c2367325f4f02e2d04e9abb55f0b24a779b69b11b9e10269abc.svg)](https://classroom.github.com/online_ide?assignment_repo_id=23650708&assignment_repo_type=AssignmentRepo)
# Lab06: Comunicación UART con PIC18F45K22

## 1. Integrantes

* [Johan Sebastian Leon Villabon](https://github.com/johanleon96)
* [Juan David Riaño Gutierrez](https://github.com/DONTFISRT)
* [Jairo Alberto Hernandez Avila](https://github.com/jairoaha)

## 2. Documentación

# Laboratorio 2 - Microprocesadores / Caracterización de osciladores con microcontrolador PIC 

## Indice
#### 1. [Resumen y Marco Teorico]

#### 1.1 [Herramientas y materiales usados]

#### 2. [Descripción del laboratorio]

#### 2.1 [Explicación del código implementado]

#### 2.2 [Análisis]

#### 3. [Evidencias de implementación]

#### 4. [Preguntas]

## 1.1 Resumen

En esta práctica hemos realizado el analisis, modelación, relacionamiento y montaje de  dos circuitos operados por un **PIC18F45K22**  que les brindará una salida digital mediante un código de programación hecho en lenguaje **C** por medio de un compilador llamado **MPLAB X IDE**.  Así, con esto, empezar a comprender un poco más el funcionamiento de los microprocesadores en un entorno relacionado a osciladores, condensadores y medidas de señales.

## * Marco Teorico

**PIC18F45K22:** Es un microcontrolador de alto desempeño, con arquitectura optimizada basado en lenguaje ```C compiler``` en encapsulado tipo ```DIP``` de 18 pines, este es un componente de bajo consumo con memoria SRAM de 1536 bytes.

**Lenguaje C:** Es un lenguaje de programación de propósito general​ originalmente desarrollado por Dennis Ritchie entre 1969 y 1972. Es muy eficiente y ofrece la posibilidad de manejar todos los aspectos de las instrucciones del ```CPU```. Su código ofrece una estructura clara y, por tanto, facilita la creación de aplicaciones de una forma rápida y potente.

**MPLAB X IDE:** Funciona como una interfaz unificada para herramientas de desarrollo de software y hardware adicionales de ```Microchip``` y de terceros. Es un  entorno de desarrollo integrado (IDE) gratuito y multiplataforma de Microchip para programar microcontroladores PIC® y AVR®, basado en NetBeans.

**PuTTY:** Es un cliente ```SSH```y Telnet gratuito y de código abierto para Windows y sistemas Unix, utilizado principalmente para gestionar servidores remotos y ```dispositivos de red``` de forma segura. Permite conexiones cifradas, emulación de terminal xterm y se destaca por ser ligero, portátil y fácil de configurar para administración de sistemas.

**UART:** (```Universal Asynchronous Receiver-Transmitter```) es un protocolo de comunicación serial asíncrono, ampliamente utilizado en electrónica para intercambiar datos entre microcontroladores, sensores y PC. Utiliza solo dos cables (```TX y RX```) y no requiere señal de reloj, basándose en la misma tasa de baudios (velocidad) para funcionar

## 1.2 Herramientas y materiales

* Microcontrolador ```PIC18F45K22``` 

* LEDS.

* Resistencias 330Ω y 1kΩ.

* Programador (PICkit $3$, PICkit $4$).

* Fuente de alimentación de $5$ V → El PICkit $3$ o $4$ para suministrar tensión directamente al circuito (típicamente $5$ V o $3.3$ V, según se configure en MPLAB X)

* Entorno de programación ```MPLAB X IDE``` con compilador ```XC8```.

* Condensadores electrolíticos de 21pF y 15pF.
  
* Programador, terminal PuTTY
  
* UART
  
* Computador con un SO relacionable

* Protoboard y cables de conexión.

### 2.1 Descripción del laboratorio



### 2.2 Implementaciòn de código. 

### Código 1 de archivo Python para lectura de gráfica. 
```
import serial
import matplotlib.pyplot as plt
import matplotlib.animation as animation
import re
from collections import deque

SERIAL_PORT = 'COM4'      
BAUDRATE = 9600

MAX_POINTS = 100

ser = serial.Serial(SERIAL_PORT, BAUDRATE, timeout=1)

voltages = deque(maxlen=MAX_POINTS)
times = deque(maxlen=MAX_POINTS)
time_counter = 0

regex = re.compile(r"Voltaje:\s*([0-9.]+)")

def update(frame):
    global time_counter
    line = ser.readline().decode('utf-8').strip()
    
    match = regex.search(line)
    if match:
        voltage = float(match.group(1))
        voltages.append(voltage)
        times.append(time_counter)
        time_counter += 1

        ax.clear()
        ax.plot(times, voltages, color='blue')
        ax.set_ylim(0, 5)
        ax.set_title("Voltaje leído por UART")
        ax.set_xlabel("Tiempo (s)")
        ax.set_ylabel("Voltaje (V)")
        ax.grid(True)

fig, ax = plt.subplots()
ani = animation.FuncAnimation(fig, update, interval=1000, cache_frame_data=False)
plt.tight_layout()
plt.show()
```

### Código 2 en el archivo **main.c** .

```
#include <xc.h>
#include "uart.h"

#pragma config FOSC = INTIO67  // Oscilador interno
#pragma config WDTEN = OFF     // Watchdog Timer apagado
#pragma config LVP = OFF       // Low Voltage Programming off


void main(void) {
    OSCCON = 0b01110000;  // Oscilador interno a 16MHz
    UART_Init();          // Inicializa UART


    while(1) {
    
        UART_WriteString("Voltaje: 0\r\n");
        __delay_ms(500);
        UART_WriteString("Voltaje: 1\r\n");
        __delay_ms(500); 
        UART_WriteString("Voltaje: 2\r\n");
        __delay_ms(500); 
        UART_WriteString("Voltaje: 3\r\n");
        __delay_ms(500); 
        UART_WriteString("Voltaje: 4\r\n");
        __delay_ms(500); 
        UART_WriteString("Voltaje: 5\r\n");
        __delay_ms(500); 
    }
}
```

### 2.3 Análisis.



## 3. Evidencias

![pic](/codec.jpg)
### Implementacion de código en MPLAB (interno)

![pic](/graficapy.jpg)
### Demostración de gráfica en Python

![pic](/montajeuart.png)
## Montaje físico UART y PICkit 

![pic](/puttyde.jpg)
## Mendaje de salida en la terminal Putty


## 4. [Preguntas]
