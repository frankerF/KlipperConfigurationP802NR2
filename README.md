# Klipper Configuration P802NR2
Configuración de impresora Zonestar doble extrusor P802NR2 en klipper

## Configuración de pines
Los pines se consultan en el código fuente de marlin del fichero pins_ZRIB.h y config/sample-aliases.cfg de klipper en el apartado [board_pins arduino-mega].
``` xaml
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


