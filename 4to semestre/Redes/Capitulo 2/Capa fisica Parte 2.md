## Conceptos basicos:
# Datos: 
Entidades que contienen significado o informacion que pueden ser digitales(bits) o analogicos.
# Señales:
Representacion electrica o electromagnetica de los datos, se propagan fisicamente por un medio
# Transmision de datos:
Proceso de enviar informacion entre un transmisor y un receptor a traves de un medio de tranmision mediante la variacion de una señal a lo largo del tiempo.
# Señal analogica: 
Varia suavemente en el tiempo
# Señal digital: 
Se mantiene en un nivel constante y luego cambia a otro nivel constante
# Señal periodica: 
Patron repetido en el tiempo y la aperiodica es que no se repite en el tiempo
# Tipos de canales segun el sentido de transmision
- Simplex: Una direccion. Ej: television 
- Duplex o semi-duplex: Cualquier direccion pero solo una vez. Ej: radio de policia
- Full duplex: Ambas direcciones al mismo tiempo. Ej: Telefono

## 2.4 De forma de ondas a bits
En esta seccion se demuestra como se transmiten las señales por los medios de transmision que pueden ser de multiplexacion o modulacion.
# Analisis de Fourier
Un matematico del sigo xix que decia que cualquier funcion periodica  con periodo T, se pueden representar como sumatorias (posiblemente infinitas) de senos y cosenos. La formula es:
$$
g(t) = \frac{1}{2}c + \sum_{n=1}^{\infty} a_n \sin(2\pi nft) + \sum_{n=1}^{\infty} b_n \cos(2\pi nft)
$$
C: es una constante que determina el valor medio de una funcion (valor de DC)
El **componente DC** (o componente de corriente continua) se refiere al **valor promedio de una señal** a lo largo del tiempo
an y bn: son la amplitud de sen y cos
T: es el periodo 
f: 1/T (frecuencia)

# Ancho de banda de la señal

El ancho de banda es el rango de frecuencias de una señal en donde se concentra la mayor parte de la energia de la señal. 
Cualquier canal de transmision tiene un rango o un ancho de banda de frecuencias que puede transmitir sin mucha atenuacion o distorcion. 
Una señal que va desde 0 hasta una frecuencia maxima se denominan señales de banda base. Las señales que cumplen un rango mayor de frecuencias se denominan señales de banda pasante. 
El ancho de banda es una propiedad fisica del medio de transmision que depende mucho de su longitud, grosor o material de un cable o fibra. 
# Señales de audio
El rango de frecuencias audibles es de 20hz a 20Khz (voz humana de 100hz a 7Khz), es facil convertir a señales electromagneticas como las que utilizan los telefonos y se limita a 3000-4000 hz a traves de filtros de frecuencia.

## Impedimentos en la transmision de datos
La señal recibida puede diferir en la señal transmitida, por ejemplo en la señal analogica puede ocurrir una degradacion de la calidad de la señal y en la señal digital puede haber errores de bits.
# Algunos de estos impedimentos:
# - Atenuacion: 
Perdida de potencia de la señal debido a la distancia y depende del medio. La señal recibida debe ser: suficientemente fuerte para ser detectada y suficientemente mas alta que el ruido.
Se implementa la potencia con amplificadores/repetidores
Mayor frecuencia = Mayor atenuacion 
$$
\text{dB} = 10 \log_{10} \frac{P_2}{P_1}
$$
- $P_2$: Es la potencia de salida (o potencia medida).
- $P_1$: Es la potencia de entrada (o potencia de referencia).
- Si el resultado es positivo, hubo una ganancia de señal. Si es negativo, hubo una atenuación.
# - Distorcion:
Es la perdida de la forma de la señal. No se solucionan con amplificadores, sino con regeneradores de señal o repetidores.
- **Distorcion por atenuacion:** Debido a que la atenuacion es diferente a distintas frecuencias, mayormente en señales analogicas
- **Distorcion por retardo:** La velocidad de propagacion varia con la frecuencia y es critico para señales digitales.
# -Ruido:
Señales adicionales insertadas entre el transmisor y receptor
- Ruido termico: ocurre por la agitacion de electrones uniformemente distruibuida en las frecuencias
- Ruido de intermodulacion: Se producen cuando señales de distintas frecuencias comparten un mismo medio de transmision.
- Crockstall o diafonia: Fenomeno de interferencia entre lineas de comunicacion proximas
- Ruidos impulsivos: Pulsos irregulares o picos. ej: truenos. Un pico de ruido podria corromper muchos bits (mayor fuente de errores en datos digitales)

