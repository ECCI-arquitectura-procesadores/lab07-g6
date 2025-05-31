[![Open in Visual Studio Code](https://classroom.github.com/assets/open-in-vscode-2e0aaae1b6195c2367325f4f02e2d04e9abb55f0b24a779b69b11b9e10269abc.svg)](https://classroom.github.com/online_ide?assignment_repo_id=19670492&assignment_repo_type=AssignmentRepo)
# lab07 - Procesador

## Integrantes
[RICARDO ANDRES MERCHAN CARREÑO](https://github.com/Andresm9871)

[ANDRES FELIPE ROMERO IBAÑEZ](https://github.com/AndresitoTuRey)

[CLAUDIA XIMENA COLMENARES MESTIZO](https://github.com/CXCM123)

[JUAN DAVID VELASCO SARMIENTO](https://github.com/juanDvelasco1013)

## Documentación

Objetivo.

El objetivo no era solo hacer que funcionara una secuencia de LEDs. La idea era:

Familiarizarse con la arquitectura RISC-V (una arquitectura de procesadores abierta, usada en muchos proyectos académicos e industriales).

Aprender a integrar un sistema en chip (SoC) con procesador, memoria y periféricos, dentro de una FPGA.

Usar herramientas profesionales (como Quartus, compilador cruzado y Makefiles) para automatizar el flujo de diseño hardware-software.

Programar la FPGA y ver resultados concretos, como la activación de LEDs y la comunicación UART.

Introducción.

Un SoC no es solo un procesador. En este caso:

PicoRV32 es el procesador: ejecuta instrucciones del firmware, compiladas en lenguaje C.

Firmware: es un pequeño programa en lenguaje C que tú compilas con el compilador riscv32-unknown-elf-gcc. Este programa hace cosas como prender/apagar LEDs o enviar texto por UART.

UART (Universal Asynchronous Receiver/Transmitter): es el módulo que permite enviar y recibir datos a través del puerto serie de tu computador.

GPIOs (General Purpose Input/Output): son pines de la FPGA que se usan como salida para controlar, por ejemplo, LEDs.

El diseño tiene dos grandes bloques:

Hardware (Verilog): describe la lógica del SoC (CPU, RAM, UART, GPIO, buses, etc.).

Software (C): el programa que corre dentro del procesador PicoRV32.

Herramientas utilizadas.

Intel Quartus Prime: Sirve para sintetizar el diseño escrito en Verilog, convertirlo a un bitstream (archivo .sof) y cargarlo a la FPGA.

MSYS2 + Make: MSYS2 da un ambiente estilo Linux donde puedes usar comandos tipo GNU. make permite automatizar tareas: compilar firmware, construir hardware y programar FPGA.

riscv32-unknown-elf-gcc: Es un compilador cruzado. Genera archivos binarios que puede entender PicoRV32, ya que este no es compatible con un procesador x86 (como el tuyo).

Procedimiento.

Paso a paso detallado:
a) Clonación del repositorio

git clone https://github.com/DianaNatali/picosoc_simple.git
Esto descarga un proyecto de ejemplo donde ya están escritos los módulos del SoC (en Verilog) y un firmware ejemplo en C.

b) Compilar el firmware
     make
Este comando ejecuta varias acciones:

main.c (firmware en C) se compila usando riscv32-unknown-elf-gcc.

El resultado es main.bin, un archivo binario que contiene las instrucciones que el PicoRV32 debe ejecutar.

Este main.bin se convierte a progmem.v: una ROM en Verilog que guarda esas instrucciones como si fueran memoria interna.

c) Construir todo el proyecto para FPGA
make build TARGET=max10
Este paso usa quartus_map, quartus_fit, quartus_asm, etc., para:

Tomar todos los archivos Verilog (incluyendo progmem.v, UART, CPU, etc.).

Sintetizarlos, asignar recursos dentro de la FPGA.

Generar un archivo .sof que puedes cargar al dispositivo físico.

d) Programar la FPGA
make TARGET=max10 program

Esto usa Quartus Programmer para:

Enviar el .sof a la FPGA real.

Una vez cargado, la FPGA empieza a ejecutar lo que dice el firmware desde el primer ciclo de reloj.

Resultados.

¿Qué hace el firmware?
Configura los GPIO como salidas.

Enciende y apaga los LEDs en una secuencia, probablemente con retardos (delays).

Envía un texto por UART. Puedes ver ese texto si abres un monitor serie (como PuTTY o Tera Term) y seleccionas el puerto COM correcto.

¿Qué ves?
Secuencia de luces (en los LEDs conectados).

Un mensaje en pantalla como: Hello from PicoSoC! a través del monitor serial.

Conclusiones.

Modularidad del diseño: aprendiste a integrar diferentes componentes (CPU, memoria, UART, GPIO) en un sistema completo.

Separación hardware/software: Verilog describe el hardware; C define el comportamiento del firmware.

Automatización del flujo: gracias al uso de Makefiles, pudiste construir firmware, hardware y programar la FPGA con un solo comando.

Importancia del compilador cruzado: el procesador PicoRV32 no entiende instrucciones de x86, así que necesitas riscv32-unknown-elf-gcc para compilar programas que pueda ejecutar.


Funcionamiento del UART en Verilog
Te explico cómo el UART recibe y transmite datos, cómo se configura el baud rate, y cómo el CPU interactúa con él.

Detalle del archivo main.c 
Revisamos el código C que controla LEDs y UART. ¿Qué hace línea por línea? ¿Cómo se comunican las instrucciones con el hardware?

Conexión entre la CPU y los periféricos
Cómo la PicoRV32 accede a la RAM, UART, GPIO y ROM usando un bus simple tipo Wishbone.

Cómo modificar o añadir periféricos nuevos
Por ejemplo: agregar un temporizador, un botón, un sensor, etc.

Ver datos UART en PuTTY o Tera Term
Cómo conectar el puerto serie de la FPGA a tu PC, configurar velocidad (baud rate) y ver el mensaje enviado por el SoC.



## Evidencias

<video controls src="FUNCIONAMIENTO_LAB7.mp4" title="Title"></video>