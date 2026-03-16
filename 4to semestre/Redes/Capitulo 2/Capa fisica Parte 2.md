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
# Analisis de Fourtier
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
# - Distorcion:

## Velocidad maxima de transmision de un canal
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
Los canales alambricos y no alambricos transportan señales analogicas como la intensidad de la luz o del sonido. Para enviar la informacion digital debemos idear señales analogicas que representen bits, ese proceso de conversion de bits a señales que lo representa se denomina  modulacion digital
Los esquemas que convierten directamente bits a señales es la transmision de banda base y los esquemas que regulan la amplitud, fase, frecuencia de la señal portadora para transmitir bits dan lugar a los esquemas de banda pasante. 
El metodo que consiste en utilizar un solo cable para tranportar varias señales se denomina multiplezacion. 
Las tecnicas de modulacion y multiplexado se utilizan en canales por cable, fibra, satelite y inalambricos terrestres.

## Transmision en banda base
La forma mas sencilla de mosulacion digital consiste en utilizar la tension positiva para representar el bit "1" y la tension negativa para representar el bit "0"
NRZ
La presencia de luz representa el "1" y la ausencia de luz puede representar un "0". El NRZ se propaga por cable y una vez enviada el receptor convierte en bits la señal muestreando la señal en intervalos regulares de tiempo. Esta señal no se recibe exactamente como se envio debido a que estara atenuada y  distorcionada por el canal y el ruido del receptor.
Los codigos de linea son los esquemas mas complejos que pueden convertir bits en señales que correspondan mejor a consideraciones tecnicas.

## Eficiencia del ancho de banda
Con NRZ la señal puede ciclar entre los niveles positivo y negativo hasta cada 2 bits (en el caso de alternar 1s y 0s). Esto significa que necesitamos una anchura de al menos B/2 Hz cuando la velocidad de los bits es de B bits/seg.
La velocidad en la que cambia la señal se denomina velocidad de simbolo. La tasa de bits es la tasa de simbolos multiplicada por el numero de bits por simbolo.