## Velocidad maxima de transmision de un canal
La tasa de bits maxima posible (esta medida en bits sobre segundo) es en funcion de:
Ancho de banda - en ciclo sobre segundo o Hz
Ruido - en enlaces comunes
Tasa de errores - de bits corruptos
# Formula de Nyquist 
Obtuvo una ecuacion que representaba la velocidad maxima transmision en un canal sin ruido con un ancho de banda finito.
$$
C = 2B \log_2 M \text{ bits/sec}
$$
C= Max. tasa de datos
B= Ancho de banda del canal
M= Numeros de niveles discretos de la señal
# Formula de Shannon
Llevo el nivel a un canal con considerando el ruido aleatorio. La cantidad de ruido termico se mide en relacion de la potencia de la señal y la potencia del ruido denominado SNR. El SNR depende mucho de la distancia
$$
\text{SNR}_{\text{db}} = 10 \log_{10} (\text{SNR})
$$
$$
M = B \log_2 (1 + \text{SNR}) \text{ bits/sec}
$$

## Modulacion digital 
Los canales alambricos y no alambricos transportan señales analogicas como la intensidad de la luz o del sonido. Para enviar la informacion digital debemos idear señales analogicas que representen bits, ese proceso de conversion de bits a señales que lo representa se denomina  **modulacion digital**.
Los esquemas que convierten directamente bits a señales es la transmision de **banda base** y los esquemas que regulan la amplitud, fase, frecuencia de la señal portadora para transmitir bits dan lugar a los esquemas de **banda pasante.** 
El metodo que consiste en utilizar un solo cable para tranportar varias señales se denomina **multiplexacion**. 
**Las tecnicas de modulacion y multiplexado** se utilizan en canales por cable, fibra, satelite y inalambricos terrestres.

## Transmision en banda base
Transmitir bits a una señal digital. Va desde cero hasta un valor maximo
- La forma mas sencilla de modulacion digital consiste en utilizar la tension positiva para representar el bit "1" y la tension negativa para representar el bit "0".
- Conversion de bits a señales digitales: Pulsos de voltaje o amplitud discretos
- Cada pulso es un "elemento de la señal".
# Aspectos de codificacion
- Ancho de banda
- Existencia o no del Componente DC
- Utilizacion del clocking simple o compleja
- Facilidad para deteccion de errores 
- Interferencia de la señal e inmune al ruido
- Costo y complejidad
## NRZ
La presencia de luz representa el "1" y la ausencia de luz puede representar un "0". El NRZ se propaga por cable y una vez enviada el receptor convierte en bits la señal muestreando la señal en intervalos regulares de tiempo. Esta señal no se recibe exactamente como se envio debido a que estara atenuada y  distorcionada por el canal y el ruido del receptor.
Los codigos de linea son los esquemas mas complejos que pueden convertir bits en señales que correspondan mejor a consideraciones tecnicas.
**NRZ-L:** eficiente pero no se utiliza mucho es alta si tenemos "cero" y baja si tenemos "uno"
**NRZ-I:** Si hay "uno" cambia y si hay "cero" se mantiene
**Algunas diferencias:** 
- El ancho de banda
- Componente DC
- Sincronismo - el mejor es el de NRZ-I porque considera las largas cadenas de "unos"

