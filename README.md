# PROJECT ATLAS
This is the most versatile MULTICOMP available nowadays, and one of the cheapest also, that's great because when you learn touching a bunch of electronics sometimes burns something...
At the first time of the proyect nearly 3 years ago, the cost of the CYC1000 was only 18€, nowadays more than doubled this cost :-(

This proyect aims to use a strong open hardware license, CERN OPEN HARDWARE LICENSE VERSION II STRICT:

https://ohwr.org/cern_ohl_s_v2.txt


#Last prototype with RP2040:

![Last prototype joy view](https://github.com/AtlasFPGA/CYC1000/blob/main/FOTOS/Ultimo_prototipo_RP2040DB9.JPG)

![Las prototype video view](https://github.com/AtlasFPGA/CYC1000/blob/main/FOTOS/Ultimo_prototipo_RP2040VIDEO.JPG)

The object of this mostly 3 year proyect, is to give some new features to fpga users, in the use of microfpgas, we focuselly at first in "CYC1000" & "MAX1000", and with the price and belonging to FPGAWARS the "Tang nano 9k".
We focuses in creating documentation and sources, in order to provide only information about the I/O Board ATLAS, the cores most of there where ported in order to learn mainly hardware descriptor languages:
the most, The Verilog language, and VHDL language.
We focuses also porting some retrocores to This inexpensive I/O Board ATLAS, because in the initial design we decided not to use any chip, only transistors/diodes, resistors or capacitors. 
Making ideal to be reproduced.

With the information we provided in this project, has been proven that it could be reproduced, "The I/O Board ATLAS" with or without multicore solution, in all over the world "Trust us!", there are relocators and fpgas/microcontrollers you can use.
We Justify the birth of this I/O Board ATLAS, because were a nastly shortage of components in the previous years, an we want to incorporate all the features inside the fpga choosen the cyclone used in CYC1000 give 25KLes at the lowest cost.
For example, the use of USB-DIRECT, it is at early stages, more future implementations will come, until now the only I/O Board that uses K1 & L1 in cyc1000 to create a pull down providing USB, is the ATLAS's one.

More than 100 units of prototypes were realized in diferent form factors, but remain the same scheme, so the main think line is.

1.- Not to use any component that uses chips.

2.- Give as much funcionality as posible like USB-DIRECT & DIGITAL-VIDEO.

3.- Sound using DIGITAL-VIDEO & Deltasigma-Stereo

4.- Proportion of an SD with SPI and SIO capabilities, that is given 4 bits for data.

5.- Maintain a bus with 6 signals in the common bus 2x20 form standarized by Rasperry PI.

6.- Give a MULTICOMP structure, that's make the implementation only FPGAish if desired.

ATLAS I/O BOARD:

https://oshwlab.com/subcritical/carrier_io_board_atlas_mini_copy#P3


# PROYECTO ATLAS

La placa ATLAS se diseña con el objetivo de crear un multicore basado en SBC (SINGLE BOARD COMPUTER).

Inicialmente la placa adaptadora ATLAS es un proyecto para la placa CYC1000 y la SBC Raspberry Pi.

Mas info y fotos en este [hilo](http://www.forofpga.es/viewtopic.php?f=28&t=376&p=1548#p1548), en su propio [foro](http://www.forofpga.es/viewforum.php?f=240&sid=bd3e070f65599ff111ad0494e4459535) de foroFPGA, en [telegram](https://t.me/CYC1000) o [discord](https://discord.gg/YDdmtwh).

### Cores disponibles

* http://www.forofpga.es/viewforum.php?f=241

### Guía rápida

Si ya tienes tu CYC1000, una raspberry Pi, y la placa ATLAS (la puedes conseguir en el grupo de [telegram](https://t.me/CYC1000)), sigue esta guía rápida para empezar a trastear. 

El proyecto está empezando y se irán simplificando los pasos en el futuro de forma que para cargar los cores ya no será necesario acceder al terminal de Linux de la placa SBC.

#### Ensamblaje de las placas:

<img src="ATLAS-ensamble.png" alt="ensamblaje" style="zoom:80%;" />

Leyenda: (1) Placa Atlas, (2) FPGA CYC1000, (3) SBC Raspberry Pi, (4) Alimentación general, (5) DB9 para joystick, (6) teclado PS2, (7) ratón PS2, (8) salida video HDMI, (9) jack 3.5 audio, (10) tarjeta microSD ATLAS, (11) tarjeta microSD rPi

Otras conexiones opcionales: 

(12) Conexiones de vídeo y teclado a la SBC rPi para acceder al terminal de linux . No es necesario si has configurado acceso ssh por WiFi.

(13) USB Blaster para cargar cores directamente a la CYC1000. No es necesario si la carga se hace a través de la SBC rPi.

#### Configuración inicial de la SBC raspberry Pi

* Descarga raspberry Pi OS Lite https://www.raspberrypi.org/software/operating-systems/ y grábalo en una tarjeta micro SD
* Arranca  la SBC con la tarjeta micro SD del paso anterior en el zócalo (11) (no es necesario tenerla conectada aún a la placa ATLAS) con el video y teclado conectados y con un cargador de adecuado amperaje (>2A). 
  * Para acceder al terminal Linux de la rPI el usuario / contraseña por defecto son: pi / raspberry
* Configuración de la raspberry Pi con el comando `sudo raspi-config` (la configuración del WiFi es opcional y puedes encontrar mas información [aquí](https://www.raspberrypi.org/documentation/configuration/wireless/wireless-cli.md)): 
  * 5 Localization Options
    * L4 WLAN country 
    * L3 keyboard
  * 1 system options
    * S1 wireless LAN

#### Instalación del cargador de cores programRBF en la FPGA 

Nota: vamos a trabajar desde el directorio /home/pi que es el que aparece por defecto al arrancar el terminal Linux con el usuario "pi".

* Instala la librería WiringPi:

  ```
  wget https://project-downloads.drogon.net/wiringpi-latest.deb
  sudo dpkg -i wiringpi-latest.deb
  ```

* Compila el ejecutable para cargar los cores:

  ```
  wget https://raw.githubusercontent.com/shaeon/programrbf/main/programrbf_v01.cpp
  g++ programrbf_v01.cpp -lwiringPi -o programrbf
  ```

#### Arranca los cores desde el terminal Linux de la SBC

* Copia los cores con extensión .rbf a la tarjeta SD, en la partición "rootfs", directorio ""/home/pi".

* Carga el core con el ejecutable programrbf (el ejemplo carga el [msx_atlas.rbf](./cores/msx_atlas.rbf) de MSX1):

  ```
  ./programrbf msx_atlas.rbf 17 22 4 27
  ```
  
  Nota: El core de MSX1 requiere soldar un reloj adicional de 50 MHz, y tener una tarjeta micro SD con [este contenido](https://mega.nz/file/20pi1aiY#FwhOZryEUyuyU1gEUCVma1ndn-2BqtvH7RUx-qwgqs0) insertada en el zócalo micro SD (10) de la placa ATLAS.

#### Script para cargar un core automáticamente al arrancar la SBC

* Ejecuta los siguientes comandos en el terminal de Línux:

  ```sh
  crontab -e
  #selecciona por ejemplo nano y al final del fichero cron añade lo siguiente:
  @reboot ~/programrbf msx_multicore2.rbf 17 22 4 27
  ```



### **Reloj adicional de 50 Mhz:** en estados muy iniciales del uso de la placa CYC1000 en la I/O Board ATLAS

La placa CYC1000 dispone de un reloj de 12 Mhz. En un principio se usó un segundo reloj a 50Mhz, pero en la actualidad todos los cores funcionan con el reloj de serie de 12Mhz.
[foto](http://www.forofpga.es/viewtopic.php?f=240&t=390).

Si sería interesante en un futuro dejar libre este relój para incorporar un pll externo tipo SI5351.
Para los que aún les interese conseguir el reloj de 50Mhz pueden pedirlo:

* [Arrow](https://www.arrow.com/en/products/ecs-2520mv-500-bn-tr/ecs-international?q=ECS-2520MV-500-BN-TR)

EL primer core que fué usado con un rejog de 50 MHz fué el core de MSX1, pero la implementación del mismo con un relój de 12Mhz es totalmente válida:

* MSX1 
Estamos hablando de hace aproximadamente 3 años, al inicio de la primera tirada de placas I/O Board ATLAS para desarrollo.

### **Colaboraciones:**

Se necesita la [colaboración](https://github.com/SoCFPGA-learning/General/tree/main/Github_ayuda) para poder incrementar los recursos disponibles para la comunidad entorno esta placa de desarrollo.    

### Comunidad:

* [Cyc1000 Atlas Anouncements](https://t.me/CYC1000) (Spanish/English)
* [Atlas FPGA international telegram group](https://t.me/ATLASFPGA) (English)
* [Discord channel](https://discord.gg/YDdmtwh) 

