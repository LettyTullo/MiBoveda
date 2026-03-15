2.4 De forma de ondas a bits
En esta seccion se demuestra como se transmiten las señales por los medios de transmision que pueden ser de multiplexacion, modulacion.
Analisis de Fourtier
un matematico del sigo xix que decia que culaquier funcion periodica  con periodo T, se pueden representar como sumatorias (posiblemente infinitas) de senos y cosenos. La formula es:
$$
g(t) = \frac{1}{2}c + \sum_{n=1}^{\infty} a_n \sin(2\pi nft) + \sum_{n=1}^{\infty} b_n \cos(2\pi nft)
$$
C: es una constante que determina el valor medio de una funcion
an y bn: son la amplitud de sen y cos
T: es el periodo 
f: 1/T (frecuencia)
Señales con ancho de banda limitado 

El ancho de banda es la anchura de la banda de frecuencias que se transmite y las informaciones que se pueden transportar dependen solo del ancho y no de las frecuencias iniciales ni finales. Una señal que va desde 0 hasta una frecuencia maxima se denominan señales de banda base. Las señales que cumplen un rango mayor de frecuencias se denominan señales de banda pasante. 
El ancho de banda es una propiedad fisica del medio de transmision que depende mucho de su longitud, grosor o material de un cable o fibra

Velocidad maxima de transmision de un canal
Formula de Nyquist 
Obtuvo una ecuacion que representaba la velocidad maxima transmision en un canal sin ruido con un ancho de banda finito.
$$
C = 2B \log_2 M \text{ bits/sec}
$$
C= Max. tasa de datos
B= Ancho de banda del canal
M= Numeros de niveles discretos de la señal
Formula de Shannon