# Eficiencia del ancho de banda
Con NRZ la señal puede ciclar entre los niveles positivo y negativo hasta cada 2 bits (en el caso de alternar 1s y 0s). Esto significa que necesitamos una anchura de al menos B/2 Hz cuando la velocidad de los bits es de B bits/seg.
La velocidad en la que cambia la señal se denomina velocidad de simbolo. La tasa de bits es la tasa de simbolos multiplicada por el numero de bits por simbolo.
- Necesidad del clocking entre el emisor y receptor
- Una cadena de 15 ceros puede confundirse con una de 16 ceros
- NRZ-I soluciona el problema de largas cadenas de "unos" pero no de "ceros"
- NRZ y NRZ-I son faciles de implementar, sin embargo no eliminan el componente DC
- Usos: Grabaciones magneticas, USB, transmision serial
# Bipolar o AMI
El nombre **AMI** significa "Inversión de Marca Alternada" y es un tipo de **señal balanceada**.
- **Funcionamiento:** Utiliza **tres niveles de voltaje** (+V, 0V, -V) para representar los datos.
    - El bit **"0"** se representa con **voltaje cero** (0V).
    - El bit **"1"** (llamado "marca") se representa alternando voltajes positivos y negativos (por ejemplo, +1V y luego -1V)
- Ventajas: Elimina el componente DC, Deteccion de errores
- Desventajas: Sigue siendo un problema las largas cadenas de "0"
# Pseudoternario
**Funcionamiento:** Es una variante del esquema AMI donde la lógica se invierte. El bit **"1"** se representa con **voltaje cero**, mientras que es el bit **"0"** el que **alterna** entre niveles positivos y negativos
# Manchester (Bifase)
- Este esquema se caracteriza por tener una **transmisión en el medio de cada periodo de bit**
- Transicion sirve como un reloj y datos
- "Bajo" a "alto" representa un 1
- "Alto" a "bajo" representa un 0
- Usado en IEEE 802.3 (Ethernet clasica)
- - **Ventaja:** Resuelve el problema de la recuperación del reloj (sincronización), ya que siempre hay una transición en cada bit, incluso con cadenas largas de ceros o unos.
- **Desventaja:** Requiere el **doble de ancho de banda** que otros esquemas (como NRZ) porque la señal cambia al doble de la tasa de bits
# Manchester(diferencial)
Los datos se representan de la siguiente manera: si existe una transición al **inicio del periodo de bit**, el valor es un **"0"** lógico; si no hay una transición al inicio, el valor es un **"1"** lógico. Este esquema se utiliza específicamente en el estándar de red **IEEE 802.5**
# Tasa de baudios
se define como la velocidad a la que se generan y cambian los **elementos de señal** o símbolos en un canal de comunicación. Es fundamental distinguir la tasa de baudios de la **tasa de bits**, ya que la tasa de bits representa el número de bits transmitidos por segundo y es igual a la tasa de símbolos multiplicada por el número de bits que transporta cada símbolo
# Mejoramiento del clocking
Cuando un valor se mantiene constante (no hay cambio por mucho tiempo) puede haber problemas del sincronismo. Algunos mejoramientos
- **Mezclar el clock con la señal (Manchester)**
- **Alterar la codificacion:**
	- **Codigo 4B/5B:** antes de codificar una señal se toman 4 bits en vez de uno a uno y se transmite como 5 bits, despues con esos 5 bits se codifica en un codigo distinto utilizando por ej NRZ-I.
		aumentamos la transmision de bits en un 20%
- **Scrambling**
	Transformar una cadena de 8 bits para pasar de una codificacion Bipolar a una B8ZS
	la V quiere decir cambio de nivel
	solo se hace cuando hay una cadena de 8 "ceros"
## Transmision de paso-banda o banda pasante
Para pasar de bits a una señal analogica. En este método, la señal se desplaza para ocupar un **rango superior de frecuencias** que no comienza en cero, situándose en torno a una frecuencia específica denominada **señal portadora** (_carrier_)
Tecnicas de modulacion
modulacion de desplazamiento por fase: se usa angulos 


fsk multinivel
Cada elemento de la señal representa mas de un bit
uso eficiente de ancho de banda
mas resistente al error
se utiliza:
