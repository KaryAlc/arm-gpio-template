#LAB 5 - CONFIGURACIÓN DE GPIO
#Karina Alcantara Segura

Funcionamiento del proyecto:


El código presentado fue basado en la plantilla arm-gpio-template, recuperado de: https://github.com/Ryuuba/arm-gpio-template .

El proyecto tiene la funcionalidad de encender 8 LED conectados a los puertos GPIO del µC. Los LED muestran el valor binario de una variable. Si se oprime un push button A, entonces se incrementa el valor de la variable en una unidad. Si se oprime un push button B, entonces el valor de la variable se decrementa. Si se oprimen los dos botones, entonces el valor de la variable se limpia.

El proyecto está compuesto de los siguientes archivos:

main.s
Costa de dos partes determinadas por medio de etiquetas, setup y loop:

En setup se configuran los puertos que usaremos como entradas y salidas, en este caso se habilitan 8 puertos de escritura de P0 al P7 y 2 de lectura el P10 y el P11, en donde se conectaran los leds y los push button respectivamente.

En loop, se colocó la lógica del programa, en donde lee los valores de los botones y decrementa o aumenta, dependiendo el caso.

El marco de esta función se ve de la siguiente manera:
-. ------------------
-.| counter |   r7   |
-. ------------------
-.|button A | r7 + 4 |
-. ------------------
-.|button B | r7 + 8 |
-. ------------------
-.|         |        |
-. ------------------
-.|    r7   |        |
-. ------------------ 
-.|    lr   |        |
-. ------------------ 

output :
Esta función se encarga de emitir el valor del contador por un puerto digital. Se utiliza la mascara correspondiente a 1032 (0x3FF) para mostrar en 10 leds. 

digital_read 
Es la que se encarga de la lectura de los botones.

read_button
Esta funcion es la encargada de generar un contador antirrebote, comenzando con leer el estado del botón (ya sea 1 o 0), dependiendo ese estado regresa un 0 si esta apagado o entra en un ciclo que verifica cuantos estados de encendidos seguidos hay.

el marco de esta función se ve de la siguiente manera:
-. ------------------
-.|   pin   |   r7   |
-. ------------------
-.|  port   | r7 + 4 |
-. ------------------
-.|         | r7 + 8 |
-. ------------------
-.|   bit   |r7 + 12 |
-. ------------------
-.|    i    |r7 + 16 |
-. ------------------ 
-.| counter |r7 + 20 |
-. ------------------  
-.|   r7    |        |
-. ------------------
-.|   lr    |        |
-. ------------------  

Compilar:
De la manera tradicional:

Primero se ensambla el archivo main.s para obtener el código objeto con el comando:

arm-as main.s -o main.o

 Se realiza la construcción del archivo binario con el comando:

arm-objcopy -O binary main.o main.bin 

Y posteriormente se graba con el comando:
st-flashwrite 'main.bin' 0x8000000

O con el STMCubeProgrammer

Con el comando Make:

Primero se clona el repositorio para tener la informacion de manera local, con el comando:
    git clone
Luego con make se desenlaza del repositorio, de esta manera:
    make unlink 
Y se remueve se la rama original:
    git remove rm origin
Se borran todos los archivos .o, .bin y . elf con el comando clean:
    make clean
    make cleanwin
En el archivo MakeFile se mopdifica la linea 10 con los nombres de tus archivos .s que vas a compilar.
Se hace make para obtener los .o, .elf y .bin:
    make
Y se escribe conel siguiente comando, como observacion para escribir el microcontrolador el jumper debe estar en la esquina superior derecha.
