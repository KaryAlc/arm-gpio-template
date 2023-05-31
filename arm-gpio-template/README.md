LAB 5 - CONFIGURACIÓN DE GPIO
Karina Alcantara Segura

Funcionamiento del proyecto:


El código presentado fue basado en la plantilla arm-gpio-template, recuperado de: https://github.com/Ryuuba/arm-gpio-template .

El proyecto tiene la funcionalidad de encender 8 LED conectados a los puertos GPIO del µC. Los LED muestran el valor binario de una variable. Si se oprime un push button A, entonces se incrementa el valor de la variable en una unidad. Si se oprime un push button B, entonces el valor de la variable se decrementa. Si se oprimen los dos botones, entonces el valor de la variable se limpia.

El proyecto está compuesto de los siguientes archivos:

main.s
Costa de dos partes determinadas por medio de etiquetas, setup y loop:

En setup se configuran los puertos que usaremos como entradas y salidas, en este caso se habilitan 8 puertos de escritura de P0 al P7 y 2 de lectura el P10 y el P11, en donde se conectaran los leds y los push button respectivamente.

En loop, se colocó la lógica del programa, en donde lee los valores de los botones y decrementa o aumenta, dependiendo el caso.

output 
Esta función se encarga de emitir el valor del contador por un puerto digital.

digital_read 
Es la que se encarga de la lectura de los botones.

read_button
Esta funcion es la encargada de generar un contador antirrebote, 

Compilar:
Primero se ensambla el archivo main.s para obtener el código objeto con el comando:

arm-as main.s -o main.o

 Se realiza la construcción del archivo binario con el comando:

arm-objcopy -O binary main.o main.bin 

Y posteriormente se graba con el comando:
st-flashwrite 'main.bin' 0x8000000


Diagrama: 

