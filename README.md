# Klipper Configuration P802NR2
Configuración de impresora Zonestar doble extrusor P802NR2 en klipper

## Instalación de la imagen para raspberry PI (RPi)

### Descarga y configuración de la imagen en una SSD.


### Configuración de la WIFI

### Averiguar la dirección IP

Lo primero es averiguar la dirección IP de la RPi.

La más sencilla es conectarla por hdmi a un monitor.

Te logueas con las credenciales en el terminal y se pone el comando:
`ifconfig`

Este comando te dará la ip, que será de la siguiente forma: 192.168.1.xxx , siendo xxx un número asignado por tu router.

Otra manera de averiguar la ip es a través del router, buscar un dispositivo RPi y ver cuál es su ip.

Una vez que tenemos esta IP, 

## Instalación de klipper

## Configuración manual de los pines de la impresora.

link: Descarga del fichero de configuración preliminar

### Configuración de pines
Los pines se consultan en el código fuente de marlin del fichero pins_ZRIB.h y config/sample-aliases.cfg de klipper en el apartado [board_pins arduino-mega].
``` cfg
    [board_pins arduino-mega]
    aliases:
    ar0=PE0, ar1=PE1, ar2=PE4, ar3=PE5, ar4=PG5,
    ar5=PE3, ar6=PH3, ar7=PH4, ar8=PH5, ar9=PH6,
    ar10=PB4, ar11=PB5, ar12=PB6, ar13=PB7, ar14=PJ1,
    ar15=PJ0, ar16=PH1, ar17=PH0, ar18=PD3, ar19=PD2,
    ar20=PD1, ar21=PD0, ar22=PA0, ar23=PA1, ar24=PA2,
    ar25=PA3, ar26=PA4, ar27=PA5, ar28=PA6, ar29=PA7,
    ar30=PC7, ar31=PC6, ar32=PC5, ar33=PC4, ar34=PC3,
    ar35=PC2, ar36=PC1, ar37=PC0, ar38=PD7, ar39=PG2,
    ar40=PG1, ar41=PG0, ar42=PL7, ar43=PL6, ar44=PL5,
    ar45=PL4, ar46=PL3, ar47=PL2, ar48=PL1, ar49=PL0,
    ar50=PB3, ar51=PB2, ar52=PB1, ar53=PB0, ar54=PF0,
    ar55=PF1, ar56=PF2, ar57=PF3, ar58=PF4, ar59=PF5,
    ar60=PF6, ar61=PF7, ar62=PK0, ar63=PK1, ar64=PK2,
    ar65=PK3, ar66=PK4, ar67=PK5, ar68=PK6, ar69=PK7,
    analog0=PF0, analog1=PF1, analog2=PF2, analog3=PF3, analog4=PF4,
    analog5=PF5, analog6=PF6, analog7=PF7, analog8=PK0, analog9=PK1,
    analog10=PK2, analog11=PK3, analog12=PK4, analog13=PK5, analog14=PK6,
    analog15=PK7,
    # Marlin adds these additional aliases
    ml70=PG4, ml71=PG3, ml72=PJ2, ml73=PJ3, ml74=PJ7,
    ml75=PJ4, ml76=PJ5, ml77=PJ6, ml78=PE2, ml79=PE6,
    ml80=PE7, ml81=PD4, ml82=PD5, ml83=PD6, ml84=PH2,
    ml85=PH7
```

## Calibración inicial de la impresora con klipper:

### Calibrar el extrusor:

`PID_CALIBRATE HEATER=extruder TARGET=210`

Guarda la configuración

`SAVE_CONFIG`

Calibrar el extruder1

### Calibrar la cama:

`PID_CALIBRATE HEATER=heater_bed TARGET=50`

Guarda la configuración

`SAVE_CONFIG`

### Calibrar el motor del extrusor:
Sacar unos 6 cm de filamento y hacer una marca a 5 cm de la entrada.
Luego poner el comando:
G1 E50 F60
La marca debería quedar justo donde hemos empezado a medir.
Si no hay que calibrar de la siguiente manera:
Hacer una marca de donde se ha quedado y medir con la marca de referencia.
Ese valor en mm lo llamaremos (k) y habrá que sumárselo o restárselo a 50. 
Hacer una regla de 3 con el valor de referencia y el actual de rotation distance:
37.647  -  50+(k)
	x	-  50
 x= 37.647 * 50+k / 50  (No es un error, así es como funciona)
 
 En mi caso 45.88
 
 37.647 - 45.88
 x		- 50

x=37.647*45.88/50 = 34.545
2ª medición:

x=34.545 * 48.80/50 = 33.71592

Seguir iterando hasta que salgan exactamente 50 mm

## Ringing:
Generar el gcode de ringing_tower a una velocidad de 100 y espiralizado sin relleno.

```
SET_PRESSURE_ADVANCE ADVANCE=0
TUNING_TOWER COMMAND=SET_VELOCITY_LIMIT PARAMETER=ACCEL START=1250 FACTOR=100 BAND=5
```

Contamos 6 ondulaciones sin tener en cuenta la 1 y la 2
Eje x, distancia ondulación 3 a 8 = 11mm
Velocidad de impresión 80
x=80*6/11 =43.64
y=80*6/7=68.571

Se añade la línea:
```
[input_shaper]
shaper_freq_x: 43.64
shaper_freq_y: 68.57
```

Luego hay que hacer estos comandos:
```
SET_PRESSURE_ADVANCE ADVANCE=0
SET_INPUT_SHAPER SHAPER_TYPE=MZV
TUNING_TOWER COMMAND=SET_VELOCITY_LIMIT PARAMETER=ACCEL START=1250 FACTOR=100 BAND=5
```
Para probar el tipo de input shaper.

Se recomienda hacer otra prueba con otro input shaper diferente:
```
SET_PRESSURE_ADVANCE ADVANCE=0
SET_INPUT_SHAPER SHAPER_TYPE=EI
TUNING_TOWER COMMAND=SET_VELOCITY_LIMIT PARAMETER=ACCEL START=1250 FACTOR=100 BAND=5
```

## Pressure advance

Primero se edita el fichero printer.cfg para poner bajo la zona de extrusor la línea:
`PRESSURE_ADVANCE: 0`

Luego madamos este comando:

`SET_VELOCITY_LIMIT SQUARE_CORNER_VELOCITY=1 ACCEL=500`

Si tienes extrusor directo se pone este comando:

`TUNING_TOWER COMMAND=SET_PRESSURE_ADVANCE PARAMETER=ADVANCE START=0 FACTOR=.005`

Si tienes bowden se pone este otro:

`TUNING_TOWER COMMAND=SET_PRESSURE_ADVANCE PARAMETER=ADVANCE START=0 FACTOR=.020`

Una vez impreso el cubo, se busca un borde (dirección z)y se busca a qué altura está lo más liso posible sin abultamientos.
Esa distancia es la que se utilizará en la fórmula.

Pressure_advance= <start> + <measured_height> * factor
Factor sería el que seleccionamos en el comando de tuning. En este caso .020

pa = 0 + 25.59 * 0.020 = 0.5118